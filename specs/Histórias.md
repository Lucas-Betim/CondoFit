# Histórias de usuário - Product Owner

Autores: Gabriela, Lucas Claro - Product Owner. Status: **Aprovadas em 18/11/2026**.

## HU-PO-01: Reservar horário na academia
Como morador do condomínio, quero me identificar pelo meu apartamento e agendar um horário de 1 hora na academia (recebendo confirmação por e-mail) para garantir minha vaga no espaço.

**Fluxo principal:** o morador informa seu apartamento, escolhe uma data futura e um horário disponível, e confirma o agendamento com duração fixa de 1 hora. O sistema valida se o apartamento não possui outra reserva ativa no mesmo dia e se há vaga disponível no horário, registra a reserva como `ativa` e envia confirmação por e-mail.

**Fluxos alternativos:** se o apartamento já possuir uma reserva ativa naquela mesma data, o sistema impede o novo agendamento. Se o horário desejado estiver bloqueado para manutenção ou com lotação máxima atingida, o sistema informa a indisponibilidade. Se o morador tentar agendar em data passada ou fora do horário de funcionamento, o sistema recusa a operação.

**Critérios de aceitação:**
1. Exigir identificação/vínculo válido com o apartamento do morador.
2. Definir duração fixa de 1 hora por reserva.
3. Permitir no máximo 1 reserva ativa por apartamento por dia, permitindo que apartamentos diferentes reservem o mesmo horário dentro da capacidade do espaço.
4. Bloquear agendamentos em períodos de manutenção, datas passadas ou horários lotados.
5. Registrar a reserva com status `ativa` e enviar confirmação por e-mail ao morador.

**Referências:** RF-01, RF-02, RF-04 e camada de serviços (ADR-001).

---

## HU-PO-02: Cancelar própria reserva e permitir remarcação
Como morador do condomínio, quero cancelar uma reserva ativa do meu apartamento para liberar a vaga e poder remarcar outro horário no mesmo dia, se necessário.

**Fluxo principal:** o morador identifica seu apartamento e localiza sua reserva ativa, solicita o cancelamento e confirma a ação. O sistema altera o status da reserva para `cancelada`, envia e-mail de confirmação do cancelamento e libera imediatamente o apartamento para realizar uma nova reserva no mesmo dia.

**Fluxos alternativos:** se a reserva já estiver cancelada ou se o horário da reserva já tiver passado, o sistema informa que o cancelamento não é permitido.

**Critérios de aceitação:**
1. Permitir alterar o status de uma reserva `ativa` para `cancelada` mediante confirmação.
2. Liberar imediatamente a vaga no horário e habilitar o apartamento para realizar uma nova reserva (remarcação) no mesmo dia.
3. Manter o registro da reserva cancelada no histórico para auditoria e consulta administrativa (sem exclusão física).
4. Impedir o cancelamento de reservas passadas ou já canceladas.
5. Enviar confirmação do cancelamento por e-mail.

**Referências:** RF-01, RF-03, RF-04 e requisito de integridade histórica (RNF-02).

---

## HU-ARQ-07 - Consultar reservas por data
Como administrador do condomínio, quero consultar as reservas da academia por data para acompanhar a utilização do espaço.

Fluxo principal: o administrador informa a data desejada; o sistema consulta as reservas registradas para aquele dia e apresenta os horários reservados, os apartamentos correspondentes e a situação de cada reserva.

Fluxos alternativos: se não houver reservas para a data selecionada, o sistema informa que não existem agendamentos. Se a data informada for inválida, o sistema solicita uma entrada válida. Reservas canceladas devem permanecer identificáveis no histórico, sem serem apresentadas como reservas ativas.

Critérios de aceitação:
1. Ao informar uma data válida, retornar as reservas correspondentes àquele dia.
2. Apresentar, para cada reserva, o horário de início, o horário de término, o apartamento e a situação (ativa ou cancelada).
3. Não apresentar reservas de outras datas na consulta.
4. Quando não houver reservas para a data, informar que não foram encontrados agendamentos.
5. A consulta não deve alterar, criar ou cancelar reservas.

**Referências:** RN-ARQ-05 e modelo conceitual de Reserva em [arquitetura](https://github.com/Lucas-Betim/CondoFit/blob/7b09e68fc9e6b4318dd28b07e57904456bb6a085/docs/arquitetura.md); operações de consulta na camada de serviços e dados em ADR-001

---

## HU-ARQ-08 -Bloquear horários para manutenção
Como administrador do condomínio, quero bloquear horários ou datas da academia para manutenção para impedir agendamentos durante períodos em que o espaço não estiver disponível.

Fluxo principal: o administrador informa a data e o período em que a academia ficará indisponível, além do motivo do bloqueio. O sistema registra o bloqueio e passa a considerar aquele período indisponível para novas reservas.

Fluxos alternativos: se os dados informados forem inválidos, o sistema solicita a correção. Se já houver reservas no período solicitado, o sistema informa o conflito e não deve cancelar essas reservas automaticamente. Se o administrador tentar bloquear um período que já esteja bloqueado, o sistema informa a situação para evitar registros duplicados.

Critérios de aceitação:
1. Permitir que o administrador registre um bloqueio informando data, horário inicial, horário final e motivo.
2. Não permitir o registro de bloqueios com datas ou horários inválidos, nem com horário final anterior ou igual ao inicial.
3. Impedir a criação de novas reservas que coincidam com um período bloqueado.
4. Informar ao administrador quando já existirem reservas no período solicitado, sem cancelá-las automaticamente.
5. Permitir consultar os bloqueios registrados, identificando suas datas, horários e motivos.

**Referências:** RF-06 e regras de negócio de indisponibilidade incorporadas à especificação e à arquitetura do sistema (ADR-001).
