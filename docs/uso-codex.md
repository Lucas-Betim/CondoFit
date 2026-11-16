# Agentes Codex - CondoFit

## Organização do E10

O projeto utiliza Codex. As instruções gerais ficam no arquivo `AGENTS.md`, e os dois agentes são definidos em arquivos TOML próprios do Codex.

| Entregável | Arquivo |
| --- | --- |
| Instruções gerais e regra arquitetural | [AGENTS.md](../AGENTS.md) |
| Agente implementador | [implementador.toml](../.codex/agents/implementador.toml) |
| Agente revisor | [revisor.toml](../.codex/agents/revisor.toml) |
| Responsabilidade pelo código integrado | Seção "Critério de conclusão e responsabilidade" do [AGENTS.md](../AGENTS.md) |
| Decisão arquitetural | [ADR-001](adr/ADR-001-arquitetura.md) |

O responsável pelo índice `ENTREGAPARCIAL01.md` deve usar esses caminhos no E10. O roteiro permite caminhos diferentes dos sugeridos quando o índice aponta os documentos correspondentes. As decisões propostas ainda precisam da revisão do grupo.

## Fluxo de trabalho

1. Uma pessoa fornece história, RF/RNF, critérios e limites da tarefa aprovados.
2. O implementador trabalha em uma história, verifica os critérios e informa os resultados reais.
3. O revisor analisa o diff e as evidências, apontando desvios sem alterar arquivos.
4. Uma pessoa avalia o parecer, solicita correções e decide sobre a integração. O agente não faz merge.

Executar os papéis em etapas separadas. Não iniciar subagentes automaticamente. Nesta entrega parcial, o foco continua sendo planejamento; a existência das configurações não autoriza gerar o backend.

## Exemplos de solicitação

### Implementação após aprovação do planejamento

> Use o agente implementador do projeto para a história aprovada que vou informar. Leia AGENTS.md e confira as entradas obrigatórias do agente. Respeite os critérios, mantenha a solução simples e informe os testes executados e seus resultados. Não faça commit, push ou merge.

A pessoa deve indicar a história real e fornecer os critérios. A solicitação autoriza apenas o escopo informado.

### Revisão

> Use o agente revisor do projeto para analisar o diff que vou fornecer, com a história, critérios e evidências de testes. Não edite arquivos nem execute comandos. Apresente achados por gravidade, com arquivo, linha, cenário e regra afetada. Deixe a aprovação para o responsável humano.

O revisor recebe o diff e os resultados de testes fornecidos pelo responsável. Ler testes não prova que eles passaram. Evidência ausente deve ser registrada como limitação.

## Configuração e permissões

Cada TOML define `name`, `description` e `developer_instructions`. Nenhum modelo específico é fixado; vale o modelo da sessão principal. As permissões de acesso também são herdadas da sessão, sem serem ampliadas por estes arquivos.

O revisor desabilita shell, apps, plugins, memórias e solicitação de permissões adicionais por meio dos campos suportados. A regra de não edição está nas instruções; a configuração não deve ser descrita como uma sandbox independente de somente leitura. O responsável deve conferir o diff após a revisão.

Os arquivos foram preparados para o mecanismo de agentes personalizados do Codex. Sintaxe e referências foram verificadas; a descoberta e execução na instalação utilizada devem ser confirmadas na primeira sessão de uso. Nenhum agente foi iniciado para esta migração.

## Registro e aprendizado

Registrar o uso real de Codex com tarefa, aceitação ou correções e autor humano. Antes de integrar, cada integrante deve conseguir explicar a mudança e os testes sem IA, como preparação para a prova individual.

## Referências técnicas

- [Instruções do projeto com AGENTS.md](https://developers.openai.com/codex/guides/agents-md).
- [Agentes personalizados do Codex](https://developers.openai.com/codex/multi-agent).
