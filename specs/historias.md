# Histórias de usuário - Scrum master

Autor: Rafael - Scrum master. Status: **Aprovadas em /11/2026**.

Utilizo os identificadores HU-ARQ para relacionar cada história aos fluxos, às regras de negócio e aos requisitos não funcionais de arquitetura. Este documento descreve as histórias; a priorização pertence ao backlog.

## HU-ARQ-05 - Identificar meu apartamento

Como morador, quero acessar o sistema com minha identificação vinculada ao apartamento para realizar agendamentos autorizados.

Fluxo principal: o morador informa sua identificação; o sistema verifica a identificação e recupera o apartamento ao qual está vinculado; após a validação, permite que o morador realize operações de reserva e cancelamento associadas ao seu apartamento.

Fluxos alternativos: se a identificação for inválida ou não estiver vinculada a um apartamento, recusar o acesso às operações de agendamento e informar o motivo. Se houver tentativa de realizar uma operação sem identificação válida, não permitir a criação ou o cancelamento da reserva.

Critérios de aceitação:
1. Com uma identificação válida e vinculada a um apartamento, permitir operações de agendamento associadas a esse apartamento.
2. Com uma identificação inválida ou sem vínculo com apartamento, impedir o acesso às operações de agendamento.
3. As reservas e os cancelamentos devem utilizar o apartamento associado à identificação validada, sem permitir que o morador informe livremente outro apartamento.

Referências: RN-ARQ-01, RN-ARQ-02 e RN-ARQ-04 em arquitetura.

## HU-ARQ-06 - Receber confirmação por e-mail

Como morador, quero receber uma confirmação por e-mail após realizar ou cancelar uma reserva para ter um registro da operação.

Fluxo principal:  após a criação ou o cancelamento de uma reserva, o sistema solicita o envio de um e-mail ao endereço associado ao morador, contendo a confirmação da operação e as informações da reserva.

Fluxos alternativos: se o envio do e-mail falhar após a criação ou o cancelamento da reserva no banco de dados, manter a operação confirmada e registrar a falha de notificação, conforme o ADR. Não comunicar que o e-mail foi enviado com sucesso quando houver falha no envio.

Critérios de aceitação:
1. Após a criação de uma reserva, solicitar o envio de um e-mail de confirmação ao morador.
2. Após o cancelamento de uma reserva, solicitar o envio de um e-mail informando o cancelamento.
3. Se o envio do e-mail falhar, manter a operação de reserva ou cancelamento registrada no banco de dados.
4. Em caso de falha no envio, registrar a ocorrência e não informar ao morador que a notificação foi enviada com sucesso.

Referências:  RN-ARQ-03 e RN-ARQ-05 em arquitetura; RNF-ARQ-01 em arquitetura e RNF-ARQ-02 em ADR-001.
