# Render + Groq デプロイ手順

このリポジトリには `render.yaml` の Blueprint 設定を追加しています。Go バックエンド、埋め込み Next.js UI、PostgreSQL、永続ディスクを Render 上で構成し、Groq の OpenAI-compatible API を使う設定です。

## 1. GitHub にアップロード

1. ZIP を展開し、`artex-global-main` の中身を自分の GitHub リポジトリに push します。
2. Render の Dashboard で **New → Blueprint** を選び、そのリポジトリを接続します。
3. `render.yaml` を確認して Blueprint を適用します。

## 2. 環境変数を設定

Render の `artex-global` Web Service → **Environment** で、次の Secret を設定します。

- `OPENAI_API_KEY`: Groq Console で発行した Groq API key を設定します（名前は OpenAI ですが、Groq の互換 API キーを入れます）。
- `ARTEX_ADMIN_PASSWORD`: 強力で一意な管理者パスワード（8文字以上）。パスワード管理ツールで生成した長い値を推奨します。

`ARTEX_LLM_PROVIDER=openai`、`ARTEX_LLM_BASE_URL=https://api.groq.com/openai/v1`、`ARTEX_LLM_MODEL=llama-3.3-70b-versatile` は Blueprint で設定済みです。Groq 側でモデル名や利用可能状況が変更された場合は、Groq Console の現行モデル ID に合わせて `ARTEX_LLM_MODEL` を更新してください。

## 3. デプロイと初回確認

1. Render の Deploy Logs でビルドが成功していることを確認します。
2. サービス URL を開き、管理者ログインを確認します。
3. LLM 設定画面またはログで Groq API の接続を確認します。キーはソースコードや GitHub に書かず、Render の Secret 環境変数にだけ保存してください。

## 注意点

- このアプリは自律的なペネトレーションテスト用ツールです。公開 URL があっても、テスト対象は自分が所有する資産、または明示的な許可を得た検証環境だけに限定してください。
- 管理者パスワードを設定し、共有 URL を不用意に配布しないでください。可能なら Render のアクセス制御や VPN などで利用者を制限してください。
- `render.yaml` はソース構成を前提にしたデプロイ設定です。Render のプラン、Go/Node の対応バージョン、ビルド時間、依存ツールの制約によって追加調整が必要になる場合があります。Render 上で実際のビルド・起動を実行した結果までは、この ZIP の生成だけでは保証できません。
- 永続ディスクはアプリのローカルデータ用、PostgreSQL はアプリの DB 用です。Render の無料プランは永続ディスク等に制限があるため、Blueprint の指定プランと料金を適用前に確認してください。
