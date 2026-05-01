# Matriz de Rastreabilidade de Requisitos (RTM)

Esta matriz vincula os Requisitos Funcionais (RF) aos seus respectivos Requisitos Não Funcionais (RNF) e aos planos de teste responsáveis por sua validação.

| ID RF | Descrição Funcional | RNFs Relacionados | Plano de Teste | Status |
|:---|:---|:---|:---|:---|
| [RF-001] | Ingestão de Arquivos CSV | RNF-01, RNF-04, RNF-11 | Carga / Modelagem | Coberto |
| [RF-002] | Validação de Esquema | RNF-05, RNF-10 | Modelagem | Coberto |
| [RF-003] | Deduplicação em Memória | RNF-03, RNF-08 | Modelagem / Carga | Coberto |
| [RF-004] | Carga via UPSERT | RNF-01, RNF-03, RNF-04 | Carga / Modelagem | Coberto |
| [RF-005] | Registro de Logs | RNF-02, RNF-04 | Modelagem | Coberto |
| [RF-006] | Tratamento de Tipos | RNF-05, RNF-10 | Modelagem | Coberto |
| [RF-007] | Notificação de Falha | RNF-04, RNF-09 | Segurança / Modelagem | Coberto |
| [RF-008] | Disponibilidade Grafana | RNF-02, RNF-09 | Carga | Coberto |

## Mapeamento Adicional de RNFs de Infraestrutura/Segurança

| ID RNF | Descrição de Qualidade | Plano de Teste Associado | Status |
|:---|:---|:---|:---|
| RNF-06 | Criptografia em Repouso | Segurança | Coberto |
| RNF-07 | Controle de Acesso Mínimo | Segurança | Coberto |
| RNF-08 | Cobertura de Testes (>80%) | Modelagem (CI/CD) | Coberto |
| RNF-11 | Independência (Docker) | Modelagem | Coberto |

## Legenda de Planos de Teste
- **Carga:** `05_test_plan_load.md` (Performance e Escalabilidade)
- **Segurança:** `06_test_plan_security.md` (Criptografia e Acessos)
- **Modelagem:** `07_test_plan_modeling.md` (Integridade, Tipagem e Lógica)
