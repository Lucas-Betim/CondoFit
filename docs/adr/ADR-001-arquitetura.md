# ADR-001 - Backend em camadas do CondoFit

Data de elaboração: 15/11/2026.

Status: **Aceito em 16/11/2026**.

Autor: Lucas - Arquiteto técnico.

Elaborei este documento com apoio de IA (Codex).

## Contexto

O CondoFit organiza reservas da academia de um condomínio. O escopo é um backend, sem frontend obrigatório. Buscamos uma solução simples de desenvolver, testar e manter, com um fluxo que permita compreender e auditar o código durante a avaliação individual, sem IA.

Adotamos Python, FastAPI e PostgreSQL, considerando minha familiaridade com essas tecnologias e o escopo do backend. Organizaremos o código em funções e módulos pequenos para facilitar manutenção, testes e auditoria.

Regras confirmadas: cada apartamento pode manter uma reserva ativa por dia, de uma hora; cancelar permite outra reserva no mesmo dia; não há limite de pessoas ou apartamentos no mesmo horário; o agendamento aparece na consulta de calendário e gera confirmação por e-mail. As reservas canceladas permanecem no histórico e não contam para o limite diário.

## Requisitos não funcionais relacionados

Esta decisão atende aos dois requisitos não funcionais especificados em [Requisitos de arquitetura](../../specs/requisitos-arquitetura.md). Utilizamos os mesmos identificadores nas histórias, na arquitetura e nos critérios de teste para manter a rastreabilidade.

| ID | Requisito | Critério verificável |
| --- | --- | --- |
| RNF-ARQ-01 | As regras de reserva devem permitir testes isolados de serviços externos. | Todos os testes unitários dos serviços de reserva e cancelamento executam sem conexão com PostgreSQL e sem envio real de e-mail, usando substitutos de teste. |
| RNF-ARQ-02 | O limite diário por apartamento deve ser preservado em solicitações simultâneas. | Com dez tentativas simultâneas válidas para o mesmo apartamento e dia, sem reserva prévia e com PostgreSQL disponível, exatamente uma reserva é persistida e as demais são recusadas por conflito. |

## Decisão

Adotamos um monólito: uma aplicação Python/FastAPI organizada em três camadas, com PostgreSQL como banco de dados.

1. Rotas: recebem e validam o formato das entradas, obtêm a identidade do solicitante e traduzem resultados para HTTP.
2. Serviços: aplicam as regras e coordenam as operações de reserva, cancelamento e consulta.
3. Dados: concentram SQL, persistência e transações. Somente esta camada acessa PostgreSQL.

O envio de e-mail fica em um componente de integração chamado pelo serviço. As rotas não acessam o banco ou o provedor de e-mail diretamente. Manteremos funções e módulos simples, sem classes base ou repositórios genéricos sem necessidade demonstrada.

Para RNF-ARQ-01, o serviço recebe as operações de dados e e-mail de forma substituível nos testes. Isso não dispensa testes de integração com PostgreSQL para transações e concorrência.

Para RNF-ARQ-02, utilizaremos no PostgreSQL um índice único parcial por apartamento e data da reserva, considerando apenas reservas ativas. Uma consulta prévia no serviço melhora a mensagem ao usuário, mas não garante exclusão mútua. A camada de dados deve tratar a violação de unicidade e o serviço deve retornar um conflito compreensível. Cancelar altera o estado da reserva em transação, liberando o limite após a confirmação da transação.

O índice não será único por horário: apartamentos diferentes podem reservar o mesmo período. A data local deve ser calculada de modo consistente no fuso configurado para o condomínio. Horários de funcionamento, fuso e demais parâmetros operacionais pertencem à spec de negócio; não são fixados por esta decisão arquitetural.

## Padrão de projeto pretendido: Adapter

Problema: o serviço de reservas não deve conhecer detalhes do provedor de e-mail.

Decisão: concentrar a tradução dos dados de confirmação para o protocolo do provedor em um módulo Adapter de e-mail. Uma operação simples recebe destinatário, data e horário. Nos testes, uma função substituta registra o envio sem acessar a rede.

Consequência: trocar o provedor concentra alterações no adaptador; há uma pequena etapa adicional de chamada. O padrão ficará restrito a essa integração, sem hierarquias de provedores ou fábricas antecipadas.

## Desvantagens e mitigação

A separação entre camadas pode ser difícil de manter, e atravessar camadas acrescenta custo de processamento, conforme a discussão de camadas indicada no roteiro (Sommerville, cap. 6, Fig. 6.8).

Mitigação: manteremos três camadas com responsabilidades claras, chamadas diretas em um único processo, consultas limitadas ao necessário e revisão humana dos pontos de acesso ao banco. A separação por camadas não representa, por si só, uma garantia de desempenho.

O monólito concentra a disponibilidade na aplicação e no banco. Aceitamos essa limitação para o escopo acadêmico. Replicação, microsserviços, cache e filas ficam fora da arquitetura adotada.

## Consequências e limites

- O fluxo fica curto e rastreável, favorecendo revisão e aprendizado.
- Restrições no banco e testes de integração passam a fazer parte da verificação do limite diário.
- A reserva é confirmada no banco antes de solicitar o e-mail. Falha no envio mantém a reserva válida e registra a falha sem expor dados sensíveis. A resposta não deve afirmar envio bem-sucedido se ele falhou. A decisão não inclui garantia de entrega nem mecanismo automático de reenvio.
- O serviço deve verificar a identidade e o vínculo com o apartamento antes de permitir acesso ou cancelamento. A implementação da autenticação e os parâmetros de funcionamento, antecedência e cancelamento são detalhados na spec de negócio; o ADR define suas fronteiras arquiteturais.
- O calendário corresponde à consulta das reservas por data pela API. A arquitetura não inclui integração com Google Calendar.

## Regra para os agentes

Arquitetura em camadas: rotas chamam serviços; serviços coordenam regras e operações de dados; somente a camada de dados acessa PostgreSQL. O acesso ao provedor de e-mail fica no Adapter. Não alterar essa organização nem remover a proteção de unicidade sem revisão humana do ADR.

## Referências

- RoteiroB02P01, seções 6 e 8: ADR, desvantagem do estilo, padrão pretendido e restrição para agentes.
- [FastAPI - organização em múltiplos arquivos](https://fastapi.tiangolo.com/tutorial/bigger-applications/): recursos de organização da API; a escolha das três camadas é do projeto.
- [PostgreSQL - índices parciais](https://www.postgresql.org/docs/current/indexes-partial.html): base técnica para unicidade apenas sobre reservas ativas.
