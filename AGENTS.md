# Agent Instructions — Showdown on Meta Quest

Quest port of Epic's PC VR `Showdown` demo, used as a reference for optimizing a PC VR Unreal application to run on Quest mobile renderers — including Application SpaceWarp integration and a render-settings menu for live profiling.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, the Meta Unreal fork link, Quest Link testing steps, the PSO-cache workflow, and the controls reference
- `Showdown.uproject` — Unreal engine version, plugins, and modules
- `Source/ShowdownQuest.Target.cs` and `Source/ShowdownQuestEditor.Target.cs` — module target rules
- `Config/` `.ini` files (`DefaultEngine.ini`, `DefaultGame.ini`, etc.) — engine/project configuration
- `Content/` — game assets (`Audio`, `Blueprints`, `Character`, `Effects`, `Env`, `Maps`, `Main`, `LUTs`, ...)
- `GeneratePSOCache.bat` — the PSO-cache helper script described in the README
- `CHANGELOG.md` — project change history
- `.gitattributes` — Git LFS configuration (LFS is required)
- `LICENSE` — license terms (project is bound by the Showdown / Unreal EULA, not MIT)

## Quest / Horizon-specific notes

- The project **requires the Meta (Oculus-VR) fork of Unreal Engine**, not stock UE from the Epic launcher — Application SpaceWarp and the Vulkan mobile tonemap subpass live in the fork. Building from source against that fork is mandatory, not optional.
- The README references both a minimum supported fork version and the version the project actually ships against; treat the `Showdown.uproject` engine association as the authoritative version, not the prose in the README.
- The PSO-cache workflow (`GeneratePSOCache.bat`) is required to avoid runtime stalls; rebuild caches after meaningful material/shader changes and after toggling render settings during recording so the cache covers the full pipeline.
- 90 FPS is a hard target — profile (Perfetto, OVR Metrics, RenderDoc) before claiming a render-side change is performance-neutral.
- The render-settings menu (B / right-stick / triggers per README) is wired specifically as a profiling demo — preserve its bindings when adding new input.
- The project license is the Showdown / Unreal EULA, *not* MIT — do not relicense files when copying snippets out.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
