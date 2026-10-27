# Keepa raw routes

`domain`: `1` us, `2` uk, `3` de, `4` fr, `5` jp, `6` ca, `8` it, `9` es, `10` in, `11` mx, `12` br. Other Amazon marketplaces are unavailable here.

| Route | Parameters |
|---|---|
| `GET /product` | `domain`, `asin` (one ASIN only, never a list); optional `stats`, `update`, `history`, `days`, `offers`, `only-live-offers`, `rating`, `buybox`, `stock`, `aplus`, `videos` |
| `GET /query` | `domain`, `selection` (finder JSON) |
| `POST /query` | `domain`; finder selection in the `--body` file |
| `GET /search` | `domain`, `type=product`, `term`, `asins-only=1`, optional `page` |
| `GET /seller` | `domain`, `seller`, optional `storefront` |
| `GET /category` | `domain`, `category`, optional `parents` |
| `GET /bestsellers` | `domain`, `category`, optional `range` |
| `GET /deal` | `selection` (deal JSON) |
| `POST /deal` | deal selection in the `--body` file |

Never name `key`; the gate attaches the credential. Admission refusals state the parameter rule before spending. A route outside this table is unavailable. Raw `/product` accepts one ASIN only; typed product asks handle lists. A failed typed request may still name kept replies; `cx raw keepa --record <id> [--part <n>]` reads one back, part 0 first.

## Request units

`stats` is in days; `update` is in hours; `category` is one category id on `/bestsellers` and up to ten comma-separated ids on `/category`. `update=0` asks for a refresh and may cost one more token.

## Retained raw consumption

These amounts record past replies, not a price list for future requests. Every raw request reserves 50 tokens; settlement uses the supplier's stated consumption.

| Route | Retained consumption |
|---|---|
| `/query` finder page | 11 tokens |
| `/search` | 10 tokens |
| `/seller` with storefront | 10 tokens |
| `/bestsellers` | 50 tokens |
| `/deal` page | 5 tokens |

Raw `/product` consumption depends on requested data; do not infer a fixed charge from the raw reservation.
