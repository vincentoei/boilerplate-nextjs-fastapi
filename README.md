# Next.js + FastAPI starter

A small fullstack base. The frontend and backend run independently, and Next.js forwards browser requests under `/api/*` to FastAPI during development.

## Stack

- `fe/`: Next.js App Router, React, TypeScript, Tailwind CSS, shadcn/ui
- `be/`: FastAPI, Uvicorn, Pydantic
- `.agents/skills/`: reusable agent skills 
- `docs/`: project decisions and artifacts

## Get started

Requirements: Node.js 20.9+, pnpm 9+, Python 3.11+, and [uv](https://docs.astral.sh/uv/).

In separate terminals:

```bash
cd be
uv sync
cp .env.example .env
uv run uvicorn app.main:app --reload --port 8000
```

```bash
cd fe
pnpm install
cp .env.example .env.local
pnpm dev
```

Open http://localhost:3000. The page should say **API: Connected**. FastAPI's interactive docs are at http://127.0.0.1:8000/docs. Set `API_INTERNAL_URL` in `fe/.env.local` if the backend runs elsewhere. Restart Next.js after changing it.

## Checks

```bash
cd be && uv run pytest && uv run ruff check .
cd fe && pnpm lint && pnpm typecheck && pnpm build
```

## Impeccable design tools

[Impeccable](https://github.com/pbakaus/impeccable/tree/skill-v4.8.0) 4.8.0 is included in `.agents/skills/impeccable/`, with Codex design hooks in `.codex/hooks.json`. Open the repository root in Codex and start a new chat to load the skill. Open `/hooks` and approve the project hooks to enable automatic design checks.

In the agent chat, use:

```text
$impeccable init
$impeccable critique fe/src/app/page.tsx
$impeccable audit fe/src
$impeccable polish fe/src/app/page.tsx
```

Run `init` when adapting the starter to a real product, using `docs/PRODUCT_SPEC.md` for its requirements. Commit the generated root `PRODUCT.md` and keep it consistent with the spec. Use `$impeccable document` to record an established visual system in `DESIGN.md`.

For a standalone source scan, run from the repository root:

```bash
.agents/skills/impeccable/scripts/impeccable detect fe/src
```

On Windows, use `.agents\skills\impeccable\scripts\impeccable.cmd detect fe/src`. The launcher downloads and checksum-verifies the pinned engine (0.1.18) on first use, requiring network access and a writable user cache. Downloaded binaries and transient scan state are ignored by Git. Exit code `0` means no primary findings, `2` means design findings, and `1` means a scan failed. Source scans do not check rendered layout or contrast; with the frontend running, scan `http://localhost:3000` for browser checks.

To update the project installation, run from the repository root:

```bash
npx impeccable update --project
```

Review and commit the updated skill and hooks, refresh the upstream `LICENSE` and `NOTICE.md` in the skill folder, and update the version documented here and in `AGENTS.md`. Approve changed hooks again in Codex. The vendored files retain Impeccable's [Apache-2.0 license](.agents/skills/impeccable/LICENSE) and [third-party notices](.agents/skills/impeccable/NOTICE.md).

## First steps for a new project

1. Fill in `docs/PRODUCT_SPEC.md` with the problem, users, scope, and acceptance criteria.
2. Record a delivery plan in `docs/artifacts/phases/`.
3. Add API routes under `be/app/` and UI routes under `fe/src/app/`.
4. Add shadcn components from `fe/` with `pnpm dlx shadcn@latest add <component>`.
5. Update `AGENTS.md` when the architecture or commands change.

This starter does not assume a database, auth provider, or AI service. Add those when the project requires them.
