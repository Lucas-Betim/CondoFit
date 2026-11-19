# Convenção de versionamento

## 1. Repositório
- GitHub: [CondoFit](https://github.com/Lucas-Betim/CondoFit); todos com escrita; .
- `main` é a mainline e deve estar sempre passando no CI.

## 2. Branches
- Uma branch por tarefa, saindo da `main` atualizada.
- Nome: `tipo/id-descricao`, ex.: `feat/HU-ARQ-01-reserva`, `fix/limite-diario`, `docs/adr-001`.
- Ninguém faz commit direto na `main`.

## 3. Fusão
- Só via Pull Request, com ao menos 1 aprovação humana de quem não é o autor.
- O autor do PR faz o merge depois da aprovação.
- Agentes (Codex) não fazem commit, push nem merge.
- A descrição do PR registra se houve uso de IA.

## 4. Teste e lint antes de integrar
- Local: lint e testes antes de abrir o PR.
- CI (GitHub Actions) a cada PR: instalar dependências, lint, testes unitários e testes de integração com PostgreSQL.
- PR com CI falhando não é integrado.
- Mudanças de reserva ou cancelamento exigem os testes de concorrência e de limite diário (RN-ARQ-01).

## Conflitos
- Quem abriu o PR por último atualiza a branch com a `main`, resolve, roda os testes de novo e avisa no PR.
- Conflito não trivial: resolver junto com quem mexeu no trecho.

## Se o build quebrar na mainline
- "Nunca quebre a construção." Quem fez o último merge reverte ou corrige em até 24 horas.
- Nenhum outro PR é integrado até a `main` voltar a passar.

## Responsabilidade
- Quem aprova e integra o PR responde pelo código integrado, não o agente.
- Detalhes em [AGENTS.md](../AGENTS.md#critério-de-conclusão-e-responsabilidade).
