# Gestão de Empréstimo de Equipamentos em Universidades e Laboratórios

> **Disciplina:** Processos de Engenharia de Software — ULBRA  
> **Equipe:** Arthur Passos de Matos & Tiago Schulz  
---

## Visão Geral e Problema Escolhido

Estudantes, pesquisadores e professores enfrentam forte burocracia e falta de previsibilidade para retirar e devolver equipamentos acadêmicos (como kits de robótica, osciloscópios, projetores e multimídia). 

### Problemas com processo Manual:
* **Inexistência de visibilidade em tempo real:** O estudante precisa se deslocar presencialmente até o laboratório apenas para verificar se um item está disponível.
* **Processos em papel e filas:** Controles via cadernos de protocolo ou planilhas descentralizadas causam rasuras, perda de registros e lentidão.
* **Inadimplência e descontrole:** Ausência de penalização efetiva para atrasos e falta de alertas automáticos sobre prazos de devolução.
* **Prejuízo patrimonial:** Equipamentos retornam danificados sem identificação do responsável ou simplesmente somem, gerando perdas por departamento.

---

## Contexto, Público-Alvo e Stakeholders

### **Contexto de Aplicação**
Cotidiano de laboratórios de ensino/pesquisa, centrais de multimídia e departamentos acadêmicos universitários.

### **Público-Alvo (Usuários Finais)**
* **Alunos e Pesquisadores:** Buscam previsibilidade de estoque, agilidade no empréstimo e autonomia para renovações remotas.
* **Técnicos e Monitores de Laboratório:** Necessitam de agilidade no atendimento de balcão e conferência padronizada do estado do material.

### **Stakeholders (Partes Interessadas)**
* **Coordenadores de Curso e Chefes de Departamento:** Foco na redução de perdas financeiras, mitigação de retenção indevida e acesso justo aos bens.
* **Setor de TI e Manutenção:** Responsáveis pela infraestrutura de dados e manutenção de equipamentos avariados.
* **Diretoria e Gestão Acadêmica:** Interessados na auditoria e preservação do patrimônio institucional.

---

## Ciclo de Vida Escolhido e Justificativa
Optou-se por uma **abordagem híbrida: Incremental + Ágil**.

### **Justificativa da Defesa:**
1. **Modelo Incremental:** Permite fatiar o projeto em módulos entregáveis. A **fase inicial** foca direto na maior dor (consulta de estoque e reserva online para eliminar viagens desnecessárias). Módulos de relatórios de avarias e alertas são adicionados nos incrementos seguintes.
2. **Modelo Ágil:** Viabiliza testar cada funcionalidade e interface diretamente no balcão de atendimento com os Monitores e Alunos. Com feedbacks frequentes, ajusta-se a usabilidade antes de fechar o código, evitando sistemas engessados ou burocráticos.

---

## Técnica de Elicitação Aplicada
A técnica aplicada foi a **Entrevista Semiestruturada** realizada com o papel de Coordenador de Curso / Chefe de Departamento (Prof.ª Helena).

### **Por que a Entrevista?**
Permitiu extrair dores reais do fluxo diário, entender exceções de prazo (ex: TCC/pesquisas de campo), identificar o impacto financeiro das perdas e capturar regras institucionais implícitas que não constavam em documentos estáticos.

---

## Requisitos e Regras de Negócio Validados

### **Regras de Negócio (RN)**
* **RN1:** Usuários com pendências ou atrasos na devolução de equipamentos ficam impedidos de realizar novos empréstimos e reservas até a regularização do débito.
* **RN2:** O prazo padrão de empréstimo para qualquer equipamento é de **7 dias corridos** a partir da confirmação da retirada.

### **Requisitos Funcionais (RF) e Histórias de Usuário (US)**
* **RF1:** O sistema deve bloquear automaticamente novas solicitações de empréstimo e reserva para usuários que possuírem pendências ou atrasos na devolução.
  * **US2:** *Como coordenadora do departamento, quero que o sistema bloqueie usuários em atraso, para garantir um acesso justo aos equipamentos para todos os alunos.*
* **RF2:** O sistema deve enviar uma notificação de lembrete por e-mail e aplicativo ao usuário 24 horas antes do prazo de devolução expirar.
* **RF3:** O sistema deve permitir a renovação remota do empréstimo de um equipamento e a assinatura digital do termo de responsabilidade, caso não existam solicitações de reserva pendentes na fila para o mesmo item.
  * **US1:** *Como aluno do curso, quero renovar o empréstimo do meu equipamento pelo celular e assinar o comprovante digitalmente, para que não precise me deslocar até o laboratório apenas para assinar documentos.*

### **Requisitos Não Funcionais (RNF)**
* **RNF1:** O serviço de mensageria interna do sistema deve processar e encaminhar à API de disparo **100% das notificações (e-mail e push) em até 30 segundos** após atingir o gatilho de 24 horas antecedentes ao vencimento.
* **RNF2:** O sistema deve manter uma disponibilidade de **99,5% durante os meses letivos** (fevereiro a junho e agosto a dezembro) e carregar a tela de renovação/solicitação em até **2 segundos** sob conexão móvel de 10 Mbps com 100 usuários simultâneos.

---

## Priorização de Requisitos (Matriz MoSCoW)

| Categoria | Item | Descrição / Justificativa |
| :--- | :--- | :--- |
| **MUST HAVE** *(Obrigatório)* | **RF1** | Bloqueio automático de inadimplentes (resolve a principal dor informada no levantamento). |
| | **RN2** | Prazo padrão de 7 dias (regra base essencial para cálculo de alertas e bloqueios). |
| | **RNF1** | Processamento veloz das notificações em até 30s (garante a confiabilidade dos avisos). |
| **SHOULD HAVE** *(Muito Importante)*| **RF2** | Notificação preventiva de 24 horas (reduz atrasos "sem querer", embora o sistema opere sem ela inicialmente). |
| | **RNF2** | Disponibilidade de 99,5% e resposta em até 2s nos meses letivos (garante desempenho nos picos acadêmicos). |
| **COULD HAVE** *(Desejável)* | **RF3 / US1**| Renovação remota com assinatura digital (agrega muita praticidade ao aluno, mas pode ser feita presencialmente no lançamento inicial). |
| **WOULD HAVE** *(Futuro)* | — | Mapeamento avançado de relatórios de manutenção preventivas e integrações patrimoniais complexas. |

---

## Critérios de Aceitação (BDD / Dado-Quando-Então)

```gherkin
Critério de Aceitação — RF1 (Bloqueio de Inadimplentes)
Dado que o aluno possui um equipamento com prazo de devolução vencido,
Quando ele tentar solicitar um novo empréstimo ou reserva no sistema,
Então o sistema deve negar a solicitação e exibir a mensagem de bloqueio por inadimplência.

Critério de Aceitação — RF2 (Lembrete de Vencimento)
Dado que o aluno possui um empréstimo ativo cujo prazo de devolução vencerá em exatamente 24 horas,
Quando o serviço de mensageria atingir esse horário limite,
Então o sistema deve disparar automaticamente um e-mail e uma notificação push no aplicativo informando a data e hora limite para a devolução.

Critério de Aceitação — RF3 (Renovação Remota)
Dado que o aluno possui um equipamento emprestado e não existem reservas pendentes de outros usuários para esse item,
Quando o aluno solicitar a renovação remota pelo aplicativo móvel e assinar o comprovante digitalmente,
Então o sistema deve estender o prazo de devolução por mais 7 dias e emitir a confirmação da renovação na tela.
```

## Histórico de uso de IA

| Etapa | Data | Ferramenta | Partes em que foi usada | O que foi feito sem apoio de IA |
|-------|------|------------|---------------------------|-----------------------------------|
| Atividade 3 | 18/08/2026 | gemini | Foi usado para conduzir a entrevista a IA criando uma persona do stakeholder escolhido pela equipe | |
| Atividade 4 | 25/08/2026 | gemini | Ia fez a reformulação da escrita e revisão geral para a entrega final dos textos escritos | Foi usado a revisão de escrita geral para explicação e outras partes da escrita |
| AP1 | *a preencher no Encontro 6* | | | |
| AP2 | *a preencher no Encontro * | | | |
| AS | *a preencher no Encontro * | | | |
