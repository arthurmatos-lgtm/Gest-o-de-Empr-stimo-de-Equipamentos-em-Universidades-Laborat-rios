# Gestão de Empréstimo de Equipamentos em Universidades e Laboratórios

> **Disciplina:** Processos de Engenharia de Software — ULBRA  
> **Versão Atual:** `v1.1.0` (Versão Final AS) | [Ver CHANGELOG.md](./CHANGELOG.md)  
> **Protótipo:** [Acessar Protótipo Interativo](https://www.figma.com/proto/mMbNANZy53PAsDArtVP9SP/Untitled?node-id=2-2&p=f&t=hU1RkH9PPpAKbjCj-0&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1)

---

## quipe e Papéis

- **Arthur Passos de Matos:** Desenvolvedor / QA / Revisor
- **Tiago Schulz:** Desenvolvedor / Arquiteto / Revisor
- **Prof.ª Helena:** Product Owner (PO) / Stakeholder (Coordenação de Curso e Chefia de Departamento)

---

## Mapa do Dossiê (Navegação)

Todos os artefatos do projeto estão organizados e rastreados por IDs padronizados:

- **[docs/01-concepcao](./docs/01-concepcao):** Problema, contexto, público-alvo, ciclo de vida e elicitação (A1 a A3).
- **[docs/02-requisitos](./docs/02-requisitos):** Requisitos (RF/RNF), Regras de Negócio (RN), Backlog (B01–B08) e Riscos (A4, A5, A8).
- **[docs/03-design](./docs/03-design):** Modelos UML, Diagrama de Casos de Uso e Diagrama de Atividades (A9).
- **[docs/04-arquitetura](./docs/04-arquitetura):** Arquitetura Conceitual em 4 Camadas, Fallback R01 e Matriz de Rastreabilidade (A10).
- **[docs/05-qualidade](./docs/05-qualidade):** Plano de Qualidade (ISO/IEC 25010), Inspeção, Suíte de Testes (CT01–CT12), Dívidas Técnicas e Entrega Final AS (A11, A12, A13, A15).
- **[docs/06-mudancas](./docs/06-mudancas):** Gestão de Mudanças, Baseline e Solicitação SM-01 (A14).

---

## Visão Geral e Problema Escolhido

Estudantes, pesquisadores e professores enfrentam forte burocracia e falta de previsibilidade para retirar e devolver equipamentos acadêmicos (como kits de robótica, osciloscópios, projetores e multimídia). 

### Problemas com o Processo Manual
- **Inexistência de visibilidade em tempo real:** O estudante precisa se deslocar presencialmente até o laboratório apenas para verificar se um item está disponível.
- **Processos em papel e filas:** Controles via cadernos de protocolo ou planilhas descentralizadas causam rasuras, perda de registros e lentidão.
- **Inadimplência e descontrole:** Ausência de penalização efetiva para atrasos e falta de alertas automáticos sobre prazos de devolução.
- **Prejuízo patrimonial:** Equipamentos retornam danificados sem identificação do responsável ou simplesmente somem, gerando perdas de R$ 20 mil a R$ 35 mil por ano no departamento.

---

## Contexto, Público-Alvo e Stakeholders

- **Contexto de Aplicação:** Cotidiano de laboratórios de ensino/pesquisa, centrais de multimídia e departamentos acadêmicos universitários.
- **Público-Alvo:**
  - **Alunos e Pesquisadores:** Buscam previsibilidade de estoque, agilidade no empréstimo e autonomia para renovações remotas.
  - **Técnicos e Monitores de Laboratório:** Necessitam de agilidade no atendimento de balcão e conferência padronizada do estado do material.
- **Stakeholders:**
  - **Coordenadores de Curso e Chefes de Departamento (Prof.ª Helena):** Foco na redução de perdas financeiras, mitigação de retenção indevida e acesso justo aos bens.
  - **Setor de TI e Manutenção:** Responsáveis pela infraestrutura de dados e manutenção de equipamentos avariados.
  - **Diretoria e Gestão Acadêmica:** Interessados na auditoria e preservação do patrimônio institucional.

---

## Ciclo de Vida e Elicitação

- **Ciclo de Vida Escolhido (Híbrido: Incremental + Ágil / Scrumban):**
  1. **Incremental:** Permite fatiar o projeto em módulos entregáveis de valor (Incremento 1 focado no bloqueio por inadimplência, prazos e fila; Incremento 2 focado em notificações preventivas e renovação remota).
  2. **Ágil (Scrumban):** Viabiliza Sprints de 2 semanas com quadro visual Kanban, permitindo testar funcionalidades diretamente com monitores e alunos e ajustar interfaces rapidamente antes de engessar o código.
- **Técnica de Elicitação Aplicada:** **Análise de Documentos** (termos de responsabilidade físicos, livros de protocolo e planilhas dos laboratórios) combinada com **Entrevista Semiestruturada** com a coordenadora Prof.ª Helena para capturar regras implícitas e exceções operacionais.

---

## Requisitos e Regras de Negócio Consolidados

### Regras de Negócio (RN)
- **RN1 (Bloqueio por Inadimplência):** Usuários com pendências financeiras/administrativas ou atrasos na devolução de equipamentos ficam impedidos de realizar novos empréstimos, reservas ou entrar na fila de espera até a regularização do débito.
- **RN2 (Prazo e Renovação — Atualizado pela SM-01):** O prazo padrão de empréstimo é de 7 dias corridos a partir da retirada. É permitida no máximo 1 (uma) renovação remota por mais 7 dias via App Mobile, desde que o aluno esteja adimplente (`RN1`), realize a reassinatura digital do termo (`US1`) e não exista nenhum outro aluno na fila de espera do equipamento (`RF4`).

### Requisitos Funcionais (RF) e Histórias de Usuário (US)
- **RF1 (`B01`):** O sistema deve bloquear automaticamente novas solicitações de empréstimo e reserva para usuários que possuírem pendências ou atrasos na devolução.
  - **US2:** *Como coordenadora do departamento, quero que o sistema bloqueie usuários em atraso, para garantir um acesso justo aos equipamentos para todos os alunos.*
- **RF2 (`B04`):** O sistema deve disparar automaticamente uma notificação preventiva de devolução (e-mail e push) ao usuário exatamente 24 horas antes do vencimento do prazo padrão de 7 dias (`RN2`).
- **RF3 / US1 (`B06` / `B07` — Atualizado pela SM-01):** O sistema deve permitir a renovação remota do empréstimo por mais 7 dias com coleta de assinatura digital do termo de responsabilidade pelo aplicativo, caso não existam reservas pendentes na fila.
  - **US1:** *Como aluno, quero solicitar a renovação remota do meu empréstimo no App por mais 7 dias e assinar o comprovante digitalmente, para não precisar ir ao balcão quando o equipamento não estiver sendo aguardado por outro colega.*
- **RF4 (`B08`):** O sistema deve validar a disponibilidade de estoque em tempo real no checkout; caso o estoque seja zero (e o aluno esteja adimplente), deve permitir a entrada na fila de espera automatizada, notificando o 1º da fila com janela de 24 horas para retirada após a devolução.

### Requisitos Não Funcionais (RNF)
- **RNF1 (`B03`):** O serviço de mensageria interna do sistema deve processar e encaminhar à API de disparo **100% das notificações em até 30 segundos** após atingir o gatilho de 24 horas antecedentes ao vencimento.
- **RNF2.1 (`B05` — Desempenho Síncrono):** **95% das requisições** do endpoint REST de checkout/renovação devem ser respondidas em **até 1,5 segundo** (máximo tolerável de 2,0s) sob carga de 100 usuários simultâneos.
- **RNF2.2 (`B05` — Disponibilidade):** O sistema deve manter índice de disponibilidade (*uptime*) de no mínimo **99,5% durante os meses letivos** (fevereiro a junho e agosto a dezembro), monitorado continuamente no Painel SLA.

---

## 📊 Priorização de Requisitos (Matriz MoSCoW)

| Categoria | Item / Backlog | Descrição / Justificativa |
| :--- | :--- | :--- |
| **MUST HAVE** *(Obrigatório)* | **RF1 / RN1 (`B01`)** | Bloqueio automático de inadimplentes (resolve a principal dor de retenção indevida). |
| | **RN2 (`B02`)** | Prazo padrão de 7 dias (regra base essencial para cálculo de alertas e bloqueios). |
| | **RNF1 (`B03`)** | Processamento assíncrono das notificações em até 30s (garante confiabilidade dos avisos e Fallback R01). |
| | **RF4 (`B08`)** | Validação de estoque e gestão de fila de espera com janela de 24h no checkout. |
| **SHOULD HAVE** *(Importante)* | **RF2 (`B04`)** | Notificação preventiva de 24 horas antes do vencimento (reduz atrasos "sem querer"). |
| | **RNF2.1 / RNF2.2 (`B05`)** | Tempo de resposta ≤ 1,5s e disponibilidade de 99,5% nos meses letivos. |
| | **RF3 / US1 (`B07` + `SM-01`)** | Renovação remota (+7 dias) com assinatura digital e trava de fila (antecipado pela SM-01). |
| **COULD HAVE** *(Desejável)* | **B06 (Adiado)** | Histórico resumido de empréstimos anteriores do aluno (movido para Sprint 3 para acomodar a SM-01). |
| **WOULD HAVE** *(Futuro)* | — | Relatórios avançados de manutenção preventiva e integração com barramentos legados (LDAP/SOAP). |

---

## Critérios de Aceitação (BDD) e Fluxo Demonstrado

O protótipo e a suíte de testes (`CT01` a `CT12`) validam os seguintes cenários críticos:

```gherkin
Cenário 1 — Bloqueio de Inadimplentes (RF1 / RN1 / CT02)
Dado que o aluno possui um equipamento com prazo de devolução vencido,
Quando ele tentar solicitar um novo empréstimo, reserva ou entrar na fila de espera,
Então o sistema deve negar a solicitação e exibir a mensagem de bloqueio por inadimplência.

Cenário 2 — Lembrete Preventivo de Vencimento (RF2 / RNF1 / CT05-CT07)
Dado que o aluno possui um empréstimo ativo cujo prazo vencerá em exatamente 24 horas,
Quando o serviço de mensageria atingir esse horário limite,
Então o sistema deve processar a fila em até 30s e disparar e-mail e push (acionando o provedor de fallback em caso de falha na API principal).

Cenário 3 — Renovação Remota com Assinatura Digital e Trava de Fila (RF3 / SM-01 / CT11-CT12)
Dado que o aluno adimplente possui um empréstimo ativo (ainda não renovado) e não existem reservas pendentes na fila para o item,
Quando o aluno solicitar a renovação remota pelo aplicativo móvel e assinar o termo digitalmente,
Então o sistema deve estender o prazo de devolução por mais 7 dias e emitir o novo comprovante.
