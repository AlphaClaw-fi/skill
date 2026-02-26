## OpenClaw Integration

OpenClaw supports Factor MCP through direct stdio execution. To integrate:

### 1. Install Factor MCP

```bash
curl -sSL https://raw.githubusercontent.com/FactorDAO/factor-mcp/main/install.sh | bash
```

### 2. Create the OpenClaw Skill

```bash
mkdir -p ~/.openclaw/workspace/skills/factor

cat > ~/.openclaw/workspace/skills/factor/SKILL.md << 'EOF'
---
name: factor
description: Manage DeFi vaults on Factor Protocol. Use when creating vaults, depositing/withdrawing assets, lending, borrowing, swapping, or executing strategies on Factor Pro Vaults across Arbitrum, Base, and Ethereum mainnet.
---

# Factor Protocol Skill

Tools for interacting with Factor Protocol vaults.

## Key Tools

| Tool | Purpose |
|------|---------|
| `factor_get_config` | Check current configuration |
| `factor_wallet_setup` | Import or generate wallet |
| `factor_create_vault` | Deploy a new Pro Vault |
| `factor_deposit` | Deposit assets into a vault |
| `factor_withdraw` | Withdraw assets from a vault |
| `factor_lend_supply` | Supply to lending protocols (Aave, Compound, Morpho) |
| `factor_lend_borrow` | Borrow from lending protocols |
| `factor_execute_manager` | Execute strategy steps (swaps, etc.) |

## Usage

Factor tools are called via JSON-RPC over stdio:

```bash
echo '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"factor_get_config","arguments":{}}}' | factor-mcp
```

See full documentation at `~/.factor-mcp/SKILL.md`.
EOF
```

### 3. Add Skills Directory to OpenClaw Config

Edit `~/.openclaw/openclaw.json` and add the `load` section under `skills`:

```json
{
  "skills": {
    "install": {
      "nodeManager": "npm"
    },
    "load": {
      "extraDirs": ["/home/parallels/.openclaw/workspace/skills"],
      "watch": true
    }
  }
}
```

### 4. Restart OpenClaw Gateway

```bash
openclaw gateway restart
```

Or if restart is disabled in your config, stop and start manually:
```bash
openclaw gateway stop
openclaw gateway start
```

### 5. Verify Installation

```bash
openclaw skills list | grep factor
```

You should see: `📦 factor` with status `✓ ready`
