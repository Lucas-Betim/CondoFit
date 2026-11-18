# Backlog do Produto — CondoFit

Autora: Gabriela - Product Owner  
Última atualização: 18/11/2026

**Nota de planejamento:** Os itens abaixo estão ordenados por prioridade de entrega de valor (1 = maior prioridade). Conforme definição do ciclo atual, não tem estimativa em horas nesta etapa.

| Prioridade | ID | Tipo | História / Item | Papel / Origem | Sprint |

| **1** | **ITEM-ARQ-01** | Arquitetura | Estruturar projeto base, modelo conceitual de `Reserva` e camada de serviços/dados conforme ADR-001 e RN-ARQ-05 | Arquiteto Técnico | **Sprint 1** |
| **2** | **HU-PO-01** | Funcionalidade | **Reservar horário na academia:** Como morador do condomínio, quero agendar um horário de uso da academia para garantir minha vaga e evitar superlotação no espaço. | Product Owner | **Sprint 1** |
| **3** | **HU-ARQ-01** | Funcionalidade | **Consultar reservas por data:** Como administrador do condomínio, quero consultar as reservas da academia por data para acompanhar a utilização do espaço. | Arquiteto Técnico | **Sprint 1** |
| **4** | **HU-PO-02** | Funcionalidade | **Cancelar própria reserva:** Como morador do condomínio, quero cancelar uma reserva ativa que fiz anteriormente para liberar o horário a outros moradores. | Product Owner | **Sprint 1** |
| **5** | **HU-ARQ-02** | Funcionalidade | **Bloquear horários para manutenção:** Como administrador do condomínio, quero bloquear horários ou datas da academia para manutenção para impedir agendamentos durante períodos indisponíveis. | Arquiteto Técnico | Backlog (Sprint 2) |
| **6** | **ITEM-DOC-01** | Documentação | Consolidar especificações de requisitos, mini-spec, contratos de API e guia de uso no repositório (`README.md` e `specs/`) | Product Owner / Equipe | **Sprint 1** |

---

### Resumo do Escopo da Sprint 1
A **Sprint 1** foca no fluxo mínimo viável (MVP) do CondoFit: estruturar a arquitetura base (`ITEM-ARQ-01`), permitir que o morador reserve e cancele horários (`HU-PO-01`, `HU-PO-02`), garantir que o administrador visualize a agenda do dia (`HU-ARQ-01`) e manter a documentação sincronizada (`ITEM-DOC-01`). O bloqueio de manutenção (`HU-ARQ-02`) entra logo na sequência como prioridade máxima da Sprint 2.
