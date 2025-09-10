# Safety

## **MULLEX Security Architecture Design**

The MULLEX protocol is designed with a securit&#x79;**-**&#x66;irst approach, implementing multi-layered protection mechanisms to ensure the safety, tamper-resistance, and attack resilience of cross-chain asset transfers. Below are the core security strategies:

***

### **1. Permission Control**

#### **Principle of Least Privilege**

* **Critical admin operations (e.g., `update_manager`, `enable_chain`)**
  * Only executable via admin signature **(`Signer`)** to prevent unauthorized calls.
  * Supports multisig for enhanced security.

#### **Hierarchical Contract Permissions**

| Permission Level | Example Operations           | Verification Method       |
| ---------------- | ---------------------------- | ------------------------- |
| **Admin**        | Update params, enable tokens | `require!(is_manager)`    |
| **User**         | Withdraw (`cash_out`), stake | Signature + balance check |
| **Listener**     | Trigger payout (`payout`)    | Event signature + nonce   |

\


### **2. Replay Attack Protection**

#### **Unique Transaction Identifiers (Nonce Mechanism)**

* Each cross-chain transaction binds to an incremental nonce, ensuring single execution.
* **Dual Verification**:
  1. **Source-chain nonce** (e.g., Solana’s `cash_out_nonce`).
  2. **Target-chain nonce** (e.g., Ethereum’s `deposit_nonce`).

#### **Event Signature Verification**

* The relayer (`Listener`) must verify:
  * Whether the event originates from a legitimate contract (`msg.sender` check).
  * Whether event data is tampered with (**signature hash comparison**).



### **3. Token & Chain Security Checks**

#### **Dynamic Enablement Mechanism**

* **Token Whitelist (`TokenConfig.enabled`)**
  * Only approved tokens (e.g., USDC、muUSD) are allowed for cross-chain transfers to prevent malicious assets.
* **Chain Whitelist (`ChainConfig.enabled`)**
  * Admins can dynamically enable/disable target chains (e.g., pause a compromised chain).

#### **Withdrawal Double-Check**

{% code fullWidth="false" %}
```rust
// Pseudocode example
fn cash_out() {
require!(token_config.enabled, "Token not supported");
require!(chain_config.enabled, "Target chain disabled");
require!(balance >= amount, "Insufficient funds");
}
```
{% endcode %}



### **4. 去中心化与机构级可信节点**

我们深知，跨链协议的安全性是用户资产保障的根本，而中继器节点的去中心化是其中的重中之重。

在初始阶段，为确保网络启动的安全与效率，由协议核心团队运行部分节点是必要的。然而，我们完全理解社区对于“单一实体控制多数节点”潜在风险的担忧，例如单点故障或恶意操作。为此，我们已制定并正在执行一项积极的节点去中心化战略：

**引入权威第三方机构节点：**\
我们正在与多家全球知名的数字资产托管机构、审计公司、区块链安全公司以及投资机构进行积极洽谈，邀请它们作为独立、可信的第三方来运行Mullex中继器节点。

这些机构节点将：

* **提供声誉背书：** 其自身在行业内的信誉和可靠性将为Mullex网络的安全性提供强有力的额外保障。
* **实现制衡与监督：** 与核心团队运行的节点共同构成中继器网络，形成相互监督、相互制衡的治理格局，确保任何单一方都无法独自控制网络。
* **增强抗审性：** 分布在不同司法辖区和实体的节点网络能显著提升整个协议的抗审性和稳定性。
* **未来持续监督：**&#x5F53;前节点的具体数量和组成架构仍在优化中，但我们的目标非常明确：最终过渡到一个完全由多元化的、声誉良好的独立机构组成的去中心化中继器网**络**。我们将定期发布节点运营商引入进展，并接受社区的持续监督。

通过这一系列机制，Mullex Protocol 致力于将中继链的安全性从“信任团队”转变为“信任由多家权威机构共同维护的、可验证的数学和机制”，真正实现安全、可信、去中心化的跨链体验。\
