---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure の Azure App Service へのアプリの発行 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808228"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Azure の Azure App Service にアプリを公開します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

最後の手順としては、Azure にアプリケーションを公開します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**発行**します。

![](part-10/_static/image1.png)

クリックすると**発行**呼び出す、 **Web の発行**ダイアログ。 オンにした場合**クラウド内のホスト**ときに接続し、プロジェクトでは、最初に作成して、設定が既に構成されています。 その場合は、クリックするだけです、**設定** タブで確認し、 &quot;Code First Migrations の実行&quot;します。 (確認しなかった場合**クラウド内のホスト**を先頭に次の手順では、[次のセクション](#new-website))。

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

アプリを展開する をクリックして**発行**します。 発行の進行状況を表示することができます、 **Web 発行アクティビティ**ウィンドウ。 (から、**ビュー**メニューの **その他の Windows**を選択し、 **Web 発行アクティビティ**)。

![](part-10/_static/image4.png)

Visual Studio では、アプリのデプロイが完了したら、デプロイされた web サイトの URL に既定のブラウザーが自動的に開き、作成したアプリケーションが、クラウドで実行されるようになりました。 ブラウザーのアドレス バーに URL では、サイトがインターネットから読み込まれていることを示します。

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>新しい web サイトに展開します。

選択しなかった場合**クラウド内のホスト**最初にプロジェクトを作成したときに、新しい web アプリを今すぐ構成できます。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**発行**します。 選択、**プロファイル** タブでをクリックし、 **Microsoft Azure Websites**します。 現在 Azure にサインインしていない場合は、サインインする促されます。

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

**既存の web サイト**ダイアログ ボックスで、をクリックして**新規**します。

![](part-10/_static/image9.png)

サイト名を入力します。 Azure サブスクリプションとリージョンを選択します。 **データベース サーバー**を選択します**新しいサーバーの作成**、または既存のサーバーを選択します。 **[作成]** をクリックします。

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

をクリックして、**設定** タブで確認し、 &quot;Code First Migrations の実行&quot;します。 クリックして**発行**します。

> [!div class="step-by-step"]
> [前へ](part-9.md)
