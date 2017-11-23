---
title: "Visual Studio と Git による Azure への継続的配置"
author: rick-anderson
description: "Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Visual Studio と Git による ASP.NET Core 向け Azure への継続的配置

作成者: [Erik Reitan](https://github.com/Erikre)

このチュートリアルでは、Visual Studio で ASP.NET Core Web アプリを作成し、それを Visual Studio から Azure App Service に継続的に配置する方法について説明します。 

「[Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)」 (VSTS と継続的配置で Azure Web アプリをビルドし、公開する) も併せてご覧ください。Visual Studio Team Services を利用した、[Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) の継続的配信 (CD) ワークフローの構成方法を紹介しています。 チーム サービスの Azure 継続的配信は、Azure App Service にアプリの更新プログラムを公開するための堅牢な配置パイプラインを簡単に設定できるようにします。 Azure ポータルで、このパイプラインのビルド、テスト実行、ステージング スロットへの展開、運用への展開を構成できます。

> [!NOTE]
> このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントをお持ち出ない場合、[MSDN サブスクライバー特典を有効にする](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)できます。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、既に以下をインストールしていることを前提としています。

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://www.microsoft.com/net/download/core) (ランタイムとツール)

* [Git](https://git-scm.com/downloads) for Windows

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core Web アプリを作成する

1. Visual Studio を起動します。

2. **[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。

3. **[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。 このテンプレートは、**[インストール済み]** > **[テンプレート]** > **[Visual C#]** > **[.NET Core]** の下にあります。 プロジェクトに `SampleWebAppDemo` という名前を付けます。 **[Create new Git repository]\(新しい Git リポジトリを作成する\)** オプションを選択し、**[OK]** をクリックします。

   ![[新しいプロジェクト] ダイアログ](azure-continuous-deployment/_static/01-new-project.png)

4. **[New ASP.NET Core Project]\(新しい ASP.NET Core プロジェクト\)** ダイアログで、ASP.NET Core の **[空]** テンプレートを選択し、**[OK]** をクリックします。

   ![[新しい ASP.NET プロジェクト] ダイアログ](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    >.NET Core の最新のリリースは 2.0 です。

### <a name="running-the-web-app-locally"></a>Web アプリのローカル実行

1. Visual Studio でアプリの作成が完了したら、**[デバッグ]**、**[デバッグの開始]** の順に選択し、アプリを実行します。 あるいは、**F5** キーを押します。

   Visual Studio と新しいアプリの初期化に時間がかかる場合があります。 完了すると、ブラウザーに実行中のアプリが表示されます。

   ![ブラウザー ウィンドウの実行中のアプリに 'Hello World!' と表示されます。](azure-continuous-deployment/_static/04-browser-runapp.png)

2. 実行中の Web アプリを確認したら、ブラウザーを閉じ、Visual Studio のツール バーにある "デバッグの停止" アイコンをクリックしてアプリを停止します。

## <a name="create-a-web-app-in-the-azure-portal"></a>Azure Portal で Web アプリを作成する

次の手順は、Azure Portal で Web アプリを作成する方法です。

1. [Azure Portal](https://portal.azure.com) にログインします。
2. ポータルの左上にある **[新規作成]** をタップします。
3. **[Web + モバイル]**、**[Web アプリ]** の順にタップします。

    ![Microsoft Azure Portal: [新規作成] ボタン: [Marketplace] の [Web + モバイル]: [おすすめアプリ] の [Web アプリ] ボタン](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  **[Web アプリ]** ブレードに **[App Service の名前]** の一意の名前を入力します。

    ![[Web アプリ] ブレード](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >**[App Service の名前]** の名前は一意にする必要があります。 ポータルでは、名前を入力しようとすると、この規則が適用されます。 別の値を入力したら、このチュートリアルで **SampleWebAppDemo** が出てくるたびに、その値を代わりに用いる必要があります。

    &nbsp;
    
    **[Web アプリ]** ブレードではまた、既存の **[App Service プラン/場所]** を選択するか、新しい App Service プラン/場所を作成できます。 新しいプランを作成した場合、価格レベル、場所、その他のオプションを選択します。 App Service プランに関する詳細については、「[Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)」 (Azure App Service プランの詳細) を参照してください。

5.  
              **[作成]**をクリックします。 Web アプリがプロビジョニングされ、起動します。

    ![Azure Portal: サンプル Web アプリ デモ 01 の [要点] ブレード](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>新しい Web アプリに対して Git 公開を有効にする

Git は分散型バージョン管理システムであり、これを利用して Azure App Service Web アプリを展開できます。 Web アプリのために記述したコードをローカル Git リポジトリに保存し、リモート リポジトリにプッシュすることでコードを Azure に展開します。

1. まだログインしていない場合、[Azure Portal](https://portal.azure.com) にログインします。

2. **[App Services]** をクリックし、Azure サブスクリプションに関連付けられているアプリ サービスの一覧を表示します。

3. このチュートリアルの前のセクションで作成した Web アプリを選択します。

4. **[デプロイ]** ブレードで、**[デプロイ オプション]** > **[ソースの選択]** > **[ローカル Git リポジトリ]** の順に選択します。

   ![[設定] ブレード: [展開元] ブレード: [ソースの選択] ブレード](azure-continuous-deployment/_static/deployment-options.png)

5. **[OK]** をクリックします。

6. Web アプリや他の App Service アプリを公開するための展開資格情報を設定していない場合、ここで設定してください。

   * **[設定]**、**[デプロイ資格情報]** の順にクリックします。 **[デプロイ資格情報の設定]** ブレードが表示されます。

   * ユーザー名とパスワードを作成します。  後で Git を設定するとき、このパスワードが必要になります。

   * **[保存]**をクリックします。

7. **[Web アプリ]** ブレードで、**[設定]**、**[プロパティ]** の順にクリックします。 展開するリモート Git リポジトリの URL が **[GIT URL]** の下に表示されます。

8. **GIT URL** の値をコピーします。後で使用します。

   ![Azure Portal: アプリケーションの [プロパティ] ブレード](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Azure App Service に Web アプリを公開する

このセクションでは、Visual Studio でローカル Git リポジトリを作成し、そのリポジトリから Azure にプッシュし、Web アプリを展開します。 実行する必要がある手順は、次のとおりです。

   * Azure にローカル リポジトリを展開できるように、GIT URL 値を利用してリモート リポジトリ設定を追加します。

   * プロジェクトの変更をコミットします。

   * ローカル リポジトリから Azure のリモート リポジトリにプロジェクトの変更をプッシュします。

&nbsp;
   
1.  **ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。 **チーム エクスプローラー**が表示されます。

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/10-team-explorer.png)

2.  **チーム エクスプローラー**で、**[ホーム]** (ホーム アイコン)、**[設定]**、**[リポジトリ設定]** の順に選択します。

3.  **[リポジトリ設定]** の **[リモート]** セクションで、**[追加]** を選択します。 **[リモートの追加]** ダイアログ ボックスが表示されます。

4.  リモートの **[名前]** を **[Azure-SampleApp]** に設定します。

5.  **[フェッチ]** の値を先にこのチュートリアルで Azure からコピーした **Git URL** に設定します。 これは **.git** で終わる URL であることにご注意ください。

    ![[リモートの編集] ダイアログ](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >あるいは、**コマンド ウィンドウ**からリモート リポジトリを指定できます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、コマンドを入力します。 例:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  **[ホーム]** (ホーム アイコン)、**[設定]**、**[グローバル設定]** の順に選択します。 自分の名前とメール アドレスが設定されていることを確認します。 場合によっては、**[更新]** を選択する必要があります。

7.  **[ホーム]**、**[変更]** の順に選択し、**[変更]** ビューに戻ります。

8.  「**Initial Push #1**」のようなコミット メッセージを入力し、**[コミット]** をクリックします。 このアクションによりローカルで*コミット*されます。 次に、Azure と*同期*する必要があります。

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >あるいは、**コマンド ウィンドウ**から変更をコミットできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。 例:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  **[ホーム]**、**[同期]**、**[アクション]**、**[コマンド プロンプトを開く]** の順に選択します。 コマンド プロンプトが開きます。ディレクトリはプロジェクト ディレクトリになります。

10.  コマンド ウィンドウで次のコマンドを入力します。

    `git push -u Azure-SampleApp master`

11.  Azure で先に作成した Azure **デプロイ資格情報**パスワードを入力します。

    >[!NOTE]
    >入力時、パスワードは表示されません。

    このコマンドによりローカル プロジェクト ファイルを Azure にプッシュするプロセスが開始されます。 上記のコマンドからの出力は、展開が成功したというメッセージで終わります。
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > プロジェクトで共同作業を行う必要がある場合、Azure へのプッシュの間に [GitHub](https://github.com) にプッシュすることを検討してください。
 
### <a name="verify-the-active-deployment"></a>アクティブ デプロイを検証する

ローカル環境から Azure に Web アプリを転送したことを確認できます。 デプロイ成功の一覧が表示されます。

1. [Azure Portal](https://portal.azure.com) で、Web アプリを選択します。 次に、**[デプロイ]** > **[デプロイ オプション]** の順に選択します。

   ![Azure Portal: [設定] ブレード: [デプロイ] ブレードにデプロイ成功が表示されています](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Azure でアプリを実行する

Web アプリを Azure にデプロイしたので、アプリを実行できます。

これは、次の 2 つの方法で行うことができます。

* Azure Portal で、Web アプリの Web アプリ ブレードを見つけ、**[参照]** をクリックしてアプリを既定のブラウザーで表示します。

* ブラウザーを開き、Web アプリの URL を入力します。 例:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Web アプリを更新し、再発行する

ローカル コードを変更したら、再発行できます。

1.  Visual Studio の**ソリューション エクスプローラー**で、*Startup.cs* ファイルを開きます。

2.  `Configure` メソッドで、次のように表示されるように `Response.WriteAsync` メソッドを変更します。

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  変更を *Startup.cs* に保存します。

4.  **ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。 **チーム エクスプローラー**が表示されます。

5.  次のようなコミット メッセージを入力します。

    ```none
    Update #2
    ```

6.  **[コミット]** ボタンを押し、プロジェクトの変更をコミットします。

7.  **[ホーム]**、**[同期]**、**[アクション]**、**[プッシュ]** の順に選択します。

>[!NOTE]
>あるいは、**コマンド ウィンドウ**から変更をプッシュできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。 例:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Azure で更新後の Web アプリを表示する

更新後の Web アプリを表示します。Azure Portal の Web アプリ ブレードから **[参照]** を選択するか、ブラウザーを開き、Web アプリの URL を入力します。 例:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>その他のリソース

* [発行と配置](index.md)

* [プロジェクト Kudu](https://github.com/projectkudu/kudu/wiki)
