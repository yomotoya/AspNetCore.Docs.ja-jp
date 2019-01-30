---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 で Web API 2 を使用して |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。 チュートリアルでは、データ レイアウトの Entity Framework 6 を使用しています.
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236524"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Entity Framework 6 で Web API 2 を使用
====================

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

> このチュートリアルは、バック エンドを ASP.NET Web API を使用して web アプリケーションの作成の基本を説明します。 チュートリアルでは、クライアント側の JavaScript アプリケーションのデータ層、および Knockout.js Entity Framework 6 を使用します。 このチュートリアルでは、アプリを Azure App Service Web Apps にデプロイする方法も示します。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
> - Web API 2.1
> - Visual Studio 2017 (Visual Studio 2017 ダウンロード[ここ](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
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

ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。 Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンを選択します。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

このソリューションを Azure にデプロイする Azure アカウントが必要です。 アカウントがいない場合は、次のオプションがあります。

- [無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- [MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio を開きます。 **ファイル**メニューの **新規**を選択し、**プロジェクト**します。 (または選択**新しいプロジェクト**スタート ページです)。

**新しいプロジェクト**ダイアログ ボックスで、 **Web**左側のウィンドウで、 **ASP.NET Web アプリケーション (.NET Framework)** 中央のペインでします。 プロジェクトに名前を**BookService**選択**OK**。

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web API**テンプレート。

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


**[OK]** をクリックして、プロジェクトを作成します。

## <a name="configure-azure-settings-optional"></a>(省略可能) Azure の設定を構成します。

プロジェクトを作成した後はいつでも Azure App Service Web Apps にデプロイすることもできます。 

1. ソリューション エクスプ ローラーでクリックし、プロジェクトを右クリックして**発行**します。

2. 表示されるウィンドウで、選択**開始**します。 **発行先を選択** ダイアログ ボックスが表示されます。

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. **[プロファイルの作成]** を選択します。 **[App Service の作成]** ダイアログ ボックスが表示されます。

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   既定値をまたはアプリケーション名、リソース グループ、ホスティング プラン、Azure サブスクリプションと地理的リージョンの別の値を入力します。 

4. 選択**SQL データベースを作成**です。 **SQL Server の構成** ダイアログ ボックスが表示されます。 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   既定値を受け入れるか、別の値を入力します。 入力、**管理者ユーザー名**と**管理者パスワード**新しいデータベースの。 選択**OK**完了したら。 **App Service の作成**ページが再表示されます。

5. 選択**作成**プロファイルを作成します。 右上隅にある展開が進行中であることを示すメッセージが表示されます。 少し時間が、後に、**発行**ウィンドウが再び表示されます。

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    アプリを展開するために作成したプロファイルは、ご利用いただけます。 


> [!div class="step-by-step"]
> [次へ](part-2.md)
