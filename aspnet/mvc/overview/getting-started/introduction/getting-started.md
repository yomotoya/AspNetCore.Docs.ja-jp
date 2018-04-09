---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 の概要 |Microsoft ドキュメント
author: Rick-Anderson
description: 'メモ: このチュートリアルの最新バージョンは Visual Studio 2015 を使用して、ここで使用できます。 新しいチュートリアルでは、多くの improvem を提供する ASP.NET Core MVC 6 を使用しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>ASP.NET MVC 5 の概要
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 このチュートリアルが ASP.NET MVC 5 web アプリケーションを使用して、作成の基本を教える[Visual Studio 2017](https://www.visualstudio.com/)です。 チュートリアルの最後のソースが上にある[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 このチュートリアルは、によって書き込まれました[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )、 [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )、および[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Azure アカウントをこのアプリを Azure に展開する必要があります。

 - 実行できます[無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。
 - 実行できます[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-、MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。


## <a name="getting-started"></a>作業の開始

インストールと実行によって開始[Visual Studio 2017](https://www.visualstudio.com/)です。

Visual Studio は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。 Visual Studio では、下部で、使用可能なさまざまなオプションを表示するリストです。 IDE でのタスクを実行する別の方法を提供するメニューもあります。 (選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>初めてアプリケーションの作成

をクリックして**新しいプロジェクト**、クリックして Visual C# の場合、左側の**Web**し、 **ASP.NET Web アプリケーション (.NET Framework)**です。 プロジェクト"MvcMovie"の名前を指定し、をクリックして**OK**です。

![](getting-started/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**  をクリックし、 **ok**です。

![](getting-started/_static/image3.png)

Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある! これは、単純です"Hello World!" プロジェクト、・ アプリケーションを起動するに適しています。

![](getting-started/_static/image4.png)

F5 キーを押してデバッグを開始する をクリックします。 F5 キーを押して Visual Studio を起動するが[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) web アプリを実行します。 Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページが開きます。 ブラウザーのアドレス バーに表示される`localhost:port#`などのメカニズムではありませんし`example.com`です。 これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 Visual Studio web プロジェクトの実行時に、ランダムなポートが、web サーバーに使用されます。 次の図では、ポート番号は、1234 です。 アプリケーションを実行するときに、別のポート番号が表示されます。

![](getting-started/_static/image5.png)

すぐこの既定テンプレートを使用して自宅、連絡先、についてのページです。 上記の図に表示されない、**ホーム**、**に関する**と**連絡先**リンクします。 ブラウザー ウィンドウのサイズによっては、これらのリンクを参照するナビゲーション アイコンをクリックする必要があります。

![](getting-started/_static/image6.png)  

アプリケーションでは、登録およびログ記録のサポートも提供します。 次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC について少し説明します。 ASP.NET MVC アプリケーションを終了し、いくつかのコードを変更してみましょう。

現在のチュートリアルの一覧は、次を参照してください。[記事をお勧め MVC](../mvc-learning-sequence.md)です。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行している完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Azure アカウントを Azure にこのソリューションを展開する必要があります。 アカウントがない場合は、次のオプションがあります。

- [Azure アカウントを無料で開いて](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)のクレジットを取得する有料の Azure サービスを試すことができますを使用して使用後もアカウントを保持する最大の使用は、Azure のサービスを解放します。
- [MSDN サブスクリプション会員の特典を有効に](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)が、MSDN サブスクリプションでは、クレジット毎月 Azure の有料のサービスに使用することができます。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
