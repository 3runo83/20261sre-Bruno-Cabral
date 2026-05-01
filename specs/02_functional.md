# Requisitos Funcionais

Este documento descreve as funcionalidades que o sistema de ETL deve fornecer para atender às necessidades do negócio.

## 1. Ingestão de Dados
*   **RF01 - Leitura de Arquivos:** O sistema deve ser capaz de ler arquivos no formato CSV contendo informações de pedidos.
*   **RF02 - Processamento de Volume:** O pipeline deve suportar a ingestão de pelo menos 100.000 registros por execução diária.
*   **RF03 - Validação de Esquema:** O sistema deve validar se o arquivo CSV possui as colunas obrigatórias e tipos de dados esperados antes de iniciar a carga.

## 2. Transformação e Carga (ETL)
*   **RF04 - Persistência em Banco Analítico:** Os dados processados devem ser armazenados em um banco de dados PostgreSQL.
*   **RF05 - Garantia de Idempotência:** O sistema deve garantir que, em caso de reprocessamento do mesmo arquivo, não ocorram duplicatas no banco de dados (ex: via operação de `UPSERT` ou limpeza prévia do lote).
*   **RF06 - Tratamento de Nulos:** Valores ausentes ou nulos em campos críticos (como ID do pedido ou valor) devem ser tratados conforme regras de negócio (ex: log de erro e descarte do registro).

## 3. Observabilidade e Logs
*   **RF07 - Registro de Execução:** O sistema deve gerar logs detalhados de cada etapa (Início, Extração, Transformação e Carga).
*   **RF08 - Status do Pipeline:** Deve ser possível identificar o sucesso ou falha de cada execução diária.
*   **RF09 - Alerta de Falha:** O sistema deve disparar uma notificação ou log de erro crítico imediatamente ao encontrar uma falha que impeça a conclusão do ETL.

## 4. Visualização de Dados
*   **RF10 - Integração com Dashboard:** Os dados no PostgreSQL devem estar estruturados de forma a facilitar a consulta pelo Grafana para geração de dashboards diários.
