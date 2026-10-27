# Reading a Keepa product file

Run these JavaScript conversions on the file's values. UTC is the time basis. `null`, `0` and `-1` are absent field dates, not dates to display; other negative Keepa minutes predate 2011.

## Clocks and units

Keepa minutes (`listedSince`, `trackingSince`, `lastUpdate`, field-update stamps and history entry times):
```js
const keepaDate = n => n == null || n === 0 || n === -1 ? null : new Date((n + 21564000) * 60000).toISOString();
```
Packed dates (`releaseDate`, `publicationDate`) keep their stated precision: YYYY, YYYYMM or YYYYMMDD:
```js
const calendarDate = n => n == null || n <= 0 ? null : String(n).match(/^\d{4}|\d{2}/g).join('-').replace(/-00.*$/, '');
```
The reply's `timestamp` is Unix milliseconds:
```js
const replyDate = n => n == null || n <= 0 ? null : new Date(n).toISOString();
```
`refillIn` is milliseconds until refill, not days; `refillRate` is tokens per minute:
```js
const refillSeconds = n => n == null || n < 0 ? null : n / 1000;
```
Prices are minor units, except Japan's whole yen; `0` is a real price/count, `-1` absent, `-2` undetermined:
```js
const price = (n, domainId) => n == null || n < 0 ? null : n / (domainId === 5 ? 1 : 100);
```
`coupon` is a pair, one-time then subscribe-and-save, with its own sign convention; never pass it to `price` directly:
```js
const couponPart = (n, domainId) => n == null || n === 0 ? null : n < 0 ? { percent: -n } : { amount: price(n, domainId) };
const coupon = (pair, domainId) => pair == null ? null : { oneTime: couponPart(pair[0], domainId), subscribeAndSave: couponPart(pair[1], domainId) };
```

`listedSince` is earliest listing, not release. Include listing and release timing in a product summary, stating absent dates plainly. For `releaseDate` `0` or `-1`, say “Keepa has no release date”. `lastUpdate: -1` is read here as no product data — the platform's reading, in no reply kept so far; the sidecar's arithmetic `as_of` is then not a valid observation time. `lastUpdate` records a check, `lastPriceChange` a change; an unchanged price is not stale just because its event is old. The sidecar's `as_of` is Keepa's `lastUpdate`, when Keepa last checked the product; tolerance is measured against `observed_at`, when the platform bought the observation. A newly bought observation need not contain newly checked product data.

## History and present state

`csv[i]` and `salesRanks[category]` are flat time/value pairs; delivered-price slots and `offerCSV` are time/price/shipping triples. Statistics use the same slot indices; `stats.min[i]` is `null` or time/value. Use Keepa's own statistics (`stats.min`, `stats.max`, `stats.isLowest`, `stats.outOfStockPercentage90`, `stats.salesRankDrops30`, `stats.totalOfferCount`), not recomputed extrema from change-only history. `stats.salesRankDrops30` is `-1` with no rank data; `0` is a real count. `stats.outOfStockPercentage90[i]` is `-1` with too little data or a non-price slot; `0` is never out of stock. Code converts the supplied values and dates; it does not reconstruct a 90-day low. A file without `stats` states the changes it holds, not a computed low. Unknown shipping stays unknown.

| Slots | Meaning |
|---|---|
| 0, 1, 2, 4, 5, 6, 8, 9, 10, 30, 33 | Amazon, new, used, list, collectible, refurbished, lightning, warehouse, FBA, trade-in, Prime prices |
| 7, 18, 19–22, 23–26, 27, 28, 29, 32 | FBM, Buy Box, used conditions, collectible conditions, refurbished, eBay new/used, used Buy Box; delivered prices |
| 3 | reference sales rank; `-1` is no rank at that time, as in `salesRanks[category]` |
| 11–14, 34, 35 | new/used/refurbished/collectible, FBA/FBM offer counts |
| 15 | offers fetched; negative means more existed, not absence |
| 16, 17 | rating in tenths (divide by 10 in code), rating count, not written reviews; `-1` in either is no data at that time, not zero |

The terminal entry is current, including a terminal absence. Do not revive a past positive price or a category ending in `-1`. Name rank categories from `categoryTree` where the id appears there; `salesRankReference` is not necessarily the leaf category. Rank drops are not unit sales. `monthlySold` is “at least” that many bought in the past month, not an exact estimate. With no current `monthlySold` there is no badge now, whatever the history held.

`couponHistory` is time/one-time/subscribe-and-save triples. An ended coupon means “no coupon now”, not the last discount. A null coupon is no current coupon reported.

Only `liveOffersOrder` indices identify current offers. If requested offers failed (`offersSuccessful: false`), their absence proves nothing. A null, `"-1"` or `"-2"` seller id in `stats.buyBoxSellerId` or `buyBoxSellerIdHistory` identifies no seller. Eligible offers are not a Buy Box winner. `stats.buyBoxPrice` is the item's price and `stats.buyBoxShipping` its shipping; `stats.current[18]` is the two together. `availabilityAmazon: -1` means no Amazon offer, not no marketplace offers.

Dimensions/weights are millimetres/grams; `0` or `-1` means unavailable. `fbaFees.pickAndPackFee` is money. `productType: 4` is no data; `5` is a variation parent.

`images` lists the product's carousel images, `null` when there are none; each names up to two sizes where Keepa has them, a large file `l` and a medium file `m`, with heights and widths in pixels (`lH`, `lW`, `mH`, `mW`). An image is at `https://m.media-amazon.com/images/I/` followed by its file name. `videos` comes only on raw `GET /product` with `videos=1`, which a typed product file never asks for, and is `null` when Keepa has none; each video has a `title`, a `duration` in seconds, a cover `image` file name read like a carousel image, and a `url`. Keepa reads live video data only when `offers` is asked too.

## Codes

These codes describe the field, not the whole marketplace.

| Offer `condition` | Meaning |
|---|---|
| 0 | unknown |
| 1 | new |
| 2 | used, like new |
| 3 | used, very good |
| 4 | used, good |
| 5 | used, acceptable |
| 6 | refurbished |
| 7 | collectible, like new |
| 8 | collectible, very good |
| 9 | collectible, good |
| 10 | collectible, acceptable |
| 11 | rental |

| `availabilityAmazon` | Meaning |
|---|---|
| -1 | no Amazon offer, not no marketplace offers |
| 0 | in stock |
| 1 | preorderable |
| 2 | unknown |
| 3 | backorderable |
| 4 | delayed |

| `productType` | Meaning |
|---|---|
| 0 | standard |
| 1 | downloadable |
| 2 | ebook |
| 3 | inaccessible: no price, rating or offers data |
| 4 | no data (invalid or deprecated ASIN) |
| 5 | variation parent |
