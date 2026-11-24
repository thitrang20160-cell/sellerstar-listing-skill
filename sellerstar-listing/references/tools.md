# 客户端接入和参数

## 外部智能体

商品采集使用塞星官方浏览器插件。工具栏权限、扩展弹窗和验证码需要人工操作时，在当前浏览器交接。插件完成后，通过门户按工作区和来源核对采集记录，然后继续铺货任务。

铺货、暂存和 Feed 使用授权门户。常用入口为 [塞星 AI Agent 工作空间](https://portal.safesellai.com/agents?view=listing)、[铺货任务](https://portal.safesellai.com/ai)、[上架暂存区](https://portal.safesellai.com/ai?view=staging) 和 [上架回执](https://portal.safesellai.com/feeds)。浏览器控制工具先找到已登录标签，读取当前界面，再按其实际工具约定点选、填写或读回。站点切换和会话失效时重新核对工作区、店铺及商品范围。

发现客户端已配置塞星业务工具时，先查看实际权限、输入和当前回执。塞星合规工具的凭证、产品内账号会话及当前店铺权限分别核对。

## 塞星内部 Pi / Agent

有 `tool_search`、`read_skill` 和真实业务工具时，当轮先 `read_skill({name:"workflow-listing"})`。查店铺、Feed 和在线发布用 `workflow-operations`。独立生图用 `workflow-images`。工具名以当前 `tool_search` 结果为准。工作区可用明确的已授权任务和商品 ID；任务侧栏以当前绑定任务、选中 SKU 为范围。侧栏能力不足时回工作区或对应产品页。

| 工具 | 核心参数与边界 |
| --- | --- |
| `list_listing_jobs` | `{}`。返回本账号自己的任务 |
| `get_listing_job_state` | `{jobId,page,limit}`。逐页处理，`limit` 最大 20，依据 `summaryPage.hasMore` |
| `get_listing_job_item` | `{jobId,itemId}`，用实际选中商品 ID |
| `wait_listing_job` | `{jobId,seconds}`，1–30 秒。继续读后台状态 |
| `create_listing_job` | `{title,urls,collectionIds,plan}`。本流程用插件入箱后读到的 collectionIds，每批最多 100，须目标店铺。该工具创建草稿 |
| `update_listing_job_plan` | `{jobId,plan}`，用于可修改的草稿或暂停任务 |
| `confirm_listing_categories` | `{jobId,selections:[{itemId,productType}]}`，按目标站点官方 Product Type 和当前任务商品 |
| `start_listing_job` / `pause_listing_job` | `{jobId}`。按整任务及当前已确认规则；勾选子集用工作台「批量开始」 |
| `edit_listing_job_item` | `{jobId,itemId,product}`，先读当前字段，保持未要求修改的数据 |
| `continue_listing_job_item` / `retry_listing_job_item` | `{jobId,itemId}`。前者用于可继续，后者用于已确认失败并修好 |
| `submit_listing_staging` | `{itemIds:[stagingId,...]}`，同一店铺 1–50 条，走准备、审批、预检、发送和回执 |
| `check_listing_item_platform` | `{jobId,itemId}`，核对平台结果并更新本地回执，按实际权限和确认执行 |

可用工具还可能包含 `list_collection`、`get_staging_item`、`list_feeds`、发布诊断和生图。先读当前说明。工具若要求确认，使用其原卡片。`batch_suffix_listing_job` 仅在用户明确要求指定任务本批全部 SKU 追加准确后缀时使用。某工具没暴露时回到对应门户入口。

### 新任务规则常用字段

| 字段 | 已核对的常用值 |
| --- | --- |
| `storeId` | 当前账号可见且用户授权的店铺 ID |
| `priceMode` | `source`、`fixed` 搭配 `fixedPrice`，或 `multiplier` 搭配 `multiplier` |
| `stockMode` | `source`，或 `fixed` 搭配整数 `fixedStock`，允许明确的 0 |
| `brandMode` | `keep` 保留来源品牌、`store` 用店铺预设、`remove_source` 按当前明确规则处理来源品牌 |
| `rewriteListing` / `generateAttributes` | 布尔值。分别确认文案和属性 |
| `imageMode` | `keep`、`dedup`、`replace`、`generate`。生图选 `imageTier` 后取系统报价 |
| `submissionMode` | `spec` 新建 Listing，`match` 匹配现有商品 |
| `destination` | `staging` 暂存。真实发布只在明确授权时选择，优先经过工作稿逐项核验 |

示例只创建草稿，使用实际读到的 ID，按实时确认设置费用。

```json
{
  "title": "已确认样品",
  "collectionIds": ["<实际采集记录ID>"],
  "plan": {
    "storeId": "<实际目标店铺ID>",
    "priceMode": "source",
    "stockMode": "fixed",
    "fixedStock": 0,
    "brandMode": "keep",
    "rewriteListing": false,
    "generateAttributes": false,
    "imageMode": "keep",
    "submissionMode": "spec",
    "destination": "staging"
  }
}
```

门户里的「使用教程」入口用于核对当前按钮与工作流。
