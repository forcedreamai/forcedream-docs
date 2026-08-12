# エージェントの実行

*Read this in [English](../../invoke/).*

## 成功時のみ課金される仕組み

課金が発生するのは、タスクが正常に完了し、スキーマに適合した出力が返された場合**のみ**です。失敗、空、または不正な結果に対する費用は £0 です。ForceDream の課金対象ルートはすべて単一の課金関数を経由しており、この保証は運用上の取り決めではなく構造として強制されています。

## リクエストとレスポンスの形

```bash
curl -X POST https://api.forcedream.ai/v1/agents/summarization-v1/invoke \
  -H "Authorization: Bearer fd_live_..." \
  -H "Content-Type: application/json" \
  -d '{"task": "...", "priority": "balanced"}'
```

即時のレスポンスは次のいずれかです。

- **`status: "pending"`** — 非同期の処理が進行中です。`GET /v1/agents/:slug/result/:task_id` をポーリングしてください。終了状態は `completed`、`failed`、`dead_letter`、`frozen` です。
- **`status: "succeeded"`** — 同期実行のエージェントのみで、まれです。結果と課金がすでに確定しています。

URL ではなくボディに `slug`（または `agent`）を渡す汎用の `POST /v1/invoke` もあります。内部のロジックは同一で、エンドポイントを 1 つに固定したい場合に利用できます。

## 優先度ティア — 価格と、現状の正直な説明

| ティア | 価格の倍率 |
|---|---|
| `cheapest` | 0.90 倍 |
| `balanced`（既定） | 1.00 倍 |
| `fastest` | 1.50 倍 |
| `quality` | 1.75 倍 |

**明示しておくべき制限:** `fastest` と `quality` は現時点で**価格にのみ**影響します。実際の実行優先度やキューの順序は変わりません。それにはディスパッチャー側の変更が必要で、まだ実装されていません。存在しない速度向上を示唆することがないよう、API のレスポンス自体にも `priority_note` フィールドとして明記されています。

## 任意: 1 日あたりの上限額

`max_daily_spend_pence` を渡すと、宣言した 1 日の上限に達した時点で ForceDream が新しい実行を拒否します。上限によって拒否された呼び出しには課金されません。

なお、上限をまたぐ処理の途中でその呼び出し自体を止めることはできません。制御できるのは、当日すでに完了した支出が上限に達した後の新規呼び出しのみです。
