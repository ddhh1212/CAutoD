<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/images/cautod-symbol-light.svg">
    <img src="docs/images/cautod-symbol.svg" alt="CAutoD" width="112">
  </picture>
</p>
<h1 align="center">CAutoD</h1>
<p align="center"><strong>From an idea to an editable CAD model. Inside SolidWorks.</strong></p>
<p align="center">Describe a part. Refine it in conversation. Build an assembly.</p>
<p align="center">
  English · <a href="README.zh-CN.md">简体中文</a><br>
  <a href="https://github.com/ddhh1212/CAutoD/releases/tag/v1.0.0">Download v1.0.0</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="docs/INSTALL.en.md">Installation guide</a> ·
  <a href="https://github.com/ddhh1212/CAutoD/issues">Feedback</a>
</p>
<p align="center">Windows x64 · SolidWorks add-in · Bring your own API key · Preview</p>

---

CAutoD brings an AI CAD agent into the SolidWorks task pane. It turns natural-language requests and reference images into native models, then helps you revise the result without leaving your design environment.

This repository hosts documentation and demonstrations. **Download the compiled application from [Releases](https://github.com/ddhh1212/CAutoD/releases).** You do not need to clone this repository or install Python/Node.js.

## See it in action

### Describe → generate

<picture>
  <img src="https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/generation-8x.png" alt="Natural-language part generation in SolidWorks" width="960">
</picture>

[▶ Open animation directly](https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/generation-8x.gif) · [Full-resolution original](https://github.com/ddhh1212/CAutoD/blob/v1.0.0/docs/images/generation-8x.gif)

### Refine an existing part

<picture>
  <img src="https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/interactive-editing-8x.png" alt="Interactive part editing" width="960">
</picture>

[▶ Open animation directly](https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/interactive-editing-8x.gif) · [Full-resolution original](https://github.com/ddhh1212/CAutoD/blob/v1.0.0/docs/images/interactive-editing-8x.gif)

### Build and animate an assembly

<picture>
  <img src="https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/assembly-motion-8x.png" alt="SolidWorks assembly motion" width="960">
</picture>

[▶ Open animation directly](https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/assembly-motion-8x.gif) · [Full-resolution original](https://github.com/ddhh1212/CAutoD/blob/v1.0.0/docs/images/assembly-motion-8x.gif)

### Mechanism in motion — continuous loop

<picture>
  <source srcset="https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/green-assembly-loop.gif" type="image/gif">
  <img src="https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/green-assembly-loop.png" alt="SolidWorks mechanism motion" width="600">
</picture>

[▶ Open animation directly](https://raw.githubusercontent.com/ddhh1212/CAutoD/main/docs/images/green-assembly-loop.gif)

Real SolidWorks footage, edited into a forward/reverse preview loop with repeated waiting frames removed. Only this lightweight preview autoplays; click the links above to watch the other demos. [Watch the original-speed HD MP4](docs/images/green-assembly-original.mp4). The source updates poses in steps; this edit does not synthesize intermediate geometry.

## What you can do

| Capability | What it means |
| --- | --- |
| **Generate native parts** | Describe a part or attach a reference image; create SolidWorks parts and features. |
| **Make local edits** | Refine holes, dimensions, chamfers and other details in the existing model. Keep the sidebar conversation and supported preview/apply/undo interactions. |
| **Create assemblies** | Build separate components, organize assembly relationships and request mechanism motion. |
| **Inspect when needed** | Use geometry, measurements and screenshots for unresolved questions, with an interactive, advisory delivery policy. |
| **Use bounded simulation tools** | Single-solid linear static analysis with pressure and fixed-face constraints, subject to a valid SolidWorks Simulation installation/license. |
| **Keep work organized** | Each new conversation gets its own workspace under `app/output/`, containing models and generated scripts. |
| **Choose your model** | Configure a supported tool-calling model through your own provider account. |

## Quick start

**Requirements:** Windows 10/11 x64, a working SolidWorks installation, .NET Framework 4.8, [WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/), and a model API key. The native test environment is SolidWorks **2026 SP0.0**; other versions have not been certified.

1. Download `CAutoD-v1.0.0-win-x64.zip` from [v1.0.0](https://github.com/ddhh1212/CAutoD/releases/tag/v1.0.0) and **extract the entire archive** into a writable folder.
2. Close SolidWorks. Run `Inspect.cmd`, then `INSTALL.cmd`; allow the administrator prompt for COM registration.
3. Start SolidWorks → **Tools → Add-ins** → enable **CAD Agent**.
4. Open the task pane and select **API 配置** (API setup). Choose a provider and enter its Base URL, exact model ID and your API key.
5. Start a new conversation and describe your design.

The ZIP includes Python and Node.js. No conda setup, source checkout or developer-specific paths are required. See the [full installation, upgrade and troubleshooting guide](docs/INSTALL.en.md).

> Start with a clear design goal, then refine the result. For example: “Design a compact mounting bracket for a small motor, with accessible fasteners and space for wiring.”

## Model providers

The setup dialog offers **DeepSeek, OpenAI, Qwen/DashScope, Kimi/Moonshot, GLM, Gemini, OpenRouter**, and a custom OpenAI-compatible endpoint. Use a model that supports **tool calls and streaming**. [Provider links and setup details →](docs/INSTALL.en.md#configure-your-model)

Configuration is required before sending tasks. Saving settings does not call the model or verify your balance/key; requests begin when you send a task. Model usage is billed by your provider. Chat subscriptions generally do not include API credits.

One provider/model is active at a time. Anthropic's native Messages API is not implemented; Claude may be accessed through a compatible gateway such as OpenRouter. Provider-specific capabilities and live interoperability vary.

## Your files and credentials

- API keys are encrypted with **Windows current-user DPAPI** in `%LOCALAPPDATA%\CAutoD\provider.json`. They are not written into system environment variables or returned to the browser.
- Models and generated scripts stay in the conversation workspace. Requests, reference images and relevant tool feedback are sent to your chosen model service.
- Developer keys, `.env` files, private signing keys, old conversations and generated CAD files are excluded from the distribution.
- Install configuration uses relative paths. If you move the installed folder, close SolidWorks and unregister it first, then register it at the new location.

## Release status

**v1.0.0 is the first binary preview.** Relocated startup (including Chinese characters/spaces in paths), first-use configuration, key protection, compiled SDK/resource access and an offline tool loop have been checked locally. A clean installation on a second computer and live tests of every provider are still pending. See [release notes](RELEASE_NOTES.en.md) and [check results](RELEASE_CHECKS.json).

The native add-in is distributed as .NET DLLs; backend/PySW implementation is compiled Python bytecode. Implementation `.cs`/`.py` files are not included. **Bytecode is not encryption and can be reverse-engineered.** Frontend assets, installation scripts, API references and skills remain readable; generated modeling scripts remain in your workspace.

This is a binary distribution, not an implementation-source release. No open-source license is assigned to project-owned implementation by this release. Third-party terms remain applicable: [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Feedback

[Open an issue](https://github.com/ddhh1212/CAutoD/issues) with your SolidWorks version, CAutoD version, provider/model ID, reproduction steps and a redacted error excerpt. **Never include API keys, credential files or confidential designs.**
