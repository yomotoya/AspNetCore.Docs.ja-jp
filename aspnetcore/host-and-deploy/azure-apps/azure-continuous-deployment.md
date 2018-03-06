---
title: "Visual Studio と Git による Azure への継続的配置"
author: rick-anderson
description: "Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: ea4788b5daead9e355e13b963c025dd110eb2bff
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Visual Studio および Git と ASP.NET core を Azure に継続的なデプロイ

作成者: [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

このチュートリアルでは、継続的なデプロイを使用して Visual Studio を使用して ASP.NET Core web アプリを作成し、Visual Studio から Azure App Service に配置する方法を示します。

「[Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)」 (VSTS と継続的配置で Azure Web アプリをビルドし、公開する) も併せてご覧ください。Visual Studio Team Services を利用した、[Azure App Service](/azure/app-service/app-service-web-overview) の継続的配信 (CD) ワークフローの構成方法を紹介しています。 Team Services で azure の継続的な配信には、Azure App Service でホストされているアプリの更新プログラムを発行する配置の堅牢パイプラインの設定が簡略化します。 パイプラインは、ビルド、テストを実行する、ステージング スロットにデプロイし、実稼働環境に展開するには、Azure ポータルから構成できます。

> [!NOTE]
> このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを取得する[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)または[無料試用版にサインアップする](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)です。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、次のソフトウェアがインストールされている前提としています。

* [Visual Studio](https://www.visualstudio.com)
* [.NET core SDK](https://www.microsoft.com/net/download/core) (ランタイムおよび tooling)
* [Git](https://git-scm.com/downloads) for Windows

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core Web アプリを作成する

1. Visual Studio を起動します。

1. **[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。

1. **[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。 このテンプレートは、**[インストール済み]** > **[テンプレート]** > **[Visual C#]** > **[.NET Core]** の下にあります。 プロジェクトに `SampleWebAppDemo` という名前を付けます。 **[Create new Git repository]\(新しい Git リポジトリを作成する\)** オプションを選択し、**[OK]** をクリックします。

   ![[新しいプロジェクト] ダイアログ](azure-continuous-deployment/_static/01-new-project.png)

1. **[New ASP.NET Core Project]\(新しい ASP.NET Core プロジェクト\)** ダイアログで、ASP.NET Core の **[空]** テンプレートを選択し、**[OK]** をクリックします。

   ![[新しい ASP.NET プロジェクト] ダイアログ](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> .NET Core の最新のリリースは 2.0 です。

### <a name="running-the-web-app-locally"></a>Web アプリのローカル実行

1. Visual Studio でアプリの作成が完了したら、**[デバッグ]**、**[デバッグの開始]** の順に選択し、アプリを実行します。 代わりに、キーを押して**f5 キーを押して**です。

   Visual Studio と新しいアプリの初期化に時間がかかる場合があります。 これが完了したら、ブラウザーは、実行中のアプリを示します。

   ![ブラウザー ウィンドウの実行中のアプリに 'Hello World!' と表示されます。](azure-continuous-deployment/_static/04-browser-runapp.png)

1. 実行中の Web アプリを確認すると、ブラウザーを終了し、アプリを停止する Visual Studio のツールバーで、「デバッグの停止」アイコンを選択します。

## <a name="create-a-web-app-in-the-azure-portal"></a>Azure Portal で Web アプリを作成する

次の手順では、Azure ポータルで web アプリを作成します。

1. ログインに、 [Azure ポータル](https://portal.azure.com)です。

1. 選択**新規**でポータルのインターフェイスの左上です。

1. 選択**Web + モバイル** > **Web アプリ**です。

   ![Microsoft Azure Portal: [新規作成] ボタン: [Marketplace] の [Web + モバイル]: [おすすめアプリ] の [Web アプリ] ボタン](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. **[Web アプリ]** ブレードに **[App Service の名前]** の一意の名前を入力します。

   ![[Web アプリ] ブレード](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **アプリ サービス名**名前は一意である必要があります。 ポータルは、名前を指定した場合、このルールを適用します。 別の値を提供する場合の発生するたびにその値に置き換えます**SampleWebAppDemo**このチュートリアルでします。

   **[Web アプリ]** ブレードではまた、既存の **[App Service プラン/場所]** を選択するか、新しい App Service プラン/場所を作成できます。 新しいプランを作成する場合は、価格レベル、場所、およびその他のオプションを選択します。 App Service プランの詳細については、次を参照してください。 [Azure App Service プランの詳細な概要](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)です。

1. **[作成]** を選択します。 Azure がプロビジョニングされ、web アプリが開始されます。

   ![Azure Portal: サンプル Web アプリ デモ 01 の [要点] ブレード](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>新しい Web アプリに対して Git 公開を有効にする

Git は、Azure App Service web アプリを配置するために使用する分散型バージョン管理システムです。 Web アプリ コードが、ローカルの Git リポジトリに格納されているし、コードがリモート リポジトリにプッシュして Azure に展開します。

1. ログイン、 [Azure ポータル](https://portal.azure.com)です。

1. 選択**App Services** Azure サブスクリプションに関連付けられているアプリ サービスの一覧を表示します。

1. このチュートリアルの前のセクションで作成された web アプリを選択します。

1. **[デプロイ]** ブレードで、**[デプロイ オプション]** > **[ソースの選択]** > **[ローカル Git リポジトリ]** の順に選択します。

   ![[設定] ブレード: [展開元] ブレード: [ソースの選択] ブレード](azure-continuous-deployment/_static/deployment-options.png)

1. **[OK]** を選択します。

1. 場合は、web アプリまたはその他の App Service アプリを発行のデプロイ資格情報を設定していない、ここで設定します。

   * 選択**設定** > **デプロイ資格情報**です。 **デプロイ資格情報を設定**ブレードが表示されます。
   * ユーザー名とパスワードを作成します。 Git を設定するときは、後で使用するパスワードを保存します。
   * **[保存]** を選択します。

1. **Web アプリ**ブレードで、**設定** > **プロパティ**です。 展開するリモート Git リポジトリの URL が下に表示された**GIT URL**です。

1. **GIT URL** の値をコピーします。後で使用します。

   ![Azure Portal: アプリケーションの [プロパティ] ブレード](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Azure App Service に web アプリを公開します。

このセクションを使用して Visual Studio とプッシュ リポジトリから Azure に web アプリを展開して、ローカル Git リポジトリを作成します。 実行する必要がある手順は、次のとおりです。

* 値を使用して、GIT の URL、ローカル リポジトリを Azure にデプロイできるようにリモート リポジトリの設定を追加します。
* プロジェクトの変更をコミットします。
* Azure で、プロジェクトの変更をローカル リポジトリからリモート リポジトリにプッシュします。

1. **ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。 **チーム エクスプ ローラー**が表示されます。

   ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/10-team-explorer.png)

1. **チーム エクスプローラー**で、**[ホーム]** (ホーム アイコン)、**[設定]**、**[リポジトリ設定]** の順に選択します。

1. **リモコン**のセクションで、**レポジトリ設定****追加**です。 **リモート追加** ダイアログ ボックスが表示されます。

1. リモートの **[名前]** を **[Azure-SampleApp]** に設定します。

1. 値を設定**フェッチ**を**Git URL**このチュートリアルで前に、Azure からコピーします。 これは **.git** で終わる URL であることにご注意ください。

   ![[リモートの編集] ダイアログ](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > 代わりに、リモート リポジトリからの指定、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、コマンドを入力します。 例:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. **[ホーム]** (ホーム アイコン)、**[設定]**、**[グローバル設定]** の順に選択します。 名前と電子メール アドレスが設定されていることを確認します。 選択**更新**必要な場合です。

1. **[ホーム]**、**[変更]** の順に選択し、**[変更]** ビューに戻ります。

1. コミット メッセージを入力します。**最初プッシュ 1**選択**コミット**です。 この操作を作成、*コミット*ローカルです。

   ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > コミットに変更する代わりに、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、git コマンドを入力します。 例:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. **[ホーム]**、**[同期]**、**[アクション]**、**[コマンド プロンプトを開く]** の順に選択します。 プロジェクト ディレクトリに、コマンド プロンプトを開きます。

1. コマンド ウィンドウで次のコマンドを入力します。

   `git push -u Azure-SampleApp master`

1. Azure の入力**デプロイ資格情報**パスワードを Azure の前半で作成します。

   このコマンドは、ローカルのプロジェクト ファイルを Azure にプッシュするというプロセスを開始します。 上記のコマンドからの出力は、展開が成功したメッセージで終了します。

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > プロジェクトで共同作業が必要な場合へのプッシュを検討して[GitHub](https://github.com)を Azure にプッシュする前にします。
 
### <a name="verify-the-active-deployment"></a>アクティブ デプロイを検証する

ローカルの環境から Azure に、web アプリ転送が成功したことを確認します。

[Azure Portal](https://portal.azure.com)、web アプリを選択します。 選択**展開** > **展開オプション**です。

![Azure Portal: [設定] ブレード: [デプロイ] ブレードにデプロイ成功が表示されています](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Azure でアプリを実行する

Azure に web アプリを配置すると、これで、アプリを実行します。

これは、2 つの方法で実行できます。

* Azure ポータルでは、web アプリ、web アプリのブレードを探します。 選択**参照**を既定のブラウザーでアプリを表示します。
* ブラウザーを開き、web アプリの URL を入力します。 例 : `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Web アプリを更新および再パブリッシュ

ローカルのコードに変更を行った後、再パブリッシュします。

1. Visual Studio の**ソリューション エクスプローラー**で、*Startup.cs* ファイルを開きます。

1. `Configure` メソッドで、次のように表示されるように `Response.WriteAsync` メソッドを変更します。

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. 変更を保存*Startup.cs*です。

1. **ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。 **チーム エクスプ ローラー**が表示されます。

1. コミット メッセージを入力します。`Update #2`です。

1. **[コミット]** ボタンを押し、プロジェクトの変更をコミットします。

1. **[ホーム]**、**[同期]**、**[アクション]**、**[プッシュ]** の順に選択します。

> [!NOTE]
> 代わりからの変更をプッシュ、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、git コマンドを入力します。 例:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Azure で更新後の Web アプリを表示する

選択して、更新された web アプリを表示**参照**またはブラウザーを開き、web アプリの URL を入力して、Azure ポータルでは、web アプリのブレードからです。 例 : `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>その他の技術情報

* [VSTS を使用してビルドし、継続的なデプロイを Azure Web アプリを公開するには](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [プロジェクト Kudu](https://github.com/projectkudu/kudu/wiki)
