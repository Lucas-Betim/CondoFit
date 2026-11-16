# Requisitos não funcionais de arquitetura

Projeto: CondoFit. Autor: Lucas - Arquiteto técnico. Status: **Aprovados em 16/11/2026**.

Elaborei estes requisitos com apoio de IA (Codex).

Especifico aqui os dois requisitos não funcionais que fundamentam o [ADR-001](../docs/adr/ADR-001-arquitetura.md). Os identificadores relacionam os requisitos às histórias, à arquitetura e aos critérios de teste.

## RNF-ARQ-01 - Testabilidade dos serviços

As regras de reserva e cancelamento devem permitir testes unitários sem conexão com PostgreSQL e sem envio real de e-mail.

Critério de aceitação: todos os testes unitários dos serviços de reserva e cancelamento executam usando substitutos das operações de dados e de e-mail. Nenhum deles abre conexão com PostgreSQL ou realiza envio externo.

Verificação planejada: executar a suíte unitária com as operações externas substituídas por funções de teste que registram as chamadas. Quando necessário, fazer qualquer tentativa de acesso real falhar explicitamente. Não usar somente a indisponibilidade da rede como prova de isolamento.

Relação com a arquitetura: os serviços recebem operações substituíveis de persistência e notificação; o Adapter concentra a integração de e-mail. Os testes de integração com PostgreSQL permanecem necessários e são separados dos testes unitários.

## RNF-ARQ-02 - Consistência do limite diário

O sistema deve preservar o limite de uma reserva ativa por apartamento e data, inclusive com solicitações simultâneas.

Critério de aceitação: em ambiente de teste com PostgreSQL disponível, sem reserva prévia para o apartamento e dia, disparar dez tentativas simultâneas válidas, usando operações de banco independentes. Ao término, exatamente uma tentativa resulta em criação, as outras nove são recusadas por conflito e uma consulta ao banco encontra exatamente uma reserva ativa.

Verificação adicional do cancelamento: após confirmar o cancelamento dessa reserva, uma nova tentativa válida para o mesmo apartamento e dia é aceita. O banco mantém a cancelada no histórico e exatamente uma nova reserva ativa.

Relação com a arquitetura: a camada de dados mantém um índice único parcial sobre apartamento e data para reservas ativas. A aplicação trata a violação de unicidade sem transformá-la em erro genérico ou informar sucesso indevido.

## Evidências previstas

Registraremos as evidências por identificador de requisito, cenário, resultado esperado, resultado obtido, aprovação ou reprovação e data. Os critérios descrevem a verificação prevista para a implementação. Esta entrega apresenta o planejamento dos testes, sem resultados de execução do backend.
