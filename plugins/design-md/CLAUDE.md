# design-md プラグイン開発ルール

## 概要

対話を通じて DESIGN.md を生成するプラグイン。
AIが一貫したUIを作るための「デザインの判断基準」を1ファイルにまとめる。

## 重要ファイル

| ファイル | 役割 |
|---|---|
| `DESIGN-MD-SPEC.md` | DESIGN.md フォーマット仕様（v1）。全セクション定義の正 |
| `skills/generate/SKILL.md` | 生成スキル本体。対話フロー・生成ルール・自己検証チェック |
| `skills/generate/resources/writing-guide.md` | Before/After 記述ガイド |
| `skills/generate/resources/examples/` | 完成例3パターン（SaaS、クリエイター、瞑想） |

## 開発方針

- DESIGN-MD-SPEC.md が仕様の正（SSOT）。SKILL.md はその実装
- 仕様変更は DESIGN-MD-SPEC.md → SKILL.md → examples の順に反映
- テンプレートバイアスの排除を常に意識する（汎用的なデフォルト値をコピーさせない）
- 「制約ではなく創造の方向性」が設計哲学

## テスト方法

ブラインド比較テスト:
1. DESIGN.md **あり** で3画面生成
2. DESIGN.md **なし** で同じ3画面生成
3. ファイル名をランダム化してブラインド評価
4. 結果は `tests/` に保存（.gitignore で除外済み）

## 核心的な発見（開発経緯から）

- 制約の非対称性: 縛りを増やすほどAIは萎縮し「均一」になる。「一貫性」とは違う
- 自由領域にトーンを添えないとAIは保守的になる
- WITHOUT版の方がパッと見は華やかになりがち（制約がないから）。一貫性 vs 華やかさのトレードオフ
