# Changelog

## 0.2.0

- The skill buys a product through the raw door, `cx raw keepa --path /product`, asking for history, statistics, offers, the Buy Box and the rating, and says to leave out what a question does not need. It no longer names `cx get` or the typed product paths, and says where a product fetched before is found.
- The MCP reference no longer compares the typed path with `get_products`.
- The package no longer names typed aspects for the Plugins page.

## 0.1.16

- The README no longer says that Usage lists every purchase.

## 0.1.15

- Under "What it costs", the README says both cases: billed to your ComChatX balance, or, once you save your own Keepa API Key, by Keepa instead.
- The MCP reference says `get_products` by `codes` is refused on the platform's key only.
- The tarball test reads `npm pack --json` as an array or as an object keyed by the package's name.

## 0.1.14

- The tile's mark on the Plugins page is the signed drawing.

## 0.1.13

- The own-key line says "API Key", as the Plugins page does: "Usage is billed to your balance. You can also use your own Keepa API Key." / 「按用量从余额扣费，也可以使用你自己的 Keepa API Key。」
- The MCP reference says that the five `tracking_*` tools, closed on the platform's key, are open on a key the seller saved for Keepa, and that a call on that key reserves and settles nothing on the balance.
- `DEVELOPING.md`: the install line, how the package works, and how to build, test and release it.

## 0.1.12

- The `rank-drop`, `top-ten` and `year-on-year` templates each send their Try it sentence, word for word, then the words you type after the template's name.

## 0.1.11

- The README's first paragraph, in Chinese, is the one signed on 2026-11-18 (「竞品和你的商品哪天调价、……cx 都能查到：Keepa 记着每个商品逐日的历史。……覆盖亚马逊 11 个站点。」), and in English the paragraph of the same meaning ("… from the day-by-day history Keepa keeps for each product. … It all comes from Keepa, on 11 Amazon marketplaces."). It no longer names Scheduled Tasks.
- The README's "What it needs" section leaves, in both languages, heading and all, as signed on 2026-11-18. The Plugins page draws what installs with it.
- `comchatx.builds` names two optional plugins, each with the reason signed on 2026-11-18: `@comchatx/cx-schedule`, "Reviews your products and rivals' on a schedule." / 「用于按你定的时间，替你复盘竞品和你的商品。」, and `cx-amazon-seller`, "Tells cx from the start that you sell on Amazon." / 「用于让 cx 从第一条消息起就知道你在亚马逊开店。」
- The skill's line is the one signed on 2026-11-17, in `comchatx.skills.keepa` as `zh` 「查商品的价格、排名、Buy Box 和销量历史，也能查卖家。」 and `en` "Looks up a product's price, rank, Buy Box and sales history, and sellers.". `SKILL.md`'s `description` is unchanged.
- The three templates read the lines signed on 2026-11-17, in English (`description`) and Chinese (`comchatx.prompts.<name>.zh`): `rank-drop` "Find the day [ASIN]'s rank turned on Amazon US, from its price, Buy Box and stock history" / 「从价格、Buy Box 和库存记录中，找出【ASIN】在美国站排名开始变化的那一天」; `top-ten` "Compare the ten best sellers for [search term] on Amazon US in one table" / 「把亚马逊美国站【搜索词】的十款畅销商品放进一张表格对比」; `year-on-year` "See whether [ASIN] sells more or less on Amazon US than at this time last year" / 「查看【ASIN】在美国站的销量比去年同期高还是低」.

## 0.1.10

- The card's name is the one signed on 2026-11-17: "Keepa API" on the English page (`comchatx.name`) and 「Keepa 数据接口」 on the Chinese page (`comchatx.nameZh`).
- The README, in English and Chinese, calls each plugin of ours it names by the card's name signed on 2026-11-17.
- The README names each other plugin it speaks of by the card's name in its own language, a link to that plugin's page on npm, never by the npm name.

## 0.1.9

- The Chinese README's heading 「依赖」 reads 「需要」, beside the English "What it needs": 「依赖」 is a developer's word, and no line on the Plugins page uses it.

## 0.1.8

- The card line says what cx does with Keepa: "Tracks and reviews price, BSR and Buy Box, yours and rivals'." in English, as npm's `description`, and 「跟踪竞品和你的商品的价格、BSR 和 Buy Box，替你复盘。」 in Chinese, as `comchatx.about`, which takes the place of `comchatx.description.zh`. They said "Amazon price and BSR history." and 「亚马逊商品的历史价格和 BSR 走势。」.
- The README, in English and Chinese, opens on what cx finds out and does with Keepa's data, with cx-schedule for reviews on your schedule, and names the data last. It has no title, since the Plugins page prints the name; its three questions are new and none repeats a prompt the page shows; the marketplaces have their own section; and the cost names each lookup beside each product read.

## 0.1.7

- cx judges when to ask you, as ComChatX's decision D233 rules. The skill states the facts its asks rested on: each marketplace is its own purchase and one you did not name is not known, a product recalled from memory is searched before it is bought, and one name can match more than one product. The README no longer promises that cx asks you to choose.
- A charge to your balance is 扣费 in Chinese, in the README and on the source's line, as the rest of ComChatX says it.
