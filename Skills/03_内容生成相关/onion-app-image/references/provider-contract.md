# KIE 生图 API 合同与验证边界

正式图片生成使用本Skill已迁入的KIE异步任务适配器：

- 任务 API：`https://api.kie.ai`，可用 `KIE_BASE_URL` 覆盖；
- 参考图上传：`https://kieai.redpandaai.co`，可用 `KIE_UPLOAD_BASE_URL` 覆盖；
- 创建任务：`POST /api/v1/jobs/createTask`；
- 查询任务：`GET /api/v1/jobs/recordInfo?taskId=...`；
- API Key 环境变量：`KIE_API_KEY`；
- 文生图模型：`gpt-image-2-text-to-image`；
- 使用明确参考图时：`gpt-image-2-image-to-image`；
- 分辨率：固定 `2K`；
- 输出：下载 KIE 任务完成后的结果并保存为 PNG；
- 任务状态：保存到输出图旁的 `.kie-task.json`，网络中断后优先恢复已有 `taskId`，不重复创建付费任务。

本 Skill 当前只开放“确认文案后直接生图”。参考图只用于配置卡中明确声明的 Logo、IP、字体、UI 或风格参考，不用于已有广告图迭代；用户提出改已有图时暂不进入本流程。

以上来自现行 KIE 适配器和本地测试产物，不表示当前账户余额、模型额度或网络一定可用。真实生成前仍需先 `--validate-only`、取得付费确认，并使用非敏感测试图验证当前环境。

正式生成前：

1. 先运行`render.py --validate-only`；
2. 明确预计生成张数并取得付费确认；
3. 使用一张不含敏感信息的测试图验证当前 KIE 模型、2K 比例、参考图上传和任务结果下载；
4. 记录供应商错误，不通过反复改 Prompt 掩盖鉴权、额度、限流或任务恢复问题；
5. 不把 Key、原始响应中的敏感信息写入产物。

实际调用还必须在当前任务命令中显式传`--approved-in-current-task`。这一参数不写入可复用配置，也不能由历史任务状态自动补齐。
