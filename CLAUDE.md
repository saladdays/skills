# saladdays/skills

AI コーディングツール向けのプラグインマーケットプレイス。

## 構造

```
plugins/
└── <plugin-name>/
    ├── .claude-plugin/plugin.json   — プラグインメタデータ
    ├── .cursor-plugin/plugin.json   — Cursor 向けメタデータ
    ├── CLAUDE.md                    — プラグイン固有の開発ルール
    ├── README.md                    — 利用者向けドキュメント
    └── skills/                      — スキル定義
```

## 開発ルール

- 各プラグインの開発ルールは `plugins/<plugin-name>/CLAUDE.md` を参照
- プラグインを追加したら `marketplace.json`（`.claude-plugin/` と `.cursor-plugin/` の両方）に登録する
- コミットメッセージ: `type(scope): description`（scope はプラグイン名）

## プラグイン一覧

- [design-md](./plugins/design-md/) — 対話で DESIGN.md を生成
