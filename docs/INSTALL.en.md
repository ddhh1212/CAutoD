# Install CAutoD on Windows

[English README](../README.md) · [中文安装指南](INSTALL.zh-CN.md)

## Before you start

- Windows 10/11 x64 with a working SolidWorks installation. The native development/test environment is SolidWorks 2026 SP0.0; other versions are not certified.
- .NET Framework 4.8 and [Microsoft Edge WebView2 Evergreen Runtime](https://developer.microsoft.com/microsoft-edge/webview2/).
- A model API account, credit and a model supporting tool calling and streaming.
- A writable installation directory, such as `D:\Apps\CAutoD-v1.0.0`. Avoid `Program Files`, read-only directories and network shares.
- Administrator permission for COM add-in registration. SolidWorks and Simulation are not included.

## Install

1. Download `CAutoD-v1.0.0-win-x64.zip` from [Releases](https://github.com/ddhh1212/CAutoD/releases/tag/v1.0.0). Optionally compare its SHA-256 against the accompanying `.sha256` file using `Get-FileHash` in PowerShell.
2. Extract **all** files using Windows Explorer. Do not run the installer from inside the ZIP.
3. Save your work and close SolidWorks.
4. Run `Inspect.cmd` to check plugin binaries against the packaged integrity manifest.
5. Run `INSTALL.cmd` and accept the Windows administrator prompt. The installer registers this add-in; it does not start a modeling job.
6. Start SolidWorks. In **Tools → Add-ins**, enable **CAD Agent**. Also check the startup column if you want automatic loading.
7. Open the task pane, configure the API, and start a new conversation.

Python and Node.js are included. You do not need to install a virtual environment, compile the add-in or configure system API-key variables.

## Configure your model

Click **API 配置** (API setup) in the task pane. The current application UI is Chinese.

Enter the provider, Base URL, exact model ID, API key, and maximum output tokens (default 8192). The Base URL must not end with `/chat/completions`. Use HTTPS; HTTP is accepted only for a local service.

| Provider | Account / API key portal | Interface |
| --- | --- | --- |
| DeepSeek | [Platform](https://platform.deepseek.com/) | OpenAI compatible |
| OpenAI | [API platform](https://platform.openai.com/) | Chat Completions, tools-capable model |
| Qwen / DashScope | [Alibaba Cloud Model Studio](https://bailian.console.aliyun.com/) | Compatible API; check your region's URL |
| Kimi | [Moonshot](https://platform.moonshot.cn/) | OpenAI compatible |
| GLM | [Z.ai / BigModel platform](https://open.bigmodel.cn/) | Configured BigModel compatible endpoint |
| Gemini | [Google AI Studio](https://aistudio.google.com/) | OpenAI compatibility API |
| OpenRouter | [API keys](https://openrouter.ai/keys) | Compatible gateway, including Claude models |
| Custom | Your provider | OpenAI Chat Completions compatible |

Configuration is required before use. Saving checks the input format but **does not verify API authentication, credit or model capabilities**. Sending a task makes real API calls and may incur charges. An ordinary chat subscription is not an API subscription.

Only one provider/model is active at a time. Anthropic's native Messages protocol is not supported. Not every model or compatible service accepts the same parameters; select one that supports streaming and tools. Regional access and quotas are controlled by the provider.

## Data and paths

- Provider settings are stored at `%LOCALAPPDATA%\CAutoD\provider.json`. The API key is protected using current-user Windows DPAPI, not plaintext or system environment variables. A different computer/Windows account must configure its own key.
- Each conversation creates a workspace under `<installation>\app\output\chat_...\`. It contains models and generated scripts.
- Requests, images and necessary tool feedback go to your chosen model service. Do not submit confidential data without appropriate authorization.
- `plugin/agent-host.local.json` uses relative paths to `app/` and bundled runtimes. SolidWorks may use absolute paths during execution; these are resolved on the current machine.
- The installed directory must remain writable. Never move a registered installation without unregistering it first.

## Upgrade or move

Save your models, close SolidWorks, and run the old installation's `UNINSTALL.cmd`. Extract the new version into a separate directory and run its installer. Back up previous workspaces separately. Do not overwrite DLLs while SolidWorks is running.

If registration says another version owns the add-in, unregister that version using its own uninstaller first.

## Uninstall

Close SolidWorks and run `UNINSTALL.cmd` as administrator. This removes registration; it does not delete models, conversations or API settings. You may remove the installation directory and `%LOCALAPPDATA%\CAutoD` yourself once no longer needed.

## Troubleshooting

| Symptom | Next step |
| --- | --- |
| Empty/missing task pane | Check WebView2 Runtime and enable CAD Agent under Tools → Add-ins. |
| API 401/403 | Check the provider, Base URL, key and model permission. Keys are not interchangeable between providers. |
| 429 / insufficient credit | Check quota/rate limits; pause the task before changing settings. |
| Unsupported tools/model parameters | Use a model with tool calling and streaming; consult the provider's model documentation. |
| Missing document template | Configure SolidWorks default part/assembly templates and confirm manual document creation works. |
| Wrong same-named document | Use unique model filenames; avoid open documents with the same name from other folders. |
| A modeling step fails | Keep the usable model and describe the specific correction instead of always regenerating everything. |
| Installer cannot register | Close SolidWorks and check administrator permissions/old registration. |
| Security software warning | This preview has no commercial code-signing certificate. Verify the source and release checksum and follow your organization's policy. |

## Validation and distribution

See [RELEASE_CHECKS.json](../RELEASE_CHECKS.json). Relocated runtime checks were performed locally, not on a second machine; all providers have not been tested live. The application release is v1.0.0 while the native DLLs retain internal 0.4.2-series compatibility versions.

The package omits implementation `.py`/`.cs` source but includes Python bytecode and .NET DLLs. These can be reverse-engineered. Frontend files, installation scripts, API documentation and skills remain readable. See [third-party notices](../THIRD_PARTY_NOTICES.md).
