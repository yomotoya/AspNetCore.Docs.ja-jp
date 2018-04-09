---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure の Azure App Service にアプリを公開する |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Azure の Azure App Service にアプリを公開します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

最後のステップとして Azure にアプリケーションを発行します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。

![](part-10/_static/image1.png)

クリックすると**発行**呼び出します、 **Web の発行**ダイアログ。 オンにした場合**クラウドでホスト**ときに、プロジェクトでは、その接続に最初に作成して、設定が既に構成されています。 その場合をクリックするだけ、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。 (確認しなかった場合**クラウドでホスト**開始時から、手順に従って、[次のセクション](#new-website))。

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

アプリを展開する をクリックして**発行**です。 発行の進行状況を表示することができます、 **Web 公開アクティビティ**ウィンドウです。 (から、**ビュー**メニューの **その他のウィンドウ**選択してから、 **Web 公開アクティビティ**)。

![](part-10/_static/image4.png)

Visual Studio では、アプリケーションの展開が完了すると、既定のブラウザーが配置された web サイトの URL を自動的に開き、作成したアプリケーションが、クラウドで実行するようになりました。 ブラウザーのアドレス バーに URL では、サイトがインターネットから読み込まれることを示します。

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>新しい web サイトに展開します。

オンにしなかった場合**クラウドでホスト**最初にプロジェクトを作成したときに、新しい web アプリを今すぐ構成できます。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。 選択、**プロファイル** タブでをクリックし、 **Microsoft Azure Websites**です。 現在 Azure にサインインしていない場合は、サインインするように求められます。

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

**既存の web サイト**ダイアログ ボックスで、をクリックして**新規**です。

![](part-10/_static/image9.png)

サイト名を入力します。 Azure サブスクリプションと地域を選択します。 **データベース サーバー****新しいサーバーの作成**、または既存のサーバーを選択します。 **[作成]**をクリックします。

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

クリックして、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。 をクリックして**発行**です。

> [!div class="step-by-step"]
> [前へ](part-9.md)
