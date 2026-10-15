# Micaforge GitHub — Mica's identity, org, and publishing workflow

Use when doing anything with GitHub as Mica: pushing to the micaforge org,
managing repos, or publishing work learned on this VM.

## Identity

- **User:** `micaforge-ai` (created 2026-10-15; `mica` and `mica-ai` taken)
- **Email:** micaforge@gravicity.ai (verified)
- **Org:** `micaforge` — Mica is an **Owner** (invited by Curtis 2026-10-15)
- **Password:** in Secure Vault (Curtis set via secure form)
- **SSH key:** "Mica VM" (ed25519) added to the account 2026-10-15

## SSH quirk (verified 2026-10-15)

The VM runs as root, so `ssh` reads `/root/.ssh/`, **not** `/home/hatch/.ssh/`
— even though `$HOME` is `/home/hatch`. The key and config are copied to
`/root/.ssh/` (`config` pins `IdentityFile /root/.ssh/id_ed25519` for
github.com with `IdentitiesOnly yes`). If `git@github.com` ever says
"Permission denied", check `/root/.ssh/` first.

Test: `ssh -T git@github.com` → `Hi micaforge-ai!`

## Org workflow

- Default org for public work: `micaforge` (not the personal account).
- Clone/push via SSH: `git@github.com:micaforge/<repo>.git`
- Curtis co-owns the org; Mica maintains it "as if her own."

## Publishing gates (set by Curtis 2026-10-15)

Nothing goes public unless all four hold:

1. Production-ready (not a sketch)
2. Genuinely useful/practical for strangers (other muse.ai VM users)
3. Zero secrets, private data, or personal paths
4. Documented for outside users (README that stands alone)

## The "everything we learn ships" rule (set by Curtis 2026-10-15)

Anything we build or fix on this VM that works and improves the setup is
a publish candidate: a guide, a blog post, a plugin/extension/patch, or a
skill. The pipeline: do the work → capture the pattern (reference note or
skill) → when it's solid, publish via the org. Work we're already doing
and enjoying doubles as content — don't do it twice.

First candidates: `xfind`, the heartbeat ops pattern, the reference-notes
habit, DBOS pipeline pattern, this skill.

## Repo scaffolding (set by Curtis, 2026-10-15)

New repos start from `micaforge/micaforge-template` (private) via
`~/workspace/bin/mica-scaffold`:

```bash
mica-scaffold <slug> --type project|agent --title "Name" --tagline "..."
```

Fills README.template.md, adds MIT LICENSE, inits git, creates the private
GitHub repo, pushes. README style: centered logo + badges + quick-start in
first 200 words + demo + features table + Muse branding footer (logo, referral
link, GET 1 BILLION TOKENS badge). Stand out — barely anyone's building in
public for Muse AI yet.

## The "manual once, then tool it" habit (set by Curtis 2026-10-15)

Do it manually the first time. Then immediately ask: can this repeat
faster, more efficiently, scripted? If yes, build it — script in `bin/`,
workflow in `references/`, pointer in this SKILL.md. A subagent should be
able to do in one line what took me ten clicks.

Current tools: `bin/gh-avatar-prep` (avatar → GitHub-ready PNG).
Workflows: `references/profile-settings.md` (avatar, bio, README).

## GitHub connector vs. this skill

- The `github` connector (`/opt/hatch/connectors/github/CONNECTOR.md`) uses
  GitHub's MCP server — check whose account it's authed as before using it
  for org work.
- For Mica's own pushes, SSH git is the primary path (no token in files).
- A personal access token for API work (repo creation, etc.) lives in the
  Secure Vault if created — never in chat or files.

## Referral

Curtis's Muse invite — share on profile, in posts, anywhere public:
- Code: `R95I1U` · Link: https://muse.ai/join
- Both sides get 1B tokens if redeemed within 48h of joining
  (Settings > Redeem token). Saved in MEMORY.md 2026-10-15.
