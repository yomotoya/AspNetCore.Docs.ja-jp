---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する

[Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)、[Rachel Appel](https://twitter.com/rachelappel)

## <a name="set-up-the-development-environment"></a>開発環境を設定する

* 最新の [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/) をインストールします。 Visual Studio をまだインストールしていない場合は、この SDK でインストールされます。

* [Azure アカウント](https://portal.azure.com/)を確認します。 [無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/)か、[Visual Studio サブスクライバー向けの特典をアクティブ化する](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ことができます。

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio のスタート ページで、**[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[.NET Core]** を選択します。

* 中央のウィンドウで、**[ASP.NET Core Web Application]** を選択します。

* **[OK]** を選択します。

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

**[新しい ASP.NET Core Web アプリケーション]** ダイアログで次の手順を実行します。

* **[Web アプリケーション]** を選択します。

* **[認証の変更]** を選択します。

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

**[認証の変更]** ダイアログが表示されます。 

* **[個人のユーザー アカウント]** を選択します。

* **[OK]** を選択して **[新しい ASP.NET Core Web アプリケーション]** に戻り、もう一度 **[OK]** を選択します。

![新しい ASP.NET Core Web 認証ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio によってソリューションが作成されます。

## <a name="run-the-app-locally"></a>アプリをローカルで実行する

* **[デバッグ]**、**[デバッグなしで開始]** の順にクリックして、アプリをローカルで実行します。

* **[バージョン情報]** リンクと [**連絡先**] リンクをクリックして、Web アプリケーションが機能することを確認します。

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

* **[登録]** を選択して、新しいユーザーを登録します。 架空の電子メール アドレスを使用できます。 送信すると、ページに次のエラーが表示されます。

    *Internal Server Error: A database operation failed while processing the request. /(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/)Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキストの既存の移行を適用すると問題が解決する場合があります。/)*

* **[Apply Migrations]/(移行を適用する/)** を選択し、移行が完了したら、ページを更新します。

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/) SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/) Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキスト用の既存の以降を適用すると問題が解決する場合があります。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

アプリには、新しいユーザーの登録に使用した電子メールと **[ログアウト]** リンクが表示されます。

![Microsoft Edge で開いている Web アプリケーション。 [Register]/(登録/) リンクは Hello email@domain.com! というテキストで置き換えられます。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Azure にアプリを配置する

Web ページを閉じて Visual Studio に戻り、**[デバッグ]** メニューから **[デバッグの停止]** を選択します。

ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

**[発行]** ダイアログで、**[Microsoft Azure App Service]** を選択し、**[発行]** をクリックします。

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

* アプリに一意の名前を付けます。 

* サブスクリプションを選択します。

* リソース グループの **[新規作成]** を選択し、新しいリソース グループの名前を入力します。

* アプリ サービス プランの **[新規作成]** を選択し、近くの場所を選択します。 既定で生成された名前をそのまま使用できます。

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* **[サービス]** タブを選択して、新しいデータベースを作成します。

* 緑色の **+** アイコンを選択して新しい SQL Database を作成します。

![新しい SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* **[SQL Database の構成]** ダイアログの **[新規作成]** を選択して新しいデータベースを作成します。

![新しい SQL Database とサーバー](publish-to-azure-webapp-using-vs/_static/conf.png)

**[SQL Server の構成]** ダイアログが表示されます。

* 管理者のユーザー名とパスワードを入力し、**[OK]** を選択します。 この手順で作成したユーザー名とパスワードは忘れないでください。 既定の **[サーバー名]** をそのまま使用できます。 

* データベースの名前と接続文字列を入力します。

> [!NOTE]
> 管理者のユーザー名に "admin" は使用できません。

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* **[OK]** を選択します。

Visual Studio が **[App Service の作成]** ダイアログに戻ります。

* **[App Service の作成]** ダイアログで **[作成]** を選択します。

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* **[発行]** ダイアログの **[設定]** リンクをクリックします。

![[発行] ダイアログ: [接続] パネル](publish-to-azure-webapp-using-vs/_static/pubc.png)

**[発行]** ダイアログの **[設定]** ページで次の手順を実行します。

  * **[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします。

  * **[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします。

* **[保存]** を選択します。 Visual Studio が **[発行]** ダイアログに戻ります。 

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

**[発行]**をクリックします。 Visual Studio から Azure にアプリが発行され、ブラウザーでクラウド アプリが起動します。

### <a name="test-your-app-in-azure"></a>Azure でアプリをテストする

* **[About]** リンクと **[Contact]** リンクをテストします

* 新しいユーザーを登録します

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>アプリを更新する

* *Pages/About.cshtml* Razor ページを編集して、その内容を変更します。 たとえば、"Hello ASP.NET Core!" と表示されるように段落を修正できます。

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* プロジェクトを右クリックし、**[発行]** をもう一度選択します。

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* アプリが発行されたら、Azure に変更内容が反映されていることを確認します。

![タスクの完了を確認します。](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>クリーンアップ

アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。

* **[リソース グループ]** を選択し、作成したリソース グループを選択します。

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* **[リソース グループ]** ページで、**[削除]** を選択します。

![Azure Portal: [リソース グループ] ページ](publish-to-azure-webapp-using-vs/_static/rgd.png)

* リソース グループ名を入力し、**[削除]** を選択します。 このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます。

### <a name="next-steps"></a>次のステップ

* [ASP.NET Core MVC と Visual Studio の概要](first-mvc-app/start-mvc.md)

* [ASP.NET Core の概要](../index.md)

* [ASP.NET Core の基礎の概要](../fundamentals/index.md)
