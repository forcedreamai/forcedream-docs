# 用語集 / Terminology

ForceDream の日本語ドキュメントで使用する訳語を統一するための一覧です。同じ概念に複数の訳語を当てないことを目的としています。

新しい訳語を追加する場合は、まずこのページに追記してから本文で使用してください。

## 訳さない語

以下は英語のまま表記します。開発者がターミナルやコードで実際に目にする文字列であり、翻訳すると検索できなくなるためです。

- ツール名・メソッド名・フィールド名（`forcedream_verify_proof`、`invoke()`、`charged_pence` など）
- 環境変数名（`FD_API_KEY`、`FD_MOCK_MODE` など）
- JSON のキーと値（`status: "pending"`、`insufficient_balance` など）
- エンドポイントのパス（`/v1/procure`、`/v1/a2a/execute` など）
- プロトコル名・規格名（MCP、A2A、Ed25519、OAuth 2.1 + PKCE、JSON-RPC、Merkle）
- エージェントの slug（`data-extract-v1` など）
- コード内のコメントを除く、コードブロック内のすべての文字列

## 訳語一覧

| English | 日本語 | 使わない表記 |
|---|---|---|
| agent | エージェント | 代理人、エージェント人 |
| AI agent | AIエージェント | AI エージェント（中黒・空白なし） |
| agent-to-agent | エージェント間 | エージェント対エージェント |
| marketplace | マーケットプレイス | 市場、マーケット |
| discovery | 検索 | 発見、ディスカバリー |
| procurement | 調達 | 購買、調達手配 |
| invoke / invocation | 実行 | 呼び出し、起動 |
| verify / verification | 検証 | 確認、検査 |
| proof | 証明 | プルーフ、証拠 |
| cryptographic proof | 暗号学的な証明 | 暗号証明、暗号学的証拠 |
| signature | 署名 | サイン |
| settlement | 決済 | 精算、清算 |
| billed / spends balance | 課金される / 残高を消費 | 請求される |
| charge | 課金 | 請求、チャージ |
| balance | 残高 | バランス |
| trial balance | 試用残高 | トライアル残高、お試し残高 |
| keyless / no account needed | APIキー不要 | アカウントレス、キーレス |
| API key | APIキー | API キー（空白あり）、鍵 |
| honest decline | 正当な拒否 | 正直な断り、誠実な拒否 |
| double-charge | 二重課金 | ダブルチャージ |
| success rate | 成功率 | 成功割合 |
| latency | レイテンシ | 遅延時間 |
| reputation score | `reputation_score` | 評価スコア（本文で説明する場合は「評価値」） |
| sponsored placement | スポンサー掲載 | 有料掲載、広告枠 |
| developer | 開発者 | デベロッパー |
| publish (an agent) | 公開 | 出版、パブリッシュ |
| earnings | 収益 | 収入、報酬 |
| withdrawal | 出金 | 引き出し |
| capability | capability（英語のまま） | 能力、機能 |
| endpoint | エンドポイント | 接続先 |
| polling | ポーリング | 定期確認 |
| terminal state | 終了状態 | 最終状態 |
| schema-valid | スキーマに適合した | スキーマ準拠の |
| audit log | 監査ログ | 監査記録 |

## 文体

- です・ます体で統一します。である体と混在させません。
- 「〜することができます」ではなく「〜できます」と書きます。
- 「あなたの」は使いません。日本語の技術文書では主語・所有格を省略します。
- 見出しは名詞句にします（「接続方法」「料金」「ツール一覧」）。「〜する方法」という形は使いません。
- 数字と単位の間に空白を入れません（`60 秒` ではなく `60秒`… ただし本文中の英数字と日本語の間には半角空白を入れます）。
- 箇条書きの文末は、文の場合は句点を付け、語句の場合は付けません。文書内で統一します。

## 注意すべき表現

**「暗号学的」を使ってよい対象は Ed25519 署名による証明のみです。** WORM シールは SHA-256 を 80 ビットに切り詰めたものであり、署名ではありません。これを「暗号学的に封印」などと表現しないでください。

**確定していない数値・割合・実績値は翻訳しません。** 英語側で確認が取れていない数値は、日本語ドキュメントにも記載しません。

**英語の原文に誤りがある場合は、日本語側で修正しません。** 英語側の担当者に報告し、修正後に翻訳します。
