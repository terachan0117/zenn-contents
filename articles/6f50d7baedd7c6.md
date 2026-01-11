---
title: "MermaidのGitGraphでGitHub flow/GitLab Flowを描いてみた"
emoji: "🔀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["git", "github", "gitlab", "markdown", "mermaid"]
published: true
---

## はじめに

Mermaid（マーメイド）ってご存知ですか？

テキストベースでフローチャート、シーケンス図、ガントチャートなどの図やグラフを生成できるツールで、Markdown内にコードブロックとして埋め込むことができます。

今回はブランチ戦略の代表例として取り上げられることの多いGitHub flow/GitLab FlowをMermaidのGitGraphとして描いていこうと思います。

この記事では、各フローをMermaidで描画してみることを目的としており、各フローの詳細については取り上げませんのでご了承ください。

## GitHub flow

`main`と`feature`のみの最小構成です。バグ修正や緊急修正も`feature`で行います。

```mermaid
gitGraph
  commit id: "Initial commit"

  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
```

````
```mermaid
gitGraph
  commit id: "Initial commit"

  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
```
````

バグ修正や緊急修正を`bugfix`と`hotfix`という別ブランチに分けたパターンです。

単一環境の場合におすすめです。

```mermaid
gitGraph
  commit id: "Initial commit"

  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"

  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"

  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix bug"
  checkout main
  merge hotfix/hotfix-a id: "Merge hotfix to main"
```

````
```mermaid
gitGraph
  commit id: "Initial commit"

  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"

  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"

  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix bug"
  checkout main
  merge hotfix/hotfix-a id: "Merge hotfix to main"
```
````

## GitLab flow

すべてのブランチを`main`->`staging`->`production`と段階的にマージしていくパターンです。`staging`は`pre-production`という名前の場合もありますが、個人的には`staging`のほうがしっくりきます。

複数環境がある場合におすすめです。

```mermaid
gitGraph
  commit id: "Initial commit"
  branch staging
  checkout staging
  commit id: "Create staging branch"
  branch production
  checkout production
  commit id: "Create production branch"
  checkout main
  commit id: "Protect main"

  %% feature: branch from main
  checkout main
  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
  checkout staging
  merge main id: "Deploy main to staging (includes feature)"
  checkout production
  merge staging id: "Release staging to production (includes feature)"

  %% bugfix: branch from main
  checkout main
  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes bugfix)"
  checkout production
  merge staging id: "Release staging to production (includes bugfix)"

  %% hotfix: branch from main
  checkout main
  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix bug"
  checkout main
  merge hotfix/hotfix-a id: "Merge hotfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes hotfix)"
  checkout production
  merge staging id: "Release staging to production (includes hotfix)"
```

````
```mermaid
gitGraph
  commit id: "Initial commit"
  branch staging
  checkout staging
  commit id: "Create staging branch"
  branch production
  checkout production
  commit id: "Create production branch"
  checkout main
  commit id: "Protect main"

  %% feature: branch from main
  checkout main
  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
  checkout staging
  merge main id: "Deploy main to staging (includes feature)"
  checkout production
  merge staging id: "Release staging to production (includes feature)"

  %% bugfix: branch from main
  checkout main
  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes bugfix)"
  checkout production
  merge staging id: "Release staging to production (includes bugfix)"

  %% hotfix: branch from main
  checkout main
  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix bug"
  checkout main
  merge hotfix/hotfix-a id: "Merge hotfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes hotfix)"
  checkout production
  merge staging id: "Release staging to production (includes hotfix)"
```
````

先ほどとは異なり、`hotfix`のみ`main`ではなく`production`から分岐させ、`production`->`staging`->`main`とマージしていくパターンです。

緊急修正をすぐに本番環境に反映できるメリットがありますが、`staging`と`main`への反映を行わないと大変なことになりますし、緊急修正であってもステージングさせたほうが良いのでは？と個人的には思います。

```mermaid
gitGraph
  commit id: "Initial commit"
  branch staging
  checkout staging
  commit id: "Create staging branch"
  branch production
  checkout production
  commit id: "Create production branch"
  checkout main
  commit id: "Protect main"

  %% feature: branch from main
  checkout main
  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
  checkout staging
  merge main id: "Deploy main to staging (includes feature)"
  checkout production
  merge staging id: "Release staging to production (includes feature)"

  %% bugfix: branch from main
  checkout main
  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes bugfix)"
  checkout production
  merge staging id: "Release staging to production (includes bugfix)"

  %% hotfix: branch from production
  checkout production
  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix critical bug"
  checkout production
  merge hotfix/hotfix-a id: "Release hotfix to production"
  checkout staging
  merge production id: "Merge production to staging"
  checkout main
  merge production id: "Merge production to main"
```

````
```mermaid
gitGraph
  commit id: "Initial commit"
  branch staging
  checkout staging
  commit id: "Create staging branch"
  branch production
  checkout production
  commit id: "Create production branch"
  checkout main
  commit id: "Protect main"

  %% feature: branch from main
  checkout main
  branch feature/feature-a
  checkout feature/feature-a
  commit id: "feat: add feature"
  checkout main
  merge feature/feature-a id: "Merge feature to main"
  checkout staging
  merge main id: "Deploy main to staging (includes feature)"
  checkout production
  merge staging id: "Release staging to production (includes feature)"

  %% bugfix: branch from main
  checkout main
  branch bugfix/bugfix-a
  checkout bugfix/bugfix-a
  commit id: "fix: fix bug"
  checkout main
  merge bugfix/bugfix-a id: "Merge bugfix to main"
  checkout staging
  merge main id: "Deploy main to staging (includes bugfix)"
  checkout production
  merge staging id: "Release staging to production (includes bugfix)"

  %% hotfix: branch from production
  checkout production
  branch hotfix/hotfix-a
  checkout hotfix/hotfix-a
  commit id: "fix: hotfix critical bug"
  checkout production
  merge hotfix/hotfix-a id: "Release hotfix to production"
  checkout staging
  merge production id: "Merge production to staging"
  checkout main
  merge production id: "Merge production to main"
```
````

## おわりに

いかがでしたか？Mermaidを推していきましょう。[公式ドキュメント](https://mermaid.js.org/syntax/gitgraph.html)もチェックしてみてください！