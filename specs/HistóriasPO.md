# Histórias de usuário - Product Owner

Autora: Gabriela - Product Owner. Status: Aprovadas em 18/11/2026.

Utilizo os identificadores HU-PO para relacionar cada história às necessidades de negócio dos moradores e às regras de uso do espaço. Este documento descreve as histórias; a priorização pertence ao backlog.

## HU-PO-01: Reservar horário na academia
Como morador do condomínio, quero agendar um horário de uso da academia para garantir minha vaga e evitar superlotação no espaço.

**Fluxo principal:** O morador acessa o sistema, escolhe uma data futura e um horário disponível, informa o número do seu apartamento e confirma o agendamento. O sistema registra a reserva como ativa e exibe a confirmação com os dados do agendamento.

**Fluxos alternativos:** se o horário desejado já estiver ocupado por outra reserva ativa ou bloqueado para manutenção, o sistema impede o agendamento e informa a indisponibilidade. Se o morador tentar agendar em uma data passada ou fora do horário de funcionamento da academia, o sistema recusa a operação e orienta sobre os horários permitidos.

**Critérios de aceitação:**
1. Permitir criar reserva informando data, horário de início, horário de término e identificador do apartamento.
2. Impedir reservas em datas passadas ou com horário final anterior ou igual ao inicial.
3. Bloquear agendamentos caso exista conflito de horário com outra reserva ativa ou com um período de manutenção registrado.
4. Registrar a nova reserva com o status inicial "ativa" e retornar um comprovante/identificador da reserva.
5. Garantir que o agendamento respeite a janela de funcionamento configurada para a academia (ex.: 06h às 22h).

**Referências:** RN-NEG-01 (regras de agendamento), RF-01 e integração com validação de disponibilidade definida na camada de serviços.

---

## HU-PO-02: Cancelar própria reserva
Como morador do condomínio, quero cancelar uma reserva ativa que fiz anteriormente para liberar o horário a outros moradores caso eu não possa comparecer.

**Fluxo principal:** o morador localiza sua reserva ativa (por identificador ou apartamento/data), solicita o cancelamento e confirma a ação. O sistema altera o status da reserva para "cancelada" e libera imediatamente a faixa de horário para novos agendamentos.

**Fluxos alternativos:** se a reserva já estiver cancelada ou se o horário da reserva já tiver passado, o sistema informa que o cancelamento não é permitido. Se o identificador informado não existir, o sistema exibe mensagem de reserva não encontrada.

**Critérios de aceitação:**
1. Permitir alterar o status de uma reserva "ativa" para "cancelada".
2. Liberar imediatamente o horário correspondente para que outros moradores possam reservá-lo.
3. Manter o registro da reserva cancelada no banco/histórico para fins de auditoria e consulta administrativa (sem excluí-la fisicamente).
4. Impedir o cancelamento de reservas já encerradas (data/hora no passado) ou já canceladas anteriormente.
5. Solicitar confirmação antes de efetivar o cancelamento.

**Referências:** RF-02, requisito de integridade histórica (RN-ARQ-05) e fluxo de consulta administrativa por data.

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

**Referências:** RN-ARQ-05 e modelo conceitual de Reserva em arquitetura; operações de consulta na camada de serviços e dados em ADR-001

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

**Referências:** Funcionalidade proposta a partir das referências visuais de sistemas de gestão e agendamento enviadas (Field Control, Trengo e Fitness Booking). A arquitetura atual não define bloqueios de manutenção; a regra precisa ser incorporada à especificação de negócio e validada pelo arquiteto.
