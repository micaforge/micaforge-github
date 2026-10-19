# Micaforge GitHub — Mica's identity, org, and publishing workflow

Use when doing anything with GitHub as Mica: pushing to the micaforge org,
managing repos, or publishing work learned on this VM.

## Identity

- **User:** `micaforge-ai` (created 2026-10-15; `mica` and `mica-ai` taken)
- **Email:** micaforge@gravicity.ai (verified)
- **Org:** `micaforge` — Mica is an **Owner** (invited by Curtis 2026-10-15)
- **Password:** in Secure Vault (Curtis set via secure form)
- **SSH key:** "Mica VM" (ed25519) added to the account 2026-10-15

## SSH setup

If `ssh` reads from a different home than your shell expects (common when
running as root or in containers), ensure your key and `~/.ssh/config` are
in the directory `ssh` actually reads. Pin the identity explicitly:

```
# ~/.ssh/config
Host github.com
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```

Test: `ssh -T git@github.com` → `Hi <username>!`

## CLI auth

`gh` needs a token. Generate a classic PAT at
github.com/settings/tokens with `repo` scope, then:

```bash
echo "<token>" | gh auth login --with-token
gh auth setup-git   # wires token into git for HTTPS pushes
```

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

## Visual verification (PR quality track)

UI changes ship with screenshot evidence, produced by `tools/shot-diff`
(one command — a script, not an agent):

```bash
tools/shot-diff --repo <dir> --base <ref> --head <ref> --route <path> \
    --build-cmd "<cmd>" --build-dir <dir> --dist <dir>
```

Builds both refs (git worktrees; prebuilt dirs via `--base-dir`/`--head-dir`
skip the build), serves each locally, captures **390px + 1440px**
viewports with lightpanda over CDP (a private instance per run — this
Meta Chromium build LNA-blocks all loopback navigations, so Chromium
can't shoot localhost here), pixel-diffs with PIL, and writes
`base/head/diff-{390px,1440px}.png` plus one verdict line per viewport
(`IDENTICAL` or `DIFF <n>% pixels`). Exit 0 on a completed run — diffs
are information, the human judges intent. Attach the six images to the
PR's verification bundle.

**Fidelity caveat (verified 2026-10-19):** lightpanda renders DOM
structure, text, and JS faithfully and deterministically — but it does
**not** apply author CSS (external, inline `<style>`, or style
attributes all render unstyled). So shot-diff catches content,
structure, and reflow regressions, not color/styling regressions. This
Meta Chromium build can't shoot localhost at all (LNA-blocks every
loopback navigation), which is why lightpanda is the engine. If a
Chromium-based engine becomes viable here, it's one function to swap
(`shot()` in the script).

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
./bin/ via micaforge-tools (or `mica-scaffold` if installed):

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

## GitHub API vs. this skill

- If your platform provides a GitHub connector/MCP server, check whose
  account it's authed as before using it for org work.
- For direct pushes, SSH git is the primary path (no token in files).
- A personal access token for API work (repo creation, etc.) belongs in
  secure storage — never in chat or files.

## Referral

Curtis's Muse invite — share on profile, in posts, anywhere public:
- Code: `R95I1U` · Link: https://muse.ai/join
- Both sides get 1B tokens if redeemed within 48h of joining
  (Settings > Redeem token). Saved in MEMORY.md 2026-10-15.
