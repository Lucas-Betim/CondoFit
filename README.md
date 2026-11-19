# CondoFit

Sistema de reserva da academia de um condomínio.

## Equipe

| Nome completo | GitHub | Papel | Perfil |
|---------------|--------|-------|--------|
| Gabriela Slonik da Silva | @Gabriela-SS | Product Owner | Convive com o problema do condomínio e conhece as necessidades dos moradores |
| Lucas Claro | @lucasclaro267 | Product Owner | Contribui na definição e priorização dos requisitos junto com a Gabriela |
| Rafael Schultz Drzevieski | @Schultz126 | Scrum Master | Facilita o processo, acompanha os prazos e organiza a entrega |
| Lucas Betim Nobre de Oliveira | @Lucas-Betim | Arquiteto técnico | Familiaridade com agentes de IA e banco de dados; assinatura Plus do Codex |
| Lavínia Borges da Silva | @Lavinia-tech | Qualidade / testes | Responsável pelos critérios de aceitação, pelos testes e pela revisão da qualidade |

### Justificativa dos papéis

- **Product Owner:** Gabriela convive com as consequências do problema, então é a mais capaz de definir o que o projeto precisa entregar e em que ordem. Lucas Claro contribui nessa função, apoiando a definição e a priorização dos requisitos; a decisão final de ordem do backlog fica com a Gabriela.
- **Scrum Master:** Facilita o processo do grupo e remove obstáculos, mantendo as entregas e o índice organizados.
- **Arquiteto técnico:** Lucas Betim é o membro mais familiarizado com agentes de IA e banco de dados e tem assinatura Plus do Codex, o que o torna a melhor escolha para as decisões técnicas críticas.
- **Qualidade e testes:** Cuida de que os critérios de aceitação sejam verificáveis e de que os testes validem as regras de reserva e cancelamento.

### Cobertura das habilidades

O grupo cobre as quatro frentes: negociar com o cliente (Gabriela e Lucas Claro), programar e decidir a arquitetura (Lucas Betim), testar (Lavínia) e documentar (Rafael).

## Tema e escopo

- **Para quem:** moradores do condomínio (reservam) e a administração (consulta a agenda).
- **Problema:** sem um controle, vários moradores do mesmo apartamento ocupam a academia no mesmo dia e a agenda é feita de forma manual e desencontrada.
- **O que não entra:** frontend, pagamento, controle de acesso físico e integração com calendário externo.

**Linguagem:** Python (FastAPI).

## Definição de pronto

1. Testes automatizados e lint passam no CI.
2. O código foi revisado e aprovado por um integrante que não é o autor do PR.
3. Os critérios de aceitação da história foram verificados.
4. Regras de arquitetura do ADR-001 foram respeitadas.
5. Spec, documentação e testes afetados foram atualizados.
6. O uso de IA, se houve, está registrado na descrição do PR.
