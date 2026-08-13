---
name: gdt-advertising-placement
description: Build or review Tencent GDT campaigns in Chuangliang using the approved batch-creation SOP. Use for importing advertiser accounts, configuring products, placements, conversion, bidding, targeting, creatives, materials, copy, landing pages, preview validation, or automation of this workflow.
---

# GDT Advertising Placement

Use this skill to build Tencent GDT ads in Chuangliang's batch-create flow. Keep the detailed SOP local; use the workflow and guardrails below as the reusable execution contract.

## Operating Boundary

- Stop after generating and validating the campaign preview.
- Request explicit user confirmation immediately before the platform's final submit action.
- Treat account IDs, products, brand images, landing pages, copy, bid range, and budget as inputs. Do not reuse values from an earlier campaign unless the user provides them again.

## Required Inputs

Collect or derive these items before starting:

1. Advertiser names, account IDs, and expected account count.
2. Product search keyword or product name.
3. Brand-image keyword based on advertiser name.
4. Targeting template.
5. Material directory, date range, filter metrics, and thresholds.
6. Exact ad copy.
7. Account-to-landing-page mapping.
8. Bid range and budget.

If a required resource varies by account, do not use a shared configuration. Configure each account individually and report any unavailable resource.

## Workflow

1. Import account IDs and select the matching account rows.
2. Create the campaign, select the optimization goal before choosing the product, and validate any shared-product source-account prompt.
3. Configure placements, conversion, bidding, schedule, naming, and targeting.
4. Configure the component creative, brand image, button copy, materials, ad copy, and landing page.
5. Generate the preview and validate every account before requesting confirmation for final submission.

## Non-Negotiable Defaults

- Use smart placements with the approved stable-exploration placement set.
- Use API reporting, form-appointment conversion, and click attribution.
- Use the approved targeting template and selected brand-image rule.
- For materials, choose average allocation for both account and creative-group allocation; set creative-group material limit to `1`.
- Add images from the global bulk-add menu, not an individual creative group.
- For copy, use all-account reuse, enable multi-copy testing, and select exactly two approved copy items.

## New-Ad Drawer Sequence

After opening the new-ad drawer, configure visible sections strictly from top to bottom. Start with `营销内容`; do not jump directly to `广告版位`, conversion, or bidding.

1. In `营销内容`, select and verify the marketing purpose.
2. Continue within the same section in displayed order: promotion product, applicable product/goal selection, and product assignment. Where the version exposes `更多目标`, search and select the approved `表单预约` optimization goal. Then inspect the conversion list returned for the selected accounts: do not hardcode a conversion name such as `优投-表单预约`; when exactly one selectable conversion is returned, select that unique option and verify it corresponds to the chosen optimization goal.
3. Only after the marketing-content section is complete, configure `广告版位`, then the lower sections in page order.
4. Treat the initial `加粉互动 + 微信公众号 + 手动版位` state as an unconfigured page default. Do not save it.

For targeting templates, inspect the current list first. If the approved template is already visibly present and matches exactly, it may be selected directly. Otherwise use the targeting search field to search `6.3-腾讯-通投-fsh`, wait for results, select the exact result, and verify the selected count before confirming.

## Creative Content Order

In `创意内容`, start with brand-image navigation before touching jump type, landing page, action button, materials, or copy. Set `品牌形象跳转` to `自定义` first; never retain the page-default `视频号`. Then search the brand image using the supplied entity keyword, select the first verified result when the SOP permits it, confirm the selection, and only then continue down the section.

## Browser Automation Rules

1. Wait for visible state changes after each selection, including selected counts, success notices, saved values, or enabled next-step controls.
2. Check product pages from the last page backward when the product is not immediately available.
3. Verify checkbox state and selected-count panels before confirming modals; search results alone are not a selection.
4. Treat shared-resource dialogs, material-risk dialogs, and preview generation as explicit checkpoints.
5. On mismatched account, product, brand image, landing page, permission, or allowlist state, stop and return an exception list instead of guessing.

After entering a product keyword, click the visible `刷新` button before inspecting results. Wait until the product rows and pagination settle, re-read the actual last-page number, then inspect every page backward from that number. Select only the product whose displayed name exactly matches the user-provided product name or approved target rule. A keyword match, a date containing the keyword, or the first displayed row is not sufficient. For example, when the target product name is the single character `1`, select only the row displayed exactly as `1`; do not select a date, package name, or any other product that merely contains `1`. If no exact unique match exists, stop and request the product name; never select an item shown under a stale page count.

## Multi-Account Import DOM Contract

For two or more account IDs, do not paste newline-separated IDs into the ordinary keyword field. In the "选择媒体账户" dialog:

1. Scope every locator to the dialog containing `选择媒体账户`.
2. Locate the ordinary keyword input (`请输入关键词`) and its batch-search control. Prefer `.cl-search-input__batch .el-popover__reference[title="批量搜索"]`; otherwise use the control labelled `批量搜索` that is immediately adjacent to that input. Do not select an icon merely because it looks like a search icon.
3. Click the batch control exactly once. It is a toggle and is not retry-safe.
4. Continue only after the batch popover exposes both `请输入账户ID` and the multiline-entry input `请粘贴或输入账户ID，回车可换行` plus its `搜索` button. If this contract is absent, stop and report the selector mismatch.
5. Enter one numeric ID per line and run the popover's search. Verify the result rows are an exact set match for the requested IDs before any checkbox is selected. Report missing, duplicate, or unexpected IDs as exceptions.
6. When the verified result set is complete, use the table-header checkbox once to select all returned rows; do not manually select accounts one by one. First ensure the header checkbox is not already selected, then verify the selected count equals the verified result count before clicking `确定`.

Use screenshots only to diagnose a selector mismatch. Do not use coordinates as the primary path for this control.

## Shared-Product Warning Gate

For both single-account and multi-account product selection, use `全部相同` by default. This selection opens the platform's business-unit warning dialog. Verify that the listed IDs are the selected IDs, then treat it as the normal confirmation path and click `确定` by default. Stop only if the platform reports an actual error after confirmation; do not change to per-account allocation merely because this warning appeared.

## Completion Criteria

Report the following before asking for final submission approval:

- Selected accounts and count.
- Product, placement, conversion, bid, targeting, brand image, materials, copy, and landing page validation results.
- Preview status and any skipped or exceptional accounts.
- Exact external action that final submission will create.
