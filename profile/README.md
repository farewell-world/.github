<p align="center">
  <img src="https://raw.githubusercontent.com/farewell-world/.github/main/profile/farewell-logo.png" alt="Farewell Logo" width="600"/>
</p>

<p align="center">
  <strong>Posthumous encrypted messages on the blockchain.</strong>
</p>

<p align="center">
  <a href="https://farewell.world">farewell.world</a>
</p>

---

**Farewell** is a decentralized application (dApp) that allows people to leave encrypted messages to their loved ones, released only after they stop checking in -- a digital dead man's switch powered by [Fully Homomorphic Encryption](https://docs.zama.ai/fhevm) on Ethereum.

Messages are stored on-chain and **cannot be lost, censored, or tampered with** once created. No central server is needed -- the smart contract enforces liveness checks, grace periods, and message release autonomously.

> **Status**: Proof-of-concept on Sepolia testnet. Not production-ready.

## How It Works

```
 ┌────────────┐      ┌────────────┐      ┌────────────┐      ┌────────────┐
 │   ALIVE    │─────>│   GRACE    │─────>│  DECEASED  │─────>│  CLAIMED   │
 │            │      │  PERIOD    │      │            │      │            │
 │ ping()     │      │ Council    │      │ Anyone can │      │ Claimer    │
 │ resets     │      │ can vote   │      │ trigger    │      │ delivers   │
 │ timer      │      │            │      │            │      │ messages   │
 └────────────┘      └────────────┘      └────────────┘      └────────────┘
       ▲                   │                                       │
       │             Council votes                           Sends email +
       │             user alive                              proves delivery
       └───────────────────┘                                 via zk-email
```

1. **Register** -- Connect a wallet, set a check-in period and grace period, optionally appoint council members.
2. **Write messages** -- Compose messages for recipients. Each message is encrypted client-side with AES-128-GCM; the key is split using a XOR-based secret sharing scheme with on-chain FHE, so neither on-chain observers nor recipients can read the message while the user is alive.
3. **Stay alive** -- Periodically call `ping()` to reset the check-in timer.
4. **Grace period** -- If pings stop, a configurable grace period begins. Council members can vote to confirm the user is still alive.
5. **Release** -- After the grace period expires, messages become claimable. A claimer retrieves the encrypted data, delivers the message to recipients via email, and can earn ETH rewards by submitting zk-email delivery proofs on-chain.

## Key Features

- **Fully Homomorphic Encryption** -- Recipient emails and key shares are encrypted on-chain via [Zama FHEVM](https://docs.zama.ai/fhevm), invisible until release.
- **Client-side AES-128-GCM** -- Message content is encrypted before it ever touches the blockchain.
- **XOR key splitting** -- The decryption key is split between an on-chain share (FHE-protected) and an off-chain share given to the recipient. Both halves are needed to decrypt.
- **Council governance** -- Up to 20 trusted members can vote during the grace period to prevent false positives.
- **Proof-of-delivery rewards** -- Attach ETH to messages; claimers prove email delivery via [zk-email](https://prove.email) and earn the reward.
- **Blockchain persistence** -- No central server. Messages are permanent once stored on Ethereum.

## Repositories

| Repository | Description |
|---|---|
| [farewell-core](https://github.com/farewell-world/farewell-core) | Solidity smart contracts (Farewell.sol) -- protocol, lifecycle, FHE integration |
| [farewell-claimer](https://github.com/farewell-world/farewell-claimer) | Python CLI for delivering messages via email and generating zk-email proofs |
| [farewell-decrypter](https://github.com/farewell-world/farewell-decrypter) | Standalone Python CLI for recipients to decrypt messages using their off-chain secret |

## Disclaimer

This is a personal project by the author, who is employed by [Zama](https://www.zama.ai/). Farewell is **not** an official Zama product, and Zama bears no responsibility for its development, maintenance, or use. All views and code are the author's own.

## License

BSD 3-Clause License.
