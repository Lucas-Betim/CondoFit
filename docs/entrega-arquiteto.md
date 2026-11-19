# Entrega parcial 01 - Contribuição do arquiteto técnico

Projeto: **CondoFit**. Autor: **Lucas - Arquiteto técnico**. Versão: **16/11/2026**.

Apresento nesta contribuição os entregáveis E6 e E10, os requisitos não funcionais de arquitetura e minhas duas histórias de usuário. As decisões estão aprovadas. Elaborei a documentação com apoio de IA (Codex).

## Índice da contribuição

| Item | Conteúdo | Documento |
| --- | --- | --- |
| E6 | ADR aceito: contexto, decisão, status, consequências, dois RNF, desvantagem e mitigação, padrão Adapter | [ADR-001](adr/ADR-001-arquitetura.md) |
| E6 | Diagrama de blocos, responsabilidades e limites do diagrama | [Arquitetura](arquitetura.md) |
| Apoio a E3/E6 | Dois RNF mensuráveis com critérios de aceitação e verificação | [Requisitos de arquitetura](../specs/requisitos-arquitetura.md) |
| Contribuição a E5 | Duas histórias com fluxos, critérios e rastreabilidade | [Histórias de usuário](../specs/historias-arquiteto.md) |
| E10 | Instruções gerais, regra arquitetural, comandos e responsabilidade humana | [AGENTS.md](../AGENTS.md) |
| E10 | Entradas, trabalho, conclusão e limites do implementador | [implementador.toml](../.codex/agents/implementador.toml) |
| E10 | Checagens, formato do parecer e decisão humana após revisão | [revisor.toml](../.codex/agents/revisor.toml) |
| E10 | Organização e uso dos agentes Codex | [Uso do Codex](uso-codex.md) |

## Decisões aprovadas

- Python, FastAPI e PostgreSQL em um único backend com camadas de rotas, serviços e dados.
- Acesso ao banco concentrado na camada de dados; Adapter para integração de e-mail.
- Uma reserva ativa de uma hora por apartamento e dia. Cancelamento libera nova reserva no mesmo dia e preserva o histórico.
- Apartamentos diferentes podem reservar o mesmo período; não há limite de pessoas por horário.
- Confirmação da reserva no banco antes da solicitação do e-mail; falha de envio mantém a reserva válida e é registrada.
- Testes unitários isolados e verificação de concorrência com PostgreSQL.
- Revisão humana obrigatória; quem aprova e integra o PR responde pelo código integrado.

## Escopo e formato de entrega

Esta contribuição abrange arquitetura, configuração dos agentes e os requisitos e histórias relacionados. A tabela acima identifica os documentos correspondentes para consulta no repositório.

O formato de envio da atividade é o link da branch `entrega-parcial-01` no Classroom, com um envio por grupo. O índice geral da atividade é `ENTREGAPARCIAL01.md`.

## Verificação documental

Revisei a redação, os links internos, os identificadores, a consistência das regras arquiteturais e a sintaxe TOML dos agentes. Esta é uma entrega de planejamento: a execução dos agentes, os testes do backend e o CI pertencem à etapa de implementação.
