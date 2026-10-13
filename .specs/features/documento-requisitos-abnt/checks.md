# Documento de levantamento e requisitos (ABNT) checks

Profile: light
Plan: `.specs\features\documento-requisitos-abnt\plan.md`

36 checks in 3 slices · 5 one-way doors · 0 open, of which 0 block

Every `-k` proof runs as
`python -m unittest discover -s docs/requisitos -p test_documento.py -k <name>`.

## Checks

### S1 - Formatted ABNT document generated · 6 files · ~45 KB · ~11k

**C1** - `python docs/requisitos/gerar.py` writes `One_Levantamento_e_Requisitos.docx` and `.pdf` in `docs/requisitos/`, prints both paths and exits 0 (AC 1) · closed
Proof: `python docs/requisitos/gerar.py`
Proof: `-k test_saidas_existem`

**C2** - With Java missing, or with Word missing, the generator exits 1, prints the missing tool's name and leaves the previous `.docx` and `.pdf` byte-identical; a step failing after both tools were found also exits 1 and leaves them byte-identical (AC 2, Observable "failure halfway") · closed
Proof: `-k test_sem_java_sai_1`
Proof: `-k test_sem_word_sai_1`
Proof: `-k test_falha_no_meio_sai_1`

**C3** - Every section is A4 (21 × 29.7 cm) with margins top 3 cm, left 3 cm, bottom 2 cm, right 2 cm (AC 3) · closed
Proof: `-k test_margens_a4`

**C4** - Body text and every style in use are Times New Roman, and the Normal style is 12 pt (AC 4) · closed
Proof: `-k test_fonte_times_12`

**C5** - Body paragraphs have 1.5 line spacing and 12 pt space after (AC 5) · closed
Proof: `-k test_espacamento`

**C6** - Capa, folha de rosto, resumo and sumário carry no PAGE field; the section holding the folha de rosto restarts numbering at 1; the first textual section shows a right-aligned PAGE field in its header and does not restart (AC 6) · closed
Proof: `-k test_paginacao`

**C7** - The capa contains, in order, "UNIVAG - CENTRO UNIVERSITÁRIO", "CURSO SUPERIOR DE TECNOLOGIA EM ANÁLISE E DESENVOLVIMENTO DE SISTEMAS", the five authors, the title, the subtitle, "Várzea Grande - MT" and "2026", with exactly this letter case, all centred (AC 7) · closed
Proof: `-k test_capa`

**C37** - The capa carries exactly one picture, centred and placed before "UNIVAG - CENTRO UNIVERSITÁRIO", whose bytes equal `docs/requisitos/logo_univag.png` (AC 35) · closed
Proof: `-k test_logo_na_capa`

**C8** - The folha de rosto contains the literal §4.1 nature text and the orientador line with "Walderson Ryutti Shimokawa" (AC 8) · closed
Proof: `-k test_folha_de_rosto`

**C9** - The resumo has at most 250 words and is followed by a "Palavras-chave:" line with 3 to 5 terms (AC 9) · closed
Proof: `-k test_resumo`

**C10** - The sumário holds a `TOC \o "1-3"` field whose stored result lists "INTRODUÇÃO" followed by a page number (AC 10) · closed
Proof: `-k test_sumario`

**C11** - Numbered headings follow 1, 1.1, 1.1.1 without gaps, and every level-1 heading is bold and uppercase (AC 11) · closed
Proof: `-k test_titulos_numerados`

**C35** - `verify.ps1` runs the document test suite as a step and exits 0 (door 4)
Proof: `.\.claude\scripts\verify.ps1`

**C36** - `docs/requisitos/requirements.txt` pins `python-docx==1.2.0` (door 2) · closed
Proof: `-k test_dependencia_fixada`

### S2 - Elicitation and requirements content · 4 files · ~90 KB · ~23k

**C12** - The level-1 sections are, in order, 1 INTRODUÇÃO, 2 LEVANTAMENTO DE REQUISITOS, 3 VISÃO GERAL DO PRODUTO, 4 REQUISITOS DO PRODUTO, 5 DOCUMENTAÇÃO DAS FUNCIONALIDADES, 6 CONSIDERAÇÕES FINAIS, then the unnumbered REFERÊNCIAS (AC 12) · closed
Proof: `-k test_secoes_primarias`

**C13** - INTRODUÇÃO holds the subsections Propósito do documento, Escopo do produto, Premissas do projeto, Convenções, termos e abreviações, Estrutura do documento (AC 13) · closed
Proof: `-k test_subsecoes_introducao`

**C14** - VISÃO GERAL DO PRODUTO holds Descrição do produto, Usuários da solução, Abrangência e soluções similares, Suposições e dependências (AC 14) · closed
Proof: `-k test_subsecoes_visao`

**C15** - DOCUMENTAÇÃO DAS FUNCIONALIDADES holds Diagrama de casos de uso, Especificações dos casos de uso, Diagrama de classes, Modelo conceitual, Modelo lógico (AC 15) · closed
Proof: `-k test_subsecoes_funcionalidades`

**C16** - LEVANTAMENTO DE REQUISITOS names the three techniques and contains the values 171, 70, 15, 256, 50%, 30%, 5 h, 3 h, 10 min, 30 min and 60 min (AC 16) · closed
Proof: `-k test_levantamento`

**C17** - The 5 h / 3 h figure and the 83 e-mails figure each have "a confirmar" in the same paragraph or table row (AC 17) · closed
Proof: `-k test_valores_a_confirmar`

**C18** - The text contains "coordenadora dos cursos de tecnologia" and does not contain the name of the course coordinator listed in the manual ("Alessandra") (AC 18) · closed
Proof: `-k test_entrevistada_anonima`

**C19** - The functional requirements quadro has the columns ID, Requisito, Origem, and every ID matches `RF-\d{2}` (AC 19, door 3) · closed
Proof: `-k test_quadro_rf`

**C20** - Every one of the 53 ids read from `requirements.md` (44 `BR-` rules, 9 `F` transitions) is cited in some RF Origem cell; a missing id fails the test and is named (AC 20) · closed
Proof: `-k test_rastreio_regras`

**C21** - The non-functional requirements quadro has the columns ID, Categoria, Requisito, Fonte, and every ID matches `RNF-\d{2}` (AC 21, door 3) · closed
Proof: `-k test_quadro_rnf`

**C22** - One RNF row contains "PWA", "React", "ASP.NET Core" and "REST" (AC 22, door 5) · closed
Proof: `-k test_rnf_stack`

**C23** - One RNF row requires a visible notice of processing by an external AI provider and states that attachments are not sent to it (AC 23) · closed
Proof: `-k test_rnf_aviso_ia`

**C24** - The premissas quadro has one row for each of Q5 to Q19, each containing "a confirmar com a coordenação" (AC 24) · closed
Proof: `-k test_premissas`

**C25** - Every use-case specification quadro has the fields Identificador, Atores, Pré-condições, Fluxo principal, Fluxos alternativos, Pós-condições and RFs relacionados, with an id matching `UC-\d{2}` (AC 25, door 3) · closed
Proof: `-k test_especificacoes_uc`

**C26** - The set of UC ids in `casos_de_uso.drawio` equals the set of specification ids in the document; any difference fails and is named (AC 26) · closed
Proof: `-k test_uc_diagrama_especificacao`

**C27** - Every RF id is cited in some use case's "RFs relacionados"; an uncited id fails and is named (AC 27) · closed
Proof: `-k test_rf_em_casos_de_uso`

**C28** - CONSIDERAÇÕES FINAIS mentions the open questions and the dates 27/10, 30/10, 13/11, 20/11 and 27/11 (AC 28) · closed
Proof: `-k test_consideracoes_finais`

### S3 - Figures, tables and references · 5 files · ~25 KB · ~6k

**C29** - The textual part (from INTRODUÇÃO on) has exactly four inline pictures; each is preceded by a caption "Figura N – <título>" (N = 1..4, in order, for casos de uso, classes, conceitual, lógico) and followed by "Fonte: elaborado pelos autores (2026)." (AC 29) · closed
Proof: `-k test_figuras`

**C30** - Every table except the capa layout is preceded by "Quadro N – <título>" with N running 1, 2, 3… and followed by a paragraph starting "Fonte:" (AC 30) · closed
Proof: `-k test_quadros`

**C31** - Every figure referenced in `conteudo.md` has exactly one `.mmd` or `.drawio` of the same stem in `docs/requisitos/diagramas/` (AC 31, door 1) · closed
Proof: `-k test_figuras_tem_fonte`

**C32** - Every entity in `modelo_conceitual.drawio` has a table of the same normalised name in `modelo_logico.mmd`; a missing one fails and is named (AC 32) · closed
Proof: `-k test_modelo_conceitual_logico`

**C33** - Every `(AUTOR, ano)` citation in the text has a REFERÊNCIAS entry starting with AUTOR and containing the year, and every entry is cited (AC 33) · closed
Proof: `-k test_citacoes_referencias`

**C34** - The REFERÊNCIAS entries are in alphabetical order (AC 34) · closed
Proof: `-k test_referencias_ordem`

## Coverage

| Set (size) | Member -> proof | Unproven |
| --- | --- | --- |
| spec ids to trace (53) | C20, table-driven over all 53 ids read from `requirements.md` | - |
| open questions (15) | C24, table-driven over all 15, Q5…Q19 | - |
| level-1 sections (7) | C12, table-driven over the 7 headings in order | - |
| INTRODUÇÃO subsections (5) | C13, table-driven over all 5 | - |
| VISÃO GERAL subsections (4) | C14, table-driven over all 4 | - |
| DOCUMENTAÇÃO subsections (5) | C15, table-driven over all 5 | - |
| figures (4) | `casos de uso` C29 · `classes` C29 · `conceitual` C29 · `lógico` C29 | - |
| generator exits (4) | `0` C1 · `1 sem Java` C2 · `1 sem Word` C2 · `1 falha no meio` C2 | - |
| unnumbered pre-textual pages (4) | `capa` C6 · `folha de rosto` C6 · `resumo` C6 · `sumário` C6 | - |
| baseline values (11) | C16, table-driven over the 11 values | - |
| unconfirmed figures (2) | `5 h / 3 h` C17 · `83 e-mails` C17 | - |
| use-case fields (7) | C25, table-driven over the 7 fields | - |
| one-way doors (5) | `1 PlantUML` C31, C1 · `2 python-docx` C36 · `3 ids` C19, C21, C25 · `4 verify step` C35 · `5 stack` C22 | - |

- Claims naming an exit code: C1, C2 - each proof runs the generator's entry point
- No other check claims more than the single case its proof exercises

## Swept

- validation: C9, C20
- failure modes: C2
- idempotency: n/a - every run rebuilds every output from the sources; nothing accumulates between runs
- authorization: n/a - a local command and a document; no access control exists
- concurrency: n/a - a single user runs the command locally; parallel runs are not a supported use
- data lifecycle: n/a - outputs are overwritten in place on each run; nothing is retained or deleted
- dependency failure: C2
- state transitions: n/a - this feature has no state machine; the ticket lifecycle described in the document is content, traced by C20 over F1–F9
- observability: C1

## Handoff

- S1 ≈ 11k + S2 ≈ 23k + S3 ≈ 6k ≈ 40k tokens (≈ 160 KB of touched and read files ÷ 4), all in `docs/requisitos` plus one step in `verify.ps1`; under the 150k budget - one builder

- **Boundary:** C1-C34 and C36 closed at the `feat(requisitos)` commit; C35 open - editing `.claude/scripts/verify.ps1` is denied by the user's permission settings
- **Settled mid-build:** Word drops direct formatting equal to the style's (Normal rFonts, capa `jc`), so C4 and C7 read the effective value (paragraph, style chain, docDefaults) instead of only direct formatting - the claims are unchanged; the class diagram's enums moved to Quadro 25 and the logical model uses `left to right direction`, both for legibility at 16 cm
- **Abandoned:** enumerations drawn inside the class diagram - they tripled its width and made the text unreadable on A4
- **Settled mid-build (round 2):** the user asked for the UNIVAG logo on the capa (AC 35, C37; C29 now counts pictures in the textual part only) and for a revised text that reads as written by the group (Tone assumption); verifier gaps 2 and 3 closed by `test_falha_no_meio_sai_1` and an exact-case C7
