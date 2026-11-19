# Plano de Testes

## 1. Objetivo

Definir como serão verificados os requisitos funcionais e não funcionais do CondoFit, garantindo que as funcionalidades atendam aos critérios de aceitação das histórias do backlog.

## 2. O que será testado

Serão testadas as funcionalidades de reserva, cancelamento e consulta de horários, além das regras de negócio relacionadas à identificação do morador, ao limite de reservas por apartamento e à persistência dos dados.

Também serão verificadas a separação entre as camadas da aplicação, a possibilidade de executar testes sem serviços externos reais e a integridade das reservas em operações simultâneas.

## 3. Níveis de teste

- **Teste unitário:** verifica regras e funções isoladamente, utilizando substitutos para o banco de dados e o serviço de e-mail.
- **Teste de integração:** verifica a comunicação entre as camadas e a persistência no PostgreSQL, incluindo operações simultâneas.
- **Teste de sistema:** verifica o funcionamento integrado das funcionalidades da aplicação.
- **Teste de aceitação:** verifica se os critérios de aceitação das histórias do backlog foram atendidos.

## 4. Item selecionado para TDD

Será utilizada a HU-ARQ-01 — Reservar a academia.

O desenvolvimento será iniciado pela regra que impede mais de uma reserva ativa por apartamento na mesma data. Primeiro será escrito um teste unitário que deverá falhar; em seguida, será implementada a regra até que o teste passe. Depois, o código poderá ser refatorado, mantendo os testes aprovados.

A situação de dez tentativas simultâneas será verificada separadamente por teste de integração com PostgreSQL.

## 5. Registro dos resultados

Cada teste deverá registrar:

- Identificação da história ou requisito;
- Cenário testado;
- Resultado esperado;
- Resultado obtido;
- Situação: aprovado ou reprovado;
- Data da execução e observações, quando necessário.

Os resultados serão registrados junto aos testes automatizados e/ou em documentação de execução. Testes reprovados deverão ser corrigidos e executados novamente.

## 6. Cronograma

| Etapa | Atividade |
|---|---|
| 1 | Revisar as histórias e seus critérios de aceitação. |
| 2 | Elaborar os testes e selecionar o cenário de TDD. |
| 3 | Implementar os testes unitários e desenvolver a funcionalidade seguindo TDD. |
| 4 | Executar testes de integração, incluindo persistência e concorrência. |
| 5 | Executar testes de sistema e aceitação. |
| 6 | Registrar resultados, corrigir falhas e atualizar a documentação. |

## 7. Critérios de aceitação prioritários

A tabela abaixo apresenta uma seleção inicial de histórias e critérios para orientar os testes. A seleção deverá ser ajustada conforme a priorização definida pela PO no `specs/backlog.md`.

| História | Critério de aceitação | Tipo de teste | Automatizável? |
|---|---|---|---|
| HU-ARQ-01 | Uma reserva válida deve ser criada quando o apartamento não possuir outra reserva ativa na mesma data. | Unitário | Sim |
| HU-ARQ-01 | Dez tentativas simultâneas para o mesmo apartamento e data devem resultar em exatamente uma reserva persistida. | Integração | Sim |
| HU-ARQ-01 | Uma reserva de outro apartamento no mesmo horário não deve impedir a criação da reserva. | Unitário/Integração | Sim |
| HU-ARQ-02 | O cancelamento autorizado deve manter o registro no histórico e liberar o limite diário do apartamento após a transação. | Unitário/Integração | Sim |
| HU-ARQ-02 | O morador não deve conseguir cancelar uma reserva pertencente a outro apartamento. | Unitário | Sim |
| HU-ARQ-02 | Os testes unitários das regras de cancelamento devem poder ser executados sem conexão real com PostgreSQL ou serviço de e-mail. | Unitário | Sim |
| HU-ARQ-03 | Somente moradores identificados e com conta válida podem consultar horários. | Sistema | Sim |
| HU-ARQ-03 | A consulta deve apresentar os horários permitidos e disponíveis para o apartamento do morador, sem considerar reservas de outros apartamentos como bloqueio. | Sistema | Sim |
| HU-ARQ-03 | A consulta não deve criar, alterar ou cancelar reservas. | Sistema | Sim |

**Observação:** a seleção das três histórias prioritárias e seus critérios deverá acompanhar a priorização final da PO. Caso outras histórias sejam escolhidas como prioritárias, a tabela deverá ser atualizada.
