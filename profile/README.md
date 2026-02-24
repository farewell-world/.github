# Farewell

**Farewell** is a decentralized application (dApp) for creating posthumous encrypted messages using Fully Homomorphic Encryption (FHE) on blockchain. Users register, store encrypted messages for recipients, and those messages are only released after the user stops checking in -- a digital dead man's switch.

[farewell.world](https://farewell.world) | [Documentation](https://github.com/farewell-world/farewell)

---

## How It Works

1. **Register** -- Connect a wallet, set a check-in period, grace period, and optional council members.
2. **Write messages** -- Compose messages for recipients. Each message is encrypted client-side with AES-GCM and its key is split using a secret sharing scheme with on-chain FHE.
3. **Stay alive** -- Periodically ping the contract to prove liveness.
4. **Grace period** -- If pings stop, a grace period begins. Council members can vote on whether the user is truly gone.
5. **Release** -- After the grace period expires, messages become claimable by recipients, who can reconstruct the decryption key and read them.

## Repositories

| Repository | Description |
|---|---|
| [farewell](https://github.com/farewell-world/farewell) | Next.js web application (main frontend and monorepo) |
| [farewell-core](https://github.com/farewell-world/farewell-core) | Solidity smart contracts (Farewell.sol) |
| [farewell-claimer](https://github.com/farewell-world/farewell-claimer) | Python CLI for claiming and decrypting messages |
| [farewell-decrypter](https://github.com/farewell-world/farewell-decrypter) | Browser-based message decryption tool |
| [admin-dashboard](https://github.com/farewell-world/admin-dashboard) | Administration dashboard |

## Status

Proof-of-concept on Sepolia testnet. Not production-ready.

## License

BSD 3-Clause License.
