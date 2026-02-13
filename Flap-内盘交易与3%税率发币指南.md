# Flap 内盘交易与 3% 税率发币指南

> 基于 [Flap 官方文档](https://docs.flap.sh/flap) 整理，涵盖：文档子链接索引、内盘买卖交易实现、创建 3% 税率代币的调用方法与参数设置。

---

## 一、Flap 文档子链接索引

### 1.1 概览与用户向

| 链接 | 说明 |
|------|------|
| [Brief Overview of Flap](https://docs.flap.sh/flap) | Flap 简介、支持钱包、税币与金库概览 |
| [Flap Official Links](https://docs.flap.sh/flap/flap-official-links) | 官方链接汇总 |
| [Flap's Vision](https://docs.flap.sh/flap/flaps-vision) | 愿景 |
| [Flap's Features](https://docs.flap.sh/flap/flaps-features) | 功能特性 |
| [Why Launch on Flap](https://docs.flap.sh/flap/why-launch-on-flap) | 为何在 Flap 发币 |
| [How to Create Your Own Coin on FLAP](https://docs.flap.sh/flap/how-to-create-your-own-coin-on-flap) | 网页端创建代币步骤 |
| [Bonding Curve](https://docs.flap.sh/flap/bonding-curve) | 绑定曲线概念与公式 |
| [List On DEX](https://docs.flap.sh/flap/list-on-dex) | 上架 DEX 机制 |
| [FAQ](https://docs.flap.sh/flap/faq) | 常见问题 |
| [Audit Reports](https://docs.flap.sh/flap/audit-reports) | 审计报告 |
| [Media Kit](https://docs.flap.sh/flap/media-kit) | 媒体资源 |

### 1.2 Developers 总览

| 链接 | 说明 |
|------|------|
| [Developers](https://docs.flap.sh/flap/developers) | 开发者入口与角色划分 |
| [Basic & Mechanism](https://docs.flap.sh/flap/developers/basic-and-mechanism) | 基础机制（曲线、迁移、Portal/VaultPortal、税币） |
| [Bonding Curve（机制）](https://docs.flap.sh/flap/developers/basic-and-mechanism/bonding-curve) | 曲线公式、多链参数、getTokenV5 |
| [Migrated To DEX](https://docs.flap.sh/flap/developers/basic-and-mechanism/list-on-dex) | 迁移至 DEX 说明 |
| [Portal vs VaultPortal](https://docs.flap.sh/flap/developers/basic-and-mechanism/portal-vs-vaultportal) | Portal 与 VaultPortal 区别 |
| [Flap Tax Token](https://docs.flap.sh/flap/developers/basic-and-mechanism/flap-tax-token) | 税币（PreBond Tax、V1、V2） |

### 1.3 Wallet / Terminal / Bot 开发者（交易与索引）

| 链接 | 说明 |
|------|------|
| [Wallet & Terminal & Bot Developers](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers) | 钱包/终端/机器人开发入口 |
| [A Quick Start](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/a-quick-start-for-wallet-terminal-bot-developers) | 快速开始 |
| [Deployed Contract Addresses](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/deployed-contract-addresses) | **Portal 等合约地址（交易用）** |
| [Index Token Created Events](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/index-token-created-events) | 代币创建事件索引 |
| [Bonding Curve In Developers' Perspective](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/bonding-curve-in-developers-perspective) | 开发者视角的曲线 |
| [Inspect A Token](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/inspect-a-token) | 查询代币（getTokenV6/V7） |
| [Parse Token Meta](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/parse-token-meta) | 解析代币元数据 |
| [Token Version Specification](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/token-version-specification) | 代币版本规范 |
| [Inspect A Tax Token](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/inspect-a-tax-token) | 查询税币 |
| [**Trade Tokens**](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/trade-tokens) | **内盘买卖：报价与 swap** |
| [Token Migration](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/token-migration) | 代币迁移 |
| [Example Indexer](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/example-code) | 示例索引器 |

### 1.4 Token Launcher 开发者（发币）

| 链接 | 说明 |
|------|------|
| [Token Launcher Developers](https://docs.flap.sh/flap/developers/token-launcher-developers) | 发币开发入口 |
| [Quick start for token launcher](https://docs.flap.sh/flap/developers/token-launcher-developers/quick-start-token-launcher-developers) | 发币快速开始 |
| [Deployed Contract Addresses (Launcher)](https://docs.flap.sh/flap/developers/token-launcher-developers/deployed-contract-addresses) | 各链 Portal / 实现合约地址 |
| [**Launch token through Portal**](https://docs.flap.sh/flap/developers/token-launcher-developers/launch-token-through-portal) | **通过 Portal 发币（含税币 newTokenV5）** |
| [**Launch token through VaultPortal**](https://docs.flap.sh/flap/developers/token-launcher-developers/launch-token-through-vaultportal) | **通过 VaultPortal 发带金库税币** |
| [Registered vaults](https://docs.flap.sh/flap/developers/token-launcher-developers/registered-vaults) | 已注册金库工厂 |

### 1.5 Vault 开发者

| 链接 | 说明 |
|------|------|
| [Vault Developers](https://docs.flap.sh/flap/developers/vault-developers) | 金库开发入口 |
| [Flap Tax Vault](https://docs.flap.sh/flap/developers/vault-developers/flap-tax-vault) | 金库规范、VaultPortal、VaultFactory、税率与手续费 |

---

## 二、在 Flap 内盘实现买卖交易

Flap 的「内盘」即**绑定曲线阶段**：在代币迁移到 DEX 之前，买卖都通过 **Portal 合约** 与绑定曲线完成。买时向曲线注入报价资产（如 BNB）获得代币，卖时向曲线返还代币获得报价资产。

### 2.1 合约与入口

- **交易统一入口**：各链的 **Portal** 合约（买卖都调用 Portal，不是直接调代币或曲线）。
- **BNB Chain 主网 Portal**：`0xe2cE6ab80874Fa9Fa2aAE65D277Dd6B8e65C9De0`
- **BNB 测试网 Portal**：`0x5bEacaF7ABCbB3aB280e80D007FD31fcE26510e9`

> 更多链与版本见：[Deployed Contract Addresses](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/deployed-contract-addresses)。

### 2.2 报价：quoteExactInput

先通过 **`quoteExactInput`** 得到「用多少输入能得到多少输出」（可用 `eth_call` 模拟，无需发交易）。

```solidity
struct QuoteExactInputParams {
    address inputToken;   // 输入代币地址，原生资产用 address(0)
    address outputToken; // 输出代币地址，原生资产用 address(0)
    uint256 inputAmount; // 输入数量（按该代币 decimals）
}

function quoteExactInput(QuoteExactInputParams calldata params) external returns (uint256 outputAmount);
```

**注意**：`quoteExactInput` 不是 view，但可用 RPC 的 `eth_call` 或 viem 的 simulate 调用，无需发送交易。

**常见场景（报价资产为 BNB/ETH 时）**：

| 场景 | inputToken | outputToken |
|------|------------|-------------|
| 用 BNB 买代币 | `address(0)` | 要买的代币地址 |
| 卖代币换 BNB | 要卖的代币地址 | `address(0)` |

**报价资产为 ERC20（如 USD1）时**：

| 场景 | inputToken | outputToken |
|------|------------|-------------|
| 用 USD1 买代币 | USD1 地址 | 代币地址 |
| 卖代币换 USD1 | 代币地址 | USD1 地址 |
| 用 BNB 买代币（需支持 nativeToQuoteSwap） | `address(0)` | 代币地址（合约内会先把 BNB 换成 USD1） |

### 2.3 执行交易：swapExactInput

拿到报价后，用 **`swapExactInput`** 执行实际买卖，并附带「最少收到数量」以防滑点。

```solidity
struct ExactInputParams {
    address inputToken;    // 输入代币，原生用 address(0)
    address outputToken;   // 输出代币，原生用 address(0)
    uint256 inputAmount;   // 输入数量
    uint256 minOutputAmount; // 最少获得的输出数量（滑点保护）
    bytes permitData;      // 可选，ERC20 permit，卖代币时可省一次 approve
}

function swapExactInput(ExactInputParams calldata params) external payable returns (uint256 outputAmount);
```

**调用要点**：

- **买入（用 BNB 买代币）**：  
  `inputToken = address(0)`，`outputToken = 代币地址`，`inputAmount` 为要花费的 BNB（wei），并 **`msg.value = inputAmount`**，`minOutputAmount` 建议设为 `quoteExactInput` 结果乘以 (1 - 滑点)。
- **卖出（卖代币换 BNB）**：  
  先对 Portal 做 `token.approve(Portal, amount)`，或使用 `permitData` 免 approve；  
  `inputToken = 代币地址`，`outputToken = address(0)`，`inputAmount = 卖出数量`，`msg.value = 0`，`minOutputAmount` 为最少接受的 BNB。

**permitData 构造**：  
文档建议将 EIP-712 的 Permit 各字段（owner, spender, value, nonce, deadline）及 v, r, s 按 ABI 编码后传入。详见 [Trade Tokens - How to construct the permitData](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/trade-tokens#how-to-construct-the-permitdata-in-exactinputparams)。

### 2.4 内盘交易事件

- **TokenBought**：内盘买入成功。
- **TokenSold**：内盘卖出成功。
- **FlapTokenProgressChanged**（v4.12.1+）：代币绑定曲线进度变化（newProgress 为 wad，1e18 = 100%）。

### 2.5 离线报价（可选）

若不想每次报价都调链上，可自建索引（监听 TokenCreated、TokenCurveSet/TokenCurveSetV2、TokenDexSupplyThreshSet、FlapTokenCirculatingSupplyChanged、LaunchedToDEX、FlapTokenTaxSet 等），用文档中的曲线库（TypeScript/Solidity）根据当前流通量和曲线参数计算：

- 买：由「投入的 BNB」算「得到的代币数量」；
- 卖：由「卖出的代币数量」算「得到的 BNB」。

税币需在协议费率（如 1%）基础上**加上税率**再参与计算（详见 [Trade Tokens - Quote for tax tokens](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/trade-tokens#off-chain-quote-4-quote-for-tax-tokens)）。

---

## 三、创建带有 3% 税率的代币：调用方法与参数

Flap 支持 **1%、3%、5%、10%** 税率。3% 即 **300 basis points**：`taxRate = 300`。

两种常用方式：

1. **仅税币、无金库**：用 **Portal.newTokenV5**。
2. **税币 + 金库**：用 **VaultPortal.newTaxTokenWithVault**。

下面分别说明**如何调用**以及**参数如何设置**（重点 3% 税率）。

### 3.1 通过 Portal 创建 3% 税率代币（newTokenV5）

**合约**：当前链的 Portal（BNB 主网：`0xe2cE6ab80874Fa9Fa2aAE65D277Dd6B8e65C9De0`）。

**方法**：

```solidity
function newTokenV5(NewTokenV5Params calldata params) external payable returns (address token);
```

**NewTokenV5Params 结构体（与 3% 税率相关及常用字段）**：

| 字段 | 类型 | 说明 | 3% 税率示例/建议 |
|------|------|------|------------------|
| name | string | 代币名称 | 如 "MyToken" |
| symbol | string | 符号 | 如 "MTK" |
| meta | string | 元数据 IPFS CID | 需先通过 [Flap 上传 API](https://funcs.flap.sh/api/upload) 上传图片与 JSON 并取得 CID |
| dexThresh | DexThreshType | DEX 供应量阈值类型 | 按文档枚举选 |
| salt | bytes32 | CREATE2 盐值 | 必须使生成的**税币地址末尾为 7777**（见下） |
| **taxRate** | **uint16** | **税率（bps）** | **3% = 300** |
| migratorType | MigratorType | 迁移器类型 | 税币用 **V2_MIGRATOR** |
| quoteToken | address | 报价资产 | BNB 用 **address(0)** |
| quoteAmt | uint256 | 初始注入的报价资产数量 | 如 0 或希望预注的 wei |
| beneficiary | address | 受益人 | 收市场费等地址 |
| permitData | bytes | 报价资产 permit | 原生资产可为 "" |
| extensionID | bytes32 | 扩展 ID | 无则 0 |
| extensionData | bytes | 扩展数据 | 无则 "" |
| dexId | DEXId | 目标 DEX | 按文档枚举 |
| lpFeeProfile | V3LPFeeProfile | V3 LP 费率 | 按文档枚举 |
| taxDuration | uint64 | 税率持续时间（秒） | 如 1 年或更长 |
| antiFarmerDuration | uint64 | 反撸持续时间 | 按需求设 |
| mktBps | uint16 | 市场分配 bps | 10000 = 100% 给 beneficiary；<10000 时通常用 Tax Token V2 实现 |
| deflationBps | uint16 | 通缩/销毁 bps | 0~10000 分配 |
| dividendBps | uint16 | 分红 bps | 0~10000 |
| lpBps | uint16 | LP bps | 0~10000 |
| minimumShareBalance | uint256 | 分红最低持仓 | 按需求 |

**3% 税率要点**：

- `taxRate = 300`（300 bps = 3%）。
- `taxRate > 0` 且 `mktBps < 10000` 时使用 **Tax Token V2 实现**，发币时需用对应链的 **Tax Token V2 Impl** 做 CREATE2 地址预测；若 `mktBps == 10000` 则用 Tax Token V1 Impl。
- **salt**：必须通过 CREATE2 预测，使**税币地址末尾为 `7777`**（与 Portal、Tax Token V2 Impl 一致），否则合约会拒绝。预测逻辑见文档 [Find the salt (vanity suffix)](https://docs.flap.sh/flap/developers/token-launcher-developers/launch-token-through-portal#id-3-find-the-salt-vanity-suffix)。

**实现合约地址（BNB Chain 主网）**：

- Standard Token Impl：`0x8b4329947e34b6d56d71a3385cac122bade7d78d`
- Tax Token V1：`0x29e6383F0ce68507b5A72a53c2B118a118332aA8`
- Tax Token V2：`0xae562c6A05b798499507c6276C6Ed796027807BA`

### 3.2 通过 VaultPortal 创建 3% 税率 + 金库代币（newTaxTokenWithVault）

**合约**：VaultPortal（BNB 主网：`0x90497450f2a706f1951b5bdda52B4E5d16f34C06`）。

**方法**：

```solidity
function newTaxTokenWithVault(NewTaxTokenWithVaultParams calldata params) external payable returns (address token);
```

**NewTaxTokenWithVaultParams**：在 Portal 的 V5 参数基础上，去掉 `beneficiary`，增加：

| 字段 | 类型 | 说明 |
|------|------|------|
| vaultFactory | address | 已注册的金库工厂地址 |
| vaultData | bytes | 该工厂要求的编码参数（依工厂而定） |

**3% 税率设置**：同样 `taxRate = 300`，其余税相关字段（taxDuration、mktBps、deflationBps、dividendBps、lpBps 等）与 Portal 一致。salt 同样需使税币地址末尾 **7777**。

### 3.3 发币前准备（简要）

1. **元数据**：上传图片与 metadata JSON 到 Flap API，获得 `meta`（CID）。
2. **Salt**：用 Portal（或 VaultPortal 使用的 Portal）+ 对应 Tax Token Impl 做 CREATE2 暴力/随机，直到地址以 `7777` 结尾。
3. **quoteToken / quoteAmt**：BNB 用 `address(0)`，如需预注流动性则设 `quoteAmt` 并随调用附 `msg.value`。
4. **税分配**：mktBps、deflationBps、dividendBps、lpBps 之和及语义需符合合约与文档要求。

---

## 四、BNB Chain 常用合约地址汇总

| 用途 | 主网地址 | 测试网地址 |
|------|----------|------------|
| Portal（交易 + 无金库发币） | `0xe2cE6ab80874Fa9Fa2aAE65D277Dd6B8e65C9De0` | `0x5bEacaF7ABCbB3aB280e80D007FD31fcE26510e9` |
| VaultPortal（带金库发币） | `0x90497450f2a706f1951b5bdda52B4E5d16f34C06` | `0x027e3704fC5C16522e9393d04C60A3ac5c0d775f` |
| Tax Token V2 Impl（主网） | `0xae562c6A05b798499507c6276C6Ed796027807BA` | — |

---

## 五、参考链接

- [Flap 文档首页](https://docs.flap.sh/flap)
- [Trade Tokens（报价与 swap）](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/trade-tokens)
- [Launch token through Portal（newTokenV5）](https://docs.flap.sh/flap/developers/token-launcher-developers/launch-token-through-portal)
- [Launch token through VaultPortal](https://docs.flap.sh/flap/developers/token-launcher-developers/launch-token-through-vaultportal)
- [Deployed Contract Addresses（Wallet/Terminal）](https://docs.flap.sh/flap/developers/wallet-and-terminal-and-bot-developers/deployed-contract-addresses)
- [Deployed Contract Addresses（Launcher）](https://docs.flap.sh/flap/developers/token-launcher-developers/deployed-contract-addresses)
- [Flap Tax Vault（金库与税率）](https://docs.flap.sh/flap/developers/vault-developers/flap-tax-vault)
