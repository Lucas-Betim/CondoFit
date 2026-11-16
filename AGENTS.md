# Instruções do Codex - CondoFit

## Objetivo e fase

Backend de reservas da academia de condomínio com Python, FastAPI e PostgreSQL. Nesta entrega parcial, produzir e revisar planejamento; não iniciar implementação automaticamente. O ADR-001 está aceito desde 16/11/2026. Mudanças nessa decisão exigem nova revisão humana.

Leia a história aprovada, a spec, o backlog, `docs/adr/ADR-001-arquitetura.md`, `docs/arquitetura.md` e o plano de testes antes de implementar. Se o documento obrigatório não existir ou a regra estiver ambígua, explique o impedimento; não invente requisitos.

## Regra arquitetural do ADR-001

Rotas chamam serviços; serviços aplicam regras e coordenam operações; somente a camada de dados acessa PostgreSQL. O provedor de e-mail é acessado pelo Adapter. Não escrever SQL nas rotas, enviar e-mail antes da confirmação da transação nem trocar arquitetura ou stack sem decisão humana registrada.

## Regras confirmadas

- Uma reserva ativa de uma hora por apartamento e dia, compartilhada entre os moradores do apartamento.
- Canceladas permanecem no histórico e não contam para o limite; após cancelar, é possível reservar novamente para o mesmo dia.
- Não há limite de pessoas nem exclusividade de um horário entre apartamentos diferentes.
- As reservas são consultadas por data e geram confirmação por e-mail. Não adicionar Google Calendar ou frontend sem solicitação.
- Prever proteção do limite diário no PostgreSQL. Não confiar apenas em consultar antes de inserir.

## Convenções e limites

Usar funções curtas, nomes claros em `snake_case`, módulos com uma responsabilidade e parâmetros SQL seguros. Usar Adapter apenas no ponto de integração de e-mail, sem fábricas ou hierarquias genéricas desnecessárias. Preferir biblioteca padrão quando suficiente.

Não expor credenciais, dados pessoais ou segredos em arquivos, mensagens, logs e testes. Usar variáveis de ambiente para configuração sensível. Não usar o banco de produção em testes. Não alterar requisitos, remover testes, suprimir verificações ou enfraquecer critérios para obter aprovação. Não instalar dependências ou acessar serviços externos sem a autorização aplicável ao ambiente.

## Comandos previstos para a implementação

Ainda não há aplicação, configuração de dependências ou CI: os comandos abaixo são a convenção adotada para a futura implementação. Não afirmar que funcionam ou que passaram nesta entrega de documentação.

| Finalidade | Comando previsto |
| --- | --- |
| Verificação de compilação Python | `python -m compileall -q app` |
| Testes | `python -m pytest` |
| Lint | `python -m ruff check .` |
| Execução local | `python -m uvicorn app.main:app --reload` |

O primeiro comando compila bytecode; não gera um pacote de distribuição. Os comandos exigem as dependências e o ambiente de teste da implementação. Os testes de integração usam PostgreSQL de teste. O CI executará testes e lint antes da integração; uma checagem ausente não equivale a aprovação.

## Critério de conclusão e responsabilidade

Uma história por sessão, com critérios definidos por pessoas. Para o item escolhido para TDD, registrar a falha esperada antes de implementar, então implementar e refatorar com testes passando. Ao concluir, informar mudanças, critérios atendidos, comandos e resultados reais, limitações e uso de IA.

Responsabilidade pelo código integrado: quem aprova e integra o PR assume a responsabilidade pelo código integrado, após revisão humana; agentes sugerem alterações e emitem pareceres, sem aprovar ou realizar merges.

O responsável humano deve ser identificado em cada PR. Não fazer commit, push, merge, deploy ou chamadas reais de e-mail sem solicitação explícita. A branch de entrega é `entrega-parcial-01`.

## Agentes do projeto

As configurações nativas estão em `.codex/agents/implementador.toml` e `.codex/agents/revisor.toml`. Use o implementador para uma história aprovada e o revisor para analisar um diff. Leia a configuração do papel solicitado antes de trabalhar.

Somente iniciar um subagente quando o responsável humano solicitar explicitamente. Trabalhar em etapas separadas: implementação, revisão e decisão humana de integração. No papel revisor, não editar arquivos nem executar comandos; solicitar ao responsável as evidências de testes ausentes.

Os agentes herdam as permissões e o modelo da sessão principal. As instruções dos papéis não ampliam essas permissões. A configuração do revisor desabilita shell e integrações externas opcionais; o limite de não edição também deve ser respeitado nas instruções.

O mapeamento do E10 e os exemplos de uso estão em `docs/uso-codex.md`. Não registrar como validada uma configuração que apenas passou por verificação de sintaxe.
