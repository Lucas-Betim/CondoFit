# CondoFit

Projeto final de Engenharia de Software.

## Documentação

- [Plano de Testes](docs/plano-de-teste.md)
- [Backlog do Projeto](specs/backlog.md)
- [Registro de revisão humana da entrega](docs/registro-revisao-humana.md)

## Equipe

| Nome completo | GitHub | Papel | Perfil |
|---------------|--------|-------|--------|
| Gabriela Slonik da Silva | @Gabriela-SS | Product Owner | Convive com o problema do condomínio e conhece as necessidades dos moradores |
| Lucas Claro de Melo Almeida | @lucasclaro267 | Product Owner | Contribui na definição e priorização dos requisitos junto com a Gabriela |
| Rafael Schultz Drzevieski | @Schultz126 | Scrum Master | Facilita o processo, acompanha os prazos e organiza a entrega |
| Lucas Betim Nobre de Oliveira | @Lucas-Betim | Arquiteto técnico | Familiaridade com agentes de IA e banco de dados; assinatura Plus do Codex |
| Lavínia Borges da Silva | @Lavinia-tech | Qualidade / testes | Responsável pelos critérios de aceitação, pelos testes e pela revisão da qualidade |

### Justificativa dos papéis

- **Product Owner:** Gabriela convive com as consequências do problema, então é a mais capaz de definir o que o projeto precisa entregar e em que ordem. Lucas Claro contribui nessa função, apoiando a definição e a priorização dos requisitos; a decisão final de ordem do backlog fica com a Gabriela.
- **Scrum Master:** Facilita o processo do grupo e remove obstáculos, mantendo as entregas e o índice organizados.
- **Arquiteto técnico:** Lucas Betim é o membro mais familiarizado com agentes de IA e banco de dados e tem assinatura Plus do Codex, o que o torna a melhor escolha para as decisões técnicas críticas.
- **Qualidade e testes:** Cuida de que os critérios de aceitação sejam verificáveis e de que os testes validem as regras de reserva e cancelamento.

## Aprovação, integração e responsabilidade

| Pergunta | Regra |
|----------|-------|
| **Quem aprova?** | Um integrante da equipe que **não seja o autor** do PR. Para mudanças em arquitetura ou ADR, a aprovação inclui o arquiteto técnico (Lucas Betim). Para mudanças em requisitos, histórias ou backlog, inclui o Product Owner (Gabriela). |
| **Quem integra?** | O **autor do PR** faz o merge na `main`, somente depois da aprovação e com o CI passando. |
| **Quem responde pelo código integrado?** | O **autor humano do PR**, mesmo que o código tenha sido gerado com ajuda de IA. O revisor responde pela qualidade da sua revisão. |

Agentes de IA (Codex) não aprovam, não integram e não respondem por código: eles não fazem commit, push nem merge, e todo código gerado passa por revisão humana antes de entrar.

Antes de integrar, o autor deve conseguir explicar a mudança e os testes sem ajuda de IA.

### Cobertura das habilidades

O grupo cobre as quatro frentes: negociar com o cliente (Gabriela e Lucas Claro), programar e decidir a arquitetura (Lucas Betim), testar (Lavínia) e documentar (Rafael).

## Tema e escopo

- **Para quem:** moradores do condomínio (reservam) e a administração (consulta a agenda).
- **Problema:** sem um controle, vários moradores do mesmo apartamento ocupam a academia no mesmo dia e a agenda é feita de forma manual e desencontrada.
- **O que não entra:** frontend, pagamento, controle de acesso físico e integração com calendário externo.

**Linguagem:** Python (FastAPI).

## Definição de Pronto

Uma funcionalidade será considerada pronta quando atender a todos os seguintes critérios:

1. Todos os critérios de aceitação da história correspondente foram atendidos.
2. Os testes automatizados relacionados à funcionalidade foram executados e aprovados.
3. O código passou pelas verificações de lint e build no CI.
4. O Pull Request foi revisado e aprovado por pelo menos um integrante diferente do autor.
5. A implementação respeita a arquitetura e os padrões definidos no ADR.
6. A documentação, as especificações e os testes relacionados à funcionalidade foram atualizados, quando necessário.
7. O uso de ferramentas de IA durante o desenvolvimento foi registrado no documento de registro de uso de IA e, quando aplicável, na descrição do Pull Request.