# ikuai-firmware-public · 爱快多机型固件公开归档

本仓库是爱快（iKuai）**多机型官方固件的公开归档镜像**，由私有归档仓库
[`ikuai-firmware-archive`](https://github.com/shiniuniu/ikuai-firmware-archive)
通过 GitHub Actions 自动同步发布而来（见下方「同步机制」）。

> 固件本体均取自爱快官方公开升级通道，按机型归档到 GitHub Releases，供研究、备份与自用下载。

---

## 目录

- [Release 说明](#release-说明)
- [机型与版本覆盖](#机型与版本覆盖)
- [资产说明](#资产说明)
- [同步机制](#同步机制)
- [使用方法](#使用方法)
- [免责声明](#免责声明)
- [许可](#许可)

---

## Release 说明

每个 Release 对应一次归档快照，**一次 Release 含多机型固件**：

| 字段 | 说明 |
|---|---|
| Tag | `archive-YYYYMMDD`（归档日期） |
| 标题 | `爱快多机型固件归档 <机型列表> (<tag>)` |
| 备注 | 本次归档新增固件的 **MD5 清单**（机型 / 固件文件 / MD5 / 版本） |
| 资产 | 各机型固件 `.bin` + 校验文件（见下） |

可手动触发同步（`sync-release-to-public` workflow 的 `workflow_dispatch`），
archive 仓库发布新 Release 时也会自动同步到此仓库。

## 机型与版本覆盖

当前覆盖 **43 个机型**，横跨 **3.7.x 与 4.0.x** 两大版本线：

- **A 系列（企业路由）**：A100-P、A120、A125、A130、A135S、A139S、A160、A220PRO、A50、A50-P、A50X-P
- **C 系列**：C20、C25-G、C3000、C50、C90
- **G 系列**：G05
- **M 系列**：M08、M1、M100、M10S、M2、M200、M360X、M5、M50、M5S、M60、M60X
- **Q 系列（Wi-Fi 6/7）**：Q1800、Q1800L、Q3000、Q3600、Q3S、Q50、Q6000、Q80、Q85、Q90
- **X86**：X86_x32、X86_x64
- **Y 系列**：Y3000G-PRO、Y6000G-PRO

版本线：`3.7.20` / `3.7.21` / `3.7.22` / `3.7.23` / `3.7.26`、`4.0.303` / `4.0.313` 等（随归档持续更新）。

## 资产说明

每个固件配套发布三类资产：

| 后缀 | 含义 |
|---|---|
| `*.bin` | 官方 sysupgrade 固件包（如 `IK-MT7981V1-Q3000_sysupgrade_4.0.313_Build202609281455.bin`） |
| `*.manifest.json` | 该固件的元数据清单（机型 / 版本 / 平台 / 构建时间等） |
| `*.sha256.txt` | 固件 SHA-256 校验值 |

固件命名遵循爱快规范：`IK-<平台>-<机型>_sysupgrade_<版本>_Build<时间戳>.bin`。

## 同步机制

由 archive 仓库的 [`.github/workflows/sync-release-public.yml`]
(https://github.com/shiniuniu/ikuai-firmware-archive/blob/main/.github/workflows/sync-release-public.yml)
驱动：

1. 遍历 archive 仓库全部 Release；
2. 保留标题与备注（含 MD5 清单），下载全部资产；
3. 在公开仓库创建同名 Tag 的 Release 并上传资产；
4. 幂等处理：目标已存在的 Tag 自动跳过，可重复运行。

## 使用方法

在 [Releases](https://github.com/shiniuniu/ikuai-firmware-public/releases) 页面
选择需要的归档快照，下载对应机型的 `.bin` 固件及校验文件即可。

```bash
# 示例：下载某 release 的资产
gh release download archive-20261001 \
  -R shiniuniu/ikuai-firmware-public
```

下载后建议用 `.sha256.txt` 校验完整性：

```bash
sha256sum -c <机型-版本>.sha256.txt
```

## 免责声明

- 固件文件均来自爱快官方公开升级通道，仅作**归档、研究与自用备份**用途；
- 本仓库与爱快官方无任何从属、授权或合作关系；
- 升级/降级设备存在风险（可能失去保修、损坏系统或触发降级限制），操作前请自行评估并备份；
- 使用本仓库内容产生的任何后果，由使用者自行承担。

## 许可

除固件文件本身（版权归原厂商所有）外，本仓库脚本与文档采用
[MIT License](LICENSE)。
