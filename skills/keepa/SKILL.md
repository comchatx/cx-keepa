---
name: keepa
description: Amazon product facts from Keepa — current price and price history, coupons, sales rank, monthly sales, offers and Buy Box, listing and release dates — by ASIN or by product name, and a seller's profile by seller id, on Amazon US, UK, DE, FR, JP, CA, IT, ES, IN, MX or BR. Use for any question about a product or a seller on Amazon, including whether a product can be fetched or looked up, before the first data call. Needs cx-sources; if no cx-sources skill is listed, it is not installed, and the seller installs it in Plugins.
---

Keepa supplies Amazon product facts through `cx`; read cx-sources first. Keepa's own MCP tools are a second way: read [references/mcp.md](references/mcp.md) before calling one. An installed skill is not an enabled source: if Keepa is off, the seller turns its plugin on in Plugins.

## Find and fetch

Use the marketplace the seller named: each marketplace is its own `domain` and its own purchase (below), and one the seller did not name is not known. Buy only an ASIN the seller gave or a search or fetched file shows, competitors and similar products too; a recalled one is searched before it is bought. A product name is a search term: search it and verify the returned product's title and model, since one name can match more than one product.

```sh
cx raw keepa --method GET --path /search --query domain=1 --query type=product --query 'term=PRODUCT NAME' --query asins-only=1
cx raw keepa --method GET --path /product --query domain=1 --query asin=ASIN --query stats=90 --query history=1 --query offers=20 --query buybox=1 --query rating=1
```

`domain` follows the seller's marketplace: `1` us, `2` uk, `3` de, `4` fr, `5` jp, `6` ca, `8` it, `9` es, `10` in, `11` mx, `12` br. `GET /product` takes one ASIN a call. Search returns ASINs only and is a separate paid call. A seller is looked up by its id on raw `GET /seller`.

The product call above asks for history, 90 days of statistics, 20 offers, the Buy Box and the rating; leave out what the question does not need, since each part asked for can raise what Keepa states it consumed. A product fetched before is found as cx-sources says, by its ASIN in the `.ask.json` records, and one fetched by an earlier version is under `/tenant/amazon/<market>/<asin>/`, dated in its `meta.json`. Every call reserves 50 tokens and settles at the consumption Keepa states; reservations are not charges; tokens are Keepa's meter, not a price. `cx raw keepa --record <id> [--part <n>]` reads a kept reply without another purchase. Never name `key`; the gate attaches the credential.

## Read the file

A product reply carries Keepa's product objects, unchanged, as an array under `products`; a `product.json` an earlier version fetched is one such object. A fetched product is answered with its facts: current price and coupon, rank in its named category, monthly sales, and listing and release timing. Each is computed by the conversions in [references/product-file.md](references/product-file.md), read before the first fact is stated — never `cat` the file whole; `listedSince` is the earliest listing, not a release date. Before a raw request other than the two above, read [references/raw-routes.md](references/raw-routes.md).
