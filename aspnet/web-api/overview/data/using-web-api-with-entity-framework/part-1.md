---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 で Web API 2 を使用して |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。 チュートリアルでは、データ レイアウトの Entity Framework 6 を使用しています.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826473"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Entity Framework 6 で Web API 2 を使用
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

> このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。 チュートリアルでは、クライアント側の JavaScript アプリケーションのデータ層、および Knockout.js Entity Framework 6 を使用します。 このチュートリアルでは、アプリを Azure App Service Web Apps にデプロイする方法も示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


このチュートリアルでは、Entity Framework 6 で ASP.NET Web API 2 を使用して、バックエンド データベースを操作する web アプリケーションを作成します。 作成するアプリケーションのスクリーン ショットを次に示します。

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

アプリでは、シングル ページ アプリケーション (SPA) のデザインを使用します。 「シングル ページ アプリケーション」は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。 最初のページの読み込み後に、アプリは、AJAX 要求を介してサーバーとで説明します。 AJAX では、アプリを使用して UI を更新する戻り値の JSON データを要求します。

AJAX は、新しいはありませんが、今日は JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理しやすくします。 このチュートリアルでは[Knockout.js](http://knockoutjs.com/)、任意の JavaScript クライアント フレームワークを使用することができます。

このアプリの主な構成要素を次に示します。

- ASP.NET MVC では、HTML ページを作成します。
- ASP.NET Web API は、AJAX 要求を処理し、JSON データを返します。
- Knockout.js データ、HTML 要素をバインド JSON データ。
- Entity Framework では、データベースに説明します。

## <a name="see-this-app-running-on-azure"></a>Azure で実行されているこのアプリを参照してください。

ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

このソリューションを Azure にデプロイする Azure アカウントが必要です。 アカウントがいない場合は、次のオプションがあります。

- [無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- [MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio を開きます。 **ファイル**メニューの **新規**を選択し、**プロジェクト**します。 (をクリックしてまたは**新しいプロジェクト**スタート ページです)。

**新しいプロジェクト**ダイアログ ボックスで、をクリックして**Web**左側のウィンドウで、 **ASP.NET Web アプリケーション**中央のペインで。 BookService プロジェクトの名前を指定し、をクリックして**OK**します。

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web API**テンプレート。

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

場合は、Azure App Service でプロジェクトをホストする、**クラウドでホスト**ボックスをオンにします。

**[OK]** をクリックして、プロジェクトを作成します。

## <a name="configure-azure-settings-optional"></a>(省略可能) Azure の設定を構成します。

中断した場合、**クラウド内のホスト**オプションのチェック、Visual Studio には、Microsoft Azure にサインインするように求められます

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Azure にサインインした後に Visual Studio では、web アプリを構成するように求められます。 サイトの名前を入力、Azure サブスクリプションを選択し、地理的リージョンを選択します。 **データベース サーバー**、**新しいサーバーの作成**です。 管理者のユーザー名とパスワードを入力します。

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [次へ](part-2.md)
