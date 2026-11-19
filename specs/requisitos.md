# Especificação de Requisitos — CondoFit

Autores: Gabriela, Lucas Claro - Product Owner  
Data: 18/11/2026

---

## 1. Requisitos Funcionais (RF)

### RF-01: Identificação e vínculo com apartamento
O sistema deve identificar o morador e vincular suas operações ao respectivo número de apartamento do condomínio antes de permitir qualquer agendamento ou cancelamento.
* **Critérios de aceitação:**
  * Validar que o apartamento informado pertence ao cadastro ativo do condomínio.
  * Associar todas as reservas e consultas de histórico do morador ao seu identificador de apartamento.

### RF-02: Realizar reserva de horário na academia
O sistema deve permitir que o morador agende um horário futuro na academia vinculado ao seu apartamento, permitindo reservas simultâneas de apartamentos diferentes no mesmo horário (respeitando a capacidade máxima do espaço).
* **Critérios de aceitação:**
  * Garantir duração fixa de **1 hora** por reserva.
  * Permitir no máximo **1 (uma) reserva ativa por apartamento por dia**.
  * Permitir que diferentes apartamentos reservem o mesmo horário, desde que não ultrapasse a lotação máxima configurada da academia e que o horário não esteja bloqueado para manutenção.
  * Permitir que um apartamento realize uma nova reserva (remarcação) no mesmo dia caso tenha cancelado sua reserva anterior.
  * Salvar a reserva com status `ativa`.

### RF-03: Cancelar reserva ativa e permitir remarcação
O sistema deve permitir que o morador cancele uma reserva ativa antes do seu horário de início, liberando o direito de nova marcação no mesmo dia.
* **Critérios de aceitação:**
  * Alterar o status da reserva de `ativa` para `cancelada` (exclusão lógica).
  * Liberar imediatamente a vaga no horário e habilitar o apartamento a realizar uma nova reserva naquele mesmo dia.
  * Bloquear cancelamentos de reservas passadas ou já canceladas.

### RF-04: Envio de confirmação por e-mail
O sistema deve enviar uma notificação de confirmação por e-mail ao morador sempre que uma reserva for criada ou cancelada.
* **Critérios de aceitação:**
  * Enviar e-mail automático contendo apartamento, data, horário (início e término) e status da reserva (`ativa` ou `cancelada`) imediatamente após a conclusão da operação.

### RF-05: Consultar reservas por data
O sistema deve permitir que o administrador do condomínio consulte todas as reservas registradas para uma data específica.
* **Critérios de aceitação:**
  * Retornar horário de início, horário de término, apartamento e situação (`ativa` ou `cancelada`) apenas da data informada.
  * Exibir aviso claro caso não existam reservas na data consultada.
  * Garantir operação estritamente de leitura (sem alterar dados).

### RF-06: Bloquear horários para manutenção
O sistema deve permitir que o administrador registre períodos de indisponibilidade da academia por motivo de manutenção.
* **Critérios de aceitação:**
  * Exigir data, hora inicial, hora final e descrição do motivo.
  * Impedir novas reservas que coincidam com o intervalo bloqueado.
  * Alertar o administrador caso já existam reservas ativas no período, sem cancelá-las automaticamente.

---

## 2. Requisitos Não Funcionais (RNF)

### RNF-01: Desempenho e tempo de resposta nas consultas e reservas
O tempo de resposta entre o envio da solicitação (criação de reserva, validação de regras ou consulta por data) e o retorno da aplicação deve ser **inferior a 2.000 ms (2 segundos) no percentil 95 (p95)**, utilizando consultas indexadas por data.

### RNF-02: Integridade histórica e persistência de dados
Nenhuma reserva ou bloqueio de manutenção deve ser excluído fisicamente do armazenamento; cancelamentos devem ser registrados por alteração de status (`cancelada`), garantindo rastreabilidade e auditoria no histórico do sistema.
