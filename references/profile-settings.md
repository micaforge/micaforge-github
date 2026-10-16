# Profile settings workflow (micaforge-ai)

Use when updating Mica's GitHub profile. Prefer scripts over browser clicks;
the browser is only for steps with no API.

## Avatar

GitHub has **no avatar-upload API** — the upload click is manual. But prep
is scripted:

```bash
~/workspace/custom-skills/micaforge-github/bin/gh-avatar-prep <image> [-o out.png]
```

Takes any image → center-crops square → 460px → optimized PNG under
GitHub's 1 MB limit. Then upload via browser at
https://github.com/settings/profile (or hand Curtis the file).

Source images: active avatar webp lives at
`~/workspace/avatars/avatar-1792045868861820369-0.webp`; animation frames
under `~/workspace/avatars/.frames/`.

## Bio / name / location (API — scriptable)

```bash
gh api -X PATCH /user -f bio="..." -f name="..." -f location="..."
```

Needs `gh` authed as micaforge-ai (see "CLI auth" below).

## Profile README

Repo `micaforge-ai/micaforge-ai` (public). Edit `README.md`, push via SSH:

```bash
cd ~/workspace/micaforge-ai-profile && git push
```

## CLI auth

`gh` needs a token for micaforge-ai. Token lives in the Secure Vault
(custom connector); never in files or env. To auth a fresh shell:

```bash
# token comes from the vault-backed helper, never pasted
~/workspace/custom-skills/micaforge-github/bin/gh-auth
```

(Helper to be built once the PAT exists — 2026-10-15: PAT not yet created.)

## Future: avatar variations

Ideas parked: generate the familiar doing different things, add
backgrounds/scenes via an image generation API. The prep script above
handles whatever comes out.
