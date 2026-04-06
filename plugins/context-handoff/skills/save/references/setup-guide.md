# セットアップガイド

## Claude Code

### 1. フック設定

`.claude/settings.json`（プロジェクトルート）に以下を追加する。
既存の settings.json がある場合は、`hooks` と `permissions` の各キーをマージすること。

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash .context/hooks/session-start.sh"
          }
        ]
      }
    ],
    "PreCompact": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash .context/hooks/pre-compact.sh"
          }
        ]
      }
    ],
    "PostCompact": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash .context/hooks/post-compact.sh"
          }
        ]
      }
    ]
  },
  "permissions": {
    "allow": [
      "Edit(.context/*)",
      "Write(.context/*)"
    ]
  }
}
```

### 2. フックスクリプトの配置

`.context/hooks/` に以下の3ファイルを作成する。

#### .context/hooks/session-start.sh

```bash
#!/bin/bash
# SessionStart: ブランチ別handoff.mdのリンク + 鮮度チェック + AI指示注入

BRANCH=$(git branch --show-current 2>/dev/null || echo "main")
SAFE_BRANCH=$(echo "$BRANCH" | sed 's/[\/:]/-/g')
HANDOFF=".context/handoff-${SAFE_BRANCH}.md"

# ブランチ別ファイルへのシンボリックリンクを更新
ln -sf "handoff-${SAFE_BRANCH}.md" .context/handoff.md 2>/dev/null

if [ ! -f "$HANDOFF" ]; then
  echo "{}"
  exit 0
fi

# 鮮度チェック（macOS/Linux 両対応）
AGE="不明"
if command -v stat >/dev/null 2>&1; then
  if stat --version 2>/dev/null | grep -q GNU; then
    MTIME=$(stat -c %Y "$HANDOFF" 2>/dev/null)
  else
    MTIME=$(stat -f %m "$HANDOFF" 2>/dev/null)
  fi
  if [ -n "$MTIME" ]; then
    NOW=$(date +%s)
    DIFF=$(( (NOW - MTIME) / 60 ))
    if [ $DIFF -lt 60 ]; then AGE="${DIFF}分前"
    elif [ $DIFF -lt 1440 ]; then AGE="$(($DIFF/60))時間前"
    else AGE="$(($DIFF/1440))日前"; fi
  fi
fi

# git HEAD の突合
HEAD=$(grep -m1 "git_head" "$HANDOFF" 2>/dev/null | sed 's/.*: *//;s/"//g;s/ //g')
CURRENT=$(git rev-parse --short HEAD 2>/dev/null)
WARN=""
if [ -n "$HEAD" ] && [ -n "$CURRENT" ] && [ "$HEAD" != "$CURRENT" ]; then
  WARN=" 注意: 保存後にコミットが追加されています。"
fi

# additionalContext で AI に指示を注入
printf '{"hookSpecificOutput":{"hookEventName":"SessionStart","additionalContext":"前回のセッションから引き継ぎ情報があります（%sに保存、ブランチ: %s）。.context/handoff.md を読んで3行サマリを表示し、この認識で合っていますか？と確認してください。%s"}}' "$AGE" "$BRANCH" "$WARN"
```

#### .context/hooks/pre-compact.sh

```bash
#!/bin/bash
# PreCompact: 機械的スナップショットを退避

mkdir -p .context
{
  echo "--- PreCompact Snapshot $(date -u +%Y-%m-%dT%H:%M:%SZ) ---"
  echo "branch: $(git branch --show-current 2>/dev/null)"
  echo "head: $(git rev-parse --short HEAD 2>/dev/null)"
  echo ""
  git diff --stat 2>/dev/null | head -20
  echo ""
  git log --oneline -5 2>/dev/null
} > .context/.pre-compact-snapshot.tmp
```

#### .context/hooks/post-compact.sh

```bash
#!/bin/bash
# PostCompact: AI にhandoff.md更新を指示

printf '{"hookSpecificOutput":{"hookEventName":"PostCompact","additionalContext":"コンテキストがコンパクションされました。他の応答に先立ち、.context/handoff.md を最新の作業状態で更新してください。.context/.pre-compact-snapshot.tmp に機械的スナップショットがあります。既存の handoff.md の Dead Ends セクションは棄却理由を保持したまま更新してください。更新が完了したら保存結果を1行で報告してください。"}}'
```

### 3. 自動保存ルールを配置

`.claude/rules/context-handoff.md` を作成し、[auto-save-rules.md](auto-save-rules.md) の「ファイルの内容」セクションをそのまま書き込む。

プロジェクトの CLAUDE.md は変更しない。`.claude/rules/` のファイルは CLAUDE.md と同等に自動読み込みされる。

### 4. 初回の handoff.md 生成

`/context-handoff:save` を実行すると、現在の作業状態から handoff.md が自動生成される。

---

## Cursor

Cursor のルールファイル（`.cursor/rules`）に以下を追加:

```
セッション開始時に .context/handoff.md が存在すれば読み込み、3行サマリを表示すること。
重要な設計判断・アプローチ棄却時に .context/handoff.md に1行追記すること。
```

フック機構は Cursor には存在しないため、L2（PostCompact）は利用不可。
L1（差分追記）と L3（手動保存）で運用する。

---

## 他の AI ツール

1. `skills/save/SKILL.md` を AI ツールのスキルディレクトリにコピー
2. セッション開始時に `.context/handoff.md` を読み込むよう設定
3. 作業中に判断・棄却があれば handoff.md に追記するルールを設定

---

## トラブルシューティング

### handoff.md が壊れた / 内容が不正確な場合

```bash
rm .context/handoff.md .context/handoff-*.md
```

その後 `/context-handoff:save` を実行して再生成してください。
`.context/archive/` に過去のスナップショットがあれば参照できます。

### シンボリックリンクが壊れた場合

`.context/handoff.md` が読み込めない場合、リンクが壊れている可能性がある:

```bash
rm .context/handoff.md
```

次の SessionStart フック実行時（新セッション開始時）に自動で再作成される。
すぐに修復したい場合:

```bash
BRANCH=$(git branch --show-current | sed 's/[\/:]/-/g')
ln -sf "handoff-${BRANCH}.md" .context/handoff.md
```

### フックが動作しない場合

`.context/hooks/` のスクリプトに実行権限があるか確認:

```bash
chmod +x .context/hooks/*.sh
```

---

## マルチユーザー環境での運用

### 個人開発（1人で複数 PC）

`.context/` を git 追跡する（デフォルト）。ブランチ別ファイルなのでコンフリクトしにくい。

```
git push   （PC-A で）
git pull   （PC-B で）
```

### チーム開発（複数人で同一リポジトリ）

handoff.md は個人の作業状態を含むため、チームメンバー間でコンフリクトが起きうる。
以下のいずれかの方式を選択:

**方式 A: .gitignore に追加（推奨）**

```bash
echo ".context/" >> .gitignore
```

チームで共有すべき情報（設計判断等）は CLAUDE.md や DESIGN.md に書く。
handoff.md は個人のローカル作業メモとして扱う。

**方式 B: ユーザー別ファイル**

handoff.md のファイル名にユーザー名を含める:
- `.context/handoff-{branch}-{username}.md`
- SessionStart フックの `SAFE_BRANCH` 行の後に `SAFE_BRANCH="${SAFE_BRANCH}-$(whoami)"` を追加

この方式なら git 追跡しても他のメンバーとコンフリクトしない。
