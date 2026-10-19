# CAutoD Windows 安装指南

[中文首页](../README.zh-CN.md) · [English installation guide](INSTALL.en.md)

在 SolidWorks 侧栏中，用自然语言设计零件、修改当前模型，并完成装配与运动展示。

**Windows x64 首次二进制预览版。** 附带 Python 与 Node.js 运行环境，不需要安装 Python、配置 conda 或下载项目源码。SolidWorks 和模型 API 账号需要自行准备。

## 功能

- **零件生成**：输入文字需求或附加参考图片，生成原生 SolidWorks 零件和特征。
- **交互式修改**：围绕已有模型做尺寸、孔、倒角等局部修改，保留侧栏对话、模型引用及适用场景的预览/应用/撤销操作。
- **装配**：生成独立零件与总装，使用装配配合组织结构；可请求由驱动零件带动的机构运动。
- **模型反馈**：按需要获取几何、结构、测量或截图，优先复用已有证据，不强制进行全部验收项目。
- **仿真入口**：当前实现范围为单实体、线性静力、压力载荷及固定面约束；需要相应 SolidWorks Simulation 许可和插件。不是完整的多物理场仿真系统。
- **独立工作区**：每个新对话在安装目录的 `app/output/` 下建立工作区；模型和生成脚本保存在其中。
- **知识与工具**：随包包含 API 文档、skill、标准资源和 agent 工具循环。生成结果受需求、模型能力和 CAD API 限制影响，可能需要交互修正。

## 安装前准备

1. Windows 10/11 x64、可正常运行的 SolidWorks。开发机原生测试版本为 **SolidWorks 2026 SP0.0**；其他版本尚未逐一认证。
2. .NET Framework 4.8 与 Microsoft Edge WebView2 Runtime。若侧栏无法显示，请先安装 [WebView2 Evergreen Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)。
3. 有权限调用模型 API 的账号、API Key 和支持工具调用/流式输出的模型。网页聊天会员通常不等于 API 额度。
4. 解压目录必须可写。推荐 `D:\Apps\CAutoD-v1.0.0` 或用户文档下的目录，避免 `Program Files`、只读目录和网络共享。

## 安装插件

1. 从 [GitHub Releases](https://github.com/ddhh1212/CAutoD/releases/tag/v1.0.0) 下载 Windows x64 ZIP。**完整解压**，不要在 ZIP 内直接运行。
2. 关闭 SolidWorks。双击 `Inspect.cmd`，检查插件二进制和安装清单。
3. 双击 `INSTALL.cmd`，在 Windows 提示时允许管理员权限。安装脚本只注册本插件，不会自动运行建模任务。
4. 启动 SolidWorks，打开“工具 → 插件”（Tools → Add-ins），勾选 **CAD Agent**。需要每次启动自动加载时，再勾选启动项。
5. 打开 CAD Agent 任务窗格，按首次配置提示完成 API 设置。
6. 新建对话并输入需求。输出位于 `app/output/chat_.../`。

没有管理员权限时无法完成 COM 注册。若提示其他版本占用注册，请先使用旧版本自己的 `UNINSTALL.cmd` 卸载注册，再安装本版本；不要直接覆盖运行中的 DLL。

## 首次配置模型 API

插件中点击 **API 配置**，填写：

- 服务商；
- Base URL（区域不同可能使用不同地址，不含 `/chat/completions`）；
- 该服务商提供的准确模型 ID；
- API Key；
- 最大输出 tokens（默认 8192，按模型能力调整）。

未配置时不能开始生成。保存配置本身不会调用模型；首次发送任务会向配置的服务商发起请求并产生相应费用。保存成功只表示格式有效，不代表密钥、额度、模型权限或工具调用能力已验证。

| 服务商 | 获取账号和 Key | 接入方式 |
| --- | --- | --- |
| DeepSeek | [开放平台](https://platform.deepseek.com/) | OpenAI 兼容接口 |
| OpenAI | [API 平台](https://platform.openai.com/) | Chat Completions；选择支持 tools 的模型 |
| 通义千问 | [阿里云百炼](https://bailian.console.aliyun.com/) | DashScope 兼容接口，注意地域 |
| Kimi | [Moonshot 平台](https://platform.moonshot.cn/) | OpenAI 兼容接口 |
| 智谱 GLM | [开放平台](https://open.bigmodel.cn/) | OpenAI 兼容接口 |
| Gemini | [Google AI Studio](https://aistudio.google.com/) | OpenAI compatibility 接口 |
| OpenRouter | [API Keys](https://openrouter.ai/keys) | 统一入口，可选 Claude 等模型 |
| 其他服务 | 服务商自己的控制台 | 自定义 OpenAI Chat Completions 兼容 URL |

当前配置页一次保存一个活动服务商/模型；不支持 Anthropic 原生 Messages 协议，也不保证所有兼容接口都支持相同参数。API 网络可达性、模型权限和地区可用性由服务商决定。

Key 使用 Windows 当前用户 DPAPI 加密保存在 `%LOCALAPPDATA%\CAutoD\provider.json`，不写入系统环境变量、不回显到浏览器。更换 Windows 用户或电脑需要重新配置。不要将这个文件、运行日志或输出目录上传到公开仓库。任务内容、参考图和必要的工具反馈会发送给你选择的模型服务商。

## 路径与运行环境

- `plugin/agent-host.local.json` 使用相对路径，启动时基于插件所在目录解析。
- `runtime/python/`、`runtime/node/` 是随包运行环境，不需要借用开发者电脑上的程序。
- 运行时生成绝对路径供 SolidWorks 使用是正常行为，不能把已经生成的旧路径当作跨电脑安装配置。
- **注册后不要直接移动安装目录**。需要迁移目录时，关闭 SolidWorks、卸载注册、移动整个文件夹，再注册。
- 不复制开发机密钥、`.env`、会话、浏览器缓存、旧模型、签名私钥或实验日志；只清理插件子进程相关环境，不删除用户系统环境变量。

## 卸载

关闭 SolidWorks，双击 `UNINSTALL.cmd` 并允许管理员权限。它注销当前版本，不删除你的模型、对话或 API 配置。确认不再需要后，可自行删除安装目录及 `%LOCALAPPDATA%\CAutoD`。

## 常见问题

- **API 401/403**：检查 Key、模型权限及 Base URL。API Key 属于对应平台，不能跨平台混用。
- **429 / 额度不足**：检查服务商额度和限流；先暂停任务再处理。
- **模型不支持工具调用**：改用支持 tools/function calling 和流式输出的模型。支持文本聊天不代表支持 CAD agent。
- **找不到默认模板**：先在 SolidWorks 中配置默认零件、装配模板，并确认可以手动新建文档。
- **同名零件冲突**：为新任务使用唯一文件名，避免 SolidWorks 复用其他目录已打开的同名零件。
- **运行时间较长或局部失败**：保留已可用模型，用具体反馈继续修复，不必反复重新生成全部零件。
- **安全软件拦截**：本预览版没有商业代码签名。核对发布 SHA-256 和来源，按组织的软件安装要求处理。

## 分发与验证边界

原生插件是编译后的 .NET DLL，后端及 PySW 实现以 CPython 3.12 编译字节码分发，不提供 `.py` / `.cs` 实现文件。字节码不是加密，不能保证无法反编译。浏览器前端、安装脚本、API 文档、skill 和示例接口说明需要可读，用户生成的建模脚本也会保存在工作区。

此发行版是二进制发布，不等同于开放实现源码。第三方组件及许可证说明见 [THIRD_PARTY_NOTICES.md](../THIRD_PARTY_NOTICES.md)。跨电脑全新安装和不同 SolidWorks 版本仍需用户环境验证；发布检查结果见 [RELEASE_CHECKS.json](../RELEASE_CHECKS.json)。
