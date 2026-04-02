# SEO Outline Generator

An Obsidian plugin that fetches live SERP results with SerpApi, then sends the keyword plus top-result titles and snippets to DeepSeek to generate:

- article outline
- SEO brief
- SERP gap analysis

## Workflow

1. Enter a keyword.
2. Enter the keyword type.
3. Optionally add business context and operation-step reference.
4. The plugin calls SerpApi and collects the top live results.
5. The plugin sends those results to DeepSeek.
6. The plugin renders the outline in the sidebar and saves a markdown brief locally.

## Settings

- `AI provider`
- `AI API key`
- `Model name`
- `Custom endpoint` for OpenAI-compatible services
- `SerpApi key`
- `Search engine` (`baidu` or `google`)
- `Search region`
- `Search language`
- `Result count`
- `Output folder`

## Notes

- Chinese mainland SEO is the default target.
- For `基础操作/命令类` keywords, operation steps are treated only as a reference for prompt calibration and are not written back as a full tutorial.
- The generated outline always keeps the CTA as the final section.

## Build

```bash
npm install
npm run build
```

# SEO Outline Generator

一个用于 Obsidian 的 SEO 内容辅助插件。

这个插件的核心流程不是“直接让模型凭空写结果”，而是先通过 `SerpApi` 获取关键词对应搜索引擎的实时结果，再把前列网页的标题和摘要作为上下文发送给 `DeepSeek`，最后输出更贴近真实 SERP 的：

- 文章框架
- SEO brief
- SERP 缺口分析
- 标题建议
- 逻辑链

插件当前默认面向中文、中国大陆 SEO 内容场景，尤其适合你这种需要稳定产出“框架 + brief”而不是直接生成整篇文章的工作流。

---

## 1. 功能概览

### 1.1 核心能力

- 输入关键词后，自动调用 `SerpApi`
- 获取 `Baidu` 或 `Google` 前列搜索结果
- 提取网页标题与摘要
- 将这些实时结果作为上下文传给 `DeepSeek`
- 输出结构化 SEO 结果，而不是散乱文本
- 结果可直接保存为本地 Markdown 文件

### 1.2 输出内容

插件会生成以下结构化结果：

- `keyword`
- `keywordType`
- `titleSuggestion`
- `logicChain`
- `outline`
- `brief`
- `gapAnalysis`
- `serpSummary`
- `sourceTitles`

### 1.3 当前适用场景

- SEO 文章选题前分析
- SEO 写手 briefing
- 基于 SERP 的文章框架设计
- 针对竞品内容的补强点提取
- 中文 B2B、SaaS、软件类内容策划
- CAD 软件类关键词框架生产

---

## 2. 插件工作原理

整体流程如下：

1. 用户输入关键词
2. 用户输入关键词类型
3. 用户可补充业务背景信息
4. 若关键词属于基础操作/命令类，可额外提供操作步骤参考
5. 插件向 `SerpApi` 请求实时搜索结果
6. 插件提取前列结果的标题、摘要、链接
7. 插件把这些数据作为上下文发送给 `DeepSeek`
8. `DeepSeek` 返回结构化 JSON
9. 插件在 Obsidian 侧边栏展示结果
10. 插件自动保存为本地 Markdown

这个设计的价值在于：

- 不是纯模型臆测
- 不是脱离当前 SERP 的静态框架
- 可以结合竞品标题和摘要判断内容缺口
- 更适合真正的 SEO 内容生产链路

---

## 3. 当前内容策略约束

这个插件的 prompt 已经内置了你当前的内容规则，尤其适用于中文中国大陆内容：

- 输出语言必须为简体中文
- 默认目标地区为中国大陆
- 语气偏专业
- 不直接生成正文
- 文章总段数不超过 6 段
- 不允许做成碎片化结构
- 每一段都必须承担明确逻辑功能
- 必须形成完整逻辑链
- CTA 必须单独成段
- CTA 必须是收口段，而不是硬切广告
- 如果是基础操作/命令类关键词，会优先生成“概念 -> 价值 -> 判断逻辑 -> 文件/协作 -> 平台能力 -> CTA”这类链路
- 如果基础操作类关键词没有提供步骤参考，brief 会提醒正式写作前补充步骤参考
- 即使用户提供了步骤参考，也不会把完整步骤教程写进文章结构

---

## 4. 安装方式

### 4.1 手动安装

把插件文件放到你的 Obsidian Vault 目录下：

```text
.obsidian/plugins/feynman-learning/
```

至少需要这些文件：

- `main.js`
- `manifest.json`
- `styles.css`

如果你自己开发和修改插件，完整目录建议保留：

- `main.ts`
- `settings.ts`
- `settings-tab.ts`
- `ui/`
- `core/`
- `api/`
- `README.md`

### 4.2 构建

在插件目录下执行：

```bash
npm install
npm run build
```

构建完成后会生成可供 Obsidian 加载的 `main.js`。

---

## 5. 配置项说明

打开 Obsidian 后，在插件设置中可以看到以下配置：

### 5.1 AI Provider

可选：

- `DeepSeek`
- `OpenAI`
- `Claude`
- `Qwen`
- `Baidu`
- `Custom`

当前 SEO 工作流最稳定的推荐组合是：

- `AI provider = deepseek`

### 5.2 AI API Key

这里填写大模型接口密钥。

如果你使用 DeepSeek，就填：

```text
DeepSeek API Key
```

### 5.3 Model Name

默认推荐：

```text
deepseek-chat
```

### 5.4 Custom Endpoint

只有当 `AI provider = custom` 时才需要。

填写兼容 OpenAI Chat Completions 的接口地址，例如：

```text
https://api.example.com/v1/chat/completions
```

### 5.5 SerpApi Key

这是搜索接口的密钥，用于获取实时 SERP。

没有这个 key，插件无法拉取实时搜索结果。

### 5.6 Search Engine

可选：

- `baidu`
- `google`

如果你的目标内容是中文、中国大陆 SEO，建议优先：

```text
baidu
```

### 5.7 Search Region

例如：

- `cn`
- `us`

### 5.8 Search Language

例如：

- `zh-cn`
- `en`

### 5.9 Result Count

表示要抓取多少条实时结果作为上下文。

默认建议：

```text
10
```

### 5.10 Output Folder

生成的 SEO 结果会自动保存到这个目录。

默认示例：

```text
SEO Briefs
```

---

## 6. 本地硬编码 API Key 方案

如果你是个人使用，不想每次都在设置里填 key，可以直接使用本地密钥文件：

文件：

[local-secrets.ts](D:/games/做的東西/FeynmanAI/local-secrets.ts)

内容如下：

```ts
export const LOCAL_SECRETS = {
    apiKey: '',
    serpApiKey: ''
};
```

你可以改成：

```ts
export const LOCAL_SECRETS = {
    apiKey: '你的 DeepSeek Key',
    serpApiKey: '你的 SerpApi Key'
};
```

然后重新构建：

```bash
npm run build
```

插件启动时会优先读取设置；如果设置中是空值，则自动回退到这个本地文件里的 key。

这个方案适合：

- 个人本地使用
- 不打算公开发布完整源码
- 后期可手动移除密钥后再对外分发

注意：

- 不要把带真实 key 的源码提交到公开仓库
- 不要把带真实 key 的插件文件夹直接发给别人

---

## 7. 使用方法

### 7.1 打开侧边栏

启用插件后：

- 点击左侧 Ribbon 图标
- 或用命令面板执行 `Open SEO Outline Generator`

### 7.2 输入字段

侧边栏会有四个主要输入项：

#### 1. 关键词

例如：

```text
cad镜像
```

#### 2. 关键词类型

例如：

```text
基础操作/命令类
```

或者：

```text
文件类型类
资源/模型类
功能介绍类
```

#### 3. 业务补充信息

用于把你的内容策略告诉模型。

例如：

```text
语言简体中文，中国大陆，语气专业，不写正文，只输出框架和brief，CTA重点为在线协作、AI协助、产品管理。
```

#### 4. 操作步骤参考

这个字段主要给基础操作/命令类关键词用。

例如：

```text
用户一般会先选择对象，再执行镜像命令，再指定镜像轴线，再决定是否保留原对象。
```

注意：

- 这里只是参考
- 插件不会把完整操作教程写进文章结构
- 它的作用是帮助模型判断逻辑和术语是否准确

### 7.3 点击生成

点击：

```text
生成框架与 Brief
```

插件会自动：

- 先调用 `SerpApi`
- 再调用 `DeepSeek`
- 最后展示结果并落盘

---

## 8. 输出结果说明

### 8.1 逻辑链

这部分用于告诉你整篇文章的推进路径。

例如：

- 先解释概念
- 再讲使用价值
- 再讲判断逻辑
- 再过渡到文件/协作
- 再承接平台能力
- 最后 CTA 收口

### 8.2 文章框架

这是最核心的部分。

每一段会包含：

- 段落标题
- 段落目标
- 段落要点

并且限制在不超过 6 段。

### 8.3 SEO Brief

一般包含：

- 关键词类型
- 搜索意图
- 目标受众
- 内容目标
- 语气要求
- 推荐角度
- 必须覆盖点
- CTA方向
- 备注

### 8.4 SERP 缺口分析

这部分会尽量说明：

- 当前 SERP 大家都写了什么
- 哪些角度已经被覆盖
- 哪些角度仍然是缺口
- 有没有机会补入“在线协作 / AI协助 / 产品管理”等差异化点

### 8.5 实时搜索结果

最终 Markdown 文件里也会附带实时 SERP 结果，方便你复查。

---

## 9. 生成文件位置

默认会保存在：

```text
SEO Briefs/
```

文件名格式类似：

```text
cad镜像-2026-04-03-153000.md
```

文件里会包含：

- 关键词
- 建议标题
- 逻辑链
- 文章框架
- SEO brief
- SERP 缺口分析
- SERP 标题参考
- 实时搜索结果

---

## 10. 推荐工作流

你现在的真实工作流建议这样用：

1. 先确定关键词
2. 人工判断或读取文档判断关键词类型
3. 把内容策略写进“业务补充信息”
4. 基础操作类关键词补一个“步骤参考”
5. 插件生成结构化框架和 brief
6. 你人工检查逻辑链是否完整
7. 如需微调，再次生成
8. 把结果交给写手或下一步正文生成流程

如果你后续做升级，最值得加的不是“多写一点内容”，而是：

- 自动读取关键词类型规则文档
- 自动判定是否属于基础操作类
- 自动要求补步骤参考
- 自动按不同关键词类型切换 prompt 策略

---

## 11. 常见报错与排查

### 11.1 生成失败，请检查 API 和网络连接

这个报错通常意味着：

- `SerpApi key` 未配置
- `DeepSeek API key` 未配置
- 网络请求被插件环境拦截
- 返回数据结构异常
- 模型没有按 JSON 返回

当前版本已经把网络请求改成了 Obsidian 的 `requestUrl`，比浏览器原生 `fetch` 更适合插件环境。

如果仍报错，请优先检查：

1. `AI provider` 是否为 `deepseek`
2. `modelName` 是否为 `deepseek-chat`
3. `SerpApi key` 是否有效
4. `search engine` 是否正确
5. `Result Count` 是否过大

### 11.2 SerpApi error

说明搜索接口调用失败。

常见原因：

- key 错误
- 额度用完
- engine 参数不合法
- 当前网络环境无法访问相关接口

### 11.3 AI request failed

说明大模型接口调用失败。

常见原因：

- DeepSeek key 错误
- 模型名错误
- 接口地址不对
- 账号额度问题
- 服务端拒绝 `response_format`

### 11.4 Failed to parse SEO outline response

说明模型没有按要求返回合法 JSON。

这是 prompt 或模型输出稳定性问题，不一定是网络问题。

后续如果你继续优化插件，可以考虑：

- 增加 JSON 修复逻辑
- 增加重试机制
- 在解析失败时把原始文本显示出来

---

## 12. 技术结构

主要文件如下：

- [main.ts](D:/games/做的東西/FeynmanAI/main.ts)
  插件入口

- [settings.ts](D:/games/做的東西/FeynmanAI/settings.ts)
  设置项定义与默认值

- [settings-tab.ts](D:/games/做的東西/FeynmanAI/settings-tab.ts)
  Obsidian 设置页面

- [ui/sidebar.ts](D:/games/做的東西/FeynmanAI/ui/sidebar.ts)
  侧边栏 UI 和交互逻辑

- [api/serpapi-service.ts](D:/games/做的東西/FeynmanAI/api/serpapi-service.ts)
  实时 SERP 获取

- [core/seo-analyzer.ts](D:/games/做的東西/FeynmanAI/core/seo-analyzer.ts)
  SEO 分析主流程

- [api/prompt-templates.ts](D:/games/做的東西/FeynmanAI/api/prompt-templates.ts)
  DeepSeek prompt 模板

- [core/file-generator.ts](D:/games/做的東西/FeynmanAI/core/file-generator.ts)
  Markdown 输出

- [local-secrets.ts](D:/games/做的東西/FeynmanAI/local-secrets.ts)
  本地硬编码密钥

---

## 13. 当前已知限制

- 目前主要针对中文 SEO 场景优化
- 对 `Claude/Qwen/Baidu` 的 SEO 流程虽然保留了入口，但最佳适配仍然是 `DeepSeek`
- 当前结果依赖搜索摘要质量
- 模型偶尔可能返回不完全符合 JSON 结构的结果
- 还没有内置“关键词类型规则文档读取”
- 还没有内置“基础操作类关键词强制检查步骤参考”
- 还没有内置“失败重试”

---

## 14. 下一步建议

如果你继续把这个插件往真实生产工具推进，我建议优先做这几个功能：

### 14.1 关键词类型规则文档接入

你前面已经明确说了：

- 不同关键词类型不能用同一种段落分布手法
- 之后会有一个文档专门判断关键词类型

所以最优先的增强是：

- 读取本地 Markdown/JSON/YAML 规则文档
- 自动根据关键词分类
- 自动切换 outline 策略

### 14.2 基础操作类强制校验

如果识别为基础操作/命令类：

- 没填步骤参考就提醒
- 但仍然不把完整步骤写进文章结构

### 14.3 输出模板切换

未来可以加入多种 brief 模式：

- 写手 brief
- 编辑 brief
- 产品 SEO brief
- 商业落地页 brief

### 14.4 错误调试面板

建议后面加一个调试开关，显示：

- SerpApi 原始状态码
- DeepSeek 原始状态码
- 原始响应片段
- 解析失败原因

这会比现在只看 Notice 好很多。

---

## 15. 适合你的使用方式

按你现在的需求，这个插件最适合这样定位：

不是“AI 写文章插件”，而是：

```text
基于实时 SERP 的 SEO 框架与 brief 生成器
```

它的作用是帮你稳定地产出：

- 合理的段落结构
- 明确的逻辑链
- 面向写手的内容 brief
- 竞品缺口分析

这样你后面不管是继续手写正文，还是再接一个“正文生成模块”，整个链路都会更稳。

---

## 16. 构建命令

```bash
npm install
npm run build
```

---

## 17. 免责声明

本插件依赖第三方服务：

- `SerpApi`
- `DeepSeek`

服务可用性、额度限制、接口变更、计费规则均由对应服务方决定。

如果你将来把这个插件公开给其他人使用，建议：

- 去掉本地硬编码密钥
- 改成必须用户自行填写 API key
- 增加错误提示和使用文档

