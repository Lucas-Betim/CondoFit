# Histórias de usuário - Responsável de qualidade e teste

Autor: Lavínia - Responsável de qualidade e teste. Status: **Aprovadas em /11/2026**.

Utilizo os identificadores HU-ARQ para relacionar cada história aos fluxos, às regras de negócio e aos requisitos não funcionais de arquitetura. Este documento descreve as histórias; a priorização pertence ao backlog.

## HU-ARQ-03 - Consultar horários disponíveis

Como morador, quero visualizar os horários disponíveis da academia em determinada data para escolher quando treinar.

Fluxo principal: o morador identificado informa a data desejada; o sistema verifica os horários permitidos para agendamento e as reservas existentes; apresenta os horários disponíveis para a data selecionada, permitindo que o morador consulte as opções sem precisar realizar uma reserva.

Fluxos alternativos:

Se a data informada for inválida, o sistema deve informar o problema e solicitar uma data válida.

Se não houver horários disponíveis na data selecionada, o sistema deve informar que não existem opções disponíveis.

Se o usuário não estiver identificado ou não possuir uma conta válida, o sistema deve impedir a consulta.

Critérios de aceitação:

Somente usuários com conta existente e identificados no sistema podem consultar os horários disponíveis.

Ao informar uma data válida, o sistema deve apresentar os horários permitidos para agendamento que estejam disponíveis naquela data.

Caso não existam horários disponíveis, o sistema deve informar essa condição ao usuário.

A consulta não deve criar, alterar ou cancelar reservas.

Referências: RN-ARQ-01, RN-ARQ-02, RN-ARQ-04 e RN-ARQ-06 em [arquitetura](../docs/arquitetura.md).

## HU-ARQ-04 - Consultar minhas reservas

Como morador, quero visualizar minhas reservas futuras e anteriores para acompanhar meus agendamentos.

Fluxo principal: o morador identificado consulta as reservas de seu apartamento; o sistema verifica mostra os agendamentos aprovadas mantidas no histórico.

Fluxos alternativos: Não há fluxo alternativo pois o morador está apenas visualizando os horários e datas agendados anteriormente.

Critérios de aceitação:
1. A conta do usuário precisas existir para que ele faça a consulta.

Referências: RN-ARQ-03 e RN-ARQ-05 em [arquitetura](../docs/arquitetura.md).
