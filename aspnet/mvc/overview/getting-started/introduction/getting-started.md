---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 の概要 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは Visual Studio 2015 を使用して、ここで使用できます。 新しいチュートリアルでは、多くの improvem を提供する ASP.NET Core MVC 6 を使用しています.'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211749"
---
<a name="getting-started-with-aspnet-mvc-5"></a>ASP.NET MVC 5 の概要
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 このチュートリアルが ASP.NET MVC 5 web アプリを使用して、構築の基礎を講義[Visual Studio 2017](https://www.visualstudio.com/)します。 チュートリアルの最後のソースの上にある[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 このチュートリアルの執筆者[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )、 [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )、および[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 このアプリを Azure にデプロイする Azure アカウントが必要です。

 - できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
 - できます[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。


## <a name="getting-started"></a>作業の開始

インストールと実行によって開始[Visual Studio 2017](https://www.visualstudio.com/)します。

Visual Studio は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。 Visual Studio を使用できるさまざまなオプションを示す下部の一覧です。 また、IDE でタスクを実行する別の方法を提供するメニューがあります。 (選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>最初のアプリケーションを作成します。

クリックして**新しいプロジェクト**、Visual c# を選択、左、 **Web**し、 **ASP.NET Web アプリケーション (.NET Framework)** します。 クリックして、プロジェクトに"MvcMovie" **OK**します。

![](getting-started/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**順にクリックします**OK**します。

![](getting-started/_static/image3.png)

Visual Studio は、何もせず、実用的なアプリケーションが現在あるため、作成した ASP.NET MVC プロジェクトの既定のテンプレートを使用! これは、単純です"Hello World!" プロジェクト、およびそのアプリケーションにお勧めです。

![](getting-started/_static/image4.png)

F5 キーを押してデバッグを開始する をクリックします。 F5 キーを押して Visual Studio を開始するが[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)し、web アプリを実行します。 Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。 ブラウザーのアドレス バーが表示されますが`localhost:port#`ようなものではありません`example.com`します。 だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 Visual Studio web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。 次の図では、ポート番号は、1234 です。 アプリケーションを実行するときに、別のポート番号を確認します。

![](getting-started/_static/image5.png)

すぐに使えるこの既定テンプレートを使用してホーム、連絡先についてのページ。 上の画像が表示されない、**ホーム**、**について**と**連絡先**リンク。 ブラウザー ウィンドウのサイズによっては、これらのリンクを参照するナビゲーション アイコンをクリックする必要があります。

![](getting-started/_static/image6.png)  

アプリケーションには、登録し、ログインのサポートも提供します。 次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC についてもう少し説明します。 ASP.NET MVC アプリケーションを終了し、いくつかのコードを変更してみましょう。

現在のチュートリアルの一覧は、次を参照してください。[の記事をお勧めします。 MVC](../mvc-learning-sequence.md)します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

このソリューションを Azure にデプロイする Azure アカウントが必要です。 アカウントがいない場合は、次のオプションがあります。

- [無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- [MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
