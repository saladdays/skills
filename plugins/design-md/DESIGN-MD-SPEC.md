# DESIGN.md Format Specification v1

> AIが一貫したビジュアル判断を行うための、プレーンテキスト・デザインシステム定義フォーマット。

## このドキュメントについて

DESIGN.md は、AIエージェントがUIを生成・修正する際の **ビジュアル判断基準** を定義するMarkdownファイルです。

デザインシステムが「法律の条文」なら、DESIGN.md は **「判例集」** です。
具体的な値（トークン）だけでなく、「なぜその値か」「どう使い分けるか」という判断の根拠を記述します。

### 配置
- プロジェクトルートに `DESIGN.md` として配置
- CLAUDE.md から `@DESIGN.md` で直接参照可能
- AGENTS.md からはテキストで参照指示を記述

---

## 設計原則

1. **骨格は固定、中身はプロダクトに委ねる**
   セクション構成は統一する。ただしテンプレートルール（特定の美学・手法）をデフォルトにしない。

2. **原則 + スケール + 対応表の3層で記述する**
   コンポーネント個別の指定ではなく、カテゴリレベルの判断基準を書く。
   AIは原則から未知のケースを推論できる。

3. **否定形は肯定形の代替とペアにする**
   `Don't: グラデーションを使わない` ではなく、
   `Don't: グラデーションを使わない → 代わりにソリッドカラーで表現する`

4. **自己完結 + 参照**
   判断に必要な情報は DESIGN.md 内で完結させる。
   具体値は外部ファイル（tokens.css等）への参照でもよい。

5. **名前付きルール + 例外パターン + 違反例**
   ルールに名前をつけて参照可能にする。例外には「なぜ」を付ける。
   境界が曖昧なルールには、違反例（Violation）を任意で付与する。
   ```
   ### Content-First Rule
   コンテンツ領域は常に最大幅を確保する。
   **Exception:** ダッシュボードではサイドバーに固定幅を割り当てる。
   理由：複数パネルの同時閲覧が業務効率に直結するため。
   **Violation:** サイドバーとコンテンツ領域が50:50で分割されている。コンテンツ領域が「最大幅」になっていない。
   ```

6. **説明は任意の言語OK、仕様値はコードと同じ英語表記**
   ```
   ### 角丸 (Border Radius)
   要素サイズに比例させる。過度に丸くしない。
   - `radius-sm`: 4px — フォーム要素、小ボタン
   - `radius-md`: 8px — カード、ダイアログ
   ```

7. **DESIGN.md は「制約」ではなく「創造の方向性」**
   DESIGN.md はAIの創造性を殺すためのものではない。方向性（トークン、原則、ルール）を示しつつ、
   書かれていない領域ではAIが自由に最適なデザインを追求できるようにする。
   各セクションに「自由領域」を明示することを推奨する。
   ```
   ### Layout Philosophy
   コンテンツ幅: 最大800px
   グリッド: 3列基本
   **自由領域:** ヒーローの構成、装飾的なグラデーション、ホバーエフェクトの演出
   ```

8. **自由領域には「トーン」を添える**
   「自由領域」と書くだけでは、AIは最も保守的な選択をしがちである。
   Creative North Star のメタファーに沿った方向性を示す。
   ```
   ✅ **自由領域:** ホバーエフェクトの演出。書斎で本を手に取る瞬間のように、静かだが確かな変化を。
   ❌ **自由領域:** ホバーエフェクトの演出は自由。
   ```
   前者はAIに創造的な判断材料を与え、後者はAIを迷わせるだけである。

9. **本仕様書の例テーブルは「一例」であり「デフォルト」ではない**
   以下のセクションに含まれる例テーブル（Typography Scale、State Handling、Spacing Scale等）の
   具体値（フォント名、line-height値、hover手法等）は、あるプロダクトの一例に過ぎない。
   プロダクトの特性に応じて判断し、例をそのままコピーしないこと。

---

## 記述の抽象度

### 判断基準
「出力がブレたときにユーザーがすぐ気づくか？」

| すぐ気づく → **具体値が必須** | 気づきにくい → **原則 + 代表例で十分** |
|---|---|
| カラーパレット（Hex値 or 変数名） | レイアウト方針 |
| タイポグラフィスケール | アニメーション/モーション |
| スペーシングスケール | トーン & ムード |
| ブレイクポイント | アイコン/イラスト方針 |

### 3層記述パターン

すべてのセクションで以下の3層構造を推奨する。

```markdown
### [トピック名]

**原則（Why）：** なぜこうするか。判断の根拠。
**スケール（What）：** 使える値の全体像。テーブル形式推奨。
**対応表（When）：** どの文脈でどの値を使うか。Reasoning列付き。

⚠️ Avoid: 避けるべきこと → 代わりにこうする
```

#### 対応表のフォーマット

対応表はテーブル形式を必須とする。**Reasoning 列は全セクションで必須**。
セクションごとに列構成は異なるが、以下の原則を守る：

| セクション | 必須列 | オプション列 |
|---|---|---|
| Color | Intent, Token, Value, Do, Don't | Reasoning（Do/Don'tが理由を兼ねる） |
| Typography | Context, Token, Size, Weight, Reasoning | Font, Line Height |
| Spacing | Token, Value, Use case | Reasoning |
| Component | 場面, コンポーネント, 理由 | — |
| State Handling | State, Visual, Reasoning | Transition |

```markdown
| Context         | Token         | Value | Reasoning                |
|-----------------|---------------|-------|--------------------------|
| Page title      | heading-xl    | 32px  | 視覚的階層の最上位        |
| Card title      | heading-sm    | 18px  | 限定領域での見出し        |
| Body text       | body-md       | 16px  | 長文の可読性基準          |
```

Color には Do/Don't 列を含める：

```markdown
| Intent          | Token         | Value   | Do               | Don't            |
|-----------------|---------------|---------|------------------|------------------|
| Primary action  | color-action  | #0066FF | CTA、主要ボタン  | 装飾、背景       |
| Destructive     | color-danger  | #DC2626 | 削除、エラー     | 警告に使わない   |
```

---

## フォーマット構成

### Frontmatter

```yaml
---
generated: 2026-03-24
mode: conversation        # url | figma | image | conversation | design-system | code | hybrid
version: 1
platform: web             # web | mobile | cross-platform
token_source: inline      # inline | ./path/to/tokens | none
---
```

### セクション構成

```markdown
# Design System: [名前]

## TL;DR
## Design Principles
## Visual Language
## Color Strategy
## Typography
## Spacing & Layout
## Component Patterns
## Motion & Transitions          — Optional
## Design Token References       — Optional（外部トークンがある場合）
## Visual References             — Optional
```

---

## 各セクション仕様

### TL;DR

5行以内で DESIGN.md の核心を要約する。人間が30秒で全体像を掴むためのセクション。
ヒアリングで死守ラインが明確になった場合のみ、6行目として追加してよい。

```markdown
## TL;DR
- 北極星: 「静かな信頼感」— 堅すぎず、かかりつけ医のような安心
- カラー: ニュートラル基調、アクションは Blue-600 の1色
- タイポ: Inter、4段階、16px基準
- スペーシング: 8px単位、余白は広めに
- 避けること: 影の多用、3色以上の同時使用、12px以下のテキスト
- 死守ライン: 本文の可読性（行間1.8、コントラスト7:1）。ここだけは絶対に妥協しない ← 任意。明確な場合のみ
```

---

### Design Principles

デザイン判断の最上位ガイドライン。3つのサブセクションで構成する。

#### Creative North Star

プロダクト全体を貫くメタファー。未定義のケースでAIが判断する際の「コンパス」。

```markdown
### Creative North Star

**Metaphor:** 「静かな水面」— 動きは最小限で、情報が自然に浮かび上がる。

**This means:**
- トランジション: ease-out、200-300ms、バウンスなし
- サーフェス: 微細なelevationで層を表現、フラットにはしない
- 情報密度: 余白を広く、1ビューにフォーカルポイントは1つ

**This does NOT mean:**
- 水のテクスチャやウェーブアニメーションを使うことではない
- コンテンツのない空虚なレイアウトではない
- 青一色のUIではない
```

#### When Principles Conflict

原則が矛盾した場合の解決ルール。抽象的な不等号ではなく、具体的な判断シナリオで記述する。

```markdown
### When Principles Conflict

- 美しいが読みにくい → 読みやすさを取る（コントラスト比 4.5:1 を死守）
- 一貫性を崩すが文脈に最適 → 文脈を取る（ただし DESIGN.md に例外として追記）
- メタファーに忠実だが使いにくい → 使いやすさを取る（機能的明瞭さが最優先）
```

#### Global Constraints

プロダクト全体に適用される制約。セクション横断的な禁止事項はここに1回だけ書く。

```markdown
### Global Constraints

- Material Design の elevation 体系を使わない → 代わりにサーフェス色差とボーダーで階層を表現
- 純黒 (#000000) をテキストに使わない → `on-surface` トークンを使用（#1A1C1B）
- アクセシビリティ: WCAG 2.2 AA 準拠を最低基準とする
```

---

### Visual Language

Design Principles（Why）と各トークンセクション（What）をつなぐ **How** の層。
5-10行で、プロダクトのビジュアル特性を言語化する。

```markdown
## Visual Language

### Mood & Tone
穏やか、誠実、控えめ。主張しすぎないが、必要な情報は明確に届ける。

### Visual Characteristics
- **角丸:** 控えめ（sm-md）。ピル型は使わない
- **影:** 最小限。サーフェス色差で階層を表現
- **密度:** 余白多め。情報密度より可読性を優先
- **コントラスト:** 中程度。高コントラストすぎない落ち着いた印象
```

---

### Color Strategy

#### Palette

トークン名（または変数名）+ 値 + 役割 + 使い方をテーブルで定義する。
値は Hex 直書きでもデザイントークン変数名でもよい。

```markdown
### Palette

| Intent          | Token              | Value              | Do                    | Don't                |
|-----------------|--------------------|--------------------|-----------------------|----------------------|
| Primary action  | `--color-primary`  | #2563EB            | CTA、主要ボタン       | 装飾、背景全体       |
| Surface default | `--color-surface`  | #FAFAFA            | ページ背景、カード    | テキスト色           |
| On surface      | `--color-on-surface`| #1A1C1B           | 本文テキスト          | 純黒(#000)は使わない |
| Danger          | `--color-danger`   | #DC2626            | 削除、エラー          | 警告（warning を使う）|
| Warning         | `--color-warning`  | #D97706            | 注意喚起              | エラー               |
| Success         | `--color-success`  | #059669            | 完了、正常状態        | 装飾                 |
```

#### Rules

名前付きルールで色の使い方を定義する。

```markdown
### Rules

#### Surface Layering Rule
背景色の明度差でUI階層を表現する。ボーダーではなく色の遷移が境界になる。
- Base: `--color-surface` (#FAFAFA)
- Card: `--color-surface-raised` (#FFFFFF)
- Inset: `--color-surface-inset` (#F3F4F6)

**Exception:** データテーブルの行間にはボーダーを使う。
理由: データの走査性は色差よりボーダーで向上するため。

**Violation:** 同一ビューに `--color-primary` と `--color-info` が装飾として共存している。意味色（ステータス）以外で複数のアクセントカラーが使われている状態。

⚠️ Avoid: 3色以上を同一ビューで装飾に使う → アクセントは1色に絞る
```

#### Contrast Verification

Color Palette を定義したら、主要なテキスト色×背景色の組み合わせのコントラスト比を検証し、HTMLコメントで記録する。

```markdown
<!-- コントラスト検証:
  --color-on-surface (#1A1C1B) × --color-surface (#FAFAFA) = 16.2:1 ✅
  --color-primary (#2563EB) × white (#FFFFFF) = 4.63:1 ✅
  --color-on-surface-muted (#6B6B6B) × --color-surface (#FAFAFA) = 5.1:1 ✅
-->
```

- 4.5:1 未満 → 不合格。色を調整して再検証すること
- 4.5〜5.0:1 → 合格だがギリギリ。レンダリング環境で不合格になるリスクあり。可能なら暗くする

#### Dark Mode

ダークモードの対応方針を明記する。

- **Light のみ:** `[Dark Mode: not planned]` と記載
- **Light / Dark 切替:** 2パレットを定義する
- **Hybrid（領域別）:** 一部ダーク・一部ライト（例：コードエディタはダーク、UIはライト）の場合、領域ごとのモード方針とトークンの分離方法を記述する

対応しない場合も必ず明記し、AIが独自にダークモード対応を行わないようにする。

---

### Typography

#### Scale

```markdown
### Scale

**原則:** 見出しは存在感を、本文は可読性を最優先する。

> **注意:** 以下は英語UIの一例（全行Inter、line-height 1.6）である。
> プロダクトの特性に応じて以下を問うこと：
> - **Line Height:** コンテンツの主要言語は何か？日本語等のCJK言語の長文では 1.7-1.9 が適切な場合がある
> - **書体:** 全場面で同じ書体が最適か？コンテンツ中心のプロダクトでは Serif/Sans の使い分けが有効な場合がある
> - **Weight:** 2ウェイト以上で視覚的階層を表現できているか？

| Context          | Token          | Font        | Size  | Weight | Line Height | Reasoning              |
|------------------|----------------|-------------|-------|--------|-------------|------------------------|
| Hero headline    | `display-lg`   | Inter       | 3rem  | 700    | 1.2         | ページの顔。視線の起点  |
| Page title       | `heading-xl`   | Inter       | 2rem  | 600    | 1.3         | セクションの区切り      |
| Section heading  | `heading-md`   | Inter       | 1.5rem| 600    | 1.4         | コンテンツの構造化      |
| Body text        | `body-md`      | Inter       | 1rem  | 400    | 1.6         | 長文の可読性基準        |
| Caption/metadata | `body-sm`      | Inter       | 0.875rem| 400  | 1.5         | 補助情報、低優先度      |
| UI label         | `label-md`     | Inter       | 0.75rem| 500   | 1.4         | ボタン、フォームラベル  |

⚠️ Avoid: 0.75rem 未満のテキスト → 可読性とアクセシビリティの限界
```

---

### Spacing & Layout

#### Spacing Scale

```markdown
### Spacing Scale

**原則:** 8px基準のリズム。要素間の余白はコンテンツの関係性を表現する。
近い要素は関連が強い、遠い要素は独立している。

汎用スケール（`space-1` ~ `space-16`）に加えて、プロダクト固有のセマンティックトークンを追加することを推奨する（例：`gallery-gap`, `feed-gap`, `space-code` 等）。

| Token       | Value | Use case                           |
|-------------|-------|------------------------------------|
| `space-1`   | 4px   | アイコンとラベルの間               |
| `space-2`   | 8px   | 関連する要素のグループ内           |
| `space-3`   | 12px  | フォームフィールド間               |
| `space-4`   | 16px  | カード内パディング                 |
| `space-6`   | 24px  | セクション内の要素間               |
| `space-8`   | 32px  | セクション間                       |
| `space-12`  | 48px  | 主要セクション間                   |
| `space-16`  | 64px  | ページレベルの区切り               |
```

#### Layout Philosophy

```markdown
### Layout Philosophy

> **注意:** 以下は長文コンテンツ中心のプロダクトの一例である。
> プロダクトの特性に応じて以下を問うこと：
> - **主要タスクは何か？** 長文を読む / データを走査する / 商品を比較する / 操作する、で最適なレイアウトは異なる
> - **情報密度は？** 余白重視か、一覧性重視か
> - **カラム数は？** 1カラムが自然か、マルチカラムが業務効率に寄与するか

プロダクトの主要タスクに最適なレイアウト方針を記述する。

- コンテンツ最大幅: 720px（長文の可読性に最適）
- サイドバーが必要な場合: 固定幅 240-280px
- グリッド: 必要な場合のみ。1カラムをデフォルトとする
```

#### Breakpoints（platform: web の場合は推奨）

```markdown
| Breakpoint | Width  | Layout Change                    |
|------------|--------|----------------------------------|
| mobile     | < 640px | 1カラム、ナビは下部タブ           |
| tablet     | 640-1024px | 1-2カラム、サイドバーは折りたたみ |
| desktop    | > 1024px | フルレイアウト                    |
```

#### Surface Hierarchy + Elevation

```markdown
### Surface Hierarchy

| Level | Surface               | Use case              | Elevation     |
|-------|-----------------------|-----------------------|---------------|
| 0     | `--color-surface`     | ページ背景            | なし          |
| 1     | `--color-surface-raised`| カード、タイル       | 色差のみ      |
| 2     | Dropdown/Popover      | 一時的な浮遊要素      | shadow-sm     |
| 3     | Modal/Dialog          | ユーザーの注意を占有  | shadow-md + overlay |
```

---

### Component Patterns

コンポーネント個別のスタイルではなく、**カテゴリレベルの判断基準** を書く。

#### Selection Guide

「この場面でどのコンポーネントを使うか」のデシジョンツリー。

```markdown
### Selection Guide

**選択肢の提示:**
- 3個以下 → Radio / SegmentedControl
- 4-7個 → Select
- 8個以上 → Combobox（検索付き）
- 理由: 認知負荷の閾値

**フィードバック:**
- 即時・軽微 → Toast（自動消去）
- 確認が必要 → Dialog
- 文脈内通知 → Inline Alert
- 理由: 中断の重さとユーザーの次のアクション

**ナビゲーション:**
- 主要セクション5個以下 → Tab
- 階層的 → Sidebar + Breadcrumb
- 理由: 情報アーキテクチャの深さ
```

#### Composition Rules

```markdown
### Composition Rules

- Card 内に Card をネストしない（フラットに並べる）
- Modal 内で別の Modal を開かない（Drawer または画面遷移で対応）
- フォーム: 1カラムレイアウト、ラベルはフィールドの上
```

#### Special Content Blocks

プロダクトに引用、コードブロック、コールアウト、注釈などの特殊コンテンツブロックが存在する場合、以下を定義する：

- **視覚的区別の原則:** スクロール中に「本文ではない」と即座に認識できること
- 手法はプロダクトのトーンに依存する。以下は判断の問い：
  - 本文と特殊ブロックの境界は、背景色・ボーダー・インデント・書体変更のうちどれがメタファーに合うか？
  - 複数種類の特殊ブロック（引用 vs コード vs 注意書き）を視覚的に区別できるか？
  - 特殊ブロックが連続した場合、互いの境界は明確か？

#### State Handling

```markdown
### State Handling

**原則:** すべてのインタラクティブ要素は default / hover / focus / active / disabled の5状態を持つ。

> **注意:** 以下は一般的なパターンの一例である。
> hover の手法を1つに統一すると単調になりやすい。
> 問い：「カードの hover、ボタンの hover、リンクの hover は同じ処理でよいか？それぞれの役割に応じた変化があるか？」

- hover: 要素の役割に応じた変化を定義する（色変化、位置変化、影の変化など。手法はプロダクトのトーンに依存する）
- focus: `--color-primary` のフォーカスリング（2px offset）
- active: 押下のフィードバック（色変化、scale等）
- disabled: opacity 0.5、カーソルは not-allowed
- error: `--color-danger` のボーダー + エラーテキスト（フィールド直下）

> **感情的インタラクション:** プロダクトに「いいね」「フォロー」等の感情的アクションがある場合、
> それらは通常のUI操作と同じ視覚処理にしない。
> 問い：「その操作はユーザーにとって機能的か、感情的か？感情的な操作に、機能的な操作と同じフィードバックを与えていないか？」
```

#### Density Guide

```markdown
### Density Guide

| Context       | Size | Inner Padding     | Use case                |
|---------------|------|-------------------|-------------------------|
| Dashboard     | sm   | `space-2` `space-3` | 情報密度優先            |
| Standard form | md   | `space-3` `space-4` | 入力の快適性            |
| Landing page  | lg   | `space-4` `space-6` | 視認性・余白の演出      |
```

---

### Motion & Transitions（Optional）

```markdown
## Motion & Transitions

**原則:** 動きは意味を持つときだけ。装飾的なアニメーションは使わない。

- Duration: micro(100ms) / standard(200ms) / emphasis(300ms)
- Easing: ease-out を基本。入場は ease-out、退場は ease-in
- Content-First Rule: コンテナが先に出現し、コンテンツは遅延して表示される
```

---

### Design Token References（Optional）

外部にデザイントークンがある場合の参照セクション。

```markdown
## Design Token References

### Token Source
`token_source: ./src/styles/tokens.css`

### Token Map
DESIGN.md のトークン名と外部ファイルのキーに差異がある場合のみ記述する。

| DESIGN.md Token  | External Key                       |
|-------------------|------------------------------------|
| color-action      | semantic.color.action.primary      |
| space-4           | spacing.400                        |

### Resolution Rules
1. 外部トークンファイルに定義がある場合、その値が正（SSOT）
2. 定義がない場合、DESIGN.md のインライン値に従う
3. 新しいトークンが必要な場合、既存スケールとの整合性を確認してから追加
```

---

### Visual References（Optional）

参考画像やスクリーンショットへのパス。

```markdown
## Visual References

- `./refs/hero-style.png` — ヒーローセクションの密度感と余白バランス
- `./refs/card-grid.png` — カードグリッドの理想的な間隔
```

---

## 推奨サイズ

| レベル | セクション | 行数 | 用途 |
|---|---|---|---|
| **ミニマム** | TL;DR + Principles + Color + Typography + Spacing | 80-120行 | 個人開発、初期段階 |
| **スタンダード** | 上記 + Visual Language + Components + Token Refs | 250-350行 | チーム開発 |
| **フル** | 上記 + Motion + Visual Refs | 350-400行 | 成熟プロダクト |

- 400行を超えたら Component Patterns を別ファイルに分離を検討する
- 名前付きルールはセクションあたり3-5個を推奨
- セクションが不要な場合は `[SKIP]` として省略可能

### ミニマムレベルの省略ルール

| セクション | ミニマム | 省略時の簡易化 |
|---|---|---|
| TL;DR | **必須** | — |
| Creative North Star | **必須** | This means 3個、This does NOT mean 2個で十分 |
| Color Palette | **必須** | Do/Don't 列なしで簡易化可。4-5色で十分 |
| Typography Scale | **必須** | 3-4ステップ。Reasoning は簡潔に |
| Spacing & Layout | **必須** | スケール表 + Philosophy の概略のみ |
| Visual Language | 省略可 | Creative North Star で代替 |
| Component Patterns | 省略可 | — |
| Motion | 省略可 | — |
| Design Token References | 省略可 | — |

---

## バージョニング

- Frontmatter の `version` フィールドで管理
- 破壊的変更（セクション構成の変更等）はメジャーバージョンを上げる
- 内容の追加・修正はバージョンを変えない（Git で管理）
