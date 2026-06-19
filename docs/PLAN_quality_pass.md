# 项目质量提升 & 文档化 —— 执行 Plan / Spec

> 分支：`claude/project-quality-and-docs`（基于 `main`，当前 v1.5.5 / versionCode 22）
> 本文件是 `/loop` 任务的总纲与进度看板。每个阶段结束后做一次提交，并在此勾选。

## 0. 总目标（来自用户）

1. **逐文件审阅**：每个文件一行一行看过去，结合上下文找更优解，使「所有代码都是最佳实践、整个项目是最佳架构」。范围含 Kotlin 源码、资源、Gradle 脚本、CI 脚本。
2. **测试覆盖**：关键流程全部覆盖单元测试 + 冒烟测试。
3. **Design.md**：网络搜索 Design.md 的标准规范，以本项目为基准产出一份 `docs/Design.md`。
4. **CLAUDE.md**：根目录建一份，记录新对话/新 Agent 需要知道的一切。
5. **功能完善**：**涉及功能但未完善的要补完**；其余 UI 与功能一律**零影响优化**（纯重构，不改变行为）。
6. **流程纪律**：用 subagent / workflow 交叉调研，最后对照代码人工审核；每个关键流程结束即提交；全部在新分支完成；先有 Plan 再执行；上下文不足时在自动压缩前把进度写回本文件。

## 1. 「零影响优化」的硬约束（贯穿全程）

- 不改变任何用户可见行为、不改数据库 schema（除非是「补完未完成功能」且明确记录）。
- 重构类改动必须有测试或可推理证明等价（命名、抽取函数、消重、去死代码、可空性收紧等）。
- 区分两类 commit：`refactor:`（零影响）与 `feat:`/`fix:`（功能完善，需在 CHANGELOG 记录并考虑版本号）。

## 2. 阶段拆解与进度

- [x] **P0 启动**：建分支、写本 Plan、写初版 CLAUDE.md。（commit）
- [ ] **P1 架构勘察**：逐目录读 `data/`（Room 实体/DAO/Repository/backup）、`ui/`（screen/component/theme/viewmodel）、入口（Application/Activity/Container）。产出「现状架构图 + 问题清单（按零影响/需完善分类）」写入本文件附录。（commit：docs）
- [ ] **P2 数据层优化**：实体/DAO/Repository 的最佳实践与一致性；可空性、协程/Flow、事务边界、命名。（commit）
- [ ] **P3 UI/状态层优化**：ViewModel 状态建模（UiState）、Compose 重组/稳定性、副作用、主题与 i18n 一致性。（commit）
- [ ] **P4 功能完善**：把 P1 标记为「未完善」的功能补齐（逐项在本文件登记 + CHANGELOG）。（commit）
- [ ] **P5 测试**：补齐关键流程单测（DAO CRUD、Repository、年费续期/有效期日期逻辑、备份导入导出、ViewModel 状态）+ 冒烟测试（应用启动、关键 Composable 渲染）。目标：关键路径全覆盖。（commit）
- [ ] **P6 脚本 & CI**：审 `build.gradle.kts`/`gradle.properties`/`build-apk.yml`/wrapper 现状，给出最佳实践改进（注意沙箱代理与假 gradlew 的权衡，见 CLAUDE.md）。（commit）
- [ ] **P7 Design.md**：网络搜索业界 Design Doc 规范（Google design doc、arc42、C4、ADR 等），结合本项目产出 `docs/Design.md`。（commit）
- [ ] **P8 交叉调研 & 终审**：用 subagent 对关键模块交叉 review，我对照代码逐条核对、收口；更新 CLAUDE.md / CHANGELOG。（commit）

## 3. 工作方式

- 每个阶段：先读 → 列改动清单（标注「零影响 / 需完善」）→ 改 → ktlint/detekt/编译/测试自检 → commit。
- 关键模块用 `Explore` / `general-purpose` subagent 交叉调研，结论回到我这里对照源码核对，再落地。
- 构建/测试自检命令见 CLAUDE.md（沙箱用系统 gradle，需补 SDK 元数据；或依赖 CI）。

## 附录 A：问题清单（P1 填充，持续更新）

_待 P1 勘察后写入。每条形如：`[零影响|需完善] 文件:行 — 问题 — 建议`_

## 附录 B：进度记忆（自动压缩前更新，供下次续作）

- 当前阶段：**P0 完成，准备进入 P1**。
- 已提交：P0（Plan + CLAUDE.md）。
- 下一步：P1 逐目录勘察 `data/` 与 `ui/`，把问题清单写入附录 A。
