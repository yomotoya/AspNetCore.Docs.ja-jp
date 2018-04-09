---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'パート 1: 概要と、プロジェクトの作成 |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>パート 1: 概要と、プロジェクトを作成します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework は、/、オブジェクト リレーショナル マッピング フレームワークです。 ドメイン オブジェクトをコードでは、リレーショナル データベース内のエンティティにマップされます。 ほとんどの場合、必要はありません、データベース レイヤーについて心配する Entity Framework は、処理をするためです。 コードは、オブジェクトを操作し、データベースへの変更は保持します。

## <a name="about-the-tutorial"></a>チュートリアルについて

このチュートリアルでは、単純なストア アプリケーションを作成します。 アプリケーションに 2 つの主要な部分があります。 通常のユーザーは、製品を表示し、注文を作成します。

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

管理者は、作成、削除、または製品を編集できます。

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>スキルの学習

学習する内容を次に示します。

- ASP.NET Web API で Entity Framework を使用する方法です。
- Knockout.js を使用して動的クライアント UI を作成する方法。
- Web API でフォーム認証を使用して、ユーザーを認証する方法です。

このチュートリアルでは自己完結型で、次のチュートリアルを最初に読み取る可能性があります。

- [初めての ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [CRUD 操作をサポートする Web API を作成します。](../creating-a-web-api-that-supports-crud-operations.md)

一定の知識[ASP.NET MVC](../../../../mvc/index.md)も便利です。

## <a name="overview"></a>概要

大まかに言えば、アプリケーションのアーキテクチャを次に示します。

- ASP.NET MVC では、クライアントの HTML ページを生成します。
- ASP.NET Web API は、データ (製品および orders) に対する CRUD 操作を公開します。
- Entity Framework では、データベースのエンティティに Web API が使用する (C#) モデルを変換します。

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

次の図は、アプリケーションのさまざまなレイヤーでのドメイン オブジェクトの表現方法: データベース層、オブジェクト モデルでは、および最後に、ワイヤ形式は、HTTP 経由でクライアントにデータを送信するために使用します。

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Web Developer Express または完全バージョンの Visual Studio を使用してチュートリアル プロジェクトを作成することができます。

**開始**] ページで [**新しいプロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクト"ProductStore"の名前し、をクリックして**OK**です。

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、**インターネット アプリケーション** をクリック**OK**です。

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

「インターネット アプリケーション」テンプレートでは、フォーム認証をサポートする ASP.NET MVC アプリケーションを作成します。 今すぐアプリケーションを実行する場合は、一部の機能が既にいます。

- 新しいユーザーは、右上隅に「登録」リンクをクリックして登録できます。
- 登録済みユーザーは、「ログイン」リンクをクリックしてログインできます。

メンバーシップ情報は、自動的に作成されるデータベースに保存されます。 ASP.NET MVC でのフォーム認証の詳細については、次を参照してください。[チュートリアル: ASP.NET MVC でのフォーム認証を使用して](https://msdn.microsoft.com/library/ff398049(VS.98).aspx)です。

## <a name="update-the-css-file"></a>CSS ファイルを更新します。

この手順は表面的なが、以前のスクリーン ショットのように表示するページになります。

ソリューション エクスプ ローラーで、コンテンツ フォルダーを展開し、Site.css をという名前のファイルを開きます。 次の CSS スタイルを追加します。

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [次へ](using-web-api-with-entity-framework-part-2.md)
