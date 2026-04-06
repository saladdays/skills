# セットアップガイド

## Claude Code

### 1. フック設定

`.claude/settings.json`（プロジェクトルート）に以下を追加:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'BRANCH=$(git branch --show-current 2>/dev/null || echo main); HANDOFF=\".context/handoff-${BRANCH}.md\"; ln -sf \"$(basename \"$HANDOFF\")\" .context/handoff.md 2>/dev/null; if [ -f \"$HANDOFF\" ]; then SAVED=$(grep -m1 \"saved_at\" \"$HANDOFF\" | sed \"s/.*: *//;s/\\\"//g\"); HEAD=$(grep -m1 \"git_head\" \"$HANDOFF\" | sed \"s/.*: *//;s/\\\"//g\"); CURRENT=$(git rev-parse --short HEAD 2>/dev/null); AGE=\"不明\"; if [ -n \"$SAVED\" ]; then SAVED_TS=$(date -d \"$SAVED\" +%s 2>/dev/null); NOW_TS=$(date +%s); DIFF=$(( (NOW_TS - SAVED_TS) / 60 )); if [ $DIFF -lt 60 ]; then AGE=\"${DIFF}分前\"; elif [ $DIFF -lt 1440 ]; then AGE=\"$(($DIFF/60))時間前\"; else AGE=\"$(($DIFF/1440))日前\"; fi; fi; printf \"{\\\"hookSpecificOutput\\\":{\\\"hookEventName\\\":\\\"SessionStart\\\",\\\"additionalContext\\\":\\\"前回のセッションから引き継ぎ情報があります（${AGE}に保存、ブランチ: ${BRANCH}）。.context/handoff.md を読んで3行サマリを表示し、この認識で合っていますか？と確認してください。$([ \\\"$HEAD\\\" != \\\"$CURRENT\\\" ] && echo 注意: 保存後にコミットが追加されています。)\\\"}}\"; else echo \"{}\"; fi'"
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
            "command": "bash -c 'mkdir -p .context && { echo \"--- PreCompact Snapshot $(date -u +%Y-%m-%dT%H:%M:%SZ) ---\"; echo \"branch: $(git branch --show-current 2>/dev/null)\"; echo \"head: $(git rev-parse --short HEAD 2>/dev/null)\"; echo \"\"; git diff --stat 2>/dev/null; echo \"\"; git log --oneline -5 2>/dev/null; } > .context/.pre-compact-snapshot.tmp'"
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
            "command": "bash -c 'printf \"{\\\"hookSpecificOutput\\\":{\\\"hookEventName\\\":\\\"PostCompact\\\",\\\"additionalContext\\\":\\\"コンテキストがコンパクションされました。他の応答に先立ち、.context/handoff.md を最新の作業状態で更新してください。.context/.pre-compact-snapshot.tmp に機械的スナップショットがあります。既存の handoff.md の Dead Ends セクションは棄却理由を保持したまま更新してください。更新が完了したら保存結果を1行で報告してください。\\\"}}\"'"
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

### 2. CLAUDE.md に自動保存ルールを追加

プロジェクトの CLAUDE.md に以下を追記（[auto-save-rules.md](auto-save-rules.md) の内容）:

```markdown
## セッション引き継ぎ

@.context/handoff.md

重要な設計判断をしたとき、アプローチを棄却したとき、.context/handoff.md の該当セクションに1行追記すること。
コンパクション後に PostCompact から更新指示が来たら全体を整理すること。
```

### 3. 初回の handoff.md 生成

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
