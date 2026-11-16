# Histórias de usuário - Arquiteto técnico

Autor: Lucas - Arquiteto técnico. Status: **Aprovadas em 16/11/2026**.

Elaborei estas histórias com apoio de IA (Codex).

Utilizo os identificadores HU-ARQ para relacionar cada história aos fluxos, às regras de negócio e aos requisitos não funcionais de arquitetura. Este documento descreve as histórias; a priorização pertence ao backlog.

## HU-ARQ-01 - Reservar a academia

Como morador, quero reservar uma hora na academia para organizar meu treino.

Fluxo principal: o morador identificado escolhe data e horário; o sistema verifica as condições de agendamento e o limite de seu apartamento; registra uma reserva ativa; disponibiliza a reserva na consulta por data e solicita a confirmação por e-mail.

Fluxos alternativos: se o apartamento já tiver reserva ativa naquele dia, recusar sem criar outra; se a entrada for inválida ou o horário não permitir agendamento, explicar a recusa. Se houver concorrência, preservar apenas uma reserva. Se o e-mail falhar após a confirmação no banco, manter a reserva e registrar a falha, conforme o ADR. Não comunicar que o e-mail foi enviado com sucesso.

Critérios de aceitação:

1. Sem reserva ativa e em horário permitido, criar uma reserva de exatamente uma hora para o apartamento identificado.
2. Se outro morador do mesmo apartamento já reservou naquele dia, recusar a nova tentativa.
3. Uma reserva de outro apartamento no mesmo horário não impede esta reserva.
4. Com dez tentativas simultâneas válidas do mesmo apartamento e dia, persistir exatamente uma reserva.

Referências: RN-ARQ-01, RN-ARQ-02, RN-ARQ-04, RN-ARQ-05 e RN-ARQ-06 em [arquitetura](../docs/arquitetura.md); RNF-ARQ-02 em [ADR-001](../docs/adr/ADR-001-arquitetura.md).

## HU-ARQ-02 - Cancelar para poder remarcar

Como morador, quero cancelar uma reserva do meu apartamento para poder agendar novamente no mesmo dia quando meus planos mudarem.

Fluxo principal: o morador identificado consulta a reserva de seu apartamento e solicita cancelamento; o sistema verifica autorização e condições aprovadas na spec; altera a reserva para cancelada e mantém o histórico; uma nova reserva para o mesmo dia passa a ser permitida, se cumprir as demais regras.

Fluxos alternativos: tentativa de cancelar reserva de outro apartamento é recusada sem alteração; reserva inexistente produz resposta de não encontrada; repetir cancelamento não apaga o histórico nem cria outra reserva. O cancelamento respeita as condições de agendamento, sem alterar a duração ou o limite diário das reservas.

Critérios de aceitação:

1. Cancelar uma reserva autorizada preserva o registro como cancelado e libera o limite do apartamento após a confirmação da transação.
2. Após cancelar, criar outra reserva válida para o mesmo dia resulta em uma única reserva ativa e mantém a cancelada no histórico.
3. Tentativa de cancelar reserva de outro apartamento não altera nenhuma reserva.
4. Os testes unitários desse serviço executam sem PostgreSQL e sem envio real de e-mail; a liberação do limite também é verificada em integração com PostgreSQL.

Referências: RN-ARQ-01 e RN-ARQ-03 e regra de autorização em [arquitetura](../docs/arquitetura.md); RNF-ARQ-01 em [ADR-001](../docs/adr/ADR-001-arquitetura.md).

Os requisitos não funcionais estão especificados em [Requisitos de arquitetura](requisitos-arquitetura.md). As referências RN-ARQ identificam as regras de negócio apresentadas no documento de arquitetura.
