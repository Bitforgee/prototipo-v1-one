# One — Atendimento Acadêmico

Projeto integrador do curso de **Análise e Desenvolvimento de Sistemas (ADS)**.
O **One** é uma central inteligente de atendimento acadêmico: o aluno pergunta,
tenta se autoatender pela base de conhecimento/assistente e, se não resolver, a
conversa vira uma **solicitação rastreável** que chega ao setor certo, é tratada
por um atendente humano e é **medida** por indicadores.

## Acesse o protótipo online

🚀 **[CLIQUE AQUI PARA ACESSAR O ONE — PROTÓTIPO V1](https://bitforgee.github.io/prototipo-v1-one/)**

O protótipo está publicado no GitHub Pages e funciona em **computadores,
tablets e smartphones**, direto no navegador, sem instalar nada. O endereço
principal abre diretamente a **seleção de perfis**, com três perfis
demonstrativos: **Aluno**, **Atendente** e **Administrador**.

> ⚠️ **Sistema demonstrativo.** Todos os dados são **simulados no navegador**
> (sem backend). O login apenas seleciona um perfil — **não é autenticação
> real** e não constitui segurança. Limpar os dados do navegador reinicia o
> protótipo.

> **Natureza do artefato.** Este repositório contém a documentação do projeto
> (requisitos, diagramas, planejamento) **e** um **protótipo visual navegável**
> de alta fidelidade em `prototipo-visual/`. O protótipo é **frontend-only**,
> com **dados simulados** no navegador: não há backend, autenticação real nem
> IA — as permissões e os fluxos são simulados e **não constituem segurança
> real**. A arquitetura oficial do MVP (React + ASP.NET Core, decisão AD-001)
> é paralela e documentada à parte.

## Objetivo acadêmico

Validar, como projeto extensionista integrador, um fluxo único de ponta a ponta:
*perguntar → tentar autoatendimento → se não resolver, virar solicitação
rastreável sem perder o contexto → chegar ao setor certo → ser tratada por um
humano → ser medida*. O protótipo serve para **demonstração e apresentação
acadêmica** desse fluxo.

## Contexto

A instituição-piloto recebe demandas de alunos por canais informais e dispersos
(WhatsApp, e-mail, mensagens diretas), sem registro, sem histórico e sem dados
sobre as demandas recorrentes. O One propõe centralizar e dar rastreabilidade a
esse atendimento. Os dados de descoberta (entrevistas, categorias, setores) são
**configuração do piloto** e estão descritos nos documentos de requisitos.

## Tecnologias

**Protótipo visual** (`prototipo-visual/`):

- HTML5 semântico
- CSS3 (design tokens, temas claro/escuro, layout responsivo)
- JavaScript Vanilla ES6+ (módulos nativos, sem framework)
- Bootstrap 5.3.3 + Bootstrap Icons 1.11.3 (**locais**, em `vendor/`, funciona offline)

Nenhuma dependência é carregada por CDN. Os testes usam apenas Node (stdlib) e o
Chrome instalado — **sem dependências externas**.

**Documentação**: Markdown, diagramas (draw.io / Mermaid / PlantUML) e documentos
gerados (DOCX/PDF) em `docs/requisitos/`.

## Estrutura do repositório

```text
.
├── README.md                     # este arquivo
├── AGENTS.md                     # guia de engenharia / verificação do projeto
├── roadmap.md                    # planejamento e fases
├── One_MVP_Visao_Produto.md      # visão de produto do MVP
├── One_Caminho_Para_Criar_Um_Bom_Produto.md
├── One_Pesquisa_Mercado_Helpdesks.md
├── One_Refinamento_Nome_Diferenciais.md
├── One_Roteiro_Entrevista_Coordenacao_2026-10-01.md
├── Manual do Projeto Extensionista Integrador - TAD.pdf
├── specs/
│   └── mvp-business-rules/        # regras de negócio BR-A..BR-I e critérios AC1..AC9
├── docs/
│   └── requisitos/                # levantamento, requisitos, diagramas (fontes + PNG/PDF/DOCX)
└── prototipo-visual/              # protótipo navegável (HTML/CSS/JS + testes)
    ├── index.html
    ├── css/  js/  data/  vendor/  assets/
    ├── tests/                     # suítes de teste (ver abaixo)
    ├── RELATORIO_HOMOLOGACAO.md         # auditoria de homologação
    ├── RELATORIO_REFORMULACAO_VISUAL.md # reformulação visual (estilo GLPI)
    └── RELATORIO_RESPONSIVIDADE.md      # correção de responsividade (layout fluido)
```

## Funcionalidades do protótipo

- **Três perfis** (login demonstrativo, sem senha): Aluno, Atendente, Administrador.
- **Assistente acadêmico** (chat): responde **apenas** a partir de artigos
  publicados, com confirmação Sim/Não; não inventa respostas.
- **Base de conhecimento**: consulta por palavra-chave e categoria; CRUD de
  artigos para o admin (rascunho/publicado/arquivado, versionamento).
- **Solicitações (chamados)**: criação a partir da conversa, protocolo
  `ONE-<n>` sequencial, roteamento por categoria/setor, listagem com filtros.
- **Atendimento**: fila por setor, assumir, resposta pública, **nota interna
  invisível ao aluno**, encaminhamento com motivo, resolução e confirmação.
- **Histórico** (linha do tempo) append-only; **indicadores** com gráficos SVG.
- **Temas claro/escuro** persistidos e **layout responsivo** (desktop → mobile).

As regras de negócio são a referência normativa: `specs/mvp-business-rules/requirements.md`.

## Como executar o protótipo

Os módulos ES exigem origem HTTP (alguns navegadores bloqueiam `file://`). Suba
um servidor estático a partir de `prototipo-visual/`:

```powershell
cd prototipo-visual
node tests/static-server.mjs 4178      # ou: py -m http.server 4178
# abra http://localhost:4178
```

A página inicial abre **diretamente a seleção de perfis** — escolha um perfil
(Aluno, Atendente ou Administrador) para entrar. Para restaurar os dados
demonstrativos: menu do usuário → **Restaurar dados demonstrativos**. Parâmetros opcionais de demonstração: `?theme=dark|light` e
`?demo-role=aluno|atendente|admin`.

## Como testar

A partir de `prototipo-visual/`, com o servidor estático ativo em `4178`:

```powershell
node tests/run-headless.mjs    # unitários + integração (67)
node tests/smoke-app.mjs       # smoke de navegação (20 cenários)
node tests/console-check.mjs   # 0 erros de JavaScript nas rotas-chave
node tests/a11y-check.mjs      # acessibilidade estrutural (0 avisos)
node tests/e2e.mjs             # E2E real por clique (CDP) — rode isolado
```

> A conformidade WCAG 2.2 AA completa exige teste manual com tecnologia
> assistiva e não é coberta pelas verificações automatizadas.

## Como utilizar o Kiro

O projeto é desenvolvido no **Kiro IDE** (com recursos ECC). Para continuar o
trabalho, abra a pasta raiz do projeto no Kiro ou em outro editor compatível
(ex.: VS Code). As configurações de desenvolvimento ficam em `.claude/`,
`.cursor/` e `AGENTS.md`. A verificação canônica do repositório é
`.\.claude\scripts\verify.ps1` (ver `AGENTS.md`); para o protótipo, a verificação
são as cinco suítes acima.

## Como contribuir / regras de colaboração

Fluxo de branches:

- **`main`** — versão integrada e estável. Não faça push direto.
- **`feat/nome-da-funcionalidade`** — novas funcionalidades.
- **`fix/nome-da-correcao`** — correções.
- **`docs/nome-da-documentacao`** — documentação.

Passos para cada integrante:

1. `git pull` para atualizar a `main` local.
2. Crie sua branch: `git checkout -b feat/minha-funcionalidade`.
3. Desenvolva.
4. Execute os testes (seção **Como testar**).
5. `git commit` com mensagem descritiva (padrão sugerido: *Conventional Commits*).
6. `git push -u origin feat/minha-funcionalidade` (ou o remote do grupo).
7. Abra um **Pull Request** para a `main`.
8. Integre **após revisão** de pelo menos um colega.

Boas práticas: não versionar segredos (`.env`, chaves, tokens — já ignorados);
não commitar caches/binários gerados (`**/.cache/` já ignorado); preferir
`git add` de arquivos específicos a `git add -A`.

### Clonar e sincronizar

```powershell
git clone https://github.com/Bitforgee/prototipo-v1-one.git
cd prototipo-v1-one
# abra no Kiro ou VS Code

# no dia a dia:
git fetch
git pull
git status
git add <arquivos>
git commit -m "feat: ..."
git push
```

## Licença e uso

Projeto acadêmico (ADS). Os dados exibidos no protótipo são **fictícios e
demonstrativos**. Marcas de terceiros citadas pertencem aos seus respectivos
donos; a organização visual é **inspirada** em sistemas de helpdesk (ex.: GLPI)
sem copiar código, identidade ou marca — a identidade do One é própria.
