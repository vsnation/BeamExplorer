# BEAM Explorer

A modern, feature-rich blockchain explorer front end for the [BEAM](https://beam.mw) privacy blockchain. Built as a single-page application with no server required - just open the HTML file in your browser.

![BEAM Explorer](https://img.shields.io/badge/BEAM-Explorer-25c2a0?style=for-the-badge)
![No Dependencies](https://img.shields.io/badge/Dependencies-None-green?style=for-the-badge)

## Features

- **No Server Required** - Works directly in your browser
- **Multiple Node Support** - Connect to any BEAM Explorer API node
- **Real-time Data** - Live blockchain statistics
- **Elegant Design** - Dark theme optimized for privacy-focused users
- **Mobile Responsive** - Works on all devices
- **Zero Dependencies** - Pure HTML, SVG, CSS and JavaScript
- **In-wallet dApp** - Packaged to run as a dApp in the BEAM desktop wallet 

## Quick Start

1. Download `BeamExplorer.html`
2. Open it in your web browser.
3. That's it! No installation, no build process, no server needed.

```bash
# Or clone and open
git clone https://github.com/user/beam-explorer.git
open BeamExplorer.html
```

## Pages

### Home
The dashboard showing key blockchain statistics:
- **Block Height** - Current blockchain height
- **Peers Connected** - Active network peers
- **Chainwork** - Total proof of work
- **Shielded Outputs (24h)** - Privacy transactions in last 24 hours
- **Total Shielded Outputs** - All-time shielded transactions
- **Recent Blocks** - Latest blocks with quick stats

### Blocks

Browse all blocks with customizable columns:

| Column Group | Available Columns |
|--------------|-------------------|
| **Basic** | Height, Timestamp, Hash, Difficulty, Chainwork |
| **Transactions** | Txs, Total Txs, Fee, Total Fee |
| **Mimblewimble** | MW Inputs, MW Outputs, UTXOs (block & total) |
| **Shielded Pool** | Shielded Inputs, Shielded Outputs (block & total) |
| **Contracts** | Contracts Deployed, Contract Calls (block & total) |
| **Size** | Compressed Size, Archive Size (delta & total) |

Click any block to view:
- Block hash and previous block link
- Difficulty and chainwork
- Block subsidy and BEAM/USD rate
- All inputs (with type, commitment, value, maturity)
- All outputs (with commitment, asset range, spent status)
- All kernels (with ID, fee, height range)

### Assets
Browse all Confidential Assets (CAs) on BEAM:
- **Search/Filter** - Find by name, symbol, or asset ID
- **Card View** - Shows name, symbol, total supply, locked amount

Click any asset to view:
- Total supply and decimals
- Lock height and owner contract
- **Distribution** - Which contracts hold this asset
- **Liquidity Pools** - All DEX pools containing this asset with reserves, rates, LP token info
- **History** - Mint, burn, and create events

### DEX (Decentralized Exchange)
Explore the BEAM AMM (Automated Market Maker):

**Pools Tab:**
- All active liquidity pools
- Token pair with reserves
- Volatility tier (High/Medium/Low)
- LP Token ID and supply
- Exchange rate

**Trades Tab:**
- Recent trades with infinite scroll
- Trade type (Trade, AddLiquidity, Withdraw)
- Input/Output tokens and amounts
- Click to view block details

### Contracts
Browse all smart contracts (Shaders) on BEAM:
- **Search/Filter** - Find by contract ID or type
- **Quick Stats** - Locked funds count, owned assets count

Click any contract to view:
- Full contract ID with copy button
- Contract type and deploy height
- **Locked Funds** - Assets held by the contract
- **Owned Assets** - Assets created by the contract
- **Liquidity Pools** - For DEX contracts, shows all pools
- **Recent Calls** - Contract interaction history

### Atomic Swaps

Cross-chain trading statistics:
- **Total Swaps** - All-time swap count
- **Supported Coins** - BTC, ETH, LTC, DOGE, DASH, USDT, DAI, WBTC, QTUM
- **Active Offers** - Current swap offers (if any)

## Node Selection

The explorer supports multiple BEAM API nodes:

| Node | URL |
|------|-----|
| explorer.0xmx.net | `https://explorer.0xmx.net/api` |
| explorer-api.beamprivacy.com | `https://explorer-api.beamprivacy.com` |
| BeamSmart.net | `https://BeamSmart.net:8000` |
| **Custom** | Enter any compatible API URL |

The connection status indicator shows:
- 🟢 Green = Connected
- 🔴 Red = Disconnected

## Running your own node

### Setup

- Download the latest explorer node binary for your platform (Windows, Linux or Mac) from https://github.com/BeamMW/beam/releases/.

- Open the `explorer-node.cfg` file and input some peer URLs (for the initial connection to the network), and two available ports to run your node and its API:

  ```
  # peer address
  peer=us-nodes.mainnet.beam.mw:8100,eu-nodes.mainnet.beam.mw:8100
  
  # port to start the local node on
  port=1000
  
  # port to start the local api server on
  api_port=8888
  
  # old logs cleanup period (days)
  log_cleanup_days=5
  ```

### Initial sync

- Launch the explorer node and let it sync. Depending on your internet connection, this initial sync might take several hours. It will create a `explorer-node.db` file of about 45Gb.
- *Remark*: For the explorer node to be able to provide human-readable smart contract information, download the `Parser.wasm` file from https://github.com/BeamMW/beam/tree/master/bvm/Shaders/Explorer and run the following command once:
  - Linux/Mac: `./explorer-node --contract_rich_parser=Parser.wasm`
  - Windows: `explorer-node.exe --contract_rich_parser=Parser.wasm`

### Frontend

- With the node synced and running, open this explorer HTML frontend, click on the node selector button and select `Custom Node...` in the dropdown menu.
- Enter the URL of your local node, including the API port you previously defined (e.g. `http://localhost:8888`).
- Done! You are now running a **fully local and private Beam explorer**, both frontend *and* node!

## API Reference

The explorer uses the BEAM Explorer API. Here are the key endpoints:

### Status
```
GET /status
```
Returns blockchain status including height, peers, shielded stats.

### Blocks
```
GET /hdrs?nMax=50&cols=HTdkfiop&exp_am=1
```
Returns blocks in table format. Use `cols` parameter to select columns:

| Code | Column | Code | Column |
|------|--------|------|--------|
| H | Height | i | MW.Inputs |
| T | Timestamp | o | MW.Outputs |
| h | Hash | y | SH.Inputs |
| d | Difficulty | z | SH.Outputs |
| D | Chainwork | p | ContractCalls |
| f | Fee | c | Size.Compressed |
| k | Txs | a | Size.Archive |

### Block Detail
```
GET /block?height=123456
```
Returns full block data including inputs, outputs, and kernels.

### Assets
```
GET /assets
GET /asset?id=174&nMaxOps=100
```
List all assets or get single asset with history.

### Contracts
```
GET /contracts
GET /contract?id={cid}&state=1&nMaxTxs=100
```
List all contracts or get single contract with state and call history.

### Atomic Swaps
```
GET /swap_totals
GET /swap_offers
```
Get swap statistics and active offers.

## Understanding BEAM Concepts

### Mimblewimble
A privacy protocol that hides transaction amounts and addresses while still allowing verification. BEAM uses this as its core technology.

### Confidential Assets (CAs)
Custom tokens created on the BEAM blockchain. Each has an Asset ID (BEAM itself is Asset ID 0). All CAs inherit BEAM's privacy features.

### Shielded Pool
An enhanced privacy feature using Lelantus-MW protocol. Shielded transactions provide even stronger privacy guarantees.

### DEX (Decentralized Exchange)
BEAM's built-in AMM allows trading between BEAM and any Confidential Asset directly on-chain, without intermediaries.

### Atomic Swaps
Trustless cross-chain exchanges between BEAM and other cryptocurrencies (BTC, ETH, etc.) without requiring a centralized exchange.

### Kernels
Transaction kernels are the cryptographic proofs that validate BEAM transactions without revealing amounts or addresses.

## Technical Details

### Response Format
The API returns data in "TABLE" format:
```javascript
{
    "type": "table",
    "value": [
        [/* headers */],
        [/* row 1 */],
        [/* row 2 */],
        // ...
    ],
    "more": { "hMax": 12345 }  // pagination
}
```

### Cell Types
Table cells can be typed objects:
- `{"type": "aid", "value": 174}` - Asset ID
- `{"type": "cid", "value": "..."}` - Contract ID
- `{"type": "amount", "value": 1000000}` - Amount (in groth)
- `{"type": "height", "value": 123}` - Block height
- `{"type": "time", "value": 1234567890}` - Unix timestamp

### Units
- **BEAM**: 1 BEAM = 100,000,000 groth (8 decimals)
- **Amounts**: All amounts are in smallest units (groth for BEAM)

## Browser Compatibility

Works in all modern browsers:
- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

MIT License - feel free to use, modify, and distribute.

## Links

- [BEAM Official Website](https://beam.mw)
- [BEAM GitHub](https://github.com/BeamMW)
- [BEAM Documentation](https://documentation.beam.mw)
- [BEAM Discord](https://discord.gg/beam)

---

Built with privacy in mind for the BEAM community.
