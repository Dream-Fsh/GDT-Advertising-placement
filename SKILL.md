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
5. Material directory, metric data-time range, filter metrics, and thresholds. When the user does not provide a material time range, use the rolling `近7天` range in `自定义指标与时间`; do not block the run for this omission. Do not treat this as the material library's `上传时间` filter: clear `上传时间` by default.
6. Exact ad copy.
7. Account-to-landing-page mapping.
8. Bid range and budget.

If a required resource varies by account, do not use a shared configuration. Configure each account individually and report any unavailable resource.

## Workflow

1. Import account IDs and select the matching account rows.
2. Create the campaign, select the optimization goal before choosing the product, and validate any shared-product source-account prompt.
3. Configure placements, conversion, bidding, schedule, and naming.
4. Configure targeting before opening or editing `创意信息`; targeting is a required prerequisite for the creative-information step.
5. Configure the component creative, brand image, button copy, materials, ad copy, and landing page.
6. Generate the preview and validate every account before requesting confirmation for final submission.

## Non-Negotiable Defaults

- Use smart placements with the approved stable-exploration placement set.
- Use API reporting, form-appointment conversion, and click attribution.
- Use the approved targeting template and selected brand-image rule.
- For materials, choose average allocation for both account and creative-group allocation; set creative-group material limit to `1`.
- For materials with no user-supplied material time range, explicitly click into `自定义指标与时间`, then select `近7天` in its opened data-time panel; never set this range through the material-library `上传时间` control. In the material library's upper filter area, clear `上传时间` by default; it is not configured from the material time input. Retain the approved default filters `花费 >= 1` and `目标转化量 >= 1` unless the user supplies replacements.
- After confirming the material filters and before selecting materials, set the material-library pagination to `100条/页`. Wait for the filtered rows and page count to settle, then inspect the header checkbox and use it to select all rows in the current filtered result.
- Do not confuse `自定义指标与时间：无数据` with an empty material library. When material rows and a total count are visible, materials exist even if metric columns such as spend and conversions are `0`. Continue the material flow: adjust pagination to `100条/页`, then apply the confirmed selection rule; report metric-data absence separately.
- For account-specific landing pages, choose `按账户分配`, then click each account ID in the dialog's left-side account list. For the active account, search and check only its mapped landing page in the right-side table, verify the selected panel, and repeat for every account. Do not treat a filter dropdown as the account selector.
- Add images from the global bulk-add menu, not an individual creative group.
- Enter material selection through the empty-state `创意素材` section's visible `选择素材` text/link at the bottom of that panel. Do not infer the entry from a disabled card button or jump into an individual creative group.
- In the material-library directory selector, search `快应用` and select the first returned directory, currently `网三-快应用`. The retired `广点通-通信素材` folder must not be used. If the exact first result is absent, stop and report the directory mismatch.
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

In the creative action-button section, explicitly open the `按钮文案` dropdown and set it to `立即领取`; an enabled action-button switch or a saved creative form is not proof that the copy was selected. If the page raises `请选择行动按钮文案`, stop and complete this selection before saving.

## Browser Automation Rules

1. Wait for visible state changes after each selection, including selected counts, success notices, saved values, or enabled next-step controls.
2. Check product pages from the last page backward when the product is not immediately available.
3. Verify checkbox state and selected-count panels before confirming modals; search results alone are not a selection.
4. Treat shared-resource dialogs, material-risk dialogs, and preview generation as explicit checkpoints.
5. On mismatched account, product, brand image, landing page, permission, or allowlist state, stop and return an exception list instead of guessing.

After entering a product keyword, click the visible `刷新` button before inspecting results. Wait until the product rows and pagination settle, re-read the actual last-page number, then inspect every page backward from that number. Select only the product whose displayed name exactly matches the user-provided product name or approved target rule. A keyword match, a date containing the keyword, or the first displayed row is not sufficient. For example, when the target product name is the single character `1`, select only the row displayed exactly as `1`; do not select a date, package name, or any other product that merely contains `1`. If no exact unique match exists, stop and request the product name; never select an item shown under a stale page count.

## Executor-Derived State Gates

Treat each module as a transaction: record the expected state before acting, perform one scoped action, then prove the state changed before entering the next module. A click, an HTTP response, or a toast alone is not success.

| Module | Semantic DOM anchor | Required post-action proof | Retry safety |
| --- | --- | --- | --- |
| Account import | Visible `选择媒体账户` dialog; header checkbox in `th.el-table-column--selection` | Requested IDs and visible row IDs are exact-set equal; selected rows and selected count equal request count | Batch-search toggle and Enter-created rows are non-retry-safe |
| Product | Visible `选择推广产品` dialog; product name is the text of its `label.el-checkbox` excluding `查看详情` | Allocation is `全部相同`; warning IDs equal selected IDs; exact product is checked; dialog closes and `已选1个产品` appears | Refresh, pagination, and selection must be re-read before retry |
| Placements | `广告版位` section, then `智能版位` and `稳步探索` controls | Only approved top-level groups are selected: `微信公众号与小程序`、`腾讯平台与内容媒体`、`腾讯营销联盟`; excluded groups are unchecked or not rendered | Reopen/reconfigure if product selection changes this section |
| Conversion | `选择转化` panel and its `优化目标` input | Goal, API reporting, click attribution, allocation `全部相同`, one selected conversion, and visible `转化归因` are all present | Do not retry child-dialog confirmation until its closed/open state is known |
| Ad settings | New-ad drawer `保存` button | Drawer closes; main table shows a non-empty ad-information summary; targeting action is enabled | Never advance after click-only confirmation |
| Targeting | Main-table `定向模板` cell and its `添加` action | Exact template row is selected and main table reports selected count `1` | Search only when exact template is not already visible |
| Creative | Main-table `创意信息` cell and `编辑` action | Saved panel closes; custom brand jump, selected brand image, custom landing-page jump, and `立即领取` are visible in saved state | Scope duplicate `自定义` labels to their section |
| Materials | Empty-state `创意素材` panel, then global `批量添加` menu | Both library `提交` and outer-panel `确定` close their own containers; material-group count and per-group material limit match the requested state | Header select-all is non-retry-safe until checked state is inspected |
| Copy / landing page | Main-table `创意文案` / `落地页` cells | Copy panel and landing-page dialog both close; selected panels contain the requested two copy items and account mapping | Do not infer selection from a search result |

If a state gate fails, stop at that module. Do not compensate by clicking a later control, reopening another module, or carrying stale values forward.

## Mandatory Continuation Ledger

Do not treat a successfully saved module as a completed campaign. In particular, the `广告信息` state gate is the handoff to `定向模板`; it is never a terminal state.

Keep this ordered ledger for every run and mark a row complete only after its state gate passes:

1. Accounts imported and selected.
2. Marketing content, product, placement, conversion, bid, schedule, status, and ad name saved to the main-table `广告信息` summary.
3. Targeting template selected and the main table reports selected count `1`.
4. Creative information saved, including custom brand-image jump, brand image, custom landing-page jump, and `立即领取`.
5. Materials saved through both the library `提交` and outer material-panel `确定` actions.
6. Multi-copy testing, all-account copy reuse, and both approved copy items saved.
7. Landing-page mapping saved for every selected account.
8. Preview generated and validated for every selected account.

After any completed row, immediately begin the next incomplete row. The only valid pause points are: an explicit user pause/checkpoint, a failed state gate, a real account/resource exception, or the final-submit confirmation after preview. Never stop merely because a drawer closed, a main-table cell gained content, or a later `添加` / `编辑` control became enabled.

Before declaring a run ready for preview, inspect the main table from top to bottom and prove that every required row in items 2-7 is populated. A blank or zero-selected `定向模板` row blocks creative, material, copy, landing-page, and preview completion; return to that row rather than reporting partial completion as success.

## Drawer Reset and Reconciliation Rule

The Chuangliang new-ad drawer can reset dependent fields when `营销目的`, `推广产品`, or product allocation changes, and it can show a fresh default when reopened. Treat any such action as invalidating all later fields in the drawer.

1. After changing `营销目的`, `推广产品`, marketing carrier, product allocation, or selected product, re-read: marketing purpose, promotion product, selected product count/name, placement mode/strategy, conversion state, bid mode/range, budget, schedule, status, and ad name.
2. If any previously configured downstream field has reverted, restart configuration from the earliest invalidated section in top-to-bottom order. Do not patch only the visibly wrong field.
3. Before saving, verify the exact approved configuration, including `运营商产品`, product name, `智能版位` + `稳步探索`, the three approved placement groups, random bid `110-130` when those are the supplied inputs, budget `200` when supplied, long-term delivery, unlimited time slot, enabled ad status, and the generated name prefix.
4. After saving, prove the drawer closed and the main-table advertisement cell is populated. If it did not close, treat the save as failed even when the button was clickable or a success message appeared.

## Dynamic Goal and Conversion Rule

Use `表单预约` only when it is available under the current account set and is the approved input. Do not assume that `更多目标` exists: open the goal control, inspect its visible choices, use `更多目标` only when the control is actually present, and otherwise choose the only visible approved option.

After the goal is applied, inspect the returned conversion rows. Select the sole eligible row that corresponds to the selected goal, not a literal conversion name. If zero or more than one eligible row remains, stop and report the names. Apply `全部相同` for conversion allocation for both one-account and multi-account runs unless the user explicitly supplies a per-account rule.

## Multi-Account Import DOM Contract

For two or more account IDs, do not paste newline-separated IDs into the ordinary keyword field. In the "选择媒体账户" dialog:

1. Scope every locator to the dialog containing `选择媒体账户`.
2. Locate the ordinary keyword input (`请输入关键词`) and its batch-search control. Prefer `.cl-search-input__batch .el-popover__reference[title="批量搜索"]`; otherwise use the control labelled `批量搜索` that is immediately adjacent to that input. Do not select an icon merely because it looks like a search icon.
3. Click the batch control exactly once. It is a toggle and is not retry-safe.
4. Continue only after the batch popover exposes both `请输入账户ID` and the multiline-entry input `请粘贴或输入账户ID，回车可换行` plus its `搜索` button. If this contract is absent, stop and report the selector mismatch.
5. Enter one numeric ID per line and run the popover's search. Verify the result rows are an exact set match for the requested IDs before any checkbox is selected. Report missing, duplicate, or unexpected IDs as exceptions.
6. When the verified result set is complete, use the table-header checkbox once to select all returned rows; do not manually select accounts one by one. First ensure the header checkbox is not already selected, then verify the selected count equals the verified result count before clicking `确定`.

Use screenshots only to diagnose a selector mismatch. Do not use coordinates as the primary path for this control.

## Control-Center Extension Contract

When this skill is executed through the bundled Chrome extension, treat the extension UI and the platform page as two separate DOMs:

- The extension form is `#jobForm`; inputs are `#accountIds`, `#productKeyword`, `#brandKeyword`, `#targetingTemplate`, `#materialFolder`, `#materialDateStart`, `#materialDateEnd`, `#copyOne`, `#copyTwo`, `#landingPages`, `#bidMin`, `#bidMax`, and `#budget`.
- Account IDs are parsed as whitespace-separated values, but the visible contract is one numeric ID per line. Reject duplicates, non-numeric IDs, and an `accountCount` that does not equal the parsed count before connecting to Chrome.
- Landing-page mappings are parsed as one `accountId | landing-page name` per line. Every requested account must have exactly one mapping; reject unknown account IDs or missing names before page automation starts.
- Material conditions are structured rows (`.material-condition-row`) with `.condition-metric`, `.condition-operator`, and `.condition-value`; do not rely on parsing a free-form string in the page agent. The default approved pair is `花费 >= 1` and `目标转化量 >= 1`.
- The extension communicates with the worker through `RUN_TO_PREVIEW`, `STOP`, `LOG`, `STATUS`, and `FINISHED`. `RUN_TO_PREVIEW` must stop at preview; never add a worker path that clicks the final submit button.

For the platform page agent, scope selectors to the smallest visible semantic container instead of global text or DOM order. The implementation's reliable primitives are:

1. `visible(element)` (computed style plus non-zero bounding box) before every interaction.
2. `waitForAction` / `stabilize` after each click, input, dropdown change, pagination change, or modal confirmation; success means a visible state change such as selected count, checked state, closed dialog, refreshed rows, or an enabled button.
3. `evaluate` for DOM inspection and state checks; use browser-level mouse/key events only as a fallback when a native Vue/Element click is blocked by an overlay.
4. Never retry non-idempotent actions (`批量搜索` toggle, multiline account entry, Enter-created account rows, or a global select-all click) without first proving the prior state is absent.

Known implementation checks:

- The extension currently ships `#materialFolder` with a legacy default in `app.html`; the approved runtime directory is `快应用` and the first exact result `网三-快应用`. The UI default must be corrected or the run must reject the legacy value before page execution.
- The conversion executor must not require the literal name `优投-表单预约`. Only the selected optimization goal is stable; after `表单预约` is applied, select and verify the sole available conversion option returned for the current account set. Any selector or validation that hard-codes a conversion name is a defect.
- The targeting step must use the visible search input and exact-row selection, but may select the template directly only when the exact approved template is already visible. Search results alone are not proof of selection.
- For material selection, the page agent must enter through the empty-state `创意素材` → `选择素材` action, then use the global `批量添加` menu. The outer material panel's `确定` is a second required confirmation after the library's `提交` closes.

When adding or changing selectors, record the semantic anchor, the expected DOM shape, the post-action signal, and whether the action is retry-safe. A screenshot or coordinate is diagnostic evidence only, never the selector contract.

## Shared-Product Warning Gate

For both single-account and multi-account product selection, use `全部相同` by default. This selection opens the platform's business-unit warning dialog. Verify that the listed IDs are the selected IDs, then treat it as the normal confirmation path and click `确定` by default. Stop only if the platform reports an actual error after confirmation; do not change to per-account allocation merely because this warning appeared.

## Completion Criteria

Report the following before asking for final submission approval:

- Selected accounts and count.
- Product, placement, conversion, bid, targeting, brand image, materials, copy, and landing page validation results.
- Preview status and any skipped or exceptional accounts.
- Exact external action that final submission will create.
