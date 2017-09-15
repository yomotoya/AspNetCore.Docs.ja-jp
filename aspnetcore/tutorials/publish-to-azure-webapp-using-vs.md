---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>開発環境を設定する

* 最新の [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs) をインストールします。 Visual Studio をまだインストールしていない場合は、この SDK でインストールされます。

> [!NOTE]
> コンピューターに多数の依存関係がない場合でも、SDK のインストール時間は 30 分を超える可能性があります。

* [.NET Core + Visual Studio ツール](http://go.microsoft.com/fwlink/?LinkID=798306)をインストールします。

* [Azure アカウント](https://portal.azure.com/)を確認します。 [無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/)か、[Visual Studio サブスクライバー向けの特典をアクティブ化する](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ことができます。

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio のスタート ページで **[新しいプロジェクト]** をタップします。

![スタート ページ](publish-to-azure-webapp-using-vs/_static/new_project.png)

または、メニューを使用して新しいプロジェクトを作成できます。 **[ファイル]、[新規]、[プロジェクト]** の順にタップします。

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[Web]** をタップします

* 中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします

* **[OK]** をタップします

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

**[新しい ASP.NET Core Web アプリケーション (.NET Core)]** ダイアログを次のように設定します。

* **[Web アプリケーション]** をタップします

* **[認証]** が **[個人のユーザー アカウント]** に設定されていることを確認します

* **[クラウドにホストする]** が**オフ**であることを確認します

* **[OK]** をタップします

![[新しい ASP.NET Core Web アプリケーション (.NET Core)] ダイアログ](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>アプリをローカルでテストする

* アプリをローカルで実行するには、**Ctrl キーを押しながら F5 キー**を押します。

* **[About]** リンクと **[Contact]** リンクをタップします。 デバイスのサイズによっては、ナビゲーション アイコンをタップしてリンクを表示する必要があります。

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

* **[Register]/(登録/)** をタップし、新しいユーザーを登録します。 架空の電子メール アドレスを使用できます。 送信すると、次のエラーが表示されます。

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/) SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/) Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキスト用の既存の以降を適用すると問題が解決する場合があります。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

この問題は、2 つの方法のいずれかで修復できます。

* **[Apply Migrations]/(移行を適用する/)** をタップし、移行が完了したら、ページを更新します。または

* プロジェクトのディレクトリでコマンド プロンプトから以下を実行します。

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

アプリには、新しいユーザーの登録に使用した電子メールと **[Log off]/(ログオフ/)** リンクが表示されます。

![Microsoft Edge で開いている Web アプリケーション。 [Register]/(登録/) リンクは Hello abc@example.com! というテキストで置き換えられます。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Azure にアプリを配置する

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

**[発行]** ダイアログで、**[Microsoft Azure App Service]** をタップします。

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

**[新規作成]** をタップして新しいリソース グループを作成します。 新しいリソース グループを作成すると、このチュートリアルで作成するすべての Azure リソースを削除しやすくなります。

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

新しいリソース グループとアプリ サービス プランを作成します。

* リソース グループの **[新規作成]** をタップし、新しいリソース グループの名前を入力します

* アプリ サービス プランの **[新規作成]** をタップし、近くの場所を選択します。 既定で生成された名前をそのまま使用できます

* **[ほかの Azure サービスを探す]** をタップして新しいデータベースを作成します

![[新しいリソース グループ] ダイアログ: [ホスティング] パネル](publish-to-azure-webapp-using-vs/_static/cas.png)

* 緑色の **+** アイコンをタップして新しい SQL Database を作成します

![[新しいリソース グループ] ダイアログ: [サービス] パネル](publish-to-azure-webapp-using-vs/_static/sql.png)

* **[SQL Database の構成]** ダイアログの **[新規作成]** をタップして新しいデータベース サーバーを作成します。

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf.png)

* 管理者のユーザー名とパスワードを入力し、**[OK]** をタップします。 この手順で作成したユーザー名とパスワードは忘れないでください。 既定の **[サーバー名]** をそのまま使用できます

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> 管理者のユーザー名に "admin" は使用できません。

* **[SQL Database の構成]** ダイアログの **[OK]** をタップします

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* **[App Service の作成]** ダイアログの **[作成]** をタップします

![[App Service の作成] ダイアログ](publish-to-azure-webapp-using-vs/_static/create_as.png)

* **[発行]** ダイアログの **[次へ]** をタップします

![[発行] ダイアログ: [接続] パネル](publish-to-azure-webapp-using-vs/_static/pubc.png)

* **[発行]** ダイアログの **[設定]** ステージで次の手順を実行します。

  * **[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします

  * **[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします

* **[発行]** をタップし、Visual Studio でアプリの発行が完了するまで待ちます

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

Visual Studio から Azure にアプリが発行され、ブラウザーでクラウド アプリが起動します。

### <a name="test-your-app-in-azure"></a>Azure でアプリをテストする

* **[About]** リンクと **[Contact]** リンクをテストします

* 新しいユーザーを登録します

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>アプリを更新する

* `Views/Home/About.cshtml` Razor ビュー ファイルを編集し、その内容を変更します。 例:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* プロジェクトを右クリックし、**[発行]** をもう一度タップします。

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* アプリが発行されたら、Azure に変更内容が反映されていることを確認します。

### <a name="clean-up"></a>クリーンアップ

アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。

* **[リソース グループ]** を選択し、作成したリソース グループをタップします

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* **[リソース グループ]** ブレードで **[削除]** をタップします

![Azure Portal: [リソース グループ] ブレード](publish-to-azure-webapp-using-vs/_static/rgd.png)

* リソース グループ名を入力し、**[削除]** をタップします。 このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます

### <a name="next-steps"></a>次のステップ

* [ASP.NET Core MVC と Visual Studio の概要](first-mvc-app/start-mvc.md)

* [ASP.NET Core の概要](../index.md)

* [ASP.NET Core の基礎の概要](../fundamentals/index.md)
