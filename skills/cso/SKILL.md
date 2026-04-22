---
name: cso
description: セキュリティレビュー（OWASP Top10 + STRIDE脅威モデル）。認証変更・APIエンドポイント追加・外部データ受け取りのPR前に自動発動。トリガー:「セキュリティ確認」「脅威モデル」「CSO視点で見て」「OWASPチェック」「STRIDEで評価」
---

# CSO Review

コード変更・設計変更をOWASP Top10 + STRIDE脅威モデルの2軸でレビューする。
実行中のプロジェクトを自動検出し、プロジェクト固有のリスクコンテキストを適用する。

## Step 0: プロジェクト自動検出

```bash
REPO_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
REMOTE=$(git -C "$REPO_ROOT" remote get-url origin 2>/dev/null || echo "")
DIR=$(basename "$REPO_ROOT")

if echo "$REMOTE$DIR" | grep -qi "axis\|AINAGOC\|Accommodation"; then
  PROJECT="axis"
elif echo "$REMOTE$DIR" | grep -qi "ironlog\|IronLog"; then
  PROJECT="ironlog"
else
  PROJECT="unknown"
fi

echo "検出プロジェクト: $PROJECT（$REPO_ROOT）"
```

**unknown の場合は処理を止めず、ユーザーに提案する:**

```
このプロジェクト（[ディレクトリ名] / remote: [URL]）はスキルの設定にありません。
共通チェック（OWASP A01-A10 + STRIDE 全項目）でレビューを続けますか？
またはスキルに追加しますか？追加する場合は以下3ファイルの該当箇所に設定を足します:
- .claude/skills/cso/SKILL.md
- .claude/skills/land-and-deploy/SKILL.md  
- .claude/skills/canary/SKILL.md
```

ユーザーが「共通でやって」と言った場合 → プロジェクト固有セクションをスキップして共通チェックのみ実行  
「追加して」と言った場合 → 3ファイルに `elif echo "$REMOTE$DIR" | grep -qi "[新プロジェクト名]"` を追記し、フロー定義を書き足す

## Step 1: 変更差分の取得

```bash
cd "$REPO_ROOT"
git diff main...HEAD --stat
git diff main...HEAD
```

## Step 2: OWASP Top10 チェック

以下の観点で差分を精査する。【AXIS固有】【IronLog固有】は該当プロジェクトのみ確認:

| # | カテゴリ | 共通確認ポイント | プロジェクト固有 |
|---|---------|----------------|----------------|
| A01 | アクセス制御の欠陥 | ロールチェックの漏れ・未認証でアクセスできるエンドポイント | 【AXIS】AINAGOC職員/一般/管理者の3ロール |
| A02 | 暗号化の失敗 | 個人情報の平文保存・HTTP転送 | 【AXIS】選手・宿泊・ボランティアデータ |
| A03 | インジェクション | ユーザー入力のサニタイズ・クエリの文字列結合 | 【AXIS】Prisma rawクエリ使用箇所 |
| A04 | 安全でない設計 | 設計レベルの欠陥（後付けで直せないもの） | 【AXIS】ボランティア管理・アクレディテーション |
| A05 | セキュリティの設定ミス | 環境変数の露出・CORS・デフォルト設定 | 【AXIS】Vercel環境変数 |
| A06 | 脆弱なコンポーネント | 依存パッケージの既知脆弱性（npm audit / swift package等） | — |
| A07 | 認証・セッション管理 | セッション固定化・トークン検証の漏れ | 【AXIS】next-auth v5・Azure Entra ID 【IronLog】なし（ローカルアプリ） |
| A08 | ソフトウェア完全性 | CI/CDパイプライン汚染・依存関係のすり替え | — |
| A09 | ログ・監視の失敗 | エラーログへの個人情報混入・ログ未記録 | — |
| A10 | SSRF | 外部URLフェッチ処理（画像・Webhook等） | 【AXIS】S3/SES経由の外部通信 |

## Step 3: STRIDE 脅威モデリング

変更箇所に対してSTRIDEで分析:

| 脅威 | 問い | プロジェクト固有 |
|------|------|----------------|
| **S**poofing（なりすまし） | 正規ユーザーになりすませないか | 【AXIS】Azure Entra IDトークン検証 |
| **T**ampering（改ざん） | データを書き換えられないか | 【AXIS】選手登録データ・スコア |
| **R**epudiation（否認） | 「やっていない」と言い逃れできるか | 操作ログ・監査証跡の有無 |
| **I**nformation disclosure（漏洩） | 見せてはいけない情報が出ないか | エラーレスポンスに内部情報が含まれないか |
| **D**enial of service（妨害） | 大量リクエストで止められないか | 【AXIS】大会当日の集中アクセス・APIレート制限 |
| **E**levation of privilege（特権昇格） | 一般ユーザーが管理者になれないか | 【AXIS】AINAGOC職員権限の昇格 |

## Step 4: プロジェクト固有リスク評価

**AXIS の場合のみ実行:**
- 大会期間中（9/19〜10/4）の可用性要件: ダウンタイム許容ゼロ
- 個人情報漏洩 → AINAGOC情報管理規程違反 → 契約解除リスク
- 複数省庁・組織委員会との連携: 信頼境界の明確化

**IronLog の場合のみ実行:**
- HealthKit データのローカル保持確認（クラウド送信していないか）
- App Store Review ガイドライン準拠（プライバシーラベルとの整合性）

## Step 5: 判定と報告

```
## CSO Review サマリー（プロジェクト: [axis/ironlog/unknown]）

### 高リスク（即時修正）
- [問題・修正方法]

### 中リスク（今スプリント内）
- [問題・修正方法]

### 低リスク・改善提案
- [観察事項]

### 問題なし
- [確認済みカテゴリ]
```

## 使い所

- `axis-codex-adversarial` との併用推奨（Codex視点 + OWASP/STRIDE視点の2重確認）
- PRマージ前: `axis-codex-adversarial` → `cso` の順で実行
- 認証フロー変更時は必ずSTRIDEのSとEを優先確認
- プロジェクトが「unknown」と検出された場合は共通チェック項目のみ実施
