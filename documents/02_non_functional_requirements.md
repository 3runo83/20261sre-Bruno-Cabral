# Requisitos Não Funcionais (RNF)

Este documento descreve os atributos de qualidade e restrições técnicas do pipeline de ETL da Olist, baseados na norma ISO 25010.

## 1. Eficiência de Desempenho (Performance Efficiency)
*   **RNF-01: Vazão de Processamento**
    *   O pipeline deve processar o lote diário de 100 mil registros em uma janela de tempo reduzida para garantir a atualização dos dashboards.
    *   **SLI:** Tempo total de execução (E2E).
    *   **SLO:** < 120 minutos por lote de 100k registros.
    *   **Prioridade:** Must have
*   **RNF-02: Latência de Consulta**
    *   As consultas realizadas pelo Grafana no PostgreSQL devem ser otimizadas para não impactar a experiência do usuário.
    *   **SLI:** Latência de query SQL no dashboard.
    *   **SLO:** P95 < 2 segundos para consultas agregadas.
    *   **Prioridade:** Should have

## 2. Confiabilidade (Reliability)
*   **RNF-03: Idempotência de Carga**
    *   O sistema deve garantir que reexecuções do mesmo lote não gerem duplicidade de dados.
    *   **SLI:** Quantidade de registros duplicados (`order_id`) após reprocessamento.
    *   **SLO:** 0 registros duplicados.
    *   **Prioridade:** Must have
*   **RNF-04: Resiliência a Falhas Transitórias**
    *   O sistema deve implementar retries automáticos para falhas de rede ou conexão com o banco.
    *   **SLI:** Taxa de sucesso de recuperação automática em falhas de conexão.
    *   **SLO:** 100% de recuperação em até 3 tentativas.
    *   **Prioridade:** Must have

## 3. Adequação Funcional (Data Integrity)
*   **RNF-05: Integridade de Dados Carregados**
    *   Garantia de que a contagem de registros na origem corresponda ao destino (descontando erros validados).
    *   **SLI:** (Registros no Destino + Erros Logados) / Registros na Origem.
    *   **SLO:** 100%.
    *   **Prioridade:** Must have

## 4. Segurança (Security)
*   **RNF-06: Criptografia em Repouso**
    *   Os dados armazenados no PostgreSQL (RDS) devem estar protegidos.
    *   **SLI:** Status de criptografia do volume de dados.
    *   **SLO:** 100% dos dados criptografados (AES-256).
    *   **Prioridade:** Must have
*   **RNF-07: Controle de Acesso Mínimo**
    *   O pipeline deve utilizar credenciais com permissões restritas ao banco (apenas INSERT/UPDATE/SELECT).
    *   **SLI:** Quantidade de permissões administrativas (Superuser) usadas pelo pipeline.
    *   **SLO:** 0.
    *   **Prioridade:** Must have

## 5. Manutenibilidade (Maintainability)
*   **RNF-08: Testabilidade da Lógica**
    *   A lógica de transformação deve ser coberta por testes automatizados.
    *   **SLI:** Cobertura de código (Code Coverage) das funções de transformação.
    *   **SLO:** > 80%.
    *   **Prioridade:** Should have

## 6. Usabilidade (Usability)
*   **RNF-09: Visibilidade de Freshness**
    *   O dashboard deve indicar claramente a data/hora da última atualização dos dados.
    *   **SLI:** Presença de metadado "Last Update Timestamp" no dashboard.
    *   **SLO:** Presente em 100% das telas.
    *   **Prioridade:** Must have

## 7. Compatibilidade (Compatibility)
*   **RNF-10: Coexistência de Versões**
    *   O pipeline deve ser compatível com a versão estável do PostgreSQL em uso pela plataforma.
    *   **SLI:** Sucesso em testes de conectividade e sintaxe com PostgreSQL 15.x+.
    *   **SLO:** 100% de compatibilidade.
    *   **Prioridade:** Must have

## 8. Portabilidade (Portability)
*   **RNF-11: Independência de Ambiente**
    *   O código do pipeline deve ser executável em containers para facilitar a migração entre ambientes (Dev/Prod).
    *   **SLI:** Sucesso na execução do pipeline via Docker em ambiente local.
    *   **SLO:** Idêntico ao comportamento em produção.
    *   **Prioridade:** Should have

---

## Tabela de Resumo de Métricas (SLI/SLO)

| ID | Atributo ISO 25010 | SLI (Indicador) | SLO (Objetivo) | Fonte de Medição | Prioridade (MoSCoW) |
|:---|:---|:---|:---|:---|:---|
| RNF-01 | Eficiência | Tempo de execução total | < 120 min | Logs do Pipeline | Must have |
| RNF-02 | Eficiência | Latência de consulta | P95 < 2s | Logs do PostgreSQL | Should have |
| RNF-03 | Confiabilidade | Contagem de duplicatas | 0 | Query de auditoria | Must have |
| RNF-04 | Confiabilidade | Recuperação de falhas | 100% (3 retries) | Logs do Pipeline | Must have |
| RNF-05 | Adequação | Reconciliação Origem/Destino | 100% | Dashboard de QA | Must have |
| RNF-06 | Segurança | Criptografia ativa | Sim | AWS Console/API | Must have |
| RNF-07 | Segurança | Permissões excessivas | 0 | IAM/DB Roles | Must have |
| RNF-08 | Manutenibilidade | Cobertura de testes | > 80% | Relatório de Testes | Should have |
| RNF-09 | Usabilidade | Timestamp de atualização | Presente | Grafana UI | Must have |
| RNF-10 | Compatibilidade | Versão PostgreSQL | 15.x+ | Config do DB | Must have |
| RNF-11 | Portabilidade | Execução em Docker | Sucesso | CI/CD Pipeline | Should have |

## Premissas e Fontes de Medição
1. **Premissa:** O volume de 100k pedidos é a média diária; picos de 500k podem ocorrer e serão tratados via escalabilidade vertical temporária.
2. **Fonte de Medição:** CloudWatch Logs e Métricas do RDS serão as fontes primárias de verdade.
3. **Premissa:** A rede entre a origem dos arquivos CSV e o banco de dados tem latência desprezível (< 10ms).
