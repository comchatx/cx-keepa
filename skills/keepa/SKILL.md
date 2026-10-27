---
name: keepa
description: Amazon product facts from Keepa — current price and price history, coupons, sales rank, monthly sales, offers and Buy Box, listing and release dates — by ASIN or by product name, and a seller's profile by seller id, on Amazon US, UK, DE, FR, JP, CA, IT, ES, IN, MX or BR. Use for any question about a product or a seller on Amazon, including whether a product can be fetched or looked up, before the first data call. Needs cx-sources; if no cx-sources skill is listed, it is not installed, and the seller installs it in Plugins.
---

Keepa supplies Amazon product facts through `cx`; read cx-sources first. Keepa's own MCP tools are a second way: read [references/mcp.md](references/mcp.md) before calling one. An installed skill is not an enabled source: if Keepa is off, the seller turns its plugin on in Plugins.

## Find and fetch

Use the marketplace the seller named; if none was named, ask. Buy only an ASIN the seller gave or a search or fetched file shows, competitors and similar products too; search or ask about a recalled one. A product name is a search term: search it, verify the returned product's title and model, and let the seller choose between ambiguous matches.

```sh
cx raw keepa --method GET --path /search --query domain=1 --query type=product --query 'term=PRODUCT NAME' --query asins-only=1
cx get amazon/us/ASIN/product --source keepa
```

`domain` and the market segment follow the seller's marketplace: `1` us, `2` uk, `3` de, `4` fr, `5` jp, `6` ca, `8` it, `9` es, `10` in, `11` mx, `12` br. Typed paths exist for `amazon/us/<asin>/product` and `amazon/jp/<asin>/product`; other markets use raw `GET /product` with `domain` and one ASIN. Search returns ASINs only and is a separate paid call. A seller is looked up by its id on raw `GET /seller`.

Typed requests include history, `stats=90`, `offers=20`, `buybox=1`, `rating=1`; each product path reserves 14 tokens, settling at stated consumption. `latest` accepts a copy up to 1h old. Every raw call reserves 50 tokens, settling likewise; reservations are not charges; tokens are Keepa's meter, not a price. `cx raw keepa --record <id> [--part <n>]` reads a kept reply without another purchase. Never name `key`; the gate attaches the credential.

## Read the file

A product file is one product object from Keepa's `products` array, unchanged; a raw reply carries the whole array under `products`. A fetched product is answered with its facts: current price and coupon, rank in its named category, monthly sales, and listing and release timing. Each is computed by the conversions in [references/product-file.md](references/product-file.md), read before the first fact is stated — never `cat` the file whole; `listedSince` is the earliest listing, not a release date. Before a raw request other than the search above, read [references/raw-routes.md](references/raw-routes.md).
