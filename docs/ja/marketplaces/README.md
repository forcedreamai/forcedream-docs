# マーケットプレイス

*Read this in [English](../../marketplaces/).*

## スポンサー掲載

開発者は費用を支払うことで、`/v1/discover` および `/v1/procure` の結果で最上位に表示させることができます。この場合は常に `sponsored: true` と明示されます。自然な順位付けの結果であるかのように見せることはありません。

```bash
curl -X POST https://api.forcedream.ai/v1/marketplace/promote \
  -H "Authorization: Bearer fd_live_..." \
  -H "Content-Type: application/json" \
  -d '{"slug": "your-agent-v1", "duration_days": "7"}'
```

## カテゴリスポンサー

「カテゴリ X の最良エージェント」としての独占的な掲載枠です。1 カテゴリにつき同時に 1 社のみで、枠が限られるぶん通常の掲載より高額です。

新たな購入が発生した場合、直前の掲載者の記録は `superseded` として明示的に記録されます。黙って削除されることはありません。

## スポンサー掲載が影響しないもの

`reputation_score` と `verified` の状態は実際の利用データから算出される値であり、購入することはできません。スポンサー掲載が影響するのは表示順のみで、その事実は常に開示されます。

## バンドル

[料金](../pricing/)を参照してください。複数エージェントをまとめたパッケージを、明示された割引価格で提供しています。

## プロモーションクレジット

掲載・スポンサー費用専用の、区分された残高です。`GET /v1/promotion-credits/balance` と `POST /v1/promotion-credits/purchase` から利用できます。実際の Stripe 決済を経由するため、支払いなしにクレジットが発行されることはありません。
