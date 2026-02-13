# Flap Tax Vault 文档解读

> 参考文档：[Flap Tax Vault - Flap Developers](https://docs.flap.sh/flap/developers/vault-developers/flap-tax-vault)

本文档对 Flap 税币金库（Tax Vault）的**税率设置**与**主要调用方法**进行中文解读，便于开发集成与发币参数配置。

---

## 一、税率相关设置

### 1.1 发币时的税率参数

通过 **VaultPortal** 发行带金库的税币时，使用结构体 `NewTaxTokenWithVaultParams`，其中与税率直接相关的字段如下：

| 参数 | 类型 | 含义 | 说明 |
|------|------|------|------|
| `taxRate` | uint16 | 税率（基点） | 例如 100 = 1%，最大 1000 = 10% |
| `taxDuration` | uint64 | 税率生效时长 | 单位：秒，最大约 100 年 |

即：**税率在发币时通过 `taxRate`（basis points）设定**，链上可通过税币合约的 `taxRate()` 读取。

### 1.2 金库手续费与税率的关系（推荐逻辑）

文档给出的**推荐手续费计算**与税率挂钩。金库在 `receive()` 中收到税币转来的 BNB 后，可按以下方式计算金库抽成：

- **若当前税率 ≤ 1%（即 `taxRateBps ≤ 100`）**  
  - 手续费 = `msg.value * 600 / 10000`  
  - 即对**本次收到的 BNB** 收 **6%** 作为金库手续费。

- **若税率 > 1%（`taxRateBps > 100`）**  
  - 手续费 = `(msg.value * 6) / taxRateBps`  
  - 示例：  
    - 税率 1%（100 bps）→ 约 6% 的 `msg.value`  
    - 税率 2%（200 bps）→ 3%  
    - 税率 10%（1000 bps）→ 0.6%

若金库内部尚未保存 `taxRateBps`，可在首次 `receive()` 时从税币合约读取：

```solidity
if (taxRateBps == 0) {
    try ITaxToken(taxToken).taxRate() returns (uint256 _taxRate) {
        if (_taxRate > 0) {
            taxRateBps = _taxRate;
        }
    } catch {
        // 获取失败则保持为 0
    }
}
```

**小结**：税率在发币时由 `taxRate` 设定；金库侧通过 `taxRate()` 或自存 `taxRateBps` 参与手续费计算。

---

## 二、主要调用方法

### 2.1 VaultPortal（金库门户）

#### 部署地址

| 网络 | VaultPortal 地址 |
|------|------------------|
| BNB 主网 | `0x90497450f2a706f1951b5bdda52B4E5d16f34C06` |
| BNB 测试网 | `0x027e3704fC5C16522e9393d04C60A3ac5c0d775f` |

#### 获取金库信息

| 方法 | 说明 | 返回值 |
|------|------|--------|
| `tryGetVault(address taxToken)` | 先查 VaultPortal 创建的金库，若无则查所有已注册工厂与手动验证金库。**推荐默认使用**，避免查不到时 revert。 | `(bool found, VaultInfo memory info)` |
| `getVault(address taxToken)` | 仅返回**通过 VaultPortal 创建**的金库，无则 revert。 | `VaultInfo memory info` |

**VaultInfo 结构体**：`vault`、`vaultFactory`、`description`、`isOfficial`、`riskLevel`。

#### 获取审计报告

| 方法 | 说明 | 返回值 |
|------|------|--------|
| `getAuditReports(address taxToken, uint256 offset, uint256 limit)` | 分页获取该税币的审计报告；若税币无报告则退回其金库所属工厂的审计报告。 | `(AuditReport[] memory reports, uint256 total)` |

#### 使用金库发币

通过 VaultPortal 的「使用金库发币」流程，传入 `NewTaxTokenWithVaultParams`，其中除常规发币参数外，末尾还有：

- `vaultFactory`：金库工厂地址  
- `vaultData`：金库类型所需编码数据  

税率由同一结构体中的 **`taxRate`** 设定。

---

### 2.2 VaultFactory（金库工厂）

实现 `IVaultFactory` 的工厂合约，由 VaultPortal 在发币时调用。

| 方法 | 说明 | 返回值 |
|------|------|--------|
| `newVault(address taxToken, address quoteToken, address creator, bytes calldata vaultData)` | 仅可由 VaultPortal 调用。注意：调用时税币**尚未部署**，`taxToken` 为预测地址。 | `address vault` |
| `isQuoteTokenSupported(address quoteToken)` | 查询是否支持某报价资产（当前文档仅支持 BNB，即 `address(0)`）。 | `bool supported` |

---

### 2.3 金库合约（VaultBase）

| 方法/能力 | 说明 |
|-----------|------|
| `description()` | **必须实现**，返回描述金库当前状态的动态字符串（如锁仓量、流式分配状态等）。若收手续费，需在描述中写清费用结构。 |
| `receive() external payable` | 接收税币转来的 BNB；内部可实现上述「按税率计算手续费」的逻辑。 |
| `_getPortal()` | 内部方法，返回当前链的 VaultPortal 地址。 |
| `_getGuardian()` | 内部方法，返回当前链的 Guardian 地址；若有权限函数须同时授权 Guardian。 |

---

### 2.4 税币合约（链上）

| 方法 | 说明 |
|------|------|
| `taxRate()` | 返回当前税率（basis points）。金库在计算手续费时可调用 `ITaxToken(taxToken).taxRate()` 获取。 |

---

## 三、小结

| 维度 | 说明 |
|------|------|
| **税率设置** | 在 **VaultPortal 发币参数** 中通过 **`taxRate`**（基点，最大 10%）和 **`taxDuration`** 设定；链上由税币的 **`taxRate()`** 暴露。 |
| **金库手续费** | 在金库 **`receive()`** 中按文档公式用 **`taxRateBps`**（可从 **`ITaxToken(taxToken).taxRate()`** 读取）计算。 |
| **常用调用** | 查金库 → **VaultPortal.tryGetVault(taxToken)**；查审计 → **VaultPortal.getAuditReports(taxToken, offset, limit)**；发带金库税币 → 调用 VaultPortal 的带 **NewTaxTokenWithVaultParams**（含 `taxRate`、`vaultFactory`、`vaultData`）的接口；工厂创建金库 → **VaultFactory.newVault(...)**（仅由 VaultPortal 调用）。 |

---

## 四、相关链接

- [Flap Tax Vault 官方文档](https://docs.flap.sh/flap/developers/vault-developers/flap-tax-vault)
- BNB 主网 Portal：`0xe2cE6ab80874Fa9Fa2aAE65D277Dd6B8e65C9De0`
- BNB 主网 Guardian：`0x9e27098dcD8844bcc6287a557E0b4D09C86B8a4b`
