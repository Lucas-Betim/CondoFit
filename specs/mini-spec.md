# Mini-Spec: Agendamento de Academia e Gestão de Indisponibilidade

**Pedido original (vago):** *"Precisamos de um jeito simples para os moradores reservarem a academia do prédio sem dar confusão de horário, e que o síndico consiga fechar o espaço quando tiver manutenção."*

---

## 1. Objetivo
Implementar o fluxo central de agendamento de horários da academia do condomínio e o controle administrativo de bloqueios por manutenção, garantindo que nenhuma faixa de horário tenha conflitos de ocupação e mantendo histórico auditável das operações.

## 2. Público-Alvo
* **Moradores do condomínio:** utilizam o sistema para verificar horários livres, realizar reservas para seu apartamento e cancelar agendamentos próprios.
* **Administrador do condomínio / Síndico:** utiliza o sistema para acompanhar a ocupação diária e registrar bloqueios preventivos ou corretivos de manutenção.

## 3. Critérios Verificáveis de Sucesso
1. **Unicidade de horário:** O sistema recusa 100% das tentativas de reserva que sobreponham total ou parcialmente uma reserva `ativa` ou um `bloqueio` existente.
2. **Ciclo de vida da reserva:** Uma reserva criada nasce como `ativa`; ao ser cancelada, muda para `cancelada` e libera o slot imediatamente, permanecendo visível no histórico administrativo.
3. **Gestão de manutenção:** O administrador consegue cadastrar um bloqueio (data, início, fim, motivo) e recebe alerta explícito caso já existam reservas ativas naquele intervalo, sem que o sistema as apague sozinho.
4. **Consulta diária:** Ao informar uma data válida (`AAAA-MM-DD`), o administrador visualiza todas as reservas daquele dia com apartamento, horário e status.

## 4. Requisito Não Funcional (RNF) Mensurável
* **Latência de validação de conflito e consulta:** O tempo de resposta entre o envio da solicitação (reserva ou consulta por data) e a resposta da API/camada de serviço deve ser **inferior a 2.000 ms (2 segundos)** no percentil 95 (p95), utilizando consultas indexadas por data.

## 5. Contexto Arquitetural
* **Camada de Serviços e Dados (ADR-001):** Toda regra de verificação de conflito (interseção de intervalos de tempo) deve residir na camada de serviços, antes da persistência, evitando lógica de negócio espalhada na interface.
* **Integridade de Histórico (RN-ARQ-05):** Entidades `Reserva` e `BloqueioManutencao` utilizam exclusão lógica (status `ativa` / `cancelada`), preservando registros para rastreabilidade.
* **Modelo Conceitual:** Uma `Reserva` vincula `apartamento`, `data`, `hora_inicio`, `hora_fim` e `status`. Um `Bloqueio` vincula `data`, `hora_inicio`, `hora_fim` e `motivo`.

## 6. Armadilha (Risco Técnico / Regra de Negócio)
* **Armadilha de Condição de Corrida (Race Condition) e Sobreposição Parcial:**
  1. *Sobreposição parcial:* Validar apenas horários de início idênticos (`hora_inicio == nova_hora_inicio`) falha quando um morador reserva das `08:00 às 10:00` e outro tenta reservar das `09:00 às 10:00`. A regra matemática deve verificar interseção de intervalos: `(InicioA < FimB) E (FimA > InicioB)`.
  2. *Cancelamento automático silencioso:* Ao criar um bloqueio de manutenção em cima de reservas já existentes, o sistema **não** deve cancelar as reservas automaticamente sem aviso, pois o morador iria até a academia sem saber. O sistema deve alertar o administrador para que ele notifique os apartamentos afetados.
