# Requisitos Não Funcionais

Este documento detalha as restrições e qualidades técnicas esperadas para o pipeline de ETL da Olist.

## 1. Confiabilidade (Reliability)
*   **Idempotência:** O sistema deve permitir múltiplas execuções do mesmo lote de dados sem gerar duplicidade.
*   **Atomicidade:** Cada carga diária deve ser completada totalmente ou falhar de forma limpa (rollback), garantindo que o banco nunca fique em um estado parcial ou inconsistente.

## 2. Observabilidade (Observability)
*   **Logging:** Logs estruturados para cada etapa do processo (Extração, Transformação e Carga).
*   **Alerting:** Notificações imediatas em caso de falhas críticas de conexão ou processamento.
*   **Métricas de SLA:** Monitoramento do tempo de execução para garantir que os dados estejam disponíveis para os dashboards no horário previsto.

## 3. Performance e Eficiência (Efficiency)
*   **Vazão (Throughput):** O pipeline deve processar 100 mil registros em menos de 2 horas.
*   **Otimização de Recursos:** Uso eficiente de queries (`COPY`, `UPSERT`) no PostgreSQL para minimizar o consumo de CPU e IO.

## 4. Escalabilidade (Scalability)
*   **Crescimento de Volume:** O sistema deve ser capaz de suportar picos de demanda (ex: Black Friday com 500 mil pedidos) através de ajuste de recursos computacionais (escalabilidade vertical ou horizontal).

## 5. Disponibilidade (Availability)
*   **Janela de Operação:** O banco analítico e o Grafana devem estar disponíveis para consulta durante o horário comercial, operando de forma independente da execução do ETL.
*   **Resiliência:** Implementação de mecanismos de *retry* para falhas transitórias de rede ou indisponibilidade momentânea de serviços.

## 6. Integridade e Segurança (Data Integrity)
*   **Deduplicação:** Garantia de unicidade da chave primária dos pedidos no banco analítico.
*   **Sanitização:** Validação de integridade dos arquivos de origem antes da ingestão para evitar a propagação de dados malformados.

## 7. Custo-Eficiência (Cost Efficiency)
*   **Gestão de Recursos AWS:** Uso otimizado de instâncias e desligamento de recursos não utilizados após o término do processamento.
*   **Retenção de Dados:** Política de gerenciamento de armazenamento para evitar custos excessivos com storage de dados históricos não utilizados.

## 8. Manutenibilidade (Maintainability)
*   **Padrões de Código:** Seguir boas práticas de desenvolvimento e garantir revisões rigorosas de códigos sugeridos por ferramentas de IA.
*   **Documentação Técnica:** Manter o mapeamento de dados e a arquitetura do pipeline atualizados para facilitar a manutenção por diferentes membros da equipe.
