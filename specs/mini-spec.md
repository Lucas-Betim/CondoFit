# Mini-Spec: Agendamento de Academia e Gestão de Indisponibilidade

**Pedido original (vago):** *"Precisamos de um jeito simples para os moradores reservarem a academia do prédio sem dar confusão de horário, e que o síndico consiga fechar o espaço quando tiver manutenção."*

---

## 1. Objetivo
Implementar o fluxo central de agendamento de horários da academia do condomínio (com identificação por apartamento e confirmação por e-mail) e o controle administrativo de bloqueios por manutenção, garantindo o cumprimento das regras de uso por apartamento/dia e mantendo histórico auditável das operações.

## 2. Público-Alvo
* **Moradores do condomínio:** identificados pelo número do apartamento, utilizam o sistema para agendar sessões de 1 hora, remarcar após cancelamento e receber confirmações por e-mail.
* **Administrador do condomínio / Síndico:** utiliza o sistema para acompanhar a ocupação diária e registrar bloqueios preventivos ou corretivos de manutenção.

## 3. Critérios Verificáveis de Sucesso
1. **Regra de agendamento (1h / 1 por dia / simultaneidade):** O sistema permite que **apartamentos diferentes** reservem o mesmo horário (até o limite de lotação do espaço), mas bloqueia 100% das tentativas de um **mesmo apartamento** possuir mais de 1 reserva `ativa` no mesmo dia, garantindo duração fixa de **1 hora** por reserva.
2. **Ciclo de vida e remarcação:** Uma reserva criada nasce como `ativa`; ao ser cancelada, muda para `cancelada`, permanece no histórico e libera imediatamente tanto a vaga no horário quanto a permissão para que aquele apartamento faça uma nova reserva (remarcação) no mesmo dia.
3. **Vínculo e confirmação:** Toda reserva exige vínculo válido com um apartamento e dispara e-mail de confirmação na criação e no cancelamento.
4. **Gestão de manutenção:** O administrador consegue cadastrar um bloqueio (data, início, fim, motivo) e recebe alerta explícito caso já existam reservas ativas naquele intervalo, sem cancelá-las automaticamente.
5. **Consulta diária:** Ao informar uma data válida (`AAAA-MM-DD`), o administrador visualiza todas as reservas daquele dia com apartamento, horário e status.

## 4. Requisito Não Funcional (RNF) Mensurável
* **Latência de validação e consulta (alinhado ao RNF-01):** O tempo de resposta entre o envio da solicitação (criação de reserva, validação de regras ou consulta por data) e o retorno da aplicação deve ser **inferior a 2.000 ms (2 segundos) no percentil 95 (p95)**, utilizando consultas indexadas por data.

## 5. Contexto Arquitetural
* **Camada de Serviços e Dados (ADR-001):** Toda regra de negócio (duração de 1 hora, limite de 1 reserva ativa por apartamento/dia, verificação de capacidade e bloqueios) reside na camada de serviços antes da persistência.
* **Integridade de Histórico:** Entidades `Reserva` e `BloqueioManutencao` utilizam exclusão lógica (status `ativa` / `cancelada`), preservando registros para rastreabilidade.
* **Modelo Conceitual:** Uma `Reserva` vincula `apartamento`, `data`, `hora_inicio`, `hora_fim` (sempre +1h), `email_contato` e `status`. Um `Bloqueio` vincula `data`, `hora_inicio`, `hora_fim` e `motivo`.

## 6. Armadilha (Risco Técnico / Regra de Negócio)
* **Armadilha de Bloqueio Exclusivo vs. Simultaneidade e Remarcação:**
  1. *Bloqueio exclusivo indevido:* Tratar a academia como sala privativa (impedindo que o Apt 101 e o Apt 102 reservem às `08:00`) viola a regra aprovada de uso compartilhado. O sistema deve validar a **duplicidade do mesmo apartamento no dia** e a **capacidade total do horário**, e não exclusividade de 1 apartamento por horário.
  2. *Bloqueio de remarcação após cancelamento:* Se a validação contar reservas com status `cancelada` no limite diário, o morador que cancelar um horário não conseguirá remarcar no mesmo dia. A regra deve filtrar estritamente `status == 'ativa'`.
  3. *Cancelamento automático silencioso na manutenção:* Ao criar um bloqueio em cima de reservas ativas, o sistema **não** deve cancelá-las automaticamente sem aviso, mas alertar o administrador.
