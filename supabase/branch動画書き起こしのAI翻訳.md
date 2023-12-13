A Postgres database for every GitHub branch 🤝 - YouTube
https://www.youtube.com/watch?v=peXKxavGnBo

Transcript:
en:(00:00) database branching means you can have separate database instances or entire superbase instances for each feature of your application so let's say you have a main branch in GitHub which is the production version of your application so on the superbas side you have a hosted production version of your database which is linked to your production application to read and write data but now you want to add some new functionality so you create a new Branch to work on your feature but what if it needs to change the structure of your
ja:(00:00) データベースのブランチングでは、アプリケーションの各機能に対して別々のデータベースインスタンスまたは完全な Supabase インスタンスを持つことができます。

例えば、GitHub の main ブランチには、アプリケーションの本番バージョンがあります。Supabase 側では、本番アプリケーションにリンクされた、ホストされた本番データベースがあります。しかし、新しい機能を追加したい場合、新しいブランチを作成してその機能を作業します。しかし、その機能がデータベースの構造を変更する必要がある場合はどうでしょうか。

en:(00:26) database now with database branching you can spin up a separate instance of super base specifically for your feature Branch any changes that are made to the structure of the database are tracked in migration files which can then be committed to GitHub along with the rest of your application code for this new feature when you push those changes up to GitHub theel kicks off a new build and preview deploy of your application and seeing that there are new migrations we'll run those against the database Branch as well now when we're ready to
ja:(00:26) データベースのブランチングでは、データベースの構造に対して行われた変更は、コミットされた migration ファイルに追跡され、新しい機能の残りのアプリケーションコードとともに GitHub にコミットすることができます。

これらの変更を GitHub にプッシュすると、Vercel はアプリケーションの新しいビルドとプレビューデプロイを開始します。新しい migration があることを確認すると、それらをデータベースブランチに対して実行します。これで、本番アプリケーションに統合する準備ができました。


(00:52) merge this feature into production we simply merge this pull request and this will automatically deploy a new version of our application and run the new migrations against the production version of superbase so it's the workflow we know and love from G but now for your database I'm going to show you the process of building a new feature using branching to keep everything in sync let's get into it here I have a simple bookmarking app where I can keep a track of my favorite sites I can sign in with John superb. and my super secure
ja:(00:52) この機能を本番アプリケーションに統合するには、このプルリクエストをマージするだけで、アプリケーションの新しいバージョンが自動的にデプロイされ、superbase の本番バージョンに新しい migration が実行されます。これは、G から知っているワークフローであり、データベースに対しても同じです。すべてを同期させるためにブランチングを使用して新しい機能を構築するプロセスをお見せします。それでは始めましょう。

(01:20) password it's totally not just the word super secure with threes for ease and then as a signed in user I can see my favorite websites and a description for each one this is linked to my superbase project running locally so I have my studio instance here on Port 54323 and down here
ja:(01:20) パスワードは、super という単語に 3 つの e を付けたものだけではありません。サインインしたユーザーとして、お気に入りの Web サイトとそれぞれの説明を見ることができます。これは、ローカルで実行されている superbase プロジェクトにリンクされています。54323 ポートでここにスタジオインスタンスがあり、ここには

I'm printing out some helpful information that we can use while developing so this shows the URL and a non key for the superbase project we are currently connected to so we can see that's our instance running locally this
ja:役立つ情報を出力しています。これは、superbase プロジェクトに接続されている現在の URL と anon key を表示しています。つまり、ローカルで実行されているインスタンスです。

(01:45) app itself is written in nextjs and it has this superbase folder for our locally running instance which has each of our database migrations so anytime the structure of our database changes like we create a new table or add a new column we document each of those changes in a migration file so anyone on the team can run those migrations and ensure their local database instance is in the right state so this first migration is setting up some tables for Out book marks and then this one is enabling roow level security and adding some policies
ja:(01:45) アプリケーション自体は nextjs で書かれており、ローカルで実行されているインスタンス用の superbase フォルダーがあり、データベースの構造が変更されるたびに、新しいテーブルを作成したり、新しいカラムを追加したりするなど、それらの変更を migration ファイルに記録しています。これにより、チームの誰もがこれらの migration を実行し、ローカルのデータベースインスタンスが正しい状態になっていることを確認できます。

(02:12) we also have this seed file to populate the database with some example data so we can see this is inserting a new user me in this case and then inserting some bookmarks of some of my favorite sites so this nextjs app has been cloned down from this repo which we can see is in the same state with those same migrations and this repo is used to deploy the live version of our application using versel so let's step through the process of building a new feature I want to add a favorite button for each of my bookmarks so over in my
ja:(02:12) また、このシードファイルを使用して、データベースにいくつかのサンプルデータを挿入します。これにより、この場合は新しいユーザー me が挿入され、いくつかのブックマークが私のお気に入りのサイトに挿入されます。この nextjs アプリは、このリポジトリからクローンされています。これは、同じ状態で同じ migration を持っています。このリポジトリは、versel を使用してアプリケーションのライブバージョンをデプロイするために使用されます。新しい機能を構築するプロセスをステップバイステップで説明します。私は、私のブックマークのそれぞれにお気に入りのボタンを追加したいと思います。

(02:38) local instance of superbase I can just use the studio UI to add a new column with the name favorite which is of type bullion we can set the default value to false and turn off allow nullable as we always want this to be a bullan value of either true or false and let's click save to add that new column now I am super efficient at writing nextjs so I know that to build this feature I just need to uncomment this line to render the favorite component and look at that we now have a favorite button for each of our bookmarks and if I toggle one of
ja:(02:38) superbase のローカルインスタンスでは、studio UI を使用して、お気に入りの名前の新しい列を追加するだけで済みます。これは bullion 型です。デフォルト値を false に設定し、nullable をオフにします。これは常に true または false のいずれかの bullan 値である必要があるためです。新しい列を追加するには、保存をクリックします。私は nextjs を書くのがとても上手なので、この機能を構築するには、お気に入りのコンポーネントをレンダリングするためにこの行のコメントを外すだけで済みます。それを見てください。ブックマークごとにお気に入りのボタンがあります。もし私が一つを切り替えたら

(03:05) these to be my favorite we can see that value is updated in superbase so our feature is ready to deploy but we now have app changes as well as database changes with a feature that's additive like this where we're just adding a new column it's not being used by anything until we deploy the new version of our app we could deploy the database first and then deploy our app but if we were changing the names of columns or deleting columns or dropping tables as I know we all love to do we could be a state where our application and our
ja:(03:05) これらを私のお気に入りにすると、その値が superbase で更新されていることがわかります。このような追加的な機能では、新しい列を追加するだけなので、アプリケーションの新しいバージョンをデプロイするまで何も使用されません。データベースを先にデプロイしてからアプリをデプロイすることができますが、列の名前を変更したり、列を削除したり、テーブルを削除したりする場合は、アプリケーションと

(03:31) database are out of sync and our requests for data are actually failing but not anymore thanks to branching we can create a diff of any structural changes we've made locally by running superbase DB diff and writing the result to a file or - F and the name of our file will be add- favorite DB button now we can check that file to confirm that it's just altering our bookmarks table and adding a column for favorite and since this folder lives within our nextjs project we can commit this migration along with the changes
ja:(03:31) データベースが同期されていないため、データのリクエストが実際に失敗しています。しかし、もうブランチングのおかげで、ローカルで行った構造的な変更の差分を superbase DB diff を実行してファイルに書き込むことで作成することができます。または、- F というファイルの名前は、add- favorite DB button となります。これで、そのファイルを確認して、ブックマークテーブルを変更してお気に入りの列を追加しているだけであることを確認できます。そして、このフォルダは nextjs プロジェクト内に存在するので、この migration を次のようにコミットすることができます。

superbase DB diff

(03:59) required for our application to implement this favorite button so this is a feat or feature and it will add favorite schema changes so let's create that commit and then we can push our feature Branch to GitHub and open a pull request we can add a description that this will add the ability to favorite my bookmarks and create the pull request so we can see versel has detected these changes and is building and deploying a new preview version of our application and now superbase has detected some changes in the schema and is deploying a
ja:(03:59) このお気に入りのボタンを実装するために必要な変更を行います。これは、feat または feature であり、お気に入りのスキーマ変更を追加します。このコミットを作成し、feature Branch を GitHub にプッシュしてプルリクエストを開きます。これにより、vercel がこれらの変更を検出し、アプリケーションの新しいプレビューバージョンをビルドしてデプロイしていることがわかります。そして、superbase はスキーマにいくつかの変更があることを検出し、デプロイしています。

(04:27) new preview database that's in exactly the same State as the feature we were working on locally while all of that is deploying we can check out our hosted superbase instance which is our production version of our database and now we have this drop down where we can select manage branches and see all the different instances or branches of our superbase project we can see our new preview branch has finished deploying so let's check it out by clicking here then we can head over to the table editor and click on our bookmarks table and see
ja:(04:27) ローカルで作業していた機能とまったく同じ状態の新しいプレビューデータベースがあることを確認できます。これらすべてがデプロイされている間に、私たちは私たちの本番データベースであるホストされた superbase インスタンスをチェックすることができます。そして、ここにクリックすることで、私たちの新しいプレビューブランチがデプロイされたことがわかります。そして、テーブルエディターに移動して、ブックマークテーブルをクリックしてみましょう。

(04:52) that our new favorite column has been added and it set the default value to false for all of our existing bookmarks so back over in verl we can see this preview deployment of our app and we can open that one up and see this login screen but if we take a look at the URL and a non key that this preview version is connected to it matches the preview branch of our superbase project so each feature the team is working on can have its own branch which is a completely isolated instance of super base specifically for this feature branch of
ja:(04:52) 新しいお気に入りの列が追加され、既存のすべてのブックマークのデフォルト値が false に設定されていることがわかります。verl に戻ると、アプリのこのプレビューデプロイを見ることができ、それを開いてこのログイン画面を見ることができますが、このプレビューバージョンが接続されている URL と non key を見ると、それは superbase プロジェクトのプレビューブランチと一致していることがわかります。チームが作業している各機能には、この機能ブランチ専用の完全に分離された super base インスタンスを持つことができます。


(05:18) the application very cool so let's sign in and we can see our new favorite buttons for each of our bookmarks and again if we favored a couple and head over to our superbase instance for this Branch those values have been updated this is is awesome let's deploy it to production so back over in our PR we can see that the checks have passed and these migrations will successfully run on the production database so all we need to do is merge this PR into our main branch that will run those migrations against our production
ja:(05:18) データベースとアプリケーションを更新します。とてもクールです。サインインして、ブックマークごとに新しいお気に入りのボタンが表示されていることがわかります。そして、もう一度、私たちはいくつかのお気に入りを作って、このブランチの私たちの superbase インスタンスに向かって頭を向けると、これらの値が更新されていることがわかります。これは素晴らしいです。本番にデプロイしましょう。PR に戻ると、チェックが合格したことがわかります。これらの migration は本番データベースで正常に実行されます。したがって、この PR を main ブランチにマージするだけで、本番データベースに対してこれらの migration を実行することができます。

(05:43) database and deploy the new version of our application so back over in our hosted superbase Studio we can change the branch back to main or whatever our production branch is and then if we look at the table editor and the bookmarks table we can see that our new column has been added we don't have any data in production so let's go to our deployments in versel and open the production version of our application and we don't yet have an account so let's sign up as John superbase doio and my super secure
ja:(05:43) データベースをデプロイします。そして、私たちのホストされた superbase スタジオに戻って、ブランチを main または本番ブランチに戻し、テーブルエディターとブックマークテーブルを見てみると、新しい列が追加されていることがわかります。本番ではデータがありませんので、versel のデプロイに移動して、アプリケーションの本番バージョンを開きます。そして、まだアカウントがありませんので、John superbase doio と私の super secure でサインアップしましょう。

en:(06:08) password and we can add a new bookmark for ver.com and another one for openai and we can favorite this one for versel because our feature has been successfully deployed to production and now that you're a pro collaborating with others why not learn how easy it is to collaborate with robots by checking out this video right here where we use superbase AI to generate RLS policies making implementing author ation a breeze but until next time keep building cool stuff
ja:(06:08) パスワードを追加して、ver.com と openai のブックマークを追加し、versel のためにこのブックマークをお気に入りにすることができます。なぜなら、私たちの機能が本番に正常にデプロイされたからです。そして、あなたが他の人と協力しているプロになったなら、なぜここでこのビデオをチェックして、superbase AI を使用して RLS ポリシーを生成することがどれほど簡単かを学ばないのですか。これにより、オーソライゼーションの実装が簡単になります。しかし、次の機会まで、クールなものを作り続けてください。



