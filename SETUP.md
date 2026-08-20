# 開発環境セットアップ

このプロジェクトは GitHub / Vercel / Supabase を連携させて開発します。
メンバー全員が同じ環境で開発できるよう、セットアップ手順をまとめました。

## 使っているサービス

| サービス | 用途 | プロジェクト名 |
|---|---|---|
| GitHub | ソースコード管理 | [pamo0827/AI-Hackathon](https://github.com/pamo0827/AI-Hackathon) |
| Vercel | ホスティング・デプロイ | `ai-hackathon`（スコープ: `yoshi0827101328-9306's projects`） |
| Supabase | DB・認証・API | `ai-hackathon`（リージョン: `ap-northeast-1`） |

mainブランチにpushすると Vercel が **GitHub連携で自動デプロイ** します。
そのためVercel・Supabaseのアカウント招待は不要です（コード管理者だけが持っていればOK）。
必要なのはGitHubリポジトリへのアクセスと、`.env.local` の中身だけです。

---

## 0. 前提: GitHub招待を受ける

始める前に、リポジトリ管理者から以下の招待を受けてください。

- [ ] GitHubリポジトリ (`pamo0827/AI-Hackathon`) への Collaborator 招待

> Vercel/Supabaseは自動デプロイの仕組み上、招待なしで開発できます。
> ローカルでDB管理（マイグレーション追加など）をしたい人だけ、管理者にSupabase組織への招待を相談してください。

---

## 1. 必要なツールのインストール

```bash
# Node.js（推奨: nvmなどでバージョン管理）
# https://nodejs.org/ からLTS版を入れてもOK

# GitHub CLI
brew install gh
```

インストール確認:

```bash
gh --version
```

---

## 2. リポジトリをクローン

```bash
gh auth login   # まだの場合
git clone https://github.com/pamo0827/AI-Hackathon.git
cd AI-Hackathon
```

---

## 3. 環境変数を受け取る

Supabaseの接続情報は管理者から直接共有してもらいます（Slack DM、1Passwordなど、平文チャットに貼らない方法で）。
プロジェクト直下に `.env.local` を作り、共有された値を貼り付けてください。

```
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE_KEY=...
```

> `.env.local` は `.gitignore` 対象です。絶対にコミットしないでください。
> `SUPABASE_SERVICE_ROLE_KEY` はRLSを無視できる強い権限です。サーバー側コード以外では使わないでください。

---

## 4. 依存パッケージのインストール & 起動

```bash
npm install
npm run dev
```

（`package.json` がまだない場合はプロジェクト初期化後にこの手順を追記します）

---

## 5. デプロイの確認方法

- mainブランチにpushすると自動で本番デプロイされます（Vercelアカウント不要）
- PRを作るとプレビューURLがPRのコメントに自動投稿されます
- 本番URLやビルドログを見たい場合は、管理者にVercelダッシュボードのスクリーンショットやプレビューURLを共有してもらってください

---

## 6. ローカルでSupabaseを動かしたい場合（管理者権限が必要・任意）

DBスキーマの変更やマイグレーション追加をしたい場合のみ、管理者にSupabase組織への招待を依頼してください。招待後:

```bash
brew install supabase/tap/supabase
supabase login
supabase link --project-ref zgqdkbjfvfllegoeellh
```

Docker Desktopがあれば、ローカルにSupabaseスタックを起動して試すこともできます。

```bash
supabase start   # ローカルにSupabaseスタックを起動
supabase status  # ローカルAPI URL/anon keyを確認
supabase stop     # 終了
```

---

## トラブルシューティング

| 症状 | 対処 |
|---|---|
| `.env.local` を作ってもアプリがSupabaseに繋がらない | 値のtypoや改行混入がないか確認。管理者に最新の値を再共有してもらう |
| `git clone` / `git push` で権限エラー | GitHubリポジトリへの招待が完了しているか確認 |
| `supabase link` で `Unauthorized` | Supabase組織への招待が必要（6章を参照） |
| 自分の変更がデプロイに反映されない | mainブランチにマージされているか、Vercelのビルドが失敗していないか管理者に確認 |

---

## 困ったら

- GitHubリポジトリ: https://github.com/pamo0827/AI-Hackathon
- 管理者（Vercel/Supabaseダッシュボードへのアクセス権を持つ人）に確認してください
