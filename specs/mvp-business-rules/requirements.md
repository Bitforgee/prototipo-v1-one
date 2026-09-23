# Requirements: mvp-business-rules

Status: **DRAFT — awaiting product owner review.** Items tagged `[Qn]` depend on an open
question and carry a *proposed default* only.

Sources: `One_MVP_Visao_Produto.md` (MVP vision), `One_Caminho_Para_Criar_Um_Bom_Produto.md`
(product path).

## Problem

Students at the pilot institution send academic questions and requests through scattered
informal channels (WhatsApp, e-mail, direct messages, walk-ins). The institution cannot tell
who received a request, who is handling it, how long it took or whether it was solved;
students do not know which sector to contact and have to repeat themselves when redirected;
the coordination absorbs low-complexity questions; and there is no data about recurring
demands. The MVP must validate one flow: *ask → attempt self-service → if unresolved, become a
traceable request without losing context → reach the right sector → be handled by a human →
be measured.*

## Discovery evidence

Interview log. Each finding is one person's account until corroborated by another source.

### E1 — Coordinator of technology courses, Univag (2026-09)

Primary persona. Coordinates the technology courses, including ADS (Análise e Desenvolvimento
de Sistemas).

| # | Finding | Status |
|---|---|---|
| E1.1 | Communication with students is decentralised across channels. | Stated |
| E1.2 | There is no record/history of messages and conversations, and keeping one is an MEC (Ministério da Educação) requirement for the institution. | Stated; exact MEC requirement not yet identified `[Q15]` |
| E1.3 | Her ~40 h/week are shared between teaching, course coordination and managing professors; little time is left for student demands. | Stated |
| E1.4 | Class representatives were created as a filter so that only important matters reach her. | Stated; how it works in practice unknown `[Q14]` |
| E1.5 | The official channel was WhatsApp and has been changed to e-mail. | Stated |
| E1.6 | She keeps a demand funnel (types and volumes of demands). | Seen; contents not yet captured `[Q16]` |
| E1.7 | Main demands named: (a) boleto for the current semester; (b) atestado de escolaridade. Both — and other services — are already self-service in the Univag Portal do Aluno. | Stated; per-type volumes not captured `[Q16]` |
Implications for this spec:

- **Institutional traceability is a compliance need, not only a convenience (E1.2).** Section H
  becomes a core requirement, and the institution's reason to adopt may be compliance before
  time savings.
- **The coordinator's time is the scarce resource (E1.3).** The primary outcome to measure is
  the number of demands that reach her and the time she spends on them.
- **One competes with an existing filter, the class representatives (E1.4).** Ignoring them
  would either bypass the filter the coordination created or duplicate it. See Q14.
- **The current official channel is e-mail (E1.5).** It is the baseline One must beat, and it is
  also the channel students already watch (consistent with D2).
- **The top demands are already solvable by the student (E1.7).** The gap is not the service
  but knowing where it is. These are the first knowledge base articles (pointing to the Portal
  do Aluno) and the clearest test of self-resolution (AC1). Why students still ask the
  coordinator instead of using the portal is not yet known.

## Precondition: pilot data comes from Discovery

Categories, sectors, routing rules and knowledge base articles are **pilot configuration
data**, not business rules. Discovery has **started** (D1, one interview); their content must
come from it (interviews, top 10–20 demands, the coordinator's funnel) before the pilot. The
rules below must hold for any such data set. The category tree in the vision document (§18) is
an example only.

## Glossary

| Term (EN) | Term (PT) | Meaning |
|---|---|---|
| Student | Aluno | End user who asks questions and owns tickets |
| Agent | Atendente | Staff member who handles tickets of the sectors they belong to |
| Admin | Administrador | Configures users, sectors, categories, routing, knowledge base |
| Sector | Setor | Organisational unit that owns a queue (e.g. Secretaria, Financeiro, Coordenação ENG-SW) |
| Triage sector | Setor de triagem | The one sector that receives tickets no routing rule covers |
| Conversation | Conversa | A student's exchange with the assistant, from first message to outcome |
| Article | Artigo | Knowledge base entry used by the assistant |
| Ticket | Solicitação / Chamado | Traceable request created from an unresolved conversation |
| Public reply | Resposta | Message on a ticket visible to the student |
| Internal note | Nota interna | Message on a ticket visible only to agents and admins |
| Forward | Encaminhar | Move a ticket to another sector's queue |

## Required behaviour

### A. Identity and access

- **BR-A1** Only pre-provisioned accounts can sign in; there is no self sign-up. Admins create
  accounts (individually or by bulk import). `[Q5]`
- **BR-A2** A user has one or more roles among Student, Agent, Admin.
- **BR-A3** A Student record holds at least one course. An Agent record holds at least one sector.
- **BR-A4** A Student sees only their own conversations and tickets.
- **BR-A5** An Agent sees only tickets currently in a sector they belong to. After a ticket is
  forwarded out of their sectors, they lose access to it.
- **BR-A6** An Admin sees all tickets, all conversations and the dashboard.
- **BR-A7** A deactivated user cannot sign in; their historical records remain intact and
  attributed to them.

### B. Assistant conversation

- **BR-B1** A conversation belongs to exactly one student and starts with a free-text message.
- **BR-B2** The assistant answers **only** from articles with status `PUBLISHED`. It never
  produces an answer that is not grounded in an article. The assistant is an LLM that
  interprets the question and is restricted to the knowledge base (D3); free-form answers are
  forbidden.
- **BR-B3** When a matching article exists, the assistant shows its answer and asks
  "Did this solve it?" with exactly two options: Yes / No.
- **BR-B4** When no article matches, the assistant says it did not find an answer and offers
  to create a request (goes to BR-D1). It must not show an unrelated article.
- **BR-B5** "Yes" ends the conversation with outcome `SELF_RESOLVED`.
- **BR-B6** "No" offers to create a request (BR-D1).
- **BR-B7** Every conversation ends with exactly one outcome:
  `SELF_RESOLVED` | `ESCALATED` (ticket created) | `DECLINED` (student refused the request
  offer) | `ABANDONED` (no Yes/No or request decision within the inactivity window). `[Q6]`
- **BR-B8** The conversation stores a snapshot of each answer shown (article id, version and
  text), so later article edits do not change history.
- **BR-B9** A conversation produces at most one ticket. A new problem requires a new conversation.

### C. Knowledge base

- **BR-C1** An article has: title, related questions (≥ 1), category, answer content, optional
  links, status, last updated at, last updated by.
- **BR-C2** Article status is `DRAFT` | `PUBLISHED` | `ARCHIVED`. Only `PUBLISHED` articles are
  used by the assistant.
- **BR-C3** Only Admins create, edit, publish and archive articles.
- **BR-C4** Every article belongs to exactly one category.
- **BR-C5** Editing a published article creates a new version; conversations keep the version
  they showed (BR-B8).

### D. Conversation becomes a ticket

- **BR-D1** A ticket is created only after the student explicitly confirms the request offer.
  Before confirming, the student may add an optional free-text description (D4).
- **BR-D2** The ticket receives, without the student retyping anything:
  requester, the student's course, the full conversation transcript (read-only), the
  article(s) shown, a title, a category and a sector.
- **BR-D3** Title = the student's first message, truncated to a fixed length.
- **BR-D4** Category = category of the last article shown and rejected; if no article was shown,
  category = `Unclassified`.
- **BR-D5** Ticket number format `ONE-<n>`, sequential, unique, never reused.
- **BR-D6** On creation the student sees the ticket number and it appears in their ticket list.

### E. Classification and routing

- **BR-E1** Every ticket is in exactly one sector at all times (invariant).
- **BR-E2** A routing rule maps a category to either (a) a fixed sector, or (b) "the
  coordination sector of the student's course".
- **BR-E3** Routing is evaluated once, at ticket creation. A category with no rule, the
  `Unclassified` category, or a course with no coordination sector configured routes to the
  triage sector.
- **BR-E4** Exactly one sector is flagged as the triage sector; the system refuses a
  configuration without one.
- **BR-E5** An Agent may reclassify a ticket (change its category). Reclassifying does **not**
  move the ticket; moving is done only by Forward (BR-G4). The original category is kept for
  metrics.

### F. Ticket lifecycle

States: `OPEN`, `IN_PROGRESS`, `WAITING_STUDENT`, `RESOLVED`, `CLOSED`.

| # | From | To | Trigger | Actor | Rules |
|---|---|---|---|---|---|
| F1 | — | OPEN | Ticket created | System | No assignee |
| F2 | OPEN | IN_PROGRESS | Assume, or first public reply | Agent of the ticket's sector | Assignee = that agent |
| F3 | IN_PROGRESS | WAITING_STUDENT | Public reply marked "needs student answer" | Assignee | — |
| F4 | WAITING_STUDENT | IN_PROGRESS | Student replies | Student | Automatic |
| F5 | IN_PROGRESS | RESOLVED | Resolve | Assignee | Resolution text required, visible to student |
| F6 | RESOLVED | IN_PROGRESS | Student rejects resolution | Student | Only within the confirmation window; reason required; reopen count +1 `[Q7]` |
| F7 | RESOLVED | CLOSED | Student confirms, or confirmation window expires | Student / System | Closure reason = `CONFIRMED` or `AUTO_CLOSED` `[Q7]` |
| F8 | OPEN / IN_PROGRESS / WAITING_STUDENT | CLOSED | Student cancels | Student | Closure reason = `CANCELED`; reason text optional |
| F9 | OPEN / IN_PROGRESS / WAITING_STUDENT | OPEN (new sector) | Forward | Agent of current sector | See BR-G4 |

- **BR-F1** Any transition not in the table is rejected.
- **BR-F2** `CLOSED` is terminal. No messages can be added to a closed ticket.
- **BR-F3** An agent may take over a ticket assigned to another agent of the same sector;
  the change is recorded. `[Q8]`

### G. Handling a ticket

- **BR-G1** The student can add public replies to their ticket in any non-closed state.
- **BR-G2** Agents can add public replies and internal notes. Internal notes are never visible
  to the student.
- **BR-G3** Messages cannot be edited or deleted after being sent.
- **BR-G4** Forward requires a target sector different from the current one and a reason
  (stored as an internal note). The ticket returns to `OPEN` in the target sector, the assignee
  is cleared and the forward count increases by 1.
- **BR-G5** Attachments may be added to public replies by student and agents, limited to
  allowed file types and maximum size. `[Q9]`
- **BR-G6** The student receives an e-mail for every agent public reply and on resolution, and the
  current status is always visible in the app (D2). Internal notes never trigger e-mail.

### H. History and audit

- **BR-H1** Every creation, transition, assignment, forward, reclassification, reply, note and
  attachment is recorded with actor and timestamp.
- **BR-H2** The history of a ticket is append-only and visible to anyone who can see the ticket
  (internal notes excepted for students).

### I. Metrics (definitions, not UI)

All times are calendar time unless `[Q10]` decides otherwise. Period filters apply by
conversation start or ticket creation date.

| Metric | Definition |
|---|---|
| Active students | Distinct students with ≥ 1 conversation in the period |
| Conversations | Conversations started in the period |
| Self-resolution rate | `SELF_RESOLVED` ÷ conversations with an outcome |
| Escalation rate | `ESCALATED` ÷ conversations with an outcome |
| Abandonment rate | `ABANDONED` ÷ conversations with an outcome |
| No-answer rate | Conversations where no article matched ÷ conversations (feeds the knowledge base backlog) |
| Open tickets | Tickets not `CLOSED` at the moment of the query |
| Resolved tickets | Tickets that reached `RESOLVED` at least once in the period |
| Time to first response | Ticket creation → first agent public reply |
| Time to resolution | Ticket creation → last transition to `RESOLVED` |
| Correct routing rate | Tickets resolved with forward count = 0 ÷ tickets resolved |
| Reopen rate | Tickets with reopen count ≥ 1 ÷ tickets resolved |
| Top categories | Ranking by conversations (article category) and by tickets (original category) |
| North Star (system proxy) | (`SELF_RESOLVED` + tickets `CLOSED` with reason `CONFIRMED` or `AUTO_CLOSED` and reopen count = 0) ÷ conversations with an outcome |

- **BR-I1** The "without looking for another channel" part of the North Star and the "fewer
  direct messages to coordination" metric cannot be measured by the system; they are measured
  by the pilot (baseline count + survey) and are outside this spec.

## Acceptance criteria

Each scenario is checkable end to end in the running system.

- [ ] **AC1 — Self-service.** Given a published article for "second copy of boleto", when a
      student asks "preciso emitir meu boleto" and answers Yes, then the conversation outcome is
      `SELF_RESOLVED` and no ticket exists.
- [ ] **AC2 — Escalation keeps context.** Given AC1's article, when the student answers No and
      confirms the request, then a ticket `ONE-<n>` exists in `OPEN`, category = article
      category, sector = routing rule target, and the agent sees the full transcript without
      the student retyping.
- [ ] **AC3 — No match.** Given no matching article, when the student asks, then no article is
      shown, and a confirmed request creates a ticket with category `Unclassified` in the
      triage sector.
- [ ] **AC4 — Course routing.** Given a rule "Academic > Grades → course coordination" and a
      student of course X, then the ticket goes to course X's coordination sector; for a course
      with no coordination configured it goes to the triage sector.
- [ ] **AC5 — Lifecycle.** Every transition F1–F9 succeeds for the allowed actor, and every
      other state/actor combination is rejected.
- [ ] **AC6 — Forward.** A forward without a reason is rejected; a valid forward moves the
      ticket to `OPEN` in the new sector, clears the assignee, increments forward count, and
      the previous sector's agents can no longer open it.
- [ ] **AC7 — Visibility.** A student cannot open another student's ticket; a student never
      sees an internal note; an agent cannot open a ticket outside their sectors.
- [ ] **AC8 — Immutable history.** Editing an article after it was shown does not change the
      text stored in past conversations and tickets; messages cannot be edited or deleted.
- [ ] **AC9 — Metrics.** Given a seeded data set with known outcomes, every metric in section I
      returns the value computed by hand.

## Out of scope

Binding for this spec. Changing it requires an explicit decision.

- Native mobile app, WhatsApp, push notifications, real-time chat.
- Moodle, ERP, Portal do Aluno or financial integrations; the assistant executing actions in
  external systems.
- Priority field and any SLA (no rule would use them in the MVP).
- AI-based routing, multiple AI agents, automatic detection of collective incidents.
- Automatic knowledge-base learning from resolved tickets (the §8.4 loop). Admins write
  articles manually.
- Article audience segmentation by course or role (all published articles serve all students).
- Multi-institution / multi-tenant, billing, full LGPD programme (Phase 7).
- Advanced analytics, reports export, per-sector dashboards for agents.
- CMDB, inventory, contracts, ITIL problem/change management, BPMN workflows.
- Satisfaction survey (CSAT) inside the product.

## Decisions

- **D1** Discovery started (interview E1); this spec stays data-agnostic until the demand
  list is captured. (was Q1)
- **D2** Student notification = e-mail + in-app status. (was Q2)
- **D3** Assistant = LLM restricted to published articles (RAG). (was Q3)
- **D4** Ticket creation needs explicit confirmation; description optional. (was Q4)

## Open questions

Each has a proposed default; nothing tagged with it should be designed until it is answered.

- **Q5 — Authentication.** Institutional SSO or local accounts? *Default:* local accounts
  provisioned by admin (bulk import) for the pilot.
- **Q6 — Inactivity window for `ABANDONED`.** *Default:* 30 minutes.
- **Q7 — Confirmation window for `RESOLVED` → `CLOSED`.** *Default:* 7 calendar days.
- **Q8 — Takeover within a sector.** Can any agent of the sector take a ticket assigned to a
  colleague? *Default:* yes, recorded in history.
- **Q9 — Attachments.** In the MVP? *Default:* yes, PDF/JPG/PNG, ≤ 10 MB each.
- **Q10 — Business hours in time metrics.** *Default:* calendar time for the MVP.
- **Q11 — Coordination as a sector.** Is "Coordenação do curso X" modelled as an ordinary
  sector with agents (the coordinator is an agent)? *Default:* yes.
- **Q12 — Who sees the dashboard.** Admin only, or also the coordination? *Default:* Admin only.
- **Q13 — Student data sent to an LLM provider (from D3).** Student messages may contain personal
  data and go to an external provider. Does the pilot institution accept that, and is a notice
  to students required? *Default:* acceptable for the pilot with a visible notice; no sensitive
  documents are sent to the LLM (attachments never are).
- **Q14 — Class representatives (from E1.4).** Previously out of scope; reopened by E1.
  Options: (a) not modelled — One replaces what representatives filter today;
  (b) representatives are agents of a "class" queue that sits before the coordination;
  (c) representatives only see/aggregate their class's tickets, without handling them.
  Needs: what representatives actually filter today and whether the coordinator wants to keep
  them in the loop. *Default:* none — decide after Q16.
- **Q15 — MEC requirement (from E1.2).** Which instrument/indicator requires the record, what
  must be kept (messages, responses, dates, who answered), for how long, and in what form it is
  shown to evaluators. This decides retention and whether a history report/export must leave
  **Out of scope**. *Default:* none — must be sourced, not assumed.
- **Q16 — Demand funnel (from E1.6).** Capture the funnel: demand types, weekly volume per type,
  who resolves each (representative / coordination / secretaria / other), and which ones only
  she can resolve. This becomes the category list, routing rules and the first articles.
  *Partial (E1.7):* boleto of the current semester and atestado de escolaridade, both via
  Portal do Aluno; volumes and the rest of the list still missing.
- **Q17 — Baseline (from E1.3/E1.5).** How many demands reach the coordinator per week today, by
  e-mail and other channels, and how many hours she spends on them? Without this number the
  pilot cannot show improvement.
  *Team guess, not evidence (not stated by the coordinator):* ~60/week, ~30% simple,
  > 4 h/week, 1–3 days to answer. Use only to prepare the interview; must be replaced by her
  numbers or by a count of her e-mails.
