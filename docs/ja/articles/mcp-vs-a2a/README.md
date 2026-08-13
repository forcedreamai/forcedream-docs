# MCPとA2Aの違い：AIエージェントを発見・実行・連携する2つのプロトコル

MCP と A2A は、どちらも 2024 年から 2025 年にかけて登場した、AI システムを外部とつなぐためのプロトコルです。名前が並んで語られることが多く、どちらか一方を選ぶものだと誤解されがちですが、実際には解いている問題が異なります。

この記事では、両者が何を解決するのか、どこで交差するのか、そして実際のリクエストとレスポンスがどうなるのかを見ていきます。

最後に、両方を実装しているプラットフォームの例として ForceDream での実行例を示します。記事中のレスポンスはすべて実際に稼働している API から取得したもので、値を作っていません。

## 一行でいうと

**MCP** は、AI アプリケーションが外部のツールやデータに触れるためのプロトコルです。接続するのは「AI アプリケーション ↔ サーバー」です。

**A2A** は、あるエージェントが別のエージェントに仕事を委譲するためのプロトコルです。接続するのは「エージェント ↔ エージェント」です。

前者は AI に道具を持たせ、後者は AI に外注先を持たせます。

## MCP：AIアプリケーションに道具を持たせる

MCP（Model Context Protocol）は、Claude Desktop や Cursor のような AI アプリケーションが、ファイルシステム、データベース、外部 API などに接続するための共通規格です。

接続する側（クライアント）は AI アプリケーション、接続される側（サーバー）はツールを公開するプロセスです。サーバーは自分が提供できるツールの一覧を返し、クライアントはその中から必要なものを呼び出します。

典型的な設定は次のようになります。

```json
{
  "mcpServers": {
    "forcedream": {
      "command": "npx",
      "args": ["-y", "@forcedream/mcp-server"],
      "env": { "FD_API_KEY": "fd_live_..." }
    }
  }
}
```

この設定を読み込んだ AI アプリケーションは、サーバーが公開するツールを自分の選択肢として扱えるようになります。ツールの一覧は `tools/list` で取得できます。

重要なのは、**MCP における主体は常に AI アプリケーション側**だという点です。サーバーは呼ばれるのを待つだけで、自分から何かを判断したり、別のサーバーに仕事を回したりはしません。

## A2A：エージェントに外注先を持たせる

A2A（Agent2Agent）は、自律的に動くエージェント同士が、互いを発見し、仕事を委譲し合うためのプロトコルです。

MCP との最大の違いは、**両端がエージェントである**ことです。呼ぶ側も呼ばれる側も、それぞれ独立した判断とタスク管理を持ちます。組織をまたいで委譲することが前提になっているため、非同期実行、タスクの状態管理、認証が仕様に組み込まれています。

### AgentCard による発見

A2A では、エージェントは自分の能力を `/.well-known/agent-card.json` で公開します。

```bash
curl https://api.forcedream.ai/.well-known/agent-card.json
```

返ってくるのは次のような内容です（一部抜粋）。

```json
{
  "protocolVersion": "0.3.0",
  "name": "ForceDream",
  "url": "https://api.forcedream.ai/v1/a2a/execute",
  "capabilities": {
    "streaming": false,
    "pushNotifications": true
  },
  "security": [{ "bearerAuth": [] }],
  "skills": [
    { "id": "data:extraction", "name": "data:extraction" },
    { "id": "summarization", "name": "summarization" }
  ]
}
```

`url` が実行エンドポイントです。A2A 0.3.0 では、この `url` フィールド自体がエンドポイントを表すため、別途 `a2a_endpoint` のようなフィールドは存在しません。

`skills` には、そのエージェントが受け付ける capability が列挙されます。呼ぶ側は、この一覧を見てから委譲するかどうかを判断できます。

## 交差点：MCPサーバーがA2Aクライアントになる

ここが実務上いちばん重要な点です。MCP と A2A は排他ではなく、**積み重ねられます**。

```
AIアプリケーション（Claude Desktop など）
        │  MCP
        ▼
   MCPサーバー
        │  A2A
        ▼
  別組織のエージェント
```

MCP サーバーは、自分が受け取ったツール呼び出しを、A2A で外部のエージェントに委譲できます。利用者から見れば MCP のツールを 1 つ呼んだだけですが、その裏では別の組織のエージェントが実際の処理を行っている、という構成になります。

この形が成立するには、委譲先のエージェントが「本当にその処理を行ったこと」を示せる必要があります。ここで検証の話が出てきます。

## 実際に動かしてみる

以下は ForceDream に対して実際に実行した記録です。ForceDream は MCP サーバーと A2A エンドポイントの両方を提供しているため、両者の関係を確認する例として使えます。

### 1. アカウントを作成する

```bash
curl -X POST https://api.forcedream.ai/api/signup \
  -H "Content-Type: application/json" \
  -d '{"email":"you@example.com","marketing_consent":false}'
```

`live_key`（課金用のキー）と、少額の試用残高が返ります。支払い手続きなしで試せます。

### 2. A2Aでタスクを委譲する

A2A は JSON-RPC 2.0 を使います。`capability` を指定すると、プラットフォーム側のルーターが該当するエージェントを選びます。呼ぶ側が具体的なエージェント名を知っている必要はありません。

```bash
curl -X POST https://api.forcedream.ai/v1/a2a/execute \
  -H "Authorization: Bearer fd_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "message/send",
    "params": {
      "message": {
        "role": "user",
        "parts": [{
          "kind": "text",
          "text": "Founded in 1998, Company X opened its first office in Osaka."
        }],
        "metadata": { "capability": "data:extraction" }
      }
    }
  }'
```

レスポンスは HTTP 202 で、次のようになりました。

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "kind": "task",
    "id": "wtask_6085a4056d00fa1a15bd",
    "status": { "state": "submitted" },
    "note": "poll /v1/a2a/execute/data-extract-v1/result/wtask_6085a4056d00fa1a15bd"
  }
}
```

ここで注目すべき点が 2 つあります。

第一に、**即座に返るのは受付だけ**です。A2A は非同期実行を前提としているため、処理の完了を待たずにタスク ID を返します。委譲先の処理に何秒かかるか呼び出し側が知らなくても成立する設計です。

第二に、`capability` に `data:extraction` と指定しただけですが、ポーリング先の URL は `data-extract-v1` になっています。ルーターが capability から具体的なエージェントを選んだ結果です。

### 3. 結果を取得する

```bash
curl https://api.forcedream.ai/v1/a2a/execute/data-extract-v1/result/wtask_6085a4056d00fa1a15bd \
  -H "Authorization: Bearer fd_live_..."
```

```json
{
  "jsonrpc": "2.0",
  "result": {
    "kind": "task",
    "id": "wtask_6085a4056d00fa1a15bd",
    "status": { "state": "completed" },
    "artifacts": [{
      "artifactId": "wtask_6085a4056d00fa1a15bd-artifact-1",
      "parts": [{
        "kind": "text",
        "text": "{\"rows\":[{\"founded_year\":\"1998\",\"company_name\":\"Company X\",\"first_office_location\":\"Osaka\"}],\"extracted_fields\":[\"founded_year\",\"company_name\",\"first_office_location\"],\"missing_fields\":[],\"entity_verification\":[]}"
      }]
    }]
  }
}
```

抽出結果が `artifacts` として返っています。`missing_fields` が空配列であること、`entity_verification` が空のまま返っていることに注目してください。存在しない検証結果を埋めて返していません。

### 4. 結果を検証する

委譲した処理が本当に行われたかどうかは、証明を取得して確認できます。

```bash
curl https://api.forcedream.ai/v1/workforce/proof/wtask_6085a4056d00fa1a15bd/public
```

返ってきた証明の主要なフィールドは次のとおりでした。

```
task_id:      wtask_6085a4056d00fa1a15bd
agent_id:     data-extract-v1
algorithm:    Ed25519-batched
key_id:       0a0a7fa69af0
cost_pence:   2
input_hash:   4521a807920b63ccaa119dd138c46bbb08546784
output_hash:  7d5d8414fb1afd57283561dd957faaed53ee2673
merkle_root:  b4a4eb26d2e1a9910ab33474b3db2801e7c12c89
```

公開鍵は別のエンドポイントから取得します。

```bash
curl https://api.forcedream.ai/v1/workforce/proof/public-key
```

```json
{
  "algorithm": "Ed25519",
  "public_key_pem": "-----BEGIN PUBLIC KEY-----\nMCowBQYDK2VwAyEAPXuxb/0mOSujALkhidGxJsHxjuKUOUXJ/moqkGcPt3I=\n-----END PUBLIC KEY-----\n",
  "key_id": "0a0a7fa69af0"
}
```

証明の `key_id` と公開鍵の `key_id` が一致しています。署名の検証は利用者の環境で完結するため、実行したプラットフォーム側に「この結果は正しいか」と問い合わせる必要がありません。

なお、署名対象となるオブジェクトのフィールド構成は複数のバリアントがあり、実装は現在サーバー側から契約を取得する方式に移行しています。自分で検証処理を書く場合は、各 SDK の `verify` 実装を参照してください。

### この実行で実際に起きたこと

- アカウント作成から実行まで、支払い手続きは不要（試用残高）
- 委譲したタスクは 2 ペンス課金された
- 抽出結果は入力に対して正確だった
- 結果には独立に検証できる Ed25519 署名が付いた

## どちらを使うべきか

| | MCP | A2A |
|---|---|---|
| 接続する対象 | AIアプリケーション ↔ ツール | エージェント ↔ エージェント |
| 主体 | クライアント側のAIアプリケーション | 両端が自律的 |
| 実行モデル | 基本的に同期 | 非同期前提、タスクIDとポーリング |
| 発見の方法 | 設定ファイルに記述 | AgentCard を公開 |
| 組織をまたぐ | 想定していない | 前提としている |

**MCP を選ぶ場面**：自分の AI アプリケーションに、自分が管理するツールやデータを接続したいとき。ローカルのファイル、社内のデータベース、自社の API など。

**A2A を選ぶ場面**：他者が運用しているエージェントに処理を任せたいとき。処理時間が読めない、相手の実装を知らない、組織が異なる、といった条件があるとき。

**両方を使う場面**：MCP サーバーを提供しつつ、その裏で外部のエージェントに委譲するとき。利用者には MCP の一つのツールとして見えます。

## まとめ

MCP と A2A は競合していません。MCP は AI アプリケーションに道具を持たせ、A2A はエージェントに外注先を持たせます。

組織をまたいで処理を委譲する場合、「相手が本当にその処理を行ったか」を確認する手段が必要になります。A2A 自体はその仕組みを定めていないため、実装側が用意することになります。上の例では Ed25519 署名による証明がその役割を担っていました。

---

この記事で使用したエンドポイントとレスポンスは、すべて実際に稼働している API から取得したものです。

- ForceDream MCP サーバー: https://github.com/forcedreamai/forcedream-mcp
- 日本語ドキュメント: https://github.com/forcedreamai/forcedream-docs/tree/main/docs/ja
- Model Context Protocol: https://modelcontextprotocol.io
