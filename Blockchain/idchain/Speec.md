# IDCHAIN Parachain Chainspec Guide

This document explains variable for IDCHAIN Parachain chainspec.

---

## Top-Level Fields

- `name`: The name of the parachain network. Example: `IDCHAIN Parachain`.
- `id`: Unique identifier for the parachain. Example: `idchain_parachain`.
- `chainType`: Type of chain (e.g., `Local`, `Live`, `Development`).
- `bootNodes`: List of network boot nodes (empty here, meaning no default nodes).
- `telemetryEndpoints`: Telemetry server endpoints for monitoring (null = disabled).
- `protocolId`: Protocol identifier for network communication.
- `properties`: Chain properties:
  - `ss58Format`: Address format (38 for custom chains).
  - `tokenDecimals`: Number of decimals for the native token.
  - `tokenSymbol`: Symbol for the native token (e.g., `IDCHAIN`).
- `relay_chain`: Name of the relay chain this parachain connects to.
- `para_id`: Parachain ID assigned by the relay chain.
- `codeSubstitutes`: Used for runtime upgrades (empty here).
- `genesis`: Initial state of the chain.

---

## Genesis Section

### `runtimeGenesis`
- `authorities`: List of initial block authoring accounts (empty here).
- `auraExt`: Aura consensus extension settings (empty object).
- `balances`: Initial account balances:
  - `balances`: Array of `[address, amount]` pairs. Each entry sets the starting balance for an account.
- `collatorSelection`: Collator (block producer) settings:
  - `candidacyBond`: Amount required to become a collator (0 here).
  - `desiredCandidates`: Number of desired collators (0 here).
  - `invulnerables`: List of invulnerable collator addresses.
- `council`: Governance council members (addresses).
- `democracy`: Democracy module settings (empty object).
- `didLookup`: DID (Decentralized Identifier) lookup links (empty array).
- `fungibles`: Fungible asset settings:
  - `accounts`: List of asset accounts (empty).
  - `assets`: List of assets (empty).
  - `metadata`: Asset metadata (empty).
- `parachainInfo`: Info about the parachain:
  - `parachainId`: Parachain ID (matches top-level `para_id`).
- `parachainSystem`: Parachain system settings (empty object).
- `polkadotXcm`: Cross-chain messaging settings:
  - `safeXcmVersion`: XCM protocol version (4).
- `session`: Session keys for validators/collators:
  - `keys`: Array of `[address, address, {aura: address}]` for session key mapping.
- `sudo`: Sudo (superuser) key:
  - `key`: Address with sudo privileges.
- `system`: System module settings (empty object).
- `technicalCommittee`: Technical committee members (addresses).
- `technicalMembership`: Technical membership members (empty array).
- `tipsMembership`: Tips membership members (empty array).
- `treasury`: Treasury module settings (empty object).
- `vesting`: Vesting schedules:
  - `vesting`: Array of vesting entries (empty).

---

## Example Address Explanation
- Addresses like `5DtE6cJDy8WjNZdfRWkvkmVyr2YepuCVHFwU4Gn2PsBnqTvf` are SS58-encoded public keys for accounts on the chain.
- Amounts like `10000000000000000000000` are in the smallest unit (based on `tokenDecimals`).

---

For more details, see [Polkadot/Substrate chainspec documentation](https://docs.substrate.io/reference/chain-specs/).
