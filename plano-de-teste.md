| História | Critério de aceitação | Tipo | Automatizável? |
|---|---|---|---|
| **HU-ARQ-01 — Reservar a academia** | 1. Sem reserva ativa e em horário permitido, o sistema deve criar uma reserva de exatamente uma hora para o apartamento identificado. | Funcional | Sim |
| | 2. Uma reserva ativa do mesmo apartamento no mesmo dia deve impedir uma nova reserva, enquanto uma reserva de outro apartamento no mesmo horário não deve impedir o agendamento. | Funcional | Sim |
| | 3. Diante de dez tentativas simultâneas válidas para o mesmo apartamento e dia, o sistema deve persistir exatamente uma reserva ativa. | Funcional | Sim |
| **HU-ARQ-02 — Cancelar para poder remarcar** | 1. Ao cancelar uma reserva autorizada, o sistema deve preservar seu registro como cancelado e liberar o limite diário do apartamento após a confirmação da transação. | Funcional | Sim |
| | 2. Após o cancelamento, uma nova reserva válida para o mesmo dia deve ser permitida, mantendo a reserva anterior no histórico e apenas uma reserva ativa. | Funcional | Sim |
| | 3. Os testes unitários do serviço devem executar sem conexão real com PostgreSQL e sem envio real de e-mail. | Não funcional (testabilidade) | Sim |
| **HU-ARQ-03 — Consultar horários disponíveis** | 1. Somente usuários com conta existente e identificados no sistema podem consultar os horários disponíveis. | Funcional | Sim |
| | 2. Ao informar uma data inválida, o sistema deve recusar a consulta e informar que é necessário fornecer uma data válida. | Funcional | Sim |
| | 3. Ao consultar uma data válida, o sistema deve apresentar somente os horários permitidos e disponíveis. Caso não existam horários disponíveis, deve informar essa condição, sem criar, alterar ou cancelar reservas. | Funcional | Sim |
