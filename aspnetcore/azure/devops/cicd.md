---
title: 継続的インテグレーションとデプロイ - ASP.NET Core および Azure を使用した DevOps
author: CamSoper
description: 継続的インテグレーションと ASP.NET Core と Azure で DevOps での展開
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: seodec18
uid: azure/devops/cicd
ms.openlocfilehash: e5bddde41291c9573f58d749bbf830de9ea9319d
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121595"
---
# <a name="continuous-integration-and-deployment"></a>継続的インテグレーションとデプロイ

前の章では、フィード リーダーの簡単なアプリのローカル Git リポジトリを作成します。 この章では、そのコードを GitHub リポジトリに発行し、Azure のパイプラインを使用して、Azure DevOps サービス パイプラインを構築します。 パイプラインは、継続的なビルドとアプリのデプロイに使用できます。 任意のコミットを GitHub リポジトリには、ビルドと Azure Web アプリのステージング スロットにデプロイをトリガーします。

このセクションで、次のタスクを完了します。

* アプリのコードを GitHub に発行します。
* ローカル Git デプロイを切断します。
* Azure DevOps 組織を作成します。
* Azure DevOps サービスのチーム プロジェクトを作成します。
* ビルド定義の作成
* リリース パイプラインを作成します。
* GitHub に変更をコミットし、自動的に Azure にデプロイ
* Azure のパイプラインのパイプラインを調べる

## <a name="publish-the-apps-code-to-github"></a>アプリのコードを GitHub に発行します。

1. ブラウザーのウィンドウを開きに移動します`https://github.com`します。
1. をクリックして、 **+** ドロップダウンで、ヘッダーと選択**新しいリポジトリ**:

    ![新しいリポジトリの GitHub オプション](media/cicd/github-new-repo.png)

1. 自分のアカウントを選択、**所有者**ドロップダウンでは、入力と*単純なフィード-リーダー*で、**リポジトリ名**テキスト ボックス。
1. をクリックして、**リポジトリの作成**ボタンをクリックします。
1. ローカル コンピューターのコマンド シェルを開きます。 ディレクトリに移動し、*単純なフィード-リーダー* Git リポジトリが格納されています。
1. 既存の名前を変更*原点*にリモート*上流*します。 次のコマンドを実行します。
    ```console
    git remote rename origin upstream
    ```
1. 新しい追加*原点*github リポジトリのコピーをリモートでポイントします。 次のコマンドを実行します。
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. 新しく作成された GitHub リポジトリをローカル Git リポジトリを発行します。 次のコマンドを実行します。
    ```console
    git push -u origin master
    ```
1. ブラウザーのウィンドウを開きに移動します`https://github.com/<GitHub_username>/simple-feed-reader/`します。 GitHub リポジトリでコードが表示されることを検証します。

## <a name="disconnect-local-git-deployment"></a>ローカル Git デプロイを切断します。

次の手順でローカル Git デプロイを削除します。 Azure のパイプライン (Azure DevOps サービス) が置き換え両方、その機能を強化します。

1. 開く、 [Azure portal](https://portal.azure.com/)に移動し、*ステージング (mywebapp\<unique_number\>ステージング/)* Web アプリ。 入力して、Web アプリをすばやく配置できます*ステージング*ポータルの検索ボックスをオンにします。

    ![ステージング Web アプリの検索語句](media/cicd/portal-search-box.png)

1. クリックして**展開オプション**します。 新しいパネルが表示されます。 クリックして**切断**前の章で追加されたローカル Git ソース管理構成を削除します。 クリックして、削除操作を確定します、**はい**ボタンをクリックします。
1. 移動し、 *mywebapp < unique_number >* App Service。 念のため、App Service をすばやく検索する、ポータルの検索ボックスを使用できます。
1. クリックして**展開オプション**します。 新しいパネルが表示されます。 クリックして**切断**前の章で追加されたローカル Git ソース管理構成を削除します。 クリックして、削除操作を確定します、**はい**ボタンをクリックします。

## <a name="create-an-azure-devops-organization"></a>Azure DevOps 組織を作成します。

1. ブラウザーを開きに移動、 [Azure DevOps 組織の作成ページ](https://go.microsoft.com/fwlink/?LinkId=307137)します。
1. 一意の名前を入力、**覚えやすい名前を選んで**テキスト ボックスに、Azure DevOps 組織にアクセスするための URL を作成します。
1. 選択、 **Git**ラジオ ボタン、GitHub リポジトリでコードがホストされているためです。
1. **[Continue]** をクリックします。 少し待つ、アカウントとチーム プロジェクトの場合は、名前付き*MyFirstProject*が作成されます。

    ![Azure DevOps 組織の作成 ページ](media/cicd/vsts-account-creation.png)

1. Azure DevOps 組織とプロジェクトが使用できることを示す確認の電子メールを開きます。 をクリックして、**プロジェクトを開始する**ボタンをクリックします。

    ![プロジェクト ボタンを開始します。](media/cicd/vsts-start-project.png)

1. ブラウザーに表示する *\<account_name\>. visualstudio.com*します。 をクリックして、 *MyFirstProject*プロジェクトの DevOps パイプラインの構成を開始するリンク。

## <a name="configure-the-azure-pipelines-pipeline"></a>Azure のパイプラインのパイプラインを構成します。

完了する 3 つの異なる手順があります。 運用の DevOps パイプラインでは、次の 3 つのセクションでは結果の作業を完了します。

### <a name="grant-azure-devops-access-to-the-github-repository"></a>GitHub リポジトリへのアクセスを与える Azure DevOps

1. 展開、**外部リポジトリからコードをビルドまたは**アコーディオンします。 をクリックして、**セットアップ ビルド**ボタン。

    ![ビルドのセットアップのボタン](media/cicd/vsts-setup-build.png)

1. 選択、 **GitHub**オプションを**ソースを選択して**セクション。

    ![GitHub のソースを選択します。](media/cicd/vsts-select-source.png)

1. GitHub リポジトリにアクセスできる Azure DevOps 前に、承認が必要です。 入力 *< GitHub_username > GitHub 接続*で、**接続名**テキスト ボックス。 例:

    ![GitHub 接続名](media/cicd/vsts-repo-authz.png)

1. GitHub アカウントで 2 要素認証が有効な場合は、個人用アクセス トークンが必要です。 この場合は、クリックして、 **GitHub 個人用アクセス トークンを使用して承認**リンク。 参照してください、 [GitHub 個人用アクセス トークンの作成手順については公式](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)ヘルプを参照します。 のみ、*リポジトリ*アクセス許可のスコープが必要です。 それ以外の場合、をクリックして、 **OAuth を使用して承認**ボタンをクリックします。
1. メッセージが表示されたら、GitHub アカウントにサインインします。 次に、Azure DevOps 組織へのアクセス許可の承認を選択します。 成功した場合、新しいサービス エンドポイントが作成されます。
1. 横にある省略記号ボタンをクリックして、**リポジトリ**ボタンをクリックします。 選択、 *< GitHub_username >/フィード リーダーの単純な*リストからのリポジトリ。 をクリックして、**選択**ボタンをクリックします。
1. 選択、*マスター*からブランチ、**手動スケジュール ビルドの既定のブランチ**ドロップダウンします。 **[Continue]** をクリックします。 テンプレートの選択 ページが表示されます。

### <a name="create-the-build-definition"></a>ビルド定義を作成します。

1. テンプレートの選択 ページで、次のように入力します。 *ASP.NET Core*検索ボックスで。

    ![[テンプレート] ページの ASP.NET Core の検索](media/cicd/vsts-template-selection.png)

1. テンプレートの検索結果が表示されます。 ポインターを合わせる、 **ASP.NET Core**テンプレート、およびクリック、**適用**ボタンをクリックします。
1. **タスク**ビルド定義のタブが表示されます。 **[トリガー]** タブをクリックします。
1. チェック、**継続的インテグレーションを有効にする**ボックス。 下、**ブランチ フィルター**セクションで、いることを確認、**型**ドロップダウンに設定されている*Include*。 設定、**ブランチ仕様**ドロップダウンを*マスター*します。

    ![継続的インテグレーションの設定を有効にします。](media/cicd/vsts-enable-ci.png)

    これらの設定により、ビルドに変更がプッシュされるときにトリガーする、*マスター* GitHub リポジトリの分岐。 継続的インテグレーションがテスト済みで、[を GitHub に変更をコミットし、自動的に Azure にデプロイ](#commit-changes-to-github-and-automatically-deploy-to-azure)セクション。

1. をクリックして、**保存 & キュー**ボタンをクリックし、選択、**保存**オプション。

    ![[保存] ボタン](media/cicd/vsts-save-build.png)

1. 次のモーダル ダイアログ ボックスが表示されます。

    ![ビルド定義のモーダル ダイアログを保存します。](media/cicd/vsts-save-modal.png)

    既定のフォルダーを使用して、 *\\*、 をクリックし、**保存**ボタンをクリックします。

### <a name="create-the-release-pipeline"></a>リリース パイプラインを作成します。

1. をクリックして、**リリース**チーム プロジェクトのタブ。 をクリックして、**新しいパイプライン**ボタンをクリックします。

    ![[リリース] タブの新しい定義のボタン](media/cicd/vsts-new-release-definition.png)

    テンプレートの選択 ウィンドウが表示されます。

1. テンプレートの選択 ページで、次のように入力します。 *App Service*検索ボックスで。

    ![リリース パイプライン テンプレートの検索ボックス](media/cicd/vsts-release-template-search.png)

1. テンプレートの検索結果が表示されます。 ポインターを合わせる、**スロットを使用した Azure App Service のデプロイ**テンプレート、およびクリック、**適用**ボタンをクリックします。 **パイプライン**リリース パイプラインのタブが表示されます。

    ![リリース パイプラインのパイプライン タブ](media/cicd/vsts-release-definition-pipeline.png)

1. をクリックして、**追加**ボタン、**成果物**ボックス。 **成果物を追加**パネルが表示されます。

    ![リリース パイプラインの成果物のパネルを追加](media/cicd/vsts-release-add-artifact.png)

1. 選択、**ビルド**タイルから、**ソースの種類**セクション。 この型は、ビルド定義、リリース パイプラインのリンクでできます。
1. 選択*MyFirstProject*から、**プロジェクト**ドロップダウンします。
1. ビルド定義の名前を選択します。 *MyFirstProject ASP.NET Core-CI*、から、**ソース (ビルド定義)** ドロップダウンします。
1. 選択*最新*から、**既定バージョン**ドロップダウンします。 このオプションは、ビルド定義の最新の実行によって生成された成果物をビルドします。
1. テキストを置き換える、**ソース エイリアス**を含む textbox*ドロップ*します。
1. **[追加]** ボタンをクリックします。 **成果物**変更を表示する更新プログラムをセクションします。
1. 継続的なデプロイを有効にする稲妻のアイコンをクリックします。

    ![リリース パイプラインの成果物の稲妻のアイコン](media/cicd/vsts-artifacts-lightning-bolt.png)

    このオプションを有効にすると、展開には、新しいビルドが利用可能な各時間に発生します。
1. A**継続的配置トリガー**右側にパネルが表示されます。 この機能を有効にするトグル ボタンをクリックします。 有効にする必要はありません、**プル要求トリガー**します。
1. をクリックして、**追加**ドロップダウンで、**ビルド ブランチ フィルター**セクション。 選択、**ビルド定義の既定のブランチ**オプション。 このフィルターによって、GitHub リポジトリからのビルドに対してのみトリガーするリリース*マスター*分岐します。
1. **[保存]** ボタンをクリックします。 をクリックして、 **OK** 、最終的なボタン**保存**モーダル ダイアログ。
1. をクリックして、**環境 1**ボックス。 **環境**右側にパネルが表示されます。 変更、*環境 1*内のテキスト、**環境名**に textbox*運用*します。

   ![リリース パイプライン - 環境名 テキスト ボックス](media/cicd/vsts-environment-name-textbox.png)

1. をクリックして、**フェーズ 1、2 つのタスク**のリンクを**運用**ボックス。

    ![リリース パイプラインの実稼働環境 link.png](media/cicd/vsts-production-link.png)

    **タスク**環境のタブが表示されます。
1. をクリックして、 **Azure App Service のデプロイ スロットを**タスク。 右側のパネルにその設定が表示されます。
1. App Service に関連付けられている Azure サブスクリプションを選択して、 **Azure サブスクリプション**ドロップダウンします。 選択すると、クリックして、 **Authorize**ボタンをクリックします。
1. 選択*Web アプリ*から、**アプリの種類**ドロップダウンします。
1. 選択*mywebapp/< unique_number/>* から、 **App service の名前**ドロップダウンします。
1. 選択*azure チュートリアル*から、**リソース グループ**ドロップダウンします。
1. 選択*ステージング*から、**スロット**ドロップダウンします。
1. **[保存]** ボタンをクリックします。
1. 既定のリリース パイプラインの名前を合わせます。 編集することにある鉛筆アイコンをクリックします。 使用*MyFirstProject ASP.NET Core-CD*名として。

    ![リリース パイプライン名](media/cicd/vsts-release-definition-name.png)

1. **[保存]** ボタンをクリックします。

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>GitHub に変更をコミットし、自動的に Azure にデプロイ

1. 開いている*SimpleFeedReader.sln* Visual Studio でします。
1. ソリューション エクスプ ローラーで開く*Pages\Index.cshtml*します。 変更`<h2>Simple Feed Reader - V3</h2>`に`<h2>Simple Feed Reader - V4</h2>`します。
1. キーを押して**Ctrl**+**Shift**+**B**アプリをビルドします。
1. ファイルを GitHub リポジトリにコミットします。 いずれかを使用して、**変更**Visual studio のページ*チーム エクスプ ローラー*タブ、または、次を実行、ローカル コンピューターのコマンド シェルの使用します。

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. 変更をプッシュ、*マスター*に分岐、*原点*GitHub リポジトリのリモート。

    ```console
    git push origin master
    ```

    GitHub リポジトリのコミットが表示されます*マスター*ブランチ。

    ![Master ブランチでの GitHub のコミット](media/cicd/github-commit.png)

    ビルド定義の継続的な統合が有効になっているために、ビルドがトリガーされる**トリガー**  タブ。

    ![継続的インテグレーションを有効にします。](media/cicd/enable-ci.png)

1. 移動し、**キューに登録済み**のタブ、 **Azure パイプライン** > **ビルド**Azure DevOps サービス内のページ。 ブランチとコミットのビルドをトリガーしたキューに入っているビルドを示しています。

    ![キューに入っているビルド](media/cicd/build-queued.png)

1. ビルドに成功すると、Azure にデプロイが発生します。 ブラウザーでアプリに移動します。 "V4"テキストが、見出しが表示されることに注意してください。

    ![更新されたアプリ](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Azure のパイプラインのパイプラインを調べる

### <a name="build-definition"></a>ビルド定義

ビルド定義を名前で作成された*MyFirstProject ASP.NET Core-CI*します。 完了したら、ビルドの生成、 *.zip*発行する資産を含むファイル。 リリース パイプラインでは、これらの資産を Azure に配置します。

ビルド定義の**タスク**タブには、使用されている個々 のステップが一覧表示されます。 5 つのビルド タスクがあります。

![ビルド定義タスク](media/cicd/build-definition-tasks.png)

1. **復元**&mdash;実行、`dotnet restore`アプリの NuGet パッケージを復元するコマンド。 既定のパッケージ フィードの使用は nuget.org します。
1. **ビルド**&mdash;実行、`dotnet build --configuration release`アプリのコードをコンパイルするコマンド。 これは、`--configuration`オプションは、運用環境に展開に適していると、コードの最適化のバージョンを生成するために使用します。 変更、 *BuildConfiguration* 、ビルド定義の変数**変数** タブの場合、たとえば、デバッグ構成が必要です。
1. **テスト**&mdash;実行、`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`アプリの単体テストを実行するコマンド。 単体テストは、すべて c# プロジェクトに一致する内部で実行される、 `**/*Tests/*.csproj` glob パターン。 テスト結果が保存、 *.trx*によって指定された場所にファイル、`--results-directory`オプション。 すべてのテストが失敗した場合、ビルドは失敗しは展開されません。

    > [!NOTE]
    > テスト作業の単位を確認するには、変更*SimpleFeedReader.Tests\Services\NewsServiceTests.cs*テストの 1 つを意図的に分割します。 たとえば、変更`Assert.True(result.Count > 0);`に`Assert.False(result.Count > 0);`で、`Returns_News_Stories_Given_Valid_Uri`メソッド。 コミットし、GitHub に変更をプッシュします。 ビルドがトリガーされ、失敗します。 ビルド パイプラインの状態に変わります**失敗**します。 変更、commit、およびプッシュを再び元に戻します。 ビルドは成功します。

1. **発行**&mdash;実行、`dotnet publish --configuration release --output <local_path_on_build_agent>`を生成するコマンド、 *.zip*デプロイするアーティファクトを含むファイル。 `--output`の発行場所を指定するオプション、 *.zip*ファイル。 渡すことによって場所が指定されている、[定義済み変数](/azure/devops/pipelines/build/variables)という`$(build.artifactstagingdirectory)`します。 など、ローカル パスにその変数を展開*c:\agent\_work\1\a*、ビルド エージェントにします。
1. **成果物の公開** &mdash; Publishes、 *.zip*で生成されるファイル、**発行**タスク。 タスクを受け入れる、 *.zip*ファイルの場所、定義済みの変数をパラメーターとして`$(build.artifactstagingdirectory)`します。 *.Zip*という名前のフォルダーとファイルをパブリッシュ*ドロップ*します。

ビルド定義のクリックして**概要**定義と共にビルドの履歴を表示するリンク。

![スクリーン ショットが表示されたビルド定義の履歴](media/cicd/build-definition-summary.png)

[結果] ページで、一意のビルド番号に対応するリンクをクリックします。

![スクリーン ショットが表示されたビルド定義の概要 ページ](media/cicd/build-definition-completed.png)

この特定のビルドの概要が表示されます。 をクリックして、**成果物** タブに注意してください、*ドロップ*ビルドによって生成されたフォルダーが一覧表示。

![ビルド定義の成果物ドロップ フォルダーを示すスクリーン ショット](media/cicd/build-definition-artifacts.png)

使用して、**ダウンロード**と**探索**へのリンクを発行された成果物を検査します。

### <a name="release-pipeline"></a>リリース パイプライン

リリース パイプラインは、名前で作成された*MyFirstProject ASP.NET Core-CD*:

![スクリーン ショットが表示されたリリース パイプラインの概要](media/cicd/release-definition-overview.png)

リリース パイプラインの 2 つの主要なコンポーネントは、**成果物**と**環境**します。 ボックスをクリックすると、**成果物**セクションが次のパネルが表示されます。

![スクリーン ショットが表示されたリリース パイプラインの成果物](media/cicd/release-definition-artifacts.png)

**ソース (ビルド定義)** 値は、このリリースのパイプラインがリンクされているビルド定義を表します。 *.Zip*ビルド定義が正常に実行によって生成されたファイルが提供されます、*運用*環境を Azure に配置します。 をクリックして、*フェーズ 1、2 つのタスク*のリンクを*運用*環境のボックスに、リリース パイプラインのタスクを表示。

![スクリーン ショットが表示されたリリース パイプラインのタスク](media/cicd/release-definition-tasks.png)

リリース パイプラインは、2 つのタスクで構成されています: *Azure App Service のデプロイ スロットを*と*管理 Azure App Service のスロット スワップ*します。 最初のタスクをクリックすると、次のタスクの構成が表示されます。

![スクリーン ショットが表示されたリリース パイプラインの展開タスク](media/cicd/release-definition-task1.png)

Azure サブスクリプション、サービスの種類、web アプリ名、リソース グループ、およびデプロイ スロットは、デプロイ タスクで定義されます。 **パッケージまたはフォルダー**を保持するテキスト ボックス、 *.zip*を抽出して展開するファイルのパス、*ステージング*のスロット、 *mywebapp\<一意数 (_n)\>*  web アプリ。

スロット スワップのタスクをクリックすると、次のタスクの構成が表示されます。

![スクリーン ショットが表示されたリリース パイプラインのスロット スワップのタスク](media/cicd/release-definition-task2.png)

サブスクリプション、リソース グループ、サービスの種類、web アプリ名、およびデプロイ スロットの詳細が提供されます。 **実稼働とスワップ**チェック ボックスがオンにします。 その結果、展開、bits、*ステージング*スロットが運用環境にスワップされます。

## <a name="additional-reading"></a>その他の参考資料

* [Azure Pipelines による最初のパイプラインの作成](/azure/devops/pipelines/get-started-yaml)
* [ビルドと .NET Core プロジェクト](/azure/devops/pipelines/languages/dotnet-core)
* [Azure のパイプラインを使用した web アプリをデプロイします。](/azure/devops/pipelines/targets/webapp)
