---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 を使用して OData v4 エンドポイントの作成 |Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) は、web 用のデータ アクセス プロトコルです。 OData は、照会および CRUD 操作を介してデータ セットを操作する一貫した方法を提供します。
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 7f2d0b8fa8ac290e5018cb5237b1fedb5f40eeb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831386"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 を使用して OData v4 エンドポイントを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> Open Data Protocol (OData) は、web 用のデータ アクセス プロトコルです。 OData クエリおよび CRUD 操作を介してデータ セットを操作する一貫した方法を提供します (作成、読み取り、更新、および削除) します。
> 
> ASP.NET Web API には、v3 と v4 のプロトコルの両方がサポートしています。 サイド バイ サイドで実行される v4 エンドポイントを持つこともできます v3 エンドポイントを使用します。
> 
> このチュートリアルでは、CRUD 操作をサポートする OData v4 エンドポイントを作成する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> OData バージョン 3 では、次を参照してください。 [OData v3 エンドポイントの作成](../odata-v3/creating-an-odata-endpoint.md)です。


## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio から、**ファイル**メニューの **新規** &gt; **プロジェクト**します。

展開**インストール済み** &gt; **テンプレート** &gt; **Visual c#** &gt; **Web**、を選択します。**ASP.NET Web アプリケーション**テンプレート。 プロジェクトに名前を&quot;ProductService&quot;します。

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

**新しいプロジェクト**ダイアログ ボックスで、**空**テンプレート。 [&quot;フォルダーを追加し、コア参照.&quot;、] をクリックして**Web API**します。 **[OK]** をクリックします。

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>OData のパッケージをインストールします。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** &gt; **[パッケージ マネージャー コンソール]** の順に選択します。 パッケージ マネージャー コンソール ウィンドウで、次のように入力します。

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

このコマンドは、最新の OData の NuGet パッケージをインストールします。

## <a name="add-a-model-class"></a>モデル クラスを追加します。

A*モデル*は、アプリケーションでのデータ エンティティを表すオブジェクトです。

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューでは、次のように選択します。**追加** &gt; **クラス**します。

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> 慣例により、モデル クラスは、Models フォルダーに配置されますが、独自のプロジェクトでこの規則に従う必要はありません。


クラスに `Product` という名前を付けます。 Product.cs ファイルでは、次のように定型コードを置き換えます。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id`プロパティがエンティティ キー。 クライアントは、キーによってエンティティを照会できます。 たとえば、id が 5 の製品を取得する URI は`/Products(5)`します。 `Id`プロパティは、バック エンド データベースで主キーにもなります。

## <a name="enable-entity-framework"></a>Entity Framework を有効にします。

このチュートリアルで使用します Entity Framework (EF) Code First バック エンド データベースを作成します。

> [!NOTE]
> Web API OData では、EF は必要ありません。 データベース エンティティをモデルに変換できる任意のデータ アクセス層を使用します。


最初に、EF の NuGet パッケージをインストールします。 **[ツール]** メニューで、**[NuGet パッケージ マネージャー]** &gt; **[パッケージ マネージャー コンソール]** の順に選択します。 パッケージ マネージャー コンソール ウィンドウで、次のように入力します。

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Web.config ファイルを開き、内の次のセクションを追加、**構成**要素後に、 **configSections**要素。

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

この設定は、LocalDB データベースの接続文字列を追加します。 アプリをローカルで実行するときに、このデータベースが使用されます。

という名前のクラスを次に、追加`ProductsContext`Models フォルダーに。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

コンス トラクターの`"name=ProductsContext"`接続文字列の名前を指定します。

## <a name="configure-the-odata-endpoint"></a>OData エンドポイントを構成します。

アプリのファイルを開く\_Start/WebApiConfig.cs します。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

次のコードを追加し、**登録**メソッド。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

このコードは 2 つの処理を行います。

- Entity Data Model (EDM) を作成します。
- ルートを追加します。

EDM は、データの抽象モデルです。 EDM を使用して、サービス メタデータ ドキュメントを作成できます。 **ODataConventionModelBuilder**クラスは、既定の名前付け規則を使用して EDM を作成します。 このアプローチでは、最小限のコードが必要です。 使用することができます、EDM より詳細に制御する場合、**に**プロパティ、キー、およびナビゲーション プロパティを明示的に追加することで、EDM を作成するクラス。

A*ルート*Web API エンドポイントに HTTP 要求をルーティングする方法を示します。 OData v4 のルートを作成するには、 **MapODataServiceRoute**拡張メソッド。

アプリケーションに複数の OData エンドポイントがある場合は、それぞれの個別のルートを作成します。 一意のルート名およびプレフィックスは、各ルートを提供します。

## <a name="add-the-odata-controller"></a>OData コント ローラーを追加します。

A*コント ローラー*は HTTP 要求を処理するクラスです。 各エンティティ セットを OData サービスでの個別のコント ローラーを作成します。 このチュートリアルでは、1 つのコント ローラーを作成、`Product`エンティティ。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックして**追加** &gt; **クラス**します。 クラスに `ProductsController` という名前を付けます。

> [!NOTE]
> このチュートリアルは、OData v3 の使用のバージョン、**コント ローラーの追加**スキャフォールディングします。 現時点では、OData v4 のスキャフォールディングではありません。


次のように ProductsController.cs の定型コードを置き換えます。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

コント ローラーの使用、 `ProductsContext` EF を使用してデータベースにアクセスするクラス。 コント ローラーのオーバーライドに注意してください、 **Dispose**を破棄するメソッド、 **ProductsContext**します。

これは、コント ローラーの開始点です。 次に、すべての CRUD 操作のメソッドを追加します。

## <a name="querying-the-entity-set"></a>エンティティ セットのクエリを実行します。

次のメソッドを追加`ProductsController`します。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

パラメーターなしのバージョンの`Get`メソッド全体の製品のコレクションを返します。 `Get`メソッドを*キー*キーで製品を次のパラメーター (この場合、`Id`プロパティ)。

**[EnableQuery]** 属性は、$filter、$sort、$page などのクエリ オプションを使用して、クエリを変更するクライアントを使用できます。 詳細については、次を参照してください。 [OData クエリ オプションをサポートしている](../supporting-odata-query-options.md)します。

## <a name="adding-an-entity-to-the-entity-set"></a>エンティティ セットへのエンティティの追加

データベースに新しい製品を追加するクライアントを有効にするには、次のメソッドを追加`ProductsController`します。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>エンティティの更新

OData は、PATCH、PUT は、エンティティを更新するための 2 つの異なるセマンティクスをサポートします。

- 修正プログラムは部分的な更新を実行します。 クライアントでは、更新するプロパティだけを指定します。
- PUT は、エンティティ全体を置き換えます。

PUT の欠点は、クライアントが変更されない値を含むエンティティでは、すべてのプロパティの値を送信する必要があります。 [OData 仕様](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)修正プログラムが優先されることを示します。

いずれの場合も、コード修正プログラムと PUT の両方のメソッドを次に示します。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

コント ローラーを使用して、修正プログラムの場合、**デルタ&lt;T&gt;** 変更を追跡する型。

## <a name="deleting-an-entity"></a>エンティティの削除

データベースから製品を削除するクライアントを有効にするには、次のメソッドを追加`ProductsController`します。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
