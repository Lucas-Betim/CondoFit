# Histórias de usuário - Responsável de qualidade e teste

Autor: Lavínia - Responsável de qualidade e teste. Status: **Aprovadas em /11/2026**.

Utilizo os identificadores HU-ARQ para relacionar cada história aos fluxos, às regras de negócio e aos requisitos não funcionais de arquitetura. Este documento descreve as histórias; a priorização pertence ao backlog.

## HU-ARQ-03 - Consultar horários disponíveis

Como morador, quero visualizar os horários disponíveis da academia em determinada data para escolher quando treinar.

Fluxo principal: o morador identificado visualiza data e horário livres. O sistema mostra as datas e horários disponíveis mas não é necessário que ele já reserve.

Fluxos alternativos: Não há fluxo alternativo pois o morador está apenas visualizando os horários e datas disponíveis.

Critérios de aceitação:
1. A conta do usuário precisas existir para que ele faça a consulta.

Referências: RN-ARQ-01, RN-ARQ-02, RN-ARQ-04 e RN-ARQ-06 em [arquitetura](../docs/arquitetura.md).

## HU-ARQ-04 - Consultar minhas reservas

Como morador, quero visualizar minhas reservas futuras e anteriores para acompanhar meus agendamentos.

Fluxo principal: o morador identificado consulta as reservas de seu apartamento; o sistema verifica mostra os agendamentos aprovadas mantidas no histórico.

Fluxos alternativos: Não há fluxo alternativo pois o morador está apenas visualizando os horários e datas agendados anteriormente.

Critérios de aceitação:
1. A conta do usuário precisas existir para que ele faça a consulta.

Referências: RN-ARQ-03 e RN-ARQ-05 em [arquitetura](../docs/arquitetura.md).
