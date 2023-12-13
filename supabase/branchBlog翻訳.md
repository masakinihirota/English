---
created: 2023-12-14T05:04:28 (UTC +09:00)
tags: []
source: https://supabase.com/blog/supabase-branching
author:
---


# Supabase Branching
> ## 要約
> 各プルリクエストごとにPostgresデータベースを作成します。
---
![Supabase Branching](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Fbranching-thumb.png&w=3840&q=75)



数ヶ月前、私たちは（多少野心的な）早期アクセスフォームを使ったブランチングに取り組んでいることをお伝えしました。
今日、早期アクセスのサブスクリプションを持つユーザーにアクセスを提供しています。内部的には、このローンチウィークに公開アクセスをすることを期待していましたが、まあ、[_これは難しいと言ったことを覚えておいてください_](https://supabase.com/blog/supabase-local-dev#supabase-branching-is-hard)。
最初に登録した順に、早期アクセスを申し込んだ有料の組織にバッチごとに提供しています。
![](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Fbranching-ui--dark.png&w=3840&q=75)
Supabaseダッシュボードの新しいブランチングUI

## ブランチングとは？

en:開発中には、おそらくPostgresデータベースで実験する必要があるでしょう。現在、Supabase CLIを使用して、ローカル開発マシンでそれが可能です。CLIで`supabase start`を実行すると、Supabaseスタック全体がローカルで実行されます。アイデアを試したり、いつでも`supabase db reset`を実行して再開することができます。データベースマイグレーションで変更をキャプチャしたい場合は、`supabase db diff`を実行できます。
ブランチングはこれの自然な拡張ですが、_ローカル_データベースだけで実験する代わりに、_リモート_データベースも取得できます。上記のワークフローを引き続き使用し、変更をGitにコミットすると、Supabaseプレビューブランチで実行されます。

![各Gitブランチには対応するSupabaseプレビューがあります。](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Ffeat-branches-examples--dark.png&w=3840&q=75)
各Gitブランチには対応するSupabaseプレビューがあります。
各Gitブランチには対応するSupabaseプレビューがあり、更新をプッシュするたびに自動的に更新されます。ワークフローの残りの部分はおなじみの感じになるはずです。Pull RequestをメインのGitブランチにマージすると、Supabaseはプロダクションデータベース内でデータベースマイグレーションを実行します。
プロジェクトのプレビューブランチは安全性を考慮して設計されています。それぞれが独立したインスタンスであり、異なるセットのAPIキーとパスワードを持っています。各インスタンスには、Postgresデータベース、Auth、ファイルストレージ、リアルタイム、エッジ関数、データAPIのすべてのSupabase機能が含まれています。
![各Supabaseプレビューは、Supabaseサービスの完全なスイートを備えた専用のインスタンスです。](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Fisolated-instances--dark.png&w=3840&q=75)
各Supabaseプレビューは、Supabaseサービスの完全なスイートを備えた専用のインスタンスです。
リラックスした開発環境でも、チームの誰かがキーを誤って漏洩させても、プロダクションブランチには影響しません。

### Vercelプレビューのサポート
Supabase Branchingは、Vercelの[プレビュー](https://vercel.com/features/previews)デプロイメントと完全に連携するように設計されています。これにより、ブランチングとともに_完全なスタック_を取得できます。
![Vercelはプレビューデプロイメントをビルドし、プレビューデプロイメントはプレビューブランチ上のSupabaseサービスに接続できます。](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Fvercel-support--dark.png&w=3840&q=75)
Vercelはプレビューデプロイメントをビルドし、プレビューデプロイメントはプレビューブランチ上のSupabaseサービスに接続できます。
Vercelの[Integration](https://vercel.com/integrations/supabase-v2)には、Vercelのエクスペリエンスをシームレスにするためのいくつかの改善を加えています。たとえば、Supabaseプレビューブランチごとに異なるセキュアなデータベース認証情報を提供するため、Vercelの環境変数にアプリがプレビューブランチに接続するために必要な接続シークレットを自動的に設定します。
![Vercelの環境変数設定ページで、Gitブランチベースの環境変数が表示されています。](https://supabase.com/_next/image?url=%2Fimages%2Fblog%2Flwx-supabase-branching%2Fvercel-env-var-settings--dark.png&w=3840&q=75)
Vercelの環境変数設定ページで、Gitブランチベースの環境変数が表示されています。



### ホストされたプレビューブランチでの開発
Supabaseの最も愛された機能の1つはダッシュボードです。[お願い](https://supabase.com/docs/guides/platform/maturity-model#in-production)しても、開発者は単純にそれをすべてに使用したいと思っているようです - 本番環境でもです。
ブランチングの素晴らしいところは、すべてのSupabaseプレビューをダッシュボードから管理できることです。スキーマの変更を行ったり、SQLエディタにアクセスしたり、[新しいAIアシスタント](https://supabase.com/blog/studio-introducing-assistant)を使用したりすることができます。変更が完了したら、ローカルマシンで単純に `supabase db diff` を実行して変更を取得し、それをGitにコミットするだけです。
ただし、引き続きローカルで開発することをお勧めします！プレビューブランチは、チームの誰かが破壊的なマイグレーションをプッシュした場合にいつでも削除される可能性がありますので、[ペットではなく家畜として](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets)扱う必要があります。


### データベースマイグレーション
ブランチングは、GitHubを始めとするGitプロバイダと連携するように開発されています。
[GitHubアプリ](https://github.com/apps/supabase)は、接続されたGitHubリポジトリ内の変更を監視します。プルリクエストを作成すると、プレビューブランチが起動し、`./supabase/migrations`でマイグレーションが実行されます。エラーがある場合は、そのgitコミットに関連付けられた[Check Run](https://docs.github.com/en/rest/checks/runs?apiVersion=2022-11-28)にログが記録されます。すべてのチェックが成功すると、新しいプレビューブランチが使用可能になります。
新しいマイグレーションファイルをGitブランチにプッシュすると、アプリはプレビューブランチでそれを段階的に実行します。これにより、既存のシードデータ上でスキーマの変更を簡単に検証できます。
最後に、そのプルリクエストをマージすると、アプリは新しいマイグレーションを本番環境で実行します。既に他のプルリクエストが開いている場合は、それらのマイグレーションファイルを本番ブランチのタイムスタンプよりも後のものに更新する必要があります。[標準的なマイグレーションの手法](https://supabase.com/docs/guides/cli/managing-environments)に従ってください。


### データのシーディング
プレビューブランチには、ローカル開発環境と同じ方法でデータをシードすることができます。リポジトリに `./supabase/seed.sql` を追加し、シードスクリプトはプレビューブランチが作成されたときに実行されます。
オプションで、`supabase db reset --db-url <branch-connection-string>` を実行することでデータベースをリセットすることもできます。ブランチ接続文字列は、Supabase CLIの[ブランチ管理](https://supabase.com/docs/reference/cli/supabase-branches-get)コマンドを使用して、個人アクセストークンを使用して取得することができます。
私たちは、コピー・オン・ライトシステムを使用したデータマスキング技術について調査しています。これにより、プレビューブランチ内で本番のワークロードをエミュレートすることができます。これは、ファイルストレージとも連携する予定です。


## 将来の考慮事項
これだけでもBranching v0には十分な機能があります。Branchingは将来の開発者のワークフローの中核となる要素になります。次に探求するテーマは次のとおりです：


### 宣言的な設定
「コード内の設定」についてはまだ作業中です。たとえば、プレビューブランチで使用するGoogle Authを本番環境とは異なるものにしてみたい場合、コードが宣言的であればはるかに簡単になるでしょう。これは、[config.toml](https://supabase.com/docs/guides/cli/config)ファイル内に記述されている場合です。


### ダッシュボードへの自動コミット
現在のバージョンでは、ダッシュボードを使用してプレビューブランチで変更を作成した場合、その変更をGitリポジトリに取り込むためにローカルで `db diff` を実行する必要があります。接続したGitリポジトリで変更を自動的にキャプチャする機能に取り組む予定です。


### シーディングの拡張動作
シードデータを生成するためのさまざまな戦略があります。私たちはシードデータを生成するためにAIを使用したこともありますが、それは楽しかったです。また、[postgresql-anonymizer](https://postgresql-anonymizer.readthedocs.io/en/stable/)や[Snaplet](https://docs.snaplet.dev/recipes/supabase)のような、安全な開発のためにデータを匿名化しながら本番データをクローンする専門家のアプローチも好きです。


### コピー・オン・ライト
開発中の機能があります :). CoWは、データベースのスナップショットからブランチを作成し、"本番のような"ワークロードでテストを実行できることを意味します。これは、[Postgres.ai](http://postgres.ai/)が使用しているアプローチです。先述のように、[ファイルストレージ](https://supabase.com/storage)とも連携する方法を見つける必要があります。


## Branchingを使用したいですか？
まだ早期アクセスの申し込みフォームが開いており、数週間かけて組織を段階的にオンボードし、これらの早期ユーザーと価格について協力しています。
[Branchingドキュメント](https://supabase.com/docs/guides/platform/branching)をご覧ください。また、フィードバックがある場合は、[ディスカッションに参加](https://github.com/orgs/supabase/discussions/18937)することもできます。


## その他のLaunch Week X
-   [Day 1 - Supabase Studioのアップデート：AIアシスタントとユーザーのなりすまし](https://supabase.com/blog/studio-introducing-assistant)
-   [Day 2 - Edge Functions：Nodeとネイティブnpmの互換性](https://supabase.com/blog/edge-functions-node-npm)
-   [Postgres Language Server：パーサーの実装](https://supabase.com/blog/postgres-language-server-implementing-parser)
-   [The Supabase Album](https://www.youtube.com/watch?v=r1POD-IdG-I)
-   [Launch Week X Hackathon](https://supabase.com/blog/supabase-hackathon-lwx)

