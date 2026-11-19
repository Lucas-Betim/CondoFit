# Plano de Testes

## 1. Objetivo

Definir como serão verificados os requisitos funcionais e não funcionais do CondoFit, garantindo que as funcionalidades atendam aos critérios de aceitação das histórias priorizadas no backlog.

## 2. O que será testado

Serão testadas as funcionalidades de reserva, cancelamento e consulta de reservas por data, incluindo as regras de identificação do morador, limite de reservas por apartamento, duração das sessões, persistência dos dados e simultaneidade.

Também será verificada a latência das operações, conforme o requisito não funcional definido na mini-spec.

## 3. Níveis de teste

- **Teste unitário:** verifica regras e funções isoladamente, utilizando substitutos para o banco de dados e o serviço de e-mail.
- **Teste de integração:** verifica a comunicação entre as camadas e a persistência no PostgreSQL, incluindo operações simultâneas.
- **Teste de sistema:** verifica o funcionamento integrado das funcionalidades da aplicação.
- **Teste de aceitação:** verifica se os critérios de aceitação das histórias priorizadas foram atendidos.

## 4. Item selecionado para TDD

Será utilizada a HU-PO-01 — Reservar horário na academia.

O desenvolvimento será iniciado pela regra que impede mais de uma reserva ativa por apartamento na mesma data. Primeiro será escrito um teste unitário que deverá falhar; em seguida, será implementada a regra até que o teste passe. Depois, o código poderá ser refatorado, mantendo os testes aprovados.

A situação de dez tentativas simultâneas será verificada separadamente por teste de integração com PostgreSQL, garantindo que apenas uma reserva seja persistida.

## 5. Registro dos resultados

Os resultados das execuções serão documentados em `docs/resultados-teste.md`.

Para cada teste, serão registrados a história ou requisito relacionado, o cenário, o resultado esperado, o resultado obtido, a situação (aprovado ou reprovado), a data da execução e eventuais observações.

Os testes automatizados ficarão no diretório `tests/`. Quando executados pelo GitHub Actions, seus logs e relatórios também poderão ser consultados na execução correspondente. Falhas deverão ser corrigidas e os testes executados novamente.

## 6. Cronograma

O cronograma será organizado conforme as sprints definidas no backlog do projeto.

| Sprint | Atividades |
|---|---|
| Sprint 1 | Revisar critérios de aceitação e preparar os casos de teste das histórias HU-PO-01, HU-ARQ-07 e HU-PO-02. |
| Sprint 1 | Desenvolver a reserva utilizando TDD e executar testes unitários e de integração, incluindo concorrência. |
| Sprint 1 | Executar testes de sistema e aceitação para reserva, consulta por data e cancelamento. |
| Sprint 1 | Registrar os resultados, corrigir falhas e atualizar a documentação. |
| Sprint 2 | Após o detalhamento arquitetural dos bloqueios de manutenção, elaborar e executar os testes correspondentes à HU-ARQ-08. |

## 7. Critérios de aceitação prioritários

A tabela apresenta critérios selecionados das três histórias funcionais de maior prioridade no backlog.

| História | Critério de aceitação | Tipo de teste | Automatizável? |
|---|---|---|---|
| HU-PO-01 | Uma reserva válida deve ser criada com duração de uma hora e vinculada ao apartamento identificado. | Unitário/Integração | Sim |
| HU-PO-01 | Dez tentativas simultâneas para o mesmo apartamento e data devem resultar em exatamente uma reserva persistida. | Integração | Sim |
| HU-PO-01 | Uma reserva de outro apartamento no mesmo horário não deve impedir a criação da reserva. | Unitário/Integração | Sim |
| HU-ARQ-07 | Uma consulta por data válida deve apresentar as reservas daquele dia, com apartamento, horário e status. | Integração/Sistema | Sim |
| HU-ARQ-07 | A consulta deve apresentar somente reservas da data informada, sem incluir registros de outras datas. | Integração | Sim |
| HU-ARQ-07 | O tempo de resposta da consulta por data deve ser inferior a 2.000 ms no percentil 95 (p95). | Desempenho | Sim |
| HU-PO-02 | O cancelamento autorizado deve alterar o status para cancelada e manter o registro no histórico. | Unitário/Integração | Sim |
| HU-PO-02 | Após o cancelamento, o apartamento deve poder realizar uma nova reserva válida na mesma data. | Integração | Sim |
| HU-PO-02 | O morador não deve conseguir cancelar uma reserva pertencente a outro apartamento. | Unitário | Sim |
