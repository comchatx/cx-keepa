# Keepa's own MCP tools

Keepa runs an MCP server, and it is carried here whole: the tools, their descriptions and the token price each description states are Keepa's words. Its tools are named `mcp__keepa__<name>`.

## Calling a tool

Call a tool from a codemode script, as `await tools.mcp__keepa__<name>({ ... })`; the result arrives whole there. Called directly, outside a script, a tool answers `Tool mcp__keepa__<name> not found`. A script told `tools.mcp__keepa__<name> does not exist` means Keepa's server did not connect when the session started.

## Money

A call reserves the most its arguments can cost and settles at what the reply states in `apiTokens.consumedByThisCall`. `product_finder_fields` and `get_price_graph` reply without that figure and settle 0 and 1 tokens. A reservation is not a charge.

A refusal starting `MCP HTTP request failed with status 402` is the wallet's floor, not Keepa's: the balance is under the call's reservation.

Closed on the platform's key, and refused before any spend: `tracking_add`, `tracking_remove`, `tracking_list`, `tracking_get_notifications`, `tracking_set_webhook`. `get_products` by `codes` is refused the same way; look a product up by ASIN.

## The two ways to a product

| | `cx get amazon/<market>/<asin>/product` | `get_products` |
|---|---|---|
| Buys | history, statistics, offers, Buy Box and rating, always | the product; Buy Box and offers only when the arguments ask |
| Settled in the trial | 6 tokens | 1 token |
| Lands | an observation, cited by its path and reused while fresh | nothing; the reply is the script's result |

In the trial the MCP way was faster and cheaper per read, and most of its prompt went to finding the tools.

## What the compact reply leaves out

`get_products` replies in a compact form that carries `listedSince` and no release date. A release date comes only from `get_products` with `raw: true`, which returns Keepa's product object; read it by the conversions in [product-file.md](product-file.md). A seller is looked up by `get_seller` with its seller id.
