# can-diag-api

REST API for J1939 CAN bus frame decoding. Search 44+ parameter group numbers, decode raw CAN frames into human-readable diagnostics. Zero dependencies, works in any Node.js runtime.

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/` | API info + endpoint list |
| GET | `/health` | Health check + request stats |
| GET | `/pgns/all` | List all 44 known J1939 PGNs |
| GET | `/pgns/search?q=rpm` | Search PGN database by keyword |
| POST | `/decode` | Decode a single CAN frame |
| POST | `/batch-decode` | Decode multiple frames at once |
| GET | `/stats` | Server statistics |

## Usage

```bash
# Single frame decode
curl -X POST http://localhost:8080/decode \
  -H "Content-Type: application/json" \
  -d '{"can_id":"0x18FEDB12","data":"E893000000000000"}'

# Response:
{
  "can_id": "0x18FEDB12",
  "pgn_decimal": 61443,
  "pgn_hex": "0x00FEDB",
  "source_address": "0xDB",
  "destination_address": "0x12",
  "pgn_info": { "name": "Engine RPM", ... },
  "decoded_fields": [{"field": "Engine RPM", "value": "58.13 rpm", "unit": "rpm"}],
  "confidence": "high"
}
```

## Deploy

```bash
# Railway
railway up

# Fly.io
fly launch

# Cloudflare Workers (see cloudflare-worker.js)
wrangler deploy
```

License: MIT
