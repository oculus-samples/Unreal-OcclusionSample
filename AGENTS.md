# Agent Instructions — Occlusion Sample

Unreal sample demonstrating passthrough occlusion via the Depth API — cycle between disabled / hard / soft occlusion modes, toggle hands removal, and inspect raw environment depth through a custom material graph node.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, occlusion-mode descriptions, hands-removal flow, and the `M_EnvironmentDepth` material walkthrough
- `OcclusionSample.uproject` — Unreal engine version, plugins, and modules
- `Config/` `.ini` files (`DefaultEngine.ini`, `DefaultGame.ini`, etc.) — engine/project configuration
- `Platforms/` — platform-specific configuration
- `Source/` — C++ project sources
- `Content/` (including `M_EnvironmentDepth`) — the materials and assets referenced by the README
- `.gitattributes` — Git LFS configuration (LFS is required)
- `LICENSE` — Meta License applies to the SDK and supporting material; MIT applies only to clearly marked documents

## Quest / Horizon-specific notes

- **Soft occlusions require the Meta (Oculus-VR) fork of Unreal Engine.** Stock UE + the MetaXR plugin from the Epic launcher will only render the hard-occlusion path; do not file a bug if soft mode looks wrong under stock UE.
- The `Environment Depth` material graph node used by `M_EnvironmentDepth` is **only present in the Meta UE fork**. Loaded under stock UE, the node is missing and the material falls back to a solid red — expected, not a regression.
- The depth visualization in `M_EnvironmentDepth` is clamped to a 0–3 m gradient (red close, blue far); anything farther shows as blue. Keep that in mind when triaging "the material looks wrong" reports.
- Occlusion modes cycle via the A / X controller buttons; hands removal toggles via pinch when controllers are set down. Preserve these bindings when modifying input.
- The Depth API requires Quest 3 / Quest 3S class hardware — older Quest devices will not produce meaningful environment depth.

# Meta Quest tooling

This is a Meta Quest / Horizon OS sample. The bespoke intro above is the source of truth for what this project is and how it's built — use it (and the files it points at) instead of restating facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: <https://github.com/meta-quest/agentic-tools>. Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
