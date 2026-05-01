# Descrição do Problema

## Contexto
A Olist necessita processar aproximadamente 100 mil pedidos diários provenientes de seu marketplace. Esses dados devem ser carregados em um banco de dados analítico para alimentar dashboards de visualização diária. O processo de ETL (Extração, Transformação e Carga) deve ser extremamente confiável, garantindo que seja idempotente, observável e resiliente a falhas parciais. É crítico que não ocorra perda de dados, duplicação ou falhas silenciosas que comprometam a integridade das decisões de negócio.

## 1. Stakeholders
*   **Operações Olist (Negócio):** Consumidores finais dos insights e dashboards para tomada de decisão.
*   **Equipe de Engenharia de Dados:** Responsáveis pelo desenvolvimento, manutenção e evolução do pipeline de ETL.
*   **Clientes Internos:** Usuários que dependem da acurácia dos dados para análises diárias.
*   **Plataforma / SRE:** Responsáveis pela infraestrutura, escalabilidade e garantia dos níveis de serviço (SLAs).

## 2. Fluxos Críticos
*   **Ingestão Diária:** Processamento de arquivos CSV e carga no banco de dados PostgreSQL.
*   **Atualização de Dashboards:** Disponibilização dos dados processados no Grafana para visualização.
*   **Monitoramento de SLA:** Acompanhamento rigoroso dos prazos de entrega e disponibilidade dos dados.

## 3. Modos de Falha Identificados
*   **Dados Corrompidos:** Recebimento de arquivos CSV incompletos ou com formatação inválida na origem.
*   **Duplicação de Registros:** Falha na idempotência durante reprocessamentos, gerando dados inconsistentes.
*   **Interrupção de Infraestrutura:** Queda de instâncias EC2 ou serviços de nuvem durante a execução do pipeline.
*   **Indisponibilidade de Banco de Dados:** Falhas de conexão ou erros de escrita no PostgreSQL.

## 4. Riscos Sistêmicos
*   **Corrupção Silenciosa:** Erros que não interrompem o processo, mas resultam em dados incorretos no destino final.
*   **Dados Obsoletos (Stale Data):** Dashboards exibindo informações desatualizadas sem que haja um alerta claro para o usuário.
*   **Ineficiência de Custos:** Consumo excessivo de recursos AWS devido a processos mal otimizados.
*   **Dívida Técnica:** Implementações rápidas ou código gerado por IA sem a devida revisão e validação técnica.
