## InsumerAPI

Wallet auth for developers and AI agents. Send a wallet and conditions, get a signed boolean, across 37 blockchains. No secrets. No identity-first. No static credentials. Access depends on what a wallet holds, right now.

Wallet auth is the primitive: read, evaluate, sign, keep. [Condition-based access](https://insumermodel.com/blog/there-is-no-key.html) is the category.

### What it does

`POST /v1/attest` takes a wallet address and up to 10 conditions: token balance, NFT ownership, EAS attestations, Farcaster, view calls, ratio rules, account code (plain key, EIP-7702 delegation, or contract) and agent standing (ERC-8004, ERC-7710). It returns a signed pass/fail attestation with `id`, `pass`, `results` (a `met` boolean per condition, with `conditionHash`, `blockNumber`, `blockTimestamp`), `attestedAt` and `expiresAt`. Boolean, not balance. Each result is signed twice, ES256 and a post-quantum ML-DSA-65 signature, and anyone can verify both against the public keys at [`/.well-known/jwks.json`](https://insumermodel.com/.well-known/jwks.json).

[Wallet trust profiles](https://insumermodel.com/developers/trust/) (`POST /v1/trust`) return 155 signed checks across 27 chains in 10 dimensions, up to 176 with optional Solana, XRPL, Bitcoin and Tron wallets. No score, no opinion: signed evidence, organized by dimension.

Chains: 31 EVM chains plus Solana, XRPL, Bitcoin, Tron, Stellar and Sui.

### Hosted MCP server

Connect any MCP client to `https://api.insumermodel.com/mcp`. No install, no key: ten tools on a shared daily allowance, for attestations, wallet trust profiles, merchant and discount checks, and signing keys. Run [`mcp-server-insumer`](https://github.com/insumerapi/mcp-server-insumer) locally with your own key for all 27 tools.

### Get a free key

```bash
curl -X POST \
  https://api.insumermodel.com/v1/keys/create \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com", "appName": "my-app", "tier": "free"}'
```

Returns an `insr_live_...` key instantly, shown once, so save it: 10 free verifications plus 100 requests a day.

### Pay per call with x402

No key, no account. x402 moves the money. InsumerAPI checks the conditions. Call `POST /v1/attest`, `/v1/trust` or `/v1/trust/batch` without a key, get a `402` with the price, pay in USDC on Base, Polygon, Arbitrum, Arc, or Solana, and retry with the payment. The payer is charged only for a successful answer, and the payer sees the answer only after the payment settled.

### Verify the signatures

Every result verifies against the published public keys, offline with a saved copy of them. [`insumer-verify`](https://github.com/insumerapi/insumer-verify) checks the ES256 signature, the post-quantum ML-DSA-65 signature, condition hashes, block freshness and expiry. Same specification, same published test vectors, in JavaScript and Python.

```bash
npm install insumer-verify
pip install "insumer-verify[pq]"
```

### Repositories

**For agents**
- [mcp-server-insumer](https://github.com/insumerapi/mcp-server-insumer): MCP server, 27 tools, on the official MCP Registry as `com.insumermodel/insumer`
- [insumer-agent-skills](https://github.com/insumerapi/insumer-agent-skills): six skills in the `SKILL.md` format for Claude Code, Cursor, Copilot, Codex and other agents
- [insumer-skill](https://github.com/insumerapi/insumer-skill): Claude Code authoring skill
- [langchain-insumer](https://github.com/insumerapi/langchain-insumer): LangChain toolkit, 26 tools ([PyPI](https://pypi.org/project/langchain-insumer/))
- [llama-index-tools-insumer](https://github.com/insumerapi/llama-index-tools-insumer): LlamaIndex tool spec ([PyPI](https://pypi.org/project/llama-index-tools-insumer/))
- [eliza-plugin-insumer](https://github.com/insumerapi/eliza-plugin-insumer): ElizaOS plugin, 10 actions ([npm](https://www.npmjs.com/package/@insumermodel/plugin-eliza))

**For builders**
- [insumer-verify](https://github.com/insumerapi/insumer-verify): signature verifier for Node.js and Python ([npm](https://www.npmjs.com/package/insumer-verify), [PyPI](https://pypi.org/project/insumer-verify/))
- [wdk-protocol-wallet-auth](https://github.com/insumerapi/wdk-protocol-wallet-auth): WDK protocol module for wallet auth ([npm](https://www.npmjs.com/package/@insumermodel/wdk-protocol-wallet-auth))
- [mppx-condition-gate](https://github.com/insumerapi/mppx-condition-gate): condition-based access for MPP routes ([npm](https://www.npmjs.com/package/@insumermodel/mppx-condition-gate))
- [insumer-examples](https://github.com/insumerapi/insumer-examples): code examples, the multi-attestation spec, on-chain verifier contracts
- [acp-ucp-onchain-eligibility-example](https://github.com/insumerapi/acp-ucp-onchain-eligibility-example): ACP and UCP examples for agent commerce

### Links

- [Developers](https://insumermodel.com/developers/): API keys, pricing, docs
- [OpenAPI spec](https://insumermodel.com/openapi.yaml) and [llms.txt](https://insumermodel.com/llms.txt)
- [AI agent guide](https://insumermodel.com/ai-agent-verification-api/): chains, trust profiles, commerce and signatures
- [XRPL live demo](https://insumermodel.com/demos/xrpl/): a real RLUSD trust-line attestation in the browser
- [insumermodel.com](https://insumermodel.com)

[Skye Meta](https://github.com/skyemeta) builds on InsumerAPI.
