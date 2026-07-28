# CLOUD_AGENT — bootstrap redirect

**You are in the wrong repository.** This is `tanstack-template`, a web app. It is
not the build-in-public video pipeline.

If you are the `daily-series-episode-builder` routine (or any run whose job is to
build a daily build-in-public episode), the real runbook lives in
`cdscaless-netizen/build-in-public-pipeline`. This file exists only because the
routine's session sometimes starts here instead of there, and its instructions say
to open `CLOUD_AGENT.md` at the repo root — so this is the first thing you read.

Do this, then discard everything else in this file:

1. Check `/workspace/build-in-public-pipeline`. If `git -C /workspace/build-in-public-pipeline rev-parse HEAD`
   succeeds, the repo is already cloned — use it and skip to step 4.
2. Call `add_repo` with `owner: "cdscaless-netizen"`, `repo: "build-in-public-pipeline"`,
   `access: "push"`.
3. Run the single clone command it returns, with a **generous ~10 minute timeout**
   (the shallow pack can take minutes through the proxy; do not interrupt
   `git index-pack`). Then call `register_repo_root` with the clone directory.
4. Open `CLOUD_AGENT.md` at the root of *that* repo and follow it exactly, top to
   bottom. That is the real runbook.

**Do not report "CLOUD_AGENT.md is missing" and stop.** A missing pipeline repo is
a setup step, not a stop condition. The only genuine preflight stop is
`cloud/sa-drive.json` or `cloud/bunny-credentials.json` being absent from a
*successful* clone.

Two things the runbook depends on that are easy to lose in a fresh container:

- `out/` is gitignored, so rendered mp4s do not survive the session. Never end a
  run with the video built but undelivered. If `ny.storage.bunnycdn.com` is blocked
  by the sandbox egress policy, use the runbook's fallback — commit both mp4s to
  `delivery/` with `git add -f`, push — **and** send the files directly into the
  session so they are watchable on a phone without waiting on the CDN.
- Read the user's EDITOR NOTES doc from Drive *before* deciding what to build.
  Re-edit requests take priority over cutting a new day.

---

*Nothing else in this repository is related to the video pipeline. If you are here
for ordinary `tanstack-template` work, ignore this file entirely.*
