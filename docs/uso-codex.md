# Agentes Codex - CondoFit

## Organização do E10

Utilizo o Codex como assistente de desenvolvimento. As instruções gerais estão em `AGENTS.md`, e os agentes implementador e revisor possuem configurações próprias em TOML.

| Entregável | Arquivo |
| --- | --- |
| Instruções gerais e regra arquitetural | [AGENTS.md](../AGENTS.md) |
| Agente implementador | [implementador.toml](../.codex/agents/implementador.toml) |
| Agente revisor | [revisor.toml](../.codex/agents/revisor.toml) |
| Responsabilidade pelo código integrado | Seção "Critério de conclusão e responsabilidade" do [AGENTS.md](../AGENTS.md) |
| Decisão arquitetural | [ADR-001](adr/ADR-001-arquitetura.md) |

A tabela apresenta os documentos do E10 na estrutura adotada para o projeto. A arquitetura e as regras de responsabilidade estão aprovadas desde 16/11/2026.

## Fluxo de trabalho

1. Forneço a história aprovada, seus RF/RNF, os critérios de aceitação e os limites da tarefa.
2. O implementador trabalha em uma história, verifica os critérios e informa os resultados reais.
3. O revisor analisa o diff e as evidências, apontando desvios sem alterar arquivos.
4. O revisor humano do PR avalia o parecer, solicita correções e decide sobre a aprovação. Após a aprovação, o autor humano do PR realiza a integração e responde pelo código integrado. O revisor responde pela qualidade da revisão. O agente não faz merge.

Executar os papéis em etapas separadas. Não iniciar subagentes automaticamente. Nesta entrega parcial, o foco continua sendo planejamento; a existência das configurações não autoriza gerar o backend.

## Exemplos de solicitação

### Implementação após aprovação do planejamento

> Use o agente implementador do projeto para a história aprovada que vou informar. Leia AGENTS.md e confira as entradas obrigatórias do agente. Respeite os critérios, mantenha a solução simples e informe os testes executados e seus resultados. Não faça commit, push ou merge.

Em cada solicitação, informo o identificador da história e seus critérios. A autorização se limita ao escopo informado.

### Revisão

> Use o agente revisor do projeto para analisar o diff que vou fornecer, com a história, critérios e evidências de testes. Não edite arquivos nem execute comandos. Apresente achados por gravidade, com arquivo, linha, cenário e regra afetada. Deixe a aprovação para o responsável humano.

Forneço ao revisor o diff e as evidências de testes. O parecer distingue a análise do código da execução dos testes e registra eventuais evidências ausentes.

## Configuração e permissões

Cada TOML define `name`, `description` e `developer_instructions`. Nenhum modelo específico é fixado; vale o modelo da sessão principal. As permissões de acesso também são herdadas da sessão, sem serem ampliadas por estes arquivos.

A configuração do revisor desabilita shell, apps, plugins, memórias e solicitação de permissões adicionais. Suas instruções proíbem a edição de arquivos. As permissões da sessão continuam aplicáveis; o agente não possui uma sandbox independente. Confiro o diff após a revisão para verificar o cumprimento desses limites.

Nesta entrega, verifiquei a sintaxe TOML, os campos obrigatórios, os links internos e a coerência dos documentos. A execução dos agentes e das verificações do backend será validada durante a implementação.

## Registro e aprendizado

Registro o uso do Codex com a tarefa realizada, a aceitação ou as correções e a autoria humana. Antes de integrar uma alteração, reviso o código e os testes para conseguir explicar seu funcionamento sem IA. A revisão humana e a responsabilidade pelo código permanecem obrigatórias.

## Referências técnicas

- [Instruções do projeto com AGENTS.md](https://developers.openai.com/codex/guides/agents-md).
- [Agentes personalizados do Codex](https://developers.openai.com/codex/multi-agent).
