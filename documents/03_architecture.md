# Arquitetura do Sistema (RM-ODP)

Este documento descreve a arquitetura do pipeline de ETL da Olist utilizando o framework RM-ODP, garantindo o alinhamento com os requisitos funcionais e não funcionais.

---

## 1. Enterprise Viewpoint (Visão de Negócio)
**Objetivo:** Processar e disponibilizar dados de marketplace para tomada de decisão diária.
- **Escopo:** Ingestão de 100k pedidos/dia.
- **Stakeholders:** Operações Olist, Engenharia de Dados, SRE.
- **Processo:** Extração de CSV -> Validação -> Transformação -> Carga Analítica -> Dashboard.
- **Componentes Atendidos:** RF-001, RF-008.

## 2. Information Viewpoint (Visão de Informação)
**Objetivo:** Definir os modelos de dados e fluxos de estado.
- **Modelos:**
    - **Bronze (Landing):** Dados brutos do CSV com metadados de ingestão.
    - **Silver (Trusted):** Dados validados, tipados e deduplicados.
    - **Gold (Analytics):** Tabelas otimizadas para consulta no Grafana.
- **Fluxos:** Validação de esquema (RF-002), Tratamento de tipos (RF-006).
- **Integridade:** RNF-05 (Reconciliação 100%).

## 3. Computational Viewpoint (Visão Computacional)
**Objetivo:** Estrutura funcional e interfaces dos componentes.
- **ETL Engine (App):**
    - `Reader`: Lê CSV (RF-001).
    - `Validator`: Checa esquema e tipos (RF-002, RF-006).
    - `Transformer`: Deduplica e limpa dados (RF-003).
    - `Writer`: Executa UPSERT no banco (RF-004).
- **Monitor:** Captura logs e métricas (RF-005).
- **AlertManager:** Notifica falhas críticas (RF-007).

## 4. Engineering Viewpoint (Visão de Engenharia)
**Objetivo:** Distribuição física e mecanismos de infraestrutura.
- **Compute Cluster:** Instâncias EC2 em Docker (RNF-11) para isolamento.
- **Storage:** S3 para persistência dos arquivos CSV brutos.
- **Database Server:** RDS PostgreSQL com criptografia ativada (RNF-06).
- **Network:** VPC com Subnets Privadas para o RDS e Acesso IAM restrito (RNF-07).
- **Mecanismos:** Retries automáticos (RNF-04), Janela de 120min (RNF-01).

## 5. Technology Viewpoint (Visão de Tecnologia)
**Objetivo:** Tecnologias específicas (Restrições: AWS Academy Learner Lab).
- **Linguagem:** Python 3.11+ (Pandas/SQLAlchemy) para lógica de ETL.
- **Banco de Dados:** Amazon RDS PostgreSQL 15.x.
- **Orquestração:** Cron jobs ou scripts acionados via CLI na EC2 (Evitando Glue/Airflow por custo/limite).
- **Visualização:** Grafana (instalado em EC2 ou via container).
- **Logs:** AWS CloudWatch Logs.

---

## Mapeamento de Componentes (Resumo)

| Componente | RFs Atendidos | RNFs Atendidos |
|:---|:---|:---|
| ETL Python Script | RF-001, RF-002, RF-003, RF-004, RF-006 | RNF-01, RNF-03, RNF-04, RNF-08 |
| RDS PostgreSQL | RF-004, RF-008 | RNF-02, RNF-06, RNF-10 |
| CloudWatch / Logger | RF-005, RF-007 | RNF-09 |
| Docker / EC2 | - | RNF-11 |
| IAM Roles | - | RNF-07 |

---

## Architecture Decision Records (ADRs)

### ADR 001: Uso de Python em EC2/Docker em vez de AWS Glue
- **Contexto:** Necessidade de processar 100k registros em ambiente AWS Academy, que possui restrições de créditos e serviços (sem Glue).
- **Decisão:** Utilizar scripts Python conteinerizados rodando em instâncias EC2.
- **Consequência:** Maior controle sobre o código e custo reduzido, porém exige gerenciamento manual de escalabilidade e patching da instância.

### ADR 002: Persistência via UPSERT no PostgreSQL
- **Contexto:** O pipeline deve ser idempotente (RNF-03) e lidar com reprocessamentos sem duplicar dados (RF-004).
- **Decisão:** Implementar a carga final utilizando a cláusula `ON CONFLICT DO UPDATE` do PostgreSQL.
- **Consequência:** Garante integridade e idempotência com baixo overhead, simplificando a lógica de recuperação de falhas.

### ADR 003: Validação de Esquema "Fail-Fast"
- **Contexto:** Evitar custos de processamento e corrupção de dados por arquivos malformados (RF-002).
- **Decisão:** O pipeline deve validar o cabeçalho e tipos básicos logo após a leitura inicial, interrompendo a execução se inválido.
- **Consequência:** Protege o banco de dados de sujeira e economiza tempo de execução, garantindo conformidade com o RNF-05.
