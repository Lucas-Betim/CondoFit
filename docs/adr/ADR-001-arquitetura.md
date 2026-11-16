# ADR-001 - Backend em camadas do CondoFit

Data: 15/11/2026.

Documento preparado com apoio de IA. O grupo deve revisar, compreender e decidir sobre as propostas antes da entrega.

## Contexto

O CondoFit organiza reservas da academia de um condomínio. O projeto será um backend sem frontend obrigatório. A equipe precisa compreender o código para desenvolver, testar e auditar um PR individualmente, sem IA, na avaliação.

Python, FastAPI e PostgreSQL foram escolhidos pelo arquiteto pela familiaridade com essas tecnologias.

Regras confirmadas: cada apartamento pode manter uma reserva ativa por dia, de uma hora; cancelar permite outra reserva no mesmo dia; não há limite de pessoas ou apartamentos no mesmo horário; o agendamento aparece na consulta de calendário e gera confirmação por e-mail. O histórico das canceladas será preservado conforme a proposta já discutida.

## Requisitos não funcionais relacionados

Os identificadores abaixo são propostas da arquitetura. O responsável pelos requisitos deve incorporá-los à spec e alinhar os identificadores e critérios antes da entrega final destes documentos.

| ID | Requisito proposto | Critério verificável |
| --- | --- | --- |
| RNF-ARQ-01 | As regras de reserva devem permitir testes isolados de serviços externos. | Todos os testes unitários dos serviços de reserva e cancelamento executam sem conexão com PostgreSQL e sem envio real de e-mail, usando substitutos de teste. |
| RNF-ARQ-02 | O limite diário por apartamento deve ser preservado em solicitações simultâneas. | Com dez tentativas simultâneas válidas para o mesmo apartamento e dia, sem reserva prévia e com PostgreSQL disponível, exatamente uma reserva é persistida e as demais são recusadas por conflito. |

## Decisão

Usar um monólito: uma aplicação Python/FastAPI, organizada em três camadas, com PostgreSQL como banco de dados.

1. Rotas: recebem e validam o formato das entradas, obtêm a identidade do solicitante e traduzem resultados para HTTP.
2. Serviços: aplicam as regras e coordenam as operações de reserva, cancelamento e consulta.
3. Dados: concentram SQL, persistência e transações. Somente esta camada acessa PostgreSQL.

O envio de e-mail fica em um componente de integração chamado pelo serviço. As rotas não acessam o banco ou o provedor de e-mail diretamente. Usar funções e módulos simples; não criar classes base ou repositórios genéricos sem necessidade demonstrada.

Para RNF-ARQ-01, o serviço recebe as operações de dados e e-mail de forma substituível nos testes. Isso não dispensa testes de integração com PostgreSQL para transações e concorrência.

Para RNF-ARQ-02, prever no PostgreSQL um índice único parcial por apartamento e data da reserva, considerando apenas reservas ativas. Uma consulta prévia no serviço melhora a mensagem ao usuário, mas não garante exclusão mútua. A camada de dados deve tratar a violação de unicidade e o serviço deve retornar um conflito compreensível. Cancelar altera o estado da reserva em transação, liberando o limite após a confirmação da transação.

O índice não será único por horário: apartamentos diferentes podem reservar o mesmo período. A data deve ser calculada de modo consistente no fuso do condomínio; o fuso e o funcionamento serão definidos pelo grupo na spec antes da implementação.

## Padrão de projeto pretendido: Adapter

Problema: o serviço de reservas não deve conhecer detalhes do provedor de e-mail.

Decisão: concentrar a tradução dos dados de confirmação para o protocolo do provedor em um módulo Adapter de e-mail. Uma operação simples recebe destinatário, data e horário. Nos testes, uma função substituta registra o envio sem acessar a rede.

Consequência: trocar o provedor concentra alterações no adaptador; há uma pequena etapa adicional de chamada. Não criar uma hierarquia de provedores ou uma fábrica antecipadamente.

## Desvantagens e mitigação

A separação entre camadas pode ser difícil de manter, e atravessar camadas acrescenta custo de processamento, conforme a discussão de camadas indicada no roteiro (Sommerville, cap. 6, Fig. 6.8).

Mitigação: três camadas com responsabilidades claras, chamadas diretas em um único processo, consultas limitadas ao necessário e revisão humana dos pontos de acesso ao banco. A equipe deve medir desempenho se houver requisito correspondente; não presumir que a divisão por si só torna a API rápida.

O monólito concentra a disponibilidade na aplicação e no banco. A equipe aceita essa limitação para o escopo acadêmico; replicação, microsserviços, cache e filas não serão introduzidos sem necessidade aprovada.

## Consequências e limites

- O fluxo fica curto e rastreável, favorecendo revisão e aprendizado.
- Restrições no banco e testes de integração passam a fazer parte da verificação do limite diário.
- A reserva deve ser confirmada no banco antes de solicitar o e-mail. Como proposta, falha de envio mantém a reserva válida e registra a falha sem expor dados sensíveis. Garantia de entrega e política de reenvio precisam ser definidas na spec; este ADR não promete entrega infalível.
- Autenticação, horários de funcionamento, antecedência e prazo de cancelamento ainda precisam de definição pelo grupo. Não implementar regras inventadas para preencher essas lacunas.
- Calendário significa consulta das reservas por data pela API; integração com Google Calendar não foi solicitada.

## Regra para os agentes

Arquitetura em camadas: rotas chamam serviços; serviços coordenam regras e operações de dados; somente a camada de dados acessa PostgreSQL. O acesso ao provedor de e-mail fica no Adapter. Não alterar essa organização nem remover a proteção de unicidade sem revisão humana do ADR.

## Referências

- RoteiroB02P01, seções 6 e 8: ADR, desvantagem do estilo, padrão pretendido e restrição para agentes.
- [FastAPI - organização em múltiplos arquivos](https://fastapi.tiangolo.com/tutorial/bigger-applications/): recursos de organização da API; a escolha das três camadas é do projeto.
- [PostgreSQL - índices parciais](https://www.postgresql.org/docs/current/indexes-partial.html): base técnica para unicidade apenas sobre reservas ativas.
