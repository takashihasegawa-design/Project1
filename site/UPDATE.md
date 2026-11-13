# 定期更新の手順（1時間ごと）

公開先: https://claude.ai/artifact/FDR1sH2GMsDV6wAYfLZgTU （`site/index.html` + `site/data.json`）

1. **公式情報を確認する**（WebSearch。コンテナからは X や status.cloud.microsoft に直接接続できないため検索経由）
   - Microsoft 365 Status（@MSFT365Status）の新しい投稿。URL の投稿 ID から時刻を算出する:
     `python3 -c "import datetime;i=ID;print(datetime.datetime.fromtimestamp(((i>>22)+1288834974657)/1000,datetime.timezone.utc))"`
   - MO1550821 / EX1550543 の状態変化（復旧宣言、根本原因、事後レビュー＝PIR の公開）
   - Azure の状態ページ、Microsoft の公式ブログ・声明
   - 報道は補足のみ。`kind: "press"` として公式と区別する
2. **`site/data.json` を更新する**
   - `meta.lastUpdated`（UTC ISO）と `meta.updateCount` を更新
   - 新しい公式投稿を `timeline` に追加（`ja` は日本語訳、`en` は原文。全文が取れない場合は末尾に「（以下省略）」）
   - `status`、`services[].state`（impacted / degraded / restored / unknown）、`cause` を最新に合わせる
   - 復旧宣言が出たら `status.level` を `resolved`、`status.resolvedAt` を設定
   - `changelog` に「何が変わったか」を 1 行追加（変化がなければ「新しい公式発表なし」）
   - 推測は書かない。未確認情報は「報道」または「未確定」と明記する
3. **検証**: `python3 -m json.tool site/data.json`
4. **公開**: Artifact を `file_path: site/index.html`、`files: {"data.json": "site/data.json"}`、`url` 上記で再公開
5. **コミット**: ブランチ `claude/microsoft-outage-tracking-apech6` にコミットして push
