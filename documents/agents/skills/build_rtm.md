# Skill: Construção da Matriz de Rastreabilidade de Requisitos (RTM)

## Objetivo
Mapear a relação entre Requisitos Funcionais (RF), Requisitos Não Funcionais (RNF) e os respectivos Planos de Teste, garantindo que cada funcionalidade seja validada sob as métricas de qualidade definidas.

## Entrada
- `documents/01_functional_requirements.md`
- `documents/02_non_functional_requirements.md`

## Processo
1. **Identificação:** Listar todos os IDs de RF e RNF.
2. **Correlação:** Identificar quais RNFs impactam ou restringem cada RF.
3. **Mapeamento de Testes:** Associar cada par (RF/RNF) a um plano de teste (Carga, Segurança, Modelagem).
4. **Validação de Cobertura:** Garantir que nenhum RF ou RNF fique sem um mecanismo de validação.

## Saída (documents/04_rtm.md)
Tabela contendo:
- ID do RF
- Descrição Curta
- RNFs Relacionados
- Plano de Teste Associado
- Status de Cobertura (Coberto / Pendente)
