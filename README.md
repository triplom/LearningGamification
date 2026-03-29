# LearningGamification

A DAML (Digital Asset Modeling Language) project that gamifies education on a blockchain ledger. Students earn points by completing courses, transfer points between accounts, and redeem them for rewards through merchant partnerships — all governed by multi-party smart contracts.

SDK version: **2.8.5**

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Parties                              │
│  Educhain (platform)  ·  Customer (student)  ·  Parceiros  │
└───────────┬───────────────────┬─────────────────────────────┘
            │                   │
    ┌───────▼──────┐   ┌────────▼────────┐
    │ Application  │   │  CourseService  │
    │  & CAccount  │   │ CourseProposal  │
    └──────────────┘   │    Course       │
            │          │ Accomplishment  │
    ┌───────▼──────┐   └─────────────────┘
    │   Rewards    │
    │   Mutual     │           ┌──────────────────┐
    │ ExchangeAssets│          │   AtomicSwap     │
    └──────────────┘           │ Asset / Swap     │
                               └──────────────────┘
                    ┌────────────────────────────┐
                    │    ExtensibleSwap          │
                    │ Points · Reward · Swap     │
                    │ (interface-based design)   │
                    └────────────────────────────┘
```

---

## Modules

### `Application.daml`
Handles customer onboarding to the Educhain platform.

| Template | Signatories | Purpose |
|---|---|---|
| `CustomerApplication` | customer | Blank application; filled via `SubmitApplication` |
| `CAccount` | customer, educhain | Active customer account holding points |

Key choices:
- `SubmitApplication` — customer fills in their details (archives blank, creates submitted)
- `ReviewApplication` — Educhain reviews and creates a `CAccount` (idempotent via key lookup)
- `AcceptApplication` — Educhain creates an account with a specified initial points balance
- `AddPoints` — Educhain credits points to an account
- `AddCompletedPoints` — Educhain awards course-completion points (with a 100-point bonus)

---

### `CourseService.daml`
Manages the full course lifecycle from proposal to accomplishment.

| Template | Signatories | Purpose |
|---|---|---|
| `CourseProposal` | educhain, customer | A proposed course pending review |
| `Course` | educhain, customer | An approved, active course |
| `AccomplishmentCourse` | educhain | A completed course with awarded points |

Key choices:
- `Revise` — Educhain requests changes (course info must differ)
- `Approve` — Educhain approves the proposal (course info may or may not change)
- `Reject` / `Cancel` — Reject or withdraw the proposal
- `Evaluate` — Educhain evaluates a course; either publishes an accomplishment or defers
- `Accomplished` — Educhain records course completion with additional points

Also includes a `Duration` typeclass that computes the number of weekdays between a course's start and end date.

---

### `Rewards.daml`
Loyalty rewards system: points are held in `Mutual` contracts and redeemed for assets.

| Template | Signatories | Purpose |
|---|---|---|
| `Mutual` | customer, merchant | Points relationship between a customer and a merchant |
| `ExchangeAssets` | merchant, customer | Governs point-to-asset redemption rules |

Key choices:
- `MutualTransfer` — customer transfers their points to a new owner
- `SendAsset` — merchant redeems points for an asset; archives the old `Mutual` and creates an updated one (prevents double-spend)

Asset types: `Cloth`, `Shoe`, `Toy`, `GiftCard`

---

### `AtomicSwap.daml`
Peer-to-peer asset management and atomic swaps.

| Template | Signatories | Purpose |
|---|---|---|
| `Asset` | issuer, owner | Holds points and reward tokens |
| `AssetTransferProposal` | (asset signatories) | Pending transfer of an asset |
| `SwapProposal` | initiator | Proposal to atomically swap two assets |

Key choices:
- `Asset_Transfer` / `AssetTransferProposal_Accept` — transfer ownership
- `Asset_Merge` — combine two assets into one
- `Asset_Split` — split an asset into two parts (returns `SplitResult`)
- `Asset_SetObservers` — update the observer list
- `SwapProposal_Accept` — atomic swap: both assets change hands simultaneously

Asset types: `GiftCard`, `Voucher`

---

### `ExtensibleSwap/`
Interface-based design for extensible asset swapping. Demonstrates DAML interfaces.

| File | Purpose |
|---|---|
| `Interfaces.daml` | `IAsset` and `ITransferProposal` interface definitions |
| `Assets.daml` | `Points` and `Reward` templates implementing both interfaces |
| `Swap.daml` | `AssetSwapProposal` — interface-driven atomic swap |
| `SwapTestScripts.daml` | End-to-end test: allocate assets, accept transfer proposals, settle a swap |

---

### `InitialState/`
Setup scripts for sandbox/Navigator initialization.

| File | Purpose |
|---|---|
| `Points.daml` | `Position` and `TransferProposal` templates |
| `Scripts.daml` | `initParties`, `initUsers`, `initCash` setup scripts |

---

### `Setup.daml`
Helper script that allocates the four standard test parties: `Natacha`, `Andre`, `Educhain`, `Parceiros`.

---

## Project Structure

```
LearningGamification/
├── daml.yaml
├── .gitignore
├── README.md
└── daml/
    ├── Application.daml
    ├── AtomicSwap.daml
    ├── CourseService.daml
    ├── Rewards.daml
    ├── Setup.daml
    ├── TestApplication.daml
    ├── TestAtomicSwap.daml
    ├── TestCourseService.daml
    ├── TestRewards.daml
    ├── ExtensibleSwap/
    │   ├── Assets.daml
    │   ├── Interfaces.daml
    │   ├── Swap.daml
    │   └── SwapTestScripts.daml
    └── InitialState/
        ├── Points.daml
        └── Scripts.daml
```

---

## How to Run

### Prerequisites
- [DAML SDK 2.8.5](https://docs.daml.com/getting-started/installation.html)

### Quick start

```bash
# Clone the repository
git clone https://github.com/triplom/LearningGamification.git
cd LearningGamification

# Open in DAML Studio (VSCode with DAML extension)
daml studio

# Start the sandbox and Navigator
daml start
```

### Run scripts individually

```bash
# Run a specific test script
daml script --dar .daml/dist/LearningGamification-3.0.1.dar \
            --script-name TestRewards:testCreateMutualContract \
            --ledger-host localhost --ledger-port 6865
```

### Build only

```bash
daml build
```

---

## Tests

| File | Tests |
|---|---|
| `TestApplication.daml` | Submit & accept application, query accounts, contract modification, review & add points |
| `TestCourseService.daml` | Proposal → revise → approve → course → accomplishment (Natacha + Andre) |
| `TestAtomicSwap.daml` | Asset merge, split, transfer, and atomic swap between parties |
| `TestRewards.daml` | Mutual creation, points transfer, asset exchange, insufficient points rejection |
| `ExtensibleSwap/SwapTestScripts.daml` | Interface-based asset transfer and swap settlement |

---

## Key Design Decisions

- **Multi-signatory contracts**: `Mutual`, `ExchangeAssets`, `CAccount`, and `Course` all require agreement from multiple parties, preventing unilateral changes.
- **Double-spend prevention**: `SendAsset` archives the old `Mutual` contract before creating an updated one — even though the choice is `nonconsuming` on `ExchangeAssets`, the `Mutual` referenced is explicitly archived.
- **Contract keys**: `CAccount` uses a `(educhain, id)` key to enable idempotent `ReviewApplication` — reviewing the same application twice does not create a duplicate account.
- **Interface-based extensibility**: `ExtensibleSwap` demonstrates how DAML interfaces decouple swap logic from specific asset types.

---

## Related Projects

| Project | Relationship |
|---------|-------------|
| [daml-learning-lab](https://github.com/triplom/daml-learning-lab) | **Prerequisite** — covers all DAML fundamentals, patterns (Propose-Accept, Delegation, Locking), interfaces, and testing techniques that this project applies in a real use case |
| [machine-learning-lab](https://github.com/triplom/machine-learning-lab) | **Extension opportunity** — point scoring models, course recommendation engines, and completion prediction can be built with the ML techniques there and integrated here |
| [python_learning](https://github.com/triplom/python_learning) | **Complementary** — Python data science tools (NumPy, Pandas, Scikit-learn) can be used to analyse ledger data exported from this project's DAML scripts |
| [cka-study-lab](https://github.com/triplom/cka-study-lab) | **Deployment target** — Canton (DAML's ledger runtime) is typically deployed on Kubernetes; CKA skills cover the infrastructure needed to run this project in production |

---

## Contributors

- [@triplom](https://github.com/triplom)

## License

Apache 2.0
