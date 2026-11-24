# 塞星 AI 铺货 Skill

用 AI 智能体协助操作塞星铺货。从官方浏览器插件采集，到铺货任务、SOP、上架暂存、沃尔玛提交和逐 SKU 回执。

## 安装

1. 从 [GitHub Releases](https://github.com/thitrang20160-cell/sellerstar-listing-skill/releases/latest) 下载 `sellerstar-listing-skill.zip`。
2. 解压出 `sellerstar-listing` 文件夹，在 Codex 中放到 `~/.codex/skills/`。使用其他兼容客户端时，导入同一个技能目录。也可以直接使用本仓库的 [`sellerstar-listing/`](sellerstar-listing/)。
3. 登录 [塞星门户](https://portal.safesellai.com/ai)。需要采集商品时，在 [商品采集](https://portal.safesellai.com/collection) 下载并安装官方插件。插件安装和使用见 [采集说明](sellerstar-listing/references/collection.md)。

## 使用示例

> 用 `$sellerstar-listing` 帮我核对这批商品的采集记录，再按确认好的规则跑到上架暂存区。逐个 SKU 回读结果，先不提交沃尔玛。

智能体根据所在客户端的能力，使用产品界面或已接入的塞星业务工具。浏览器工具栏、插件权限或验证码需要人工操作时，完成后继续由智能体读回采集结果。商品操作遵循当前账号的店铺权限、费用和提交确认。

从 [SKILL.md](sellerstar-listing/SKILL.md) 开始，也可查阅 [铺货和暂存](sellerstar-listing/references/listing-workflow.md)、[审核和提交](sellerstar-listing/references/submission.md) 以及 [接入与参数](sellerstar-listing/references/tools.md)。

## 许可

本仓库的 Skill 和配套说明采用 [MIT License](LICENSE)。塞星门户及采集插件从官方入口获取，使用时遵循产品条款。
