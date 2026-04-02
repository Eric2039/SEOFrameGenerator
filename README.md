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


#SEO outline generator
通过serp api和deepseek api先进行文章研究 再做出文章结构的obsidian插件
