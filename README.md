# Zenn Contents

Zenn投稿コンテンツの管理用リポジトリです。`main`ブランチに変更があると、zenn.devへ自動で同期（デプロイ）が行われます。

## 新しい記事を作成する

```bash
npx zenn new:article
```

## 新しい本を作成する

```bash
npx zenn new:book
```

## 投稿をプレビューする

```bash
npx zenn preview
```

## Zenn CLIをアップデートする

```bash
npm install zenn-cli@latest
```

## コンテンツを削除する

安全のため、コンテンツの削除はダッシュボードからのみ行うことができます。

連携したGitHubリポジトリにコンテンツが残った状態でダッシュボードから削除しても、次回デプロイ時にコンテンツが復活します。

コンテンツを完全に削除するには、ダッシュボードから削除することに加え、GitHubリポジトリ側でも当該コンテンツを削除してください。