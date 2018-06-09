---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 を使用して OData v4 エンドポイントを作成 |Microsoft ドキュメント
author: MikeWasson
description: Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。 OData は、クエリおよび CRUD 操作からのデータ セットを操作する一貫した方法を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508051"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 を使用して OData v4 エンドポイントを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。 OData クエリおよび CRUD 操作からのデータ セットを操作する一貫した方法には (作成、読み取り、更新、および削除) します。
> 
> ASP.NET Web API には、v3 と v4 のプロトコルの両方がサポートしています。 サイド バイ サイドを実行している v4 エンドポイントを持つこともできます v3 エンドポイントを使用します。
> 
> このチュートリアルでは、CRUD 操作をサポートする OData v4 エンドポイントを作成する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
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
> OData バージョン 3 を参照してください。 [OData v3 エンドポイントを作成する](../odata-v3/creating-an-odata-endpoint.md)です。


## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio から、**ファイル**メニューの **新規** &gt; **プロジェクト**です。

展開**インストール** &gt; **テンプレート** &gt; **Visual c#** &gt; **Web**、し、を選択**ASP.NET Web アプリケーション**テンプレート。 プロジェクトに名前を&quot;ProductService&quot;です。

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

**新しいプロジェクト**ダイアログで、選択、**空**テンプレート。 &quot;フォルダーを追加し、コア参照しています.&quot;をクリックして**Web API**です。 **[OK]** をクリックします。

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>OData のパッケージをインストールします。

**ツール**メニューの  **NuGet Package Manager** &gt; **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで次のように入力します。

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

このコマンドは、最新の OData NuGet パッケージをインストールします。

## <a name="add-a-model-class"></a>モデル クラスを追加します。

A*モデル*アプリケーションにデータ エンティティを表すオブジェクトです。

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューから選択**追加** &gt; **クラス**です。

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> 慣例により、モデルのクラスが Models フォルダーに配置されますが、独自のプロジェクトでこの規則に準拠する必要はありません。


クラスに `Product` という名前を付けます。 Product.cs ファイルでは、次のように定型コードを置き換えます。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id`プロパティがエンティティ キー。 クライアントは、キーによってエンティティをクエリできます。 たとえば、5 の ID を持つ製品を取得するには、URI は`/Products(5)`します。 `Id`プロパティは、バックエンド データベースの主キーにもなります。

## <a name="enable-entity-framework"></a>Entity Framework を有効にします。

このチュートリアルで使用されます Entity Framework (EF) Code First バックエンド データベースを作成します。

> [!NOTE]
> Web API OData には、EF は不要です。 データベース エンティティをモデルに変換できる任意のデータ アクセス層を使用します。


最初に、EF の NuGet パッケージをインストールします。 **ツール**メニューの  **NuGet Package Manager** &gt; **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで次のように入力します。

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Web.config ファイルを開き、内の次のセクションを追加、**構成**要素の後に、 **configSections**要素。

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

この設定は、LocalDB データベースの接続文字列を追加します。 このデータベースは、アプリをローカルで実行するときに使用されます。

という名前のクラスを次に、追加`ProductsContext`Models フォルダーに。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

コンス トラクターで`"name=ProductsContext"`接続文字列の名前が付けられます。

## <a name="configure-the-odata-endpoint"></a>OData エンドポイントを構成します。

アプリのファイルを開く\_Start/WebApiConfig.cs です。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

次のコードを追加、**登録**メソッド。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

このコードでは、2 つの処理が行われます。

- Entity Data Model (EDM) を作成します。
- ルートを追加します。

EDM は、データの抽象モデルです。 EDM を使用して、サービス メタデータ ドキュメントを作成できます。 **ODataConventionModelBuilder**クラスは、既定の名前付け規則を使用して、EDM を作成します。 このアプローチには、最小限のコードが必要です。 EDM より詳細に制御する場合を使えば、**ため**プロパティ、キー、およびナビゲーションのプロパティを明示的に追加することによって、EDM を作成するクラス。

A*ルート*Web API HTTP 要求をエンドポイントにルーティングする方法について説明します。 OData v4 ルートを作成するには、 **MapODataServiceRoute**拡張メソッド。

アプリケーションに複数の OData エンドポイントがある場合は、ごとに個別のルートを作成します。 一意のルート名とプレフィックスは、各ルートを提供します。

## <a name="add-the-odata-controller"></a>OData コント ローラーを追加します。

A*コント ローラー* HTTP 要求を処理するクラスです。 エンティティごとに、OData サービスで設定したコント ローラーを作成します。 このチュートリアルでは、1 つのコント ローラーを作成、`Product`エンティティです。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックして**追加** &gt; **クラス**です。 クラスに `ProductsController` という名前を付けます。

> [!NOTE]
> OData v3 の使用は、このチュートリアルのバージョン、**コント ローラーの追加**スキャフォールディングできます。 現時点では、OData v4 のスキャフォールディングではありません。


ProductsController.cs で定型コードを次のように置き換えます。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

コント ローラーの使用、 `ProductsContext` EF を使用してデータベースにアクセスするクラス。 コント ローラーをオーバーライドする通知、 **Dispose**を破棄する方法、 **ProductsContext**です。

これは、コント ローラーの開始点です。 次に、CRUD 操作のすべてのメソッドを追加します。

## <a name="querying-the-entity-set"></a>エンティティ セットのクエリを実行します。

次のメソッドを追加`ProductsController`です。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

パラメーターなしのバージョンの`Get`メソッド全体の製品のコレクションを返します。 `Get`メソッドを*キー*パラメーターがキーを使用して、製品を検索 (ここで、`Id`プロパティ)。

**[EnableQuery]** 属性により、クライアントは、$filter、$sort、$page などのクエリ オプションを使用して、クエリを変更します。 詳細については、次を参照してください。 [OData クエリ オプションをサポートする](../supporting-odata-query-options.md)です。

## <a name="adding-an-entity-to-the-entity-set"></a>エンティティ セットにエンティティを追加します。

データベースに新しい製品を追加するクライアントを有効にするには、次のメソッドを追加`ProductsController`です。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>エンティティの更新

OData は、PATCH、PUT、エンティティの更新の 2 つの異なるセマンティクスをサポートします。

- 修正プログラムでは、部分的な更新を実行します。 クライアントは、更新するプロパティだけを指定します。
- PUT は、エンティティ全体を置換します。

PUT の欠点は、クライアントがエンティティでは、値を変更しないを含むすべてのプロパティの値を送信する必要があります。 [OData 仕様](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)修正プログラムが優先されることを示すです。

いずれの場合は、修正プログラムと PUT の両メソッドのコードを次に示します。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

コント ローラーを使用して、修正プログラムの場合、**デルタ&lt;T&gt;** 変更を追跡する型。

## <a name="deleting-an-entity"></a>エンティティの削除

データベースから製品を削除するクライアントを有効にするには、次のメソッドを追加`ProductsController`です。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
