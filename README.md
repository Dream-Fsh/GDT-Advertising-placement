# GDT-Advertising-placement

广告投放自动化，支持广点通在创量上的自动化搭建。

本项目将创量平台的广点通批量新建流程封装为可复用的 Codex skill，可用于账户导入、广告搭建、素材筛选、文案与落地页配置、预览校验，以及后续自动化实现。

## 联系方式

![微信二维码](微信.png)

## 项目内容

- `SKILL.md`：skill 的触发条件、执行边界与核心操作规则。
- `agents/openai.yaml`：Codex 界面元数据。

## 使用方式

在 Codex 中调用：

```text
Use $gdt-advertising-placement to build a Tencent GDT campaign in Chuangliang following the approved SOP.
```

skill 默认执行到广告预览和核对阶段。最终提交会在外部平台创建广告，因此必须在提交前取得明确确认。

## 核心规则

- 账户、产品、品牌形象、素材、文案、落地页、出价和预算均作为本次任务的输入。
- 产品、品牌形象和落地页等资源在账户间不一致时，按账户分别配置。
- 素材的多账户分配与创意组分配均为「平均分配」，创意组素材上限为 `1`。
- 文案启用多文案测试，并选择两条已确认文案。
- 自动化必须等待页面可见状态变化，不能仅依赖固定等待时间。
