# Requisitos Funcionais (RF)

Este documento detalha as funcionalidades obrigatórias do pipeline de ETL da Olist, conforme definido pelo processo de elicitação de requisitos.

---

### [RF-001] Ingestão de Arquivos CSV
- **Descrição:** O sistema deve ser capaz de localizar e ler arquivos no formato CSV em um diretório de origem pré-definido.
- **Prioridade:** Alta
- **Critério de Aceite:** O sistema deve carregar todos os registros presentes no CSV para a memória ou buffer de processamento sem truncar dados.

### [RF-002] Validação de Esquema de Origem
- **Descrição:** O sistema deve validar se o arquivo CSV contém os cabeçalhos obrigatórios (ex: `order_id`, `customer_id`, `order_status`, `order_purchase_timestamp`) antes de iniciar o processamento.
- **Prioridade:** Alta
- **Critério de Aceite:** Arquivos com colunas ausentes devem ser rejeitados com um log de erro específico.

### [RF-003] Deduplicação de Registros em Memória
- **Descrição:** O sistema deve identificar e remover registros duplicados dentro do próprio lote de processamento diário antes da carga no banco.
- **Prioridade:** Média
- **Critério de Aceite:** Se o CSV contiver duas linhas idênticas para o mesmo `order_id`, apenas uma deve ser processada para carga.

### [RF-004] Carga de Dados via UPSERT
- **Descrição:** O sistema deve realizar a carga no banco PostgreSQL utilizando a lógica de "UPSERT" (Update or Insert) baseada na chave primária do pedido.
- **Prioridade:** Alta
- **Critério de Aceite:** Se um pedido já existir no banco, seus dados devem ser atualizados; caso contrário, um novo registro deve ser inserido.

### [RF-005] Registro de Logs de Execução
- **Descrição:** O sistema deve gerar logs detalhados para cada etapa do pipeline: Início, Leitura, Validação, Transformação e Carga Final.
- **Prioridade:** Alta
- **Critério de Aceite:** Os logs devem ser consultáveis e conter carimbo de tempo (timestamp) e o status da operação.

### [RF-006] Tratamento de Tipos de Dados
- **Descrição:** O sistema deve converter as strings do CSV para os tipos corretos no banco de dados (ex: converter carimbos de tempo para o tipo `TIMESTAMP`, valores monetários para `DECIMAL`).
- **Prioridade:** Alta
- **Critério de Aceite:** Nenhuma falha de conversão de tipo deve ocorrer silenciosamente; erros de conversão devem ser logados.

### [RF-007] Notificação de Falha Crítica
- **Descrição:** O sistema deve emitir um alerta (ou registro de erro crítico no log) caso a conexão com o banco PostgreSQL falhe ou o disco de origem esteja inacessível.
- **Prioridade:** Alta
- **Critério de Aceite:** Em caso de erro de infraestrutura, o processo deve ser interrompido e o erro reportado imediatamente.

### [RF-008] Disponibilização para Dashboards
- **Descrição:** Os dados devem ser persistidos em tabelas normalizadas ou otimizadas para leitura que permitam a conexão direta do Grafana.
- **Prioridade:** Média
- **Critério de Aceite:** Uma consulta SQL simples no Grafana deve ser capaz de retornar o volume total de pedidos por dia.
