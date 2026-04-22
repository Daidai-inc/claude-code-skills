# Skill: verify-loop — 修正後の検証ループ（強制ゲート）

コード修正・改善・新規実装の完了後に自動発動する検証ゲート。
手動確認で「やった」と言うのは禁止。ツールが実際に走り、結果がログに残る。

## トリガー条件

scripts/*.sh / .claude/rules/*.md / .agents/roles/*.yml / .claude/agents/*.md / .claude/skills/*/SKILL.md の修正完了時に自動発動。
triggers.mdの「仕組み変更完了時」トリガーと連動。

## 実行フロー

### Step 1: プリセット判定

修正ファイルの種類からimprovement_loop.shのプリセットを自動選択:

| ファイルパターン | プリセット |
|------------------|-----------|
| scripts/*.sh | script |
| .claude/rules/*.md, triggers.md | rule |
| .agents/roles/*.yml, .claude/agents/*.md | agent-role |
| .claude/skills/*/SKILL.md | skill |
| memory/*.md | memory |
| note_drafts/ | note |
| **上記以外（plist/json/yaml/設定ファイル等）** | **最も近いプリセットを使うか、理由を明示してStep 2をスキップ → Step 3（実証テスト）は必須。スキップ不可** |

### Step 2: improvement_loop.sh --score-only 実行

```bash
bash scripts/improvement_loop.sh --preset <preset> --score-only -i <modified_file> --max-iter 1 --timeout 300
```

- スコアがJSONで出力される
- ログは logs/improvement_loop_*.log に自動保存

### Step 3: 実証テスト

**原則: 修正したものが「実際に目的を果たすこと」を証明する。証明方法は修正対象の文脈に合わせて判断する。**

#### 実証の判断フレームワーク

修正対象を見て「これが正しく動いた状態とは何か？」を先に定義し、それを満たす最小の実行で証明する。

1. **何が変わったかを確認する** — 修正の目的を1行で言える状態にする
2. **その目的が達成されたことを示す証拠を決める** — ファイル生成・出力内容・exit code・ログの変化など
3. **その証拠が得られる最小の実行をする** — 本番と同じ入力・環境で走らせる
4. **証拠が出たら合格** — 出なければ修正してStep 2に戻る

#### 合格の判断基準

- **実行した**: 実際に動かして結果が出ている（シミュレーションや構文チェックのみは不可）
- **本番と同じ条件**: launchd相当のクリーン環境、実在するファイルや引数を使う
- **目的を達成している**: 修正の意図通りの変化が観測できる

```bash
# クリーン環境での実行（launchd相当）
env -i HOME="$HOME" \
    PATH="/usr/local/opt/node@22/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin" \
    bash scripts/<対象>.sh
```

**合格しない例:**
- `bash -n`（構文チェックのみ）で「動いた」と言う
- `--dry-run` で通ったと言う
- 現セッションの PATH に依存した実行をする

**クリーン環境で通らない場合の修正パターン:**
- PATH不足 → スクリプト先頭の `export PATH=` に `/usr/local/opt/node@22/bin` を追加
- 環境変数不足 → `.env` 読み込みを確認、またはスクリプト先頭で明示
- 日付上書き不可 → `TODAY=${TODAY:-$(date +%Y-%m-%d)}` に変更

### Step 4: 判定

| スコア結果 | アクション |
|-----------|-----------|
| PASS (80+) | 検証完了。次のステップへ |
| MINOR (65-79) | 指摘内容を確認し、対応要否をユーザーに報告。ユーザー判断で完了可 |
| MAJOR (50-64) | 指摘を修正 → Step 2に戻る（ループ） |
| REJECT (<50) | 指摘を修正 → Step 2に戻る（ループ） |

### Step 5: ループ終了条件

以下の**両方**を満たしたらループ終了:
1. improvement_loop.shのスコアがMINOR以上（65点+）
2. 全体通しテストが exit 0

## 非対話環境での扱い

SSH/モバイル/launchd環境ではimprovement_loop.shが使えない。
→ tasks.md に「要検証ループ: [ファイル名]」で積み、次回対話セッションで実行。

## このスキル自体の除外

verify-loop/SKILL.md への変更は循環防止のためこのスキルの対象外。
変更はユーザー指示のみ。
