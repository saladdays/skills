# DESIGN.md Generator

AIが一貫したUIを生成するための **DESIGN.md** を、対話形式で作成する Claude Code スキル。

## DESIGN.md とは

DESIGN.md は、AIエージェントがUIを生成・修正する際の **ビジュアル判断基準** を定義するMarkdownファイルです。

```
README.md              = 人間向けプロジェクト概要
CLAUDE.md / AGENTS.md  = AIエージェント向け開発指示
DESIGN.md              = AIエージェント向けビジュアル判断基準
```

### なぜ必要か

AIにUIを生成させると、**1画面目は良いが、改善するたびにデザインがズレていく**。複数画面を作ると、画面間でトーン・色・スペーシングがバラバラになる。

根本原因は、デザインの「判断基準」がAIに渡されていないこと。値（トークン）はあっても「なぜその値か」「どう使い分けるか」がないため、AIは毎回独自に判断し、一貫性が崩れる。

DESIGN.md はこの「判断基準」を明文化し、AIが一貫した判断を行うためのドキュメントです。

### WITHOUT vs WITH の比較

DESIGN.md **なし** で5画面を生成した場合:
- 画面ごとにカラーパレットが異なる（4セット以上）
- 見出しフォントが毎回変わる
- ボタンの角丸やホバー表現が不統一
- 手直しに **6-10時間** 必要

DESIGN.md **あり** で5画面を生成した場合:
- 全画面でカラー・フォント・コンポーネントが完全統一
- Creative North Star（メタファー）に基づいたブランド表現
- DESIGN.md の生成は **数分**

## Quick Start

### 1. スキルをインストール

```bash
# このリポジトリの skills/ を .claude/skills/ にコピー
cp -r skills/generate-design-md ~/.claude/skills/generate-design-md
```

### 2. DESIGN.md を生成

Claude Code で対話形式で生成します:

```
あなた: このプロジェクトのDESIGN.mdを作りたい
Claude: どんなプロダクトですか？ 誰が使いますか？
あなた: 20-30代向けのレシピ共有サービス。温かく親しみやすい印象で。
Claude: （DESIGN.md を生成）
```

### 3. プロジェクトに配置

```bash
# プロジェクトルートに配置
mv DESIGN.md /path/to/your/project/

# CLAUDE.md から参照（推奨）
echo "@DESIGN.md" >> /path/to/your/project/CLAUDE.md
```

これだけで、以降のAI生成UIに一貫性が生まれます。

## DESIGN.md のフォーマット

```markdown
---
generated: 2026-03-26
mode: conversation
version: 1
platform: web
token_source: inline
---

# Design System: [名前]

## TL;DR                    ← 5行で核心を要約
## Design Principles        ← Creative North Star + 判断基準
## Visual Language           ← Mood & Tone + Visual Characteristics
## Color Strategy            ← Palette + Rules
## Typography                ← Scale + Rules
## Spacing & Layout          ← Scale + Philosophy + Surface Hierarchy
## Component Patterns        ← Selection Guide + State Handling
## Motion & Transitions      ← Optional
## Design Token References   ← Optional
```

詳細な仕様は [DESIGN-MD-SPEC.md](./DESIGN-MD-SPEC.md) を参照してください。

## 設計原則

1. **骨格は固定、中身はプロダクトに委ねる** — テンプレートルールをデフォルトにしない
2. **原則 + スケール + 対応表の3層** — コンポーネント個別指定ではなくカテゴリ判断を書く
3. **否定形は肯定形の代替とペアにする** — Don'tだけで終わらせない
4. **自己完結 + 参照** — 判断に必要な情報は DESIGN.md 内で完結。具体値は外部参照可
5. **名前付きルール + 例外パターン** — ルールに名前をつけて参照可能に
6. **説明は任意の言語OK、仕様値はコードと同じ英語表記**
7. **DESIGN.md は「制約」ではなく「創造の方向性」** — AIの創造性を殺さない
8. **自由領域には「トーン」を添える** — 「自由」だけではAIは保守的になる
9. **本仕様書の例テーブルは「一例」であり「デフォルト」ではない**

## ファイル構成

```
design-md-generator/
├── DESIGN-MD-SPEC.md                    ← フォーマット仕様書
├── README.md                            ← このファイル
├── LICENSE
└── skills/
    └── generate-design-md/
        ├── SKILL.md                     ← 対話生成スキル
        └── resources/
            ├── writing-guide.md         ← 良い/悪い記述の Before/After
            └── examples/
                ├── saas-dashboard.md    ← BtoB SaaS ダッシュボードの例
                ├── creator-platform.md  ← クリエイタープラットフォームの例
                └── minimal-zen.md       ← ミニマル瞑想アプリの例
```

## 生成モード

| モード | 入力 | 用途 |
|---|---|---|
| **対話生成** | なし（会話で引き出す） | ゼロからデザイン意図を言語化 |
| **URL抽出** | 既存サイトURL（複数可） | 既存サービスのデザインを言語化 |
| **画像から生成** | スクリーンショット | ビジュアルからデザイン意図を抽出 |
| **Figma抽出** | FigmaファイルURL | デザインデータから意図を棚卸し |
| **デザインシステムから** | tokens.css, tailwind.config等 | 既存定義から原則を帰納 |
| **実装コードから** | GitHubリポジトリ | 実態からパターンを抽出 |

MVP では **対話生成** をサポートしています。

## Examples

### SaaS ダッシュボード（情報密度型）
→ [saas-dashboard.md](./skills/generate-design-md/resources/examples/saas-dashboard.md)

### クリエイタープラットフォーム（コンテンツ型）
→ [creator-platform.md](./skills/generate-design-md/resources/examples/creator-platform.md)

### 瞑想タイマー（ミニマル型）
→ [minimal-zen.md](./skills/generate-design-md/resources/examples/minimal-zen.md)

## 背景

Google Stitch が発表した DESIGN.md フォーマットを分析し、Stitch非依存の汎用 DESIGN.md ジェネレーターとして開発しました。Stitch の思想を参考にしつつ、独自に進化させています。

### 開発で得られた知見

- **制約の非対称性**: AIへの Don't は Do より圧倒的に強く作用する。禁止が多いと創造性が死ぬ
- **テンプレートバイアス**: 仕様書の例文がデフォルトとしてコピーされる。例には「一例」と明記する
- **自由領域**: DESIGN.md に「ここは自由に工夫してよい」を明示するとビジュアルの魅力が回復する
- **問いで書く**: 具体値ではなく判断基準（問い）をスキルに入れると、プロダクト固有の良い判断が生まれる

## License

MIT
