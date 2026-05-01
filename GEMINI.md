# Projeto ETL Olist - Engenharia de Dados & SRE

## Visão Geral do Projeto
Este projeto visa a criação de um pipeline de ETL robusto para o processamento de aproximadamente 100 mil pedidos diários do marketplace Olist. O fluxo consiste na ingestão de arquivos CSV, transformação dos dados e carga em um banco de dados analítico PostgreSQL, alimentando dashboards de visualização no Grafana.

O foco principal é a confiabilidade sistêmica, garantindo que o processo seja idempotente, observável e resiliente a falhas, evitando perda de dados ou processamentos duplicados.

### Arquitetura Planejada
- **Fonte de Dados:** Arquivos CSV.
- **Processamento:** Pipeline de ETL (tecnologia a definir).
- **Destino:** Banco de Dados Analítico (PostgreSQL).
- **Visualização:** Grafana.
- **Infraestrutura:** AWS (EC2, RDS).

## Comandos e Execução
O projeto encontra-se atualmente na fase de **Especificação de Requisitos**.
- [ ] **TODO:** Definir ferramenta de orquestração/linguagem (ex: Python, SQL, Airflow).
- [ ] **TODO:** Criar scripts de provisionamento de infraestrutura (Terraform/CloudFormation).
- [ ] **TODO:** Implementar lógica de ingestão e validação.

## Convenções de Desenvolvimento
- **Idioma:** Documentação e especificações em Português (pt-br).
- **Idempotência:** Toda carga de dados deve permitir reexecuções sem gerar duplicidade.
- **Atomicidade:** Operações de carga devem ser atômicas (sucesso total ou falha sem resíduos).
- **Observabilidade:** Logs estruturados para cada fase do ETL e alertas para falhas críticas.
- **Qualidade de Código:** Revisão obrigatória de qualquer lógica sugerida por IA para garantir conformidade técnica e evitar dívida técnica.

## Estrutura de Diretórios
- `specs/`: Contém os documentos de requisitos de negócio, funcionais e não funcionais.
  - `00_problem.md`: Contexto e stakeholders.
  - `01_non_functional.md`: Requisitos técnicos e restrições.
  - `02_functional.md`: Funcionalidades esperadas do sistema.
- `Documentos/`: Reservado para documentação complementar.
