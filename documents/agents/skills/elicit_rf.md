# Skill: Elicitação de Requisitos Funcionais (RF)

## Objetivo
Atuar como um Analista de Requisitos Sênior para identificar, extrair e documentar as funcionalidades necessárias para o sucesso do pipeline de ETL da Olist, garantindo que o comportamento esperado do sistema esteja claro e testável.

## Contexto de Atuação
O foco é o pipeline de dados: Processamento de arquivos CSV -> Banco PostgreSQL -> Dashboard Grafana (100k pedidos/dia).

## Diretrizes de Escrita
Cada requisito funcional deve seguir o formato:
- **ID:** [RF-XXX]
- **Nome:** Título curto e direto.
- **Descrição:** Explicação clara do que o sistema deve fazer (deve começar com um verbo de ação).
- **Prioridade:** [Alta / Média / Baixa]
- **Critério de Aceite:** Como validar que o requisito foi atendido.

## Processo de Elicitação
1. **Análise de Origem:** Revisar o `specs/00_problem.md` para entender as necessidades dos stakeholders.
2. **Decomposição do Fluxo:**
   - **Ingestão:** Como os dados entram?
   - **Processamento:** O que acontece com o dado (limpeza, filtros, transformações)?
   - **Carga:** Como o dado é persistido?
   - **Observabilidade:** Como o sistema reporta o que fez?
3. **Validação:** Verificar se o requisito é "O Que" (Funcional) e não "Como" (Não Funcional).

## Template de Saída (para 01_functional_requirements.md)
```markdown
### [RF-001] Nome do Requisito
- **Descrição:** O sistema deve...
- **Prioridade:** Alta
- **Critério de Aceite:** Ao executar X, o resultado deve ser Y.
```
