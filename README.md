# ikuai-firmware-public · 爱快多机型固件公开归档
# iKuai Multi-Model Firmware Public Archive

本仓库是爱快（iKuai）**多机型官方固件的公开归档镜像**，由私有归档仓库
[`ikuai-firmware-archive`](https://github.com/shiniuniu/ikuai-firmware-archive)
通过 GitHub Actions 自动同步发布而来。

This repository is a **public archive mirror of official iKuai firmware for multiple device models**,
automatically synced from the private archive repo
[`ikuai-firmware-archive`](https://github.com/shiniuniu/ikuai-firmware-archive) via GitHub Actions.

> 固件本体均取自爱快官方公开升级通道，按机型归档到 GitHub Releases，供研究、备份与自用下载。
> Firmware files are obtained from iKuai's official public upgrade channel, archived to GitHub Releases by model,
> for research, backup, and personal use.

---

## 目录 / Table of Contents

- [Release 说明 · Releases](#release-说明--releases)
- [机型与版本覆盖 · Models & Versions](#机型与版本覆盖--models--versions)
- [资产说明 · Assets](#资产说明--assets)
- [同步机制 · Sync Mechanism](#同步机制--sync-mechanism)
- [使用方法 · Usage](#使用方法--usage)
- [自愿捐助 · Voluntary Donation](#自愿捐助--voluntary-donation)
- [免责声明 · Disclaimer](#免责声明--disclaimer)
- [许可 · License](#许可--license)

---

## Release 说明 · Releases

每个 Release 对应一次归档快照，**一次 Release 含多机型固件**。
Each Release is an archive snapshot containing firmware for **multiple models in one Release**.

| 字段 / Field | 说明 / Description |
|---|---|
| Tag | `archive-YYYYMMDD`（归档日期 / archive date） |
| 标题 / Title | `爱快多机型固件归档 <机型列表> (<tag>)` |
| 备注 / Body | 本次归档新增固件的 **MD5 清单**（机型 / 固件文件 / MD5 / 版本） |
| 资产 / Assets | 各机型固件 `.bin` + 校验文件（见下 / see below） |

可手动触发同步（`sync-release-to-public` workflow 的 `workflow_dispatch`），
archive 仓库发布新 Release 时也会自动同步到此仓库。
Sync can be triggered manually (`workflow_dispatch` of the `sync-release-to-public` workflow),
and runs automatically whenever the archive repo publishes a new Release.

## 机型与版本覆盖 · Models & Versions

当前覆盖 **43 个机型**，横跨 **3.7.x 与 4.0.x** 两大版本线。
Currently covers **43 models** across the **3.7.x and 4.0.x** version lines.

- **A 系列（企业路由 / Enterprise）**：A100-P、A120、A125、A130、A135S、A139S、A160、A220PRO、A50、A50-P、A50X-P
- **C 系列**：C20、C25-G、C3000、C50、C90
- **G 系列**：G05
- **M 系列**：M08、M1、M100、M10S、M2、M200、M360X、M5、M50、M5S、M60、M60X
- **Q 系列（Wi-Fi 6/7）**：Q1800、Q1800L、Q3000、Q3600、Q3S、Q50、Q6000、Q80、Q85、Q90
- **X86**：X86_x32、X86_x64
- **Y 系列**：Y3000G-PRO、Y6000G-PRO

版本线 / Version lines：`3.7.20` / `3.7.21` / `3.7.22` / `3.7.23` / `3.7.26`、`4.0.303` / `4.0.313` 等（随归档持续更新 / updated continuously）。

## 资产说明 · Assets

每个固件配套发布三类资产。Each firmware ships with three assets.

| 后缀 / Suffix | 含义 / Meaning |
|---|---|
| `*.bin` | 官方 sysupgrade 固件包 / official sysupgrade firmware (e.g. `IK-MT7981V1-Q3000_sysupgrade_4.0.313_Build202609281455.bin`) |
| `*.manifest.json` | 固件元数据清单（机型 / 版本 / 平台 / 构建时间等）/ metadata manifest |
| `*.sha256.txt` | 固件 SHA-256 校验值 / SHA-256 checksum |

固件命名遵循爱快规范：`IK-<平台>-<机型>_sysupgrade_<版本>_Build<时间戳>.bin`.
Firmware naming follows iKuai convention: `IK-<platform>-<model>_sysupgrade_<version>_Build<timestamp>.bin`.

## 同步机制 · Sync Mechanism

由 archive 仓库的 [`.github/workflows/sync-release-public.yml`]
(https://github.com/shiniuniu/ikuai-firmware-archive/blob/main/.github/workflows/sync-release-public.yml)
驱动 / Driven by the workflow in the archive repo:

1. 遍历 archive 仓库全部 Release / enumerate all Releases in the archive repo;
2. 保留标题与备注（含 MD5 清单），下载全部资产 / keep title & body (with MD5 list), download all assets;
3. 在公开仓库创建同名 Tag 的 Release 并上传资产 / create the Release with the same Tag in the public repo and upload assets;
4. 幂等处理：目标已存在的 Tag 自动跳过，可重复运行 / idempotent: existing tags are skipped, safe to re-run.

## 使用方法 · Usage

在 [Releases](https://github.com/shiniuniu/ikuai-firmware-public/releases) 页面
选择需要的归档快照，下载对应机型的 `.bin` 固件及校验文件即可。
Pick a snapshot on the [Releases](https://github.com/shiniuniu/ikuai-firmware-public/releases) page
and download the `.bin` firmware and checksum files for your model.

```bash
# 下载某 release 的资产 / Download assets of a release
gh release download archive-20261001 \
  -R shiniuniu/ikuai-firmware-public
```

下载后建议用 `.sha256.txt` 校验完整性 / Verify integrity with `.sha256.txt`:

```bash
sha256sum -c <机型-版本>.sha256.txt
```

---

## 自愿捐助 · Voluntary Donation

如果这个项目对你有帮助，欢迎自愿捐助支持维护。所有捐助均为**自愿**，绝不强制，感谢你的支持！
If this project is helpful to you, voluntary donations are welcome to support maintenance.
All donations are **voluntary** — never required. Thank you for your support!

**TRON (TRC-20 / USDT 等)**：`TUdDk8spLGGLUiEPJ1bLCB85isF7hgf117`

**Solana (SOL/SPL)**：`5TMsi7uryC5GCWEvZXBLgQLvJR3MoqdYT4RQt2CxGbCL`

> 请核对地址后转账，链上转账不可逆，本仓库不对误操作负责。
> Please double-check the address before transferring. On-chain transfers are irreversible;
> this repository is not responsible for mistaken transactions.

---

## 免责声明 · Disclaimer

- 固件文件均来自爱快官方公开升级通道，仅作**归档、研究与自用备份**用途；
  Firmware files come from iKuai's official public upgrade channel, for **archival, research, and personal backup** only;
- 本仓库与爱快官方无任何从属、授权或合作关系；
  This repository has no affiliation, authorization, or cooperation with iKuai;
- 升级/降级设备存在风险（可能失去保修、损坏系统或触发降级限制），操作前请自行评估并备份；
  Upgrading/downgrading a device carries risk (warranty loss, system damage, or downgrade restrictions) — assess and back up first;
- 使用本仓库内容产生的任何后果，由使用者自行承担。
  Any consequences of using this repository's content are borne by the user.

## 许可 · License

除固件文件本身（版权归原厂商所有）外，本仓库脚本与文档采用
Except for the firmware files themselves (copyright owned by their original vendors),
the scripts and documents in this repository are licensed under the
[MIT License](LICENSE).
