# 10 — E-commerce Channels

Integrations that connect external sales channels/marketplaces to the ERP for centralized order + inventory sync.

## ecommerce/manage/setting — Channel settings
- Connect/configure marketplaces and social channels. Observed: Shopee, Tiktok (Shop), Lazada, Tiki, Facebook (and similar).
- Per-channel: connection/auth status, shop name/id, sync toggles (orders, products, stock, price), depot mapping, last-sync timestamp.

### Business logic
- OAuth/token-based connection to each marketplace (auth performed by the user/owner, not auto-filled).
- Product/listing mapping: internal product -> channel listing (SKU mapping).
- Inbound: channel orders import into order/manage as orders tagged with the source channel.
- Outbound: stock and price changes push to mapped listings.
- Stock is shared from depot on-hand; overselling guarded by sync rules.

## Entities (for schema)
- Channel (id, businessId, type, name, shopId, status, depotId, syncOrders, syncStock, syncPrice, lastSyncAt)
- ChannelListing (id, channelId, productId, variantId, channelSku, channelPrice, channelStock, listingStatus)
- ChannelOrderMap (id, channelId, channelOrderId, orderId, importedAt)
