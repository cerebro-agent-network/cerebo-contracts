# Cerebro Contracts
[![Soroban](https://img.shields.io/badge/Network-Stellar_Soroban-blue)](https://soroban.stellar.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Stable](https://img.shields.io/badge/Status-Stable-green)]()
Cerebro is a decentralized infrastructure for education streaming and scholarship management, built on the Stellar network using Soroban smart contracts. It enables a fair, transparent, and automated ecosystem where students gain time-based access to course content, and scholarships are distributed and managed programmatically.
## 🌟 Core Value Proposition
Cerebro solves the challenge of content piracy and inefficient scholarship distribution by implementing:
- **Streaming Access**: Content is not "bought" as a static asset but "streamed" through time-based access control.
- **Programmable Scholarships**: Funds are locked in contracts and disbursed to teachers based on verified student progress.
- **Anti-Sharing Security**: A unique session-locking mechanism prevents multi-device streaming of a single account.
- **Incentivized Learning**: Integration with Soulbound Tokens (SBTs) to certify course completion on-chain.
---
## 🛠 Technical Architecture
### 1. Access Streaming & Heartbeat Mechanism
Cerebro uses a **heartbeat-based access model**. Instead of a simple "yes/no" access flag, the contract requires students to send periodic `heartbeat` transactions.
- **Heartbeat Logic**: Each heartbeat extends the `total_watch_time` and verifies the student's current balance/subscription.
- **Session Locking**: To prevent account sharing, each heartbeat must include a `session_hash`. If a different hash is provided while a session is still active (within the `heartbeat_interval`), the contract rejects the request.
### 2. Dynamic Pricing Engine
The contract implements a loyalty-based pricing model via `calculate_dynamic_rate`.
- **Base Rate**: The standard cost per second of access.
- **Loyalty Discount**: Once a student exceeds a specific `discount_threshold` of watch time, the contract automatically applies a `discount_percentage`, making long-term learning more affordable.
### 3. Scholarship Lifecycle
Scholarships are handled as dedicated accounts within the contract:
- **Funding**: Sponsors fund a student's scholarship using any supported Stellar token.
- **Disbursement**: Students can transfer funds from their scholarship balance to verified `Teacher` addresses, ensuring funds are spent only on approved educators.
### 4. SBT (Soulbound Token) Triggers
The contract tracks cumulative watch time against the `CourseDuration`. When a student's `total_watch_time` $\ge$ `course_duration`, the contract emits an `SBT_Mint` event, allowing an external minting service to issue a non-transferable certificate of completion.
---
## 📖 API Reference
### Public Functions
| Function | Parameters | Description | Access |
| :--- | :--- | :--- | :--- |
| `init` | `base_rate, discount_threshold, discount_percentage, min_deposit, heartbeat_interval` | Initializes contract global parameters. | Admin |
| `buy_access` | `student, course_id, amount, token` | Purchases time-based access to a specific course. | Student |
| `buy_subscription` | `subscriber, course_ids, duration_months, amount, token` | Purchases a multi-course subscription for a fixed duration. | Subscriber |
| `heartbeat` | `student, course_id, session_hash` | Verifies active session and increments watch time. | Student |
| `fund_scholarship` | `funder, student, amount, token` | Deposits funds into a student's scholarship account. | Funder |
| `transfer_scholarship_to_teacher` | `student, teacher, amount` | Transfers scholarship funds to a verified teacher. | Student |
| `set_teacher` | `admin, teacher, status` | Authorizes or revokes a teacher's status. | Admin |
| `veto_course_globally` | `admin, course_id, status` | Globally disables access to a specific course. | Admin |
| `veto_course_access` | `admin, student, course_id` | Revokes a specific student's access to a course. | Admin |
| `has_access` | `student, course_id` | Checks if a student has active access or a valid subscription. | Public |
### Data Structures
- **`Access`**: Tracks `student`, `course_id`, `expiry_time`, `total_watch_time`, and `last_heartbeat`.
- **`Scholarship`**: Tracks `balance` and the `token` used for the scholarship.
- **`SubscriptionTier`**: Maps a `subscriber` to a list of `course_ids` and an `expiry_time`.
---
## 🚀 Getting Started
### Prerequisites
- [Rust](https://www.rust-lang.org/tools/install)
- [Soroban CLI](https://soroban.stellar.org/docs/install)
- Stellar Testnet Account
### Installation & Testing
```bash
# Clone the repository
git clone https://github.com/cerebro-agent-network/cerebo-contracts.git
cd cerebo-contracts
# Build the contracts
cargo build
# Run the test suite
cargo test
Deployment
# Deploy to Stellar Testnet
soroban contract deploy \
  --network testnet \
  --source target/wasm32-unknown-unknown/release/scholar_contracts.wasm \
  --source-account <YOUR_ACCOUNT>
🌐 Deployment Details
- 
Network: Stellar Testnet
- 
Contract ID: CB7OZPTIUENDWJWNHRGDPZLIEIS6TXMFRYT4WCGHIZVYLCTXEONC6VHY
📜 License
Distributed under the MIT License. See LICENSE for more information.
