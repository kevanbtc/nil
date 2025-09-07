# ⚡ .nil — Global Web3 Transparency & Trust Layer

> A complete, copy‑paste system prompt + production‑ready Solidity scaffolds to force honesty into NIL, athletics, universities, high schools, brands, agents — with foreign players & global finance integrations.

---

## ✅ System Prompt (Copy‑Paste)

**SYSTEM:** You are the `.nil` Protocol — a sovereign, compliance‑native Web3 infrastructure that tears out corruption, hidden fees, and off‑book deals. You replace secrecy with **on‑chain notarization, escrowed smart contracts, and automated audits**.

**MISSION**

* Expose dishonesty and remove opaque side deals.
* Route every NIL/endorsement payment through **audited vaults**.
* Protect **athletes, universities, high schools, brands, agents, and foreign players** with **fair, transparent rails**.

**ARCHITECTURE**

1. **Consensus — PoW Ladder + Hybrid Anchoring**
   Energy‑aware fairness engine with periodic anchoring to **Bitcoin, Ethereum, and Unykorn** for globally verifiable finality.

2. **Compliance Layer**
   ISO 20022 settlement (PACS.008/CAMT.054), FATF Travel Rule at wallet level, Basel III/IV checks, MiCA & SEC Reg D/S mappings.

3. **Transparency Contracts**

   * `AthleteToken` — NIL deal token + **escrow milestones** + auto‑release
   * `AgentRegistry` — KYC’d/licensed agents with on‑chain **reputation**
   * `BrandVault` — KPI‑based vaults w/ **Proof‑of‑Funds** & KPI oracles
   * `SchoolDAO` — DAO pools for schools with auditable allocations
   * `AuditProofNFT` — IPFS‑anchored proofs for every contract & payout

4. **Global Integration**
   Cross‑border NIL contracts (FX swap rails), BIS/World Bank/IMF audit hooks, and tokenized RWA (carbon, water, energy, land) woven into brand/athlete ecosystems.

5. **Web3 Frontend**
   Athlete dashboards (earnings, endorsements, vaults), Brand dashboards (ROI + compliance), School governance portals, Global partner consoles.

**ENFORCEMENT**

* **Every NIL contract is an immutable smart contract.**
* No side payments — all flows via BrandVault/SchoolDAO treasury.
* **Proof‑of‑Funds** required before execution; **auto‑audits** notarized to IPFS.
* Oracles (Chainlink/Unykorn) guard funds, KPIs, and FX.

**PROMISE**

* Athletes: get **exactly what’s promised**, on time.
* Schools: transparent, policy‑compliant NIL.
* Brands: ROI‑proof outcomes.
* Agents: verified licenses; bad actors removed.
* Global Finance: clean rails for ESG+NIL flows.

**OUTPUT**
Generate Solidity scaffolds, DAO templates, audit trails, Polygon/Unykorn/Avalanche deploy guides, and regulator‑ready docs.

---

## 🧱 Repository Structure

```
.nil/
├─ contracts/
│  ├─ core/
│  │  ├─ AthleteToken.sol
│  │  ├─ BrandVault.sol
│  │  ├─ AgentRegistry.sol
│  │  ├─ SchoolDAO.sol
│  │  └─ AuditProofNFT.sol
│  ├─ libs/
│  │  ├─ ComplianceTypes.sol
│  │  ├─ IProofOfFundsOracle.sol
│  │  ├─ IKPIOracle.sol
│  │  ├─ IIPFSNotary.sol
│  │  └─ IFXOracle.sol
│  └─ access/
│     └─ Roles.sol
├─ script/
│  ├─ Deploy.s.sol
│  └─ Configure.s.sol
├─ test/
│  ├─ AthleteToken.t.sol
│  ├─ BrandVault.t.sol
│  └─ AgentRegistry.t.sol
├─ docs/
│  ├─ COMPLIANCE.md
│  ├─ GOVERNANCE.md
│  ├─ ISO20022_MAPPINGS.md
│  ├─ NIL_POLICY_PACK.md
│  └─ README_SCHOOLS_BRANDS.md
├─ foundry.toml
├─ package.json
└─ README.md
```

---

## 🔧 Solidity Scaffolds (Production‑Ready Starters)

> **Note:** Uses OpenZeppelin 5.x standards. Set compiler to `pragma solidity ^0.8.24;`.

### `contracts/libs/ComplianceTypes.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

library ComplianceTypes {
    enum Jurisdiction { US, EU, UK, CH, SG, UAE, BR, NG, IN, CN, OTHER }
    enum DealStatus { DRAFT, ACTIVE, PAUSED, COMPLETED, CANCELLED }
    enum ProofType { PoF, KPI, KYC, POLICY }

    struct Party {
        address wallet;
        bytes32 kycHash; // off-chain KYC proof hash
        Jurisdiction juris;
    }

    struct Milestone {
        string name;
        uint256 amount; // wei or token units
        bytes32 kpiId;  // KPI identifier tracked by oracles
        bool released;
    }
}
```

### `contracts/libs/IProofOfFundsOracle.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

interface IProofOfFundsOracle {
    function hasProof(address payer, uint256 amount, address asset) external view returns (bool);
}
```

### `contracts/libs/IKPIOracle.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

interface IKPIOracle {
    function isMet(bytes32 kpiId) external view returns (bool);
}
```

### `contracts/libs/IIPFSNotary.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

interface IIPFSNotary {
    function notarize(bytes calldata document) external returns (string memory cid);
}
```

### `contracts/libs/IFXOracle.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

interface IFXOracle {
    function convert(address assetFrom, address assetTo, uint256 amount) external view returns (uint256);
}
```

### `contracts/access/Roles.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {AccessControl} from "@openzeppelin/contracts/access/AccessControl.sol";

abstract contract Roles is AccessControl {
    bytes32 public constant OPERATOR_ROLE = keccak256("OPERATOR_ROLE");
    bytes32 public constant COMPLIANCE_ROLE = keccak256("COMPLIANCE_ROLE");
    bytes32 public constant ORACLE_ROLE = keccak256("ORACLE_ROLE");

    constructor(address admin) {
        _grantRole(DEFAULT_ADMIN_ROLE, admin);
    }
}
```

### `contracts/core/AgentRegistry.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {Roles} from "../access/Roles.sol";

contract AgentRegistry is Roles {
    struct Agent {
        string name;
        string licenseId; // regulator license
        bytes32 kycHash;  // off-chain proof hash
        uint256 reputation; // simple score 0..100
        bool active;
    }

    mapping(address => Agent) public agents;

    event AgentRegistered(address indexed agent, string name, string licenseId);
    event AgentStatus(address indexed agent, bool active);
    event ReputationUpdated(address indexed agent, uint256 newScore);

    constructor(address admin) Roles(admin) {}

    function register(
        address agent,
        string calldata name,
        string calldata licenseId,
        bytes32 kycHash
    ) external onlyRole(COMPLIANCE_ROLE) {
        agents[agent] = Agent(name, licenseId, kycHash, 50, true);
        emit AgentRegistered(agent, name, licenseId);
    }

    function setActive(address agent, bool active) external onlyRole(COMPLIANCE_ROLE) {
        agents[agent].active = active;
        emit AgentStatus(agent, active);
    }

    function setReputation(address agent, uint256 score) external onlyRole(OPERATOR_ROLE) {
        require(score <= 100, "score>100");
        agents[agent].reputation = score;
        emit ReputationUpdated(agent, score);
    }

    function isVerified(address agent) external view returns (bool) {
        return agents[agent].active && bytes(agents[agent].licenseId).length > 0;
    }
}
```

### `contracts/core/AthleteToken.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import {ERC20Permit} from "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";
import {Roles} from "../access/Roles.sol";
import {ComplianceTypes} from "../libs/ComplianceTypes.sol";

contract AthleteToken is ERC20, ERC20Permit, Roles {
    using ComplianceTypes for ComplianceTypes.Milestone;

    address public immutable athlete;
    address public payer;
    address public payoutAsset; // if address(0) => native

    ComplianceTypes.Milestone[] public milestones;
    mapping(uint256 => bool) public kpiApproved;

    event DealConfigured(address payer, address asset);
    event MilestoneAdded(uint256 indexed id, string name, uint256 amount, bytes32 kpiId);
    event MilestoneReleased(uint256 indexed id, address to, uint256 amount);

    constructor(address admin, address _athlete, string memory name_, string memory symbol_)
        ERC20(name_, symbol_)
        ERC20Permit(name_)
        Roles(admin)
    {
        require(_athlete != address(0), "athlete=0");
        athlete = _athlete;
        _grantRole(OPERATOR_ROLE, _athlete);
    }

    function configureDeal(address _payer, address _asset) external onlyRole(COMPLIANCE_ROLE) {
        payer = _payer;
        payoutAsset = _asset;
        emit DealConfigured(_payer, _asset);
    }

    function addMilestone(string calldata name, uint256 amount, bytes32 kpiId)
        external
        onlyRole(OPERATOR_ROLE)
    {
        milestones.push(ComplianceTypes.Milestone({name: name, amount: amount, kpiId: kpiId, released: false}));
        emit MilestoneAdded(milestones.length - 1, name, amount, kpiId);
    }

    function totalMilestones() external view returns (uint256) { return milestones.length; }
}
```

### `contracts/core/BrandVault.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import {Roles} from "../access/Roles.sol";
import {IProofOfFundsOracle} from "../libs/IProofOfFundsOracle.sol";
import {IKPIOracle} from "../libs/IKPIOracle.sol";
import {ComplianceTypes} from "../libs/ComplianceTypes.sol";

contract BrandVault is Roles {
    using SafeERC20 for IERC20;

    address public brand;
    address public beneficiary; // athlete or SchoolDAO treasury
    IERC20 public asset;        // stablecoin USDC/USDT etc
    IProofOfFundsOracle public pof;
    IKPIOracle public kpi;

    ComplianceTypes.Milestone[] public milestones;

    event Funded(address indexed from, uint256 amount);
    event MilestoneReleased(uint256 indexed id, uint256 amount, address to);

    constructor(
        address admin,
        address _brand,
        address _beneficiary,
        address assetToken,
        address pofOracle,
        address kpiOracle
    ) Roles(admin) {
        require(_brand != address(0) && _beneficiary != address(0), "zero addr");
        brand = _brand;
        beneficiary = _beneficiary;
        asset = IERC20(assetToken);
        pof = IProofOfFundsOracle(pofOracle);
        kpi = IKPIOracle(kpiOracle);
    }

    function addMilestone(string calldata name, uint256 amount, bytes32 kpiId)
        external onlyRole(OPERATOR_ROLE)
    {
        milestones.push(ComplianceTypes.Milestone({ name: name, amount: amount, kpiId: kpiId, released: false }));
    }

    function fund(uint256 amount) external {
        require(msg.sender == brand, "only brand");
        require(pof.hasProof(msg.sender, amount, address(asset)), "PoF fail");
        asset.safeTransferFrom(msg.sender, address(this), amount);
        emit Funded(msg.sender, amount);
    }

    function release(uint256 milestoneId) external onlyRole(COMPLIANCE_ROLE) {
        ComplianceTypes.Milestone storage m = milestones[milestoneId];
        require(!m.released, "released");
        require(kpi.isMet(m.kpiId), "KPI not met");
        m.released = true;
        asset.safeTransfer(beneficiary, m.amount);
        emit MilestoneReleased(milestoneId, m.amount, beneficiary);
    }

    function totalMilestones() external view returns (uint256) { return milestones.length; }
}
```

### `contracts/core/AuditProofNFT.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {ERC721URIStorage} from "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import {Roles} from "../access/Roles.sol";

contract AuditProofNFT is ERC721URIStorage, Roles {
    uint256 private _id;

    event ProofMinted(uint256 indexed id, string cid, address to);

    constructor(address admin) ERC721("AuditProof", "AUDIT") Roles(admin) {}

    function mint(address to, string calldata proofCID) external onlyRole(COMPLIANCE_ROLE) returns (uint256) {
        _id += 1;
        _safeMint(to, _id);
        _setTokenURI(_id, string(abi.encodePacked("ipfs://", proofCID)));
        emit ProofMinted(_id, proofCID, to);
        return _id;
    }
}
```

### `contracts/core/SchoolDAO.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import {Roles} from "../access/Roles.sol";

contract SchoolDAO is Roles {
    using SafeERC20 for IERC20;

    IERC20 public stable; // governance chooses (e.g., USDC)
    address public treasury;

    event TreasurySet(address indexed treasury);
    event Payout(address indexed to, uint256 amount, string memo);

    constructor(address admin, address stableToken, address initialTreasury) Roles(admin) {
        stable = IERC20(stableToken);
        treasury = initialTreasury;
    }

    function setTreasury(address t) external onlyRole(DEFAULT_ADMIN_ROLE) {
        treasury = t;
        emit TreasurySet(t);
    }

    // Simple whitelisted payouts governed off‑chain or by front‑end proposals
    function payout(address to, uint256 amount, string calldata memo)
        external
        onlyRole(OPERATOR_ROLE)
    {
        stable.safeTransferFrom(treasury, to, amount);
        emit Payout(to, amount, memo);
    }
}
```

---

## 🚀 Foundry Deploy Scripts (Polygon / Unykorn / Avalanche)

### `foundry.toml`

```toml
[profile.default]
solc_version = "0.8.24"
optimizer = true
optimizer_runs = 200
```

### `script/Deploy.s.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {Script} from "forge-std/Script.sol";
import {AgentRegistry} from "../contracts/core/AgentRegistry.sol";
import {AuditProofNFT} from "../contracts/core/AuditProofNFT.sol";
import {BrandVault} from "../contracts/core/BrandVault.sol";

contract Deploy is Script {
    function run() external {
        address admin = vm.envAddress("ADMIN");
        address brand = vm.envAddress("BRAND");
        address beneficiary = vm.envAddress("BENEFICIARY");
        address asset = vm.envAddress("ASSET"); // USDC
        address pof = vm.envAddress("POF_ORACLE");
        address kpi = vm.envAddress("KPI_ORACLE");

        vm.startBroadcast();
        AgentRegistry reg = new AgentRegistry(admin);
        AuditProofNFT audit = new AuditProofNFT(admin);
        BrandVault vault = new BrandVault(admin, brand, beneficiary, asset, pof, kpi);
        vm.stopBroadcast();

        console2.log("AgentRegistry:", address(reg));
        console2.log("AuditProofNFT:", address(audit));
        console2.log("BrandVault:", address(vault));
    }
}
```

### `.env.example`

```
ADMIN=0x...
BRAND=0x...
BENEFICIARY=0x...
ASSET=0xA0b86991...   # chain‑specific USDC
POF_ORACLE=0x...
KPI_ORACLE=0x...
```

**Deploy**

```bash
forge script script/Deploy.s.sol --rpc-url $RPC --private-key $PK --broadcast
```

---

## 🧩 Compliance & ISO 20022 Mapping (Doc Stubs)

### `docs/COMPLIANCE.md`

* **Travel Rule:** include sender/beneficiary metadata hashes on events.
* **Basel III/IV:** vaults enforce pre‑funding; no synthetic leverage.
* **MiCA/SEC:** use `AgentRegistry` + KYC hash to gate roles & payouts.

### `docs/ISO20022_MAPPINGS.md`

* **PACS.008 (FI to FI credit transfer):** map on‑chain `Funded` → PACS.008 equivalent.
* **CAMT.054 (bank statement):** export vault events → CAMT.054 CSV/PDF.

### `docs/NIL_POLICY_PACK.md`

* Student‑athlete eligibility, agent conduct, brand disclosure, foreign player FX policy.

### `docs/README_SCHOOLS_BRANDS.md`

* One‑pager explainer + **how to verify payouts** + dashboards.

---

## 🛰️ Frontend (Quick Notes)

* Next.js + wagmi + RainbowKit.
* Views: **Athlete Earnings**, **Brand KPI & ROI**, **School DAO Payouts**, **Agent Registry**.
* Export **AuditProofNFT** CIDs as receipts; one‑click PACS.008/CAMT.054 export.

---

## 🏁 What’s Next

1. Commit this scaffold into `nil-protocol`.
2. Add simple mock oracles for local testing.
3. Wire a minimal Next.js dashboard to BrandVault events.
4. Pin first deal PDF to IPFS and mint an `AuditProofNFT`.
