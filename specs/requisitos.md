# Especificação de Requisitos — CondoFit

Autora: Gabriela - Product Owner  
Data: 18/11/2026

---

## 1. Requisitos Funcionais (RF)

### RF-01: Realizar reserva de horário na academia
O sistema deve permitir que o morador agende um horário futuro na academia informando apartamento, data, hora de início e hora de término.
* **Critérios de aceitação:**
  * Validar que a data é presente ou futura e que `hora_fim > hora_inicio`.
  * Impedir sobreposição com reservas ativas existentes ou bloqueios de manutenção.
  * Salvar a reserva com status `ativa`.

### RF-02: Cancelar reserva ativa
O sistema deve permitir que o morador cancele uma reserva ativa antes do seu horário de início.
* **Critérios de aceitação:**
  * Alterar o status da reserva de `ativa` para `cancelada` (exclusão lógica).
  * Disponibilizar imediatamente a faixa de horário para novos agendamentos.
  * Bloquear cancelamentos de reservas passadas ou já canceladas.

### RF-03: Consultar reservas por data
O sistema deve permitir que o administrador do condomínio consulte todas as reservas registradas para uma data específica.
* **Critérios de aceitação:**
  * Retornar horário de início, horário de término, apartamento e situação (`ativa` ou `cancelada`) apenas da data informada.
  * Exibir aviso claro caso não existam reservas na data consultada.
  * Garantir operação estritamente de leitura (sem alterar dados).

### RF-04: Bloquear horários para manutenção
O sistema deve permitir que o administrador registre períodos de indisponibilidade da academia por motivo de manutenção.
* **Critérios de aceitação:**
  * Exigir data, hora inicial, hora final e descrição do motivo.
  * Impedir novas reservas que coincidam com o intervalo bloqueado.
  * Alertar o administrador caso já existam reservas ativas no período, sem cancelá-las automaticamente.

### RF-05: Consultar disponibilidade de horários
O sistema deve exibir ao morador os horários livres e ocupados de uma determinada data antes da confirmação da reserva.
* **Critérios de aceitação:**
  * Indicar visualmente ou textualmente faixas livres, reservadas e bloqueadas por manutenção.

---

## 2. Requisitos Não Funcionais (RNF)

### RNF-01: Desempenho e tempo de resposta nas consultas
As operações de consulta de disponibilidade e listagem de reservas por data devem retornar os resultados em **no máximo 2 segundos** sob carga normal de uso simultâneo do condomínio (até 50 requisições simultâneas).

### RNF-02: Integridade histórica e persistência de dados
Nenhuma reserva ou bloqueio de manutenção deve ser excluído fisicamente do armazenamento; cancelamentos devem ser registrados por alteração de status (`cancelada`), garantindo rastreabilidade e auditoria conforme definido na regra arquitetural `RN-ARQ-05`.
