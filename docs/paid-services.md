# Paid API Services

Use the installed CLIs directly. `awal` covers Base x402 services; `tempo` covers
Tempo/MPP services. Both work for Codex and Claude when running as the current
macOS user.

## Base x402

Search and inspect without paying:

```bash
awal x402 bazaar search "web search" --network base --max-price 0.01
awal x402 bazaar list --network base
awal x402 details <ENDPOINT_URL> --json
```

Call the selected endpoint with an explicit cap:

```bash
awal x402 pay <ENDPOINT_URL> --max-amount 10000 --json
```

`--max-amount` uses USDC atomic units: `10000` is $0.01, `100000` is $0.10,
and `500000` is $0.50. Add `-q '{"key":"value"}'` for query parameters or
`-X POST -d '{"key":"value"}'` for a JSON request body.

## Tempo/MPP

Search the service directory, then inspect a candidate before constructing the
request:

```bash
tempo wallet services --search "web search" -t --network tempo
tempo wallet services <SERVICE_ID> -t --network tempo
```

Use the exact URL, method, path, and input schema returned by the service
details. Preview the payment challenge before paying:

```bash
tempo request <SERVICE_URL>/<ENDPOINT_PATH> --network tempo --dry-run --max-spend 0.01
tempo request <SERVICE_URL>/<ENDPOINT_PATH> --network tempo --max-spend 0.01
```

`--network tempo` is mainnet and spends real funds; `--network tempo-moderato` is
testnet. Always pass it explicitly — never rely on the default.

For a POST endpoint:

```bash
tempo request <SERVICE_URL>/<ENDPOINT_PATH> -X POST --network tempo \
  --json '{"input":"..."}' --dry-run --max-spend 0.01
```

## Safety

- Confirm the wallet is signed in and funded before the first purchase of a
  session. Discovering this mid-payment wastes a call and obscures the failure.
- Pass `--network` explicitly on every `tempo` command.
- In `awal x402 pay`, `-h` is `--headers`, not help — every other awal command
  uses `-h` for help. Use `--help` there.
- Search first and never guess an endpoint path or request schema.
- Inspect the price before paying and use the smallest practical command-level cap.
- Search and inspection do not authorize a purchase. Pay only when it is part of
  the current request and within its stated scope.
- If the price is unclear, exceeds the requested cap, or a wallet limit is hit,
  stop and report it.
- Never print, copy, or commit wallet credentials, private keys, or session data.

## When this file is not enough

Both agents have skills installed for flows beyond the commands above — sign-in,
funding and onramp, balances, swaps, monetizing an endpoint, and onchain queries.
Load the relevant one rather than improvising:

- `agentic-wallet` — the `awal` CLI and Base/x402.
- `tempo-request` — the `tempo` CLI, service discovery, and MPP payments.
