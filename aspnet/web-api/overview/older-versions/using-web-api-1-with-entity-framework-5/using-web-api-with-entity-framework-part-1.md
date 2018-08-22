---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'パート 1: 概要と、プロジェクトの作成 |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0e4021402e8deccd2395f23b6b512679b5e9d281
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829892"
---
<a name="part-1-overview-and-creating-the-project"></a>パート 1: 概要と、プロジェクトを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework は、オブジェクト/リレーショナル マッピング フレームワークです。 ドメイン オブジェクトをコードでは、リレーショナル データベース内のエンティティにマップされます。 ほとんどの場合がありません、データベース層について心配する Entity Framework が自動的に処理ができます。 コードは、オブジェクトを操作して、変更がデータベースに保存されます。

## <a name="about-the-tutorial"></a>チュートリアルの概要

このチュートリアルでは、単純なストア アプリケーションを作成します。 アプリケーションに 2 つの主要な部分があります。 通常のユーザーは、製品を表示し、注文を作成します。

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

管理者は、作成、削除、または製品を編集することができます。

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- ASP.NET Web API を使用した Entity Framework を使用する方法。
- Knockout.js を使用して、動的なクライアント UI を作成する方法。
- Web API を使用したフォーム認証を使用してユーザーを認証する方法。

このチュートリアルでは、自己完結型で、最初に、次のチュートリアルをお読みする可能性があります。

- [ASP.NET Web API の入門](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [CRUD 操作をサポートする Web API を作成します。](../creating-a-web-api-that-supports-crud-operations.md)

知識が[ASP.NET MVC](../../../../mvc/index.md)も便利です。

## <a name="overview"></a>概要

大まかに言えば、アプリケーションのアーキテクチャを示します。

- ASP.NET MVC では、クライアントの HTML ページを生成します。
- ASP.NET Web API は、データ (製品と注文) に対する CRUD 操作を公開します。
- Entity Framework では、データベースのエンティティに Web API で使用される c# モデルを変換します。

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

次の図は、アプリケーションのさまざまなレイヤーでのドメイン オブジェクトの表現方法を示しています。 データベース層、オブジェクト モデルでは、および最後に、ワイヤ形式は、HTTP 経由でクライアントにデータを送信するために使用します。

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Web Developer Express または完全なバージョンの Visual Studio のいずれかを使用して、チュートリアルのプロジェクトを作成することができます。

**開始**] ページで [**新しいプロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトに"ProductStore"という名前にして**OK**します。

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション** をクリック**OK**します。

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

「インターネット アプリケーション」テンプレートは、フォーム認証をサポートする ASP.NET MVC アプリケーションを作成します。 これでアプリケーションを実行する場合は、既にいくつか機能があります。

- 右上隅に「登録」リンクをクリックして新しいユーザーを登録できます。
- 登録済みユーザーは、「ログイン」リンクをクリックしてログインできます。

メンバーシップ情報は、自動的に作成されるデータベースに保存されます。 ASP.NET MVC でのフォーム認証の詳細については、次を参照してください。[チュートリアル: ASP.NET MVC でのフォーム認証を使用して](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)します。

## <a name="update-the-css-file"></a>CSS ファイルを更新します。

この手順は、表面的なが、以前のスクリーン ショットのようなレンダリング ページが行われます。

ソリューション エクスプ ローラーでは、コンテンツのフォルダーを展開し、Site.css という名前のファイルを開きます。 次の CSS スタイルを追加します。

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [次へ](using-web-api-with-entity-framework-part-2.md)
