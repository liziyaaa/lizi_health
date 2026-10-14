# lizi Health Daily

把小米手环 8 已同步到 Apple 健康的睡眠、心率和步数，整理为 JSON 健康日报，保存到 iCloud Drive，并通过 iPhone 自带「邮件」发送到自己指定的邮箱。

**当前状态：设计资料版（`1.0-design`）。本仓库没有已编译或已签名的 `.shortcut` 文件，也没有一键安装链接。** 资料可用于参考、手动搭建或继续开发；不能直接导入快捷指令 App。尚未进行 iPhone 实机验证。

## 为什么做这个项目

希望在 Windows + iPhone 的设备组合下，优先使用免费工具，减少每天导出和上传健康数据的手动操作。日报文件也可提供给 ChatGPT 分析，不依赖 ChatGPT Health 的地区开放状态。

## 数据流程

小米手环 8 → Mi Fitness → Apple 健康 → iOS 快捷指令 → JSON → iCloud Drive / Apple Mail。

Outlook 在这里是邮箱账户；计划使用 **Apple Mail 的发信动作**，并不依赖 Outlook App 的专用动作。邮件到达后不会自动进入某个 ChatGPT 对话，也不会自动触发 AI 分析。

## 完成情况

| 内容 | 状态 |
| --- | --- |
| 原始设计包、流程说明、配置资料 | 已提供 |
| JSON 数据结构和虚构示例 | 已提供，属于拟定的数据契约 |
| 首次运行、自动化与验收说明 | 已提供，待实机验证 |
| Apple 健康数据采集动作 | 待实现 |
| 快捷指令编译、签名和一键导入 | 未完成 |
| iCloud 保存、邮件发送、锁屏运行 | 未验证 |

## 环境要求

- iPhone，已安装「快捷指令」和「健康」。当前没有实测支持的最低 iOS 版本。
- 小米手环 8 与 Mi Fitness 已配对；Apple 健康中能查看来自 Mi Fitness 的相关记录。
- iCloud Drive 可用，用于保存日报。
- 如需发信，先在 iPhone 自带「邮件」App 登录自己的邮箱账户。首次运行时确认收件人。
- Windows 电脑用于编辑资料或后续开发，不负责读取 iPhone 上的健康数据。

## 获取项目

```powershell
git clone https://github.com/liziyaaa/lizi_health.git
cd lizi_health
```

没有 Git 时，也可在 GitHub 仓库页面选择 **Code → Download ZIP**。私有仓库需要获得访问权限。下载的是设计资料，不是安装包。

## 从哪里开始

1. 在 Apple 健康中检查睡眠、心率、步数各自的数据源；只有睡眠已同步，不能证明另外两项也已同步。
2. 阅读 [配置与数据规范](docs/DESIGN.md)，确认日期归属和来源筛选方式。
3. 按 [搭建与运行说明](docs/SETUP.md) 逐段配置，先读取一类健康样本并预览输出，再加入文件保存和邮件。
4. 使用 [验收清单](docs/VALIDATION.md) 对照 Apple 健康验证结果。
5. 想继续开发一键导入版本，参考 [开发任务](docs/DEVELOPMENT.md)。

## 日报格式

见 [完整虚构示例](examples/health_example.json)。下面仅展示主要字段：

```json
{
  "schema_version": "1.0",
  "is_example": true,
  "report_date": "2026-10-14",
  "generated_at": "2026-10-15T09:00:00+08:00",
  "timezone": "Asia/Shanghai",
  "sleep": [],
  "heart_rate": [],
  "steps": []
}
```

正式日报使用 `is_example: false`，每条样本保留起止时间、原始值或阶段、数据来源。示例文件内的数据都是虚构的。空数组表示未导出样本，不能解释为「睡眠为零」或「心率为零」。

## 使用时需要注意

- **同步先于导出。** 快捷指令不能替代 Mi Fitness 与手环的同步；数据是否最新需要核对。
- **睡眠跨午夜。** 拟定睡眠窗口是前一天 18:00 至当天 12:00；实际读取截止时间不能晚于运行时间。早上运行时日报可能仍不完整。
- **步数不能无脑相加。** iPhone 和手环可能重复记录同一活动。V1 保存样本与来源，不声称得到 Apple 健康已去重的总步数。
- **首次发信要预览。** 建议先保持邮件编写表单开启，确认地址和附件后，再测试关闭表单及每天自动运行。
- **定时触发不等于保证锁屏成功。** Apple 支持部分个人自动化不逐次询问，但健康读取、保存、邮件等动作仍需在实际设备上验证。

## 目录

| 路径 | 用途 |
| --- | --- |
| `config.example.json` | 拟定配置，不会被快捷指令自动加载 |
| `design/lizi_health_design.zip` | 原始设计包，保留原始字节 |
| `design/original/` | 原始包解压内容，保留历史配置 |
| `examples/health_example.json` | 虚构的结构化日报示例 |
| `docs/` | 设计、搭建、验收和后续开发说明 |
| `.gitignore` | 排除常见健康记录、个人配置和密钥文件 |

## 隐私与协作

不要把自己的健康日报、Apple 健康完整导出、真实邮箱地址、账户凭据或 API 密钥提交到仓库。`.gitignore` 只是辅助，提交前仍需检查内容。问题反馈请使用虚构样本或经过脱敏的动作截图。

本仓库尚未指定开源许可证；如需再分发，请先联系维护者确认授权。

## 参考资料

- [Apple：个人自动化介绍](https://support.apple.com/guide/shortcuts/apd690170742/ios)
- [Apple：启用或停用个人自动化](https://support.apple.com/guide/shortcuts/apd602971e63/ios)
- [Apple：在 Mac 上从命令行运行与签名快捷指令](https://support.apple.com/guide/shortcuts-mac/apd455c82f02/mac)
- [Wuvomi/shortcut-cli](https://github.com/Wuvomi/shortcut-cli)：候选生成工具，尚未在本项目采用或验证。
- [wynx1123/shortcut-signer](https://github.com/wynx1123/shortcut-signer)：候选签名工具，不保证第三方服务当前可用或免费。

以上工具链接是后续调研入口，不代表本项目已有构建依赖或可用签名流程。
