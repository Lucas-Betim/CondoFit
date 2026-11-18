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
