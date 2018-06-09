---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Web API 2 OData v3 エンドポイントの作成 |Microsoft ドキュメント
author: MikeWasson
description: Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。 OData は、データ構造体、データをクエリおよびデータを操作する一貫した方法を提供します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874630"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Web API 2 OData v3 エンドポイントを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Open Data Protocol](http://www.odata.org/) (OData) は、web のデータ アクセス プロトコル。 OData にはデータを構造体、データに対するクエリおよび CRUD 操作を通じてデータ セットを操作する方法を統一 (作成、読み取り、更新、および削除) します。 OData は、AtomPub (XML) と JSON 形式の両方をサポートします。 OData では、データに関するメタデータを公開する方法も定義します。 クライアントは、型情報と、データ セットの関係を検出するのにメタデータを使用できます。
> 
> ASP.NET Web API では、簡単にデータ セットの OData エンドポイントを作成します。 エンドポイントをサポートしている OData 操作だけを制御できます。 非 OData エンドポイントと共に、複数の OData エンドポイントをホストすることができます。 データ モデル、バックエンド ビジネス ロジック、およびデータ層を完全に制御があります。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6
> - [Fiddler Web デバッグ プロキシ (省略可能)](http://www.fiddler2.com)
> 
> Web API OData のサポートが追加された[ASP.NET および Web ツール 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)です。 ただし、このチュートリアルでは、Visual Studio 2013 で追加されたスキャフォールディングを使用します。


このチュートリアルでは、クライアントが照会できる単純な OData エンドポイントを作成します。 エンドポイントの c# クライアントも作成します。 このチュートリアルを完了すると、チュートリアルの次のセットが、アクション、エンティティ リレーションシップを含む、多くの機能を追加する方法を表示し、$展開/$ を選択します。

- [Visual Studio プロジェクトを作成します。](#create-project)
- [エンティティ モデルを追加します。](#add-model)
- [OData コント ローラーを追加します。](#add-controller)
- [EDM およびルートを追加します。](#edm)
- [(省略可能) のデータベースをシードします。](#seed-db)
- [OData エンドポイントの探索](#explore)
- [OData のシリアル化形式](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

このチュートリアルでは、基本的な CRUD 操作をサポートする OData エンドポイントを作成します。 エンドポイントは、1 つのリソース、製品のリストを公開します。 後のチュートリアルより多くの機能を追加します。

Visual Studio を起動し、選択**新しいプロジェクト**スタート ページからです。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**[Visual c#] ノードを展開します。 **Visual c#** **Web**です。 選択**ASP.NET Web アプリケーション**テンプレート。

![](creating-an-odata-endpoint/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。 &quot;フォルダーを追加し、コアを参照しています.&quot;、確認**Web API**です。 **[OK]** をクリックします。

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>エンティティ モデルを追加します。

*モデル*は、アプリケーションのデータを表すオブジェクトです。 このチュートリアルでは、製品を表すモデルが必要です。 モデルは、OData エンティティ型に対応しています。

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューから選択**追加**を選択し、**クラス**です。

![](creating-an-odata-endpoint/_static/image3.png)

**新規追加**ダイアログの項目に、クラスを名前&quot;製品&quot;です。

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> 慣例により、モデルのクラスは、Models フォルダーに配置されます。 独自のプロジェクトでこの規則に従う必要はありませんが、このチュートリアルで使用します。


Product.cs ファイルでは、次のクラス定義を追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID プロパティは、エンティティ キーになります。 クライアントは ID によって製品を照会できます。 このフィールドは、バックエンド データベースの主キーもなります。

今すぐプロジェクトをビルドします。 次の手順では、リフレクションを使用して、製品の種類を検索するいくつかの Visual Studio スキャフォールディングを使用します。

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>OData コント ローラーを追加します。

A*コント ローラー* HTTP 要求を処理するクラスです。 各エンティティに設定する OData サービスで別のコント ローラーを定義します。 このチュートリアルでは単一のコント ローラーを作成します。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**し、**コント ローラー**です。

![](creating-an-odata-endpoint/_static/image5.png)

**追加 Scaffold**ダイアログで、 &quot;Web API 2 OData コント ローラー アクション、Entity Framework を使用して&quot;です。

![](creating-an-odata-endpoint/_static/image6.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー"ProductsController"です。 選択、&quot;非同期コント ローラー アクションを使用して&quot;チェック ボックスをオンします。 **モデル**ドロップダウン リストで、Product クラスを選択します。

![](creating-an-odata-endpoint/_static/image7.png)

クリックして、**新しいデータ コンテキストしています.** ボタンをクリックします。 データ コンテキスト型の既定の名前のままにし、をクリックして**追加**です。

![](creating-an-odata-endpoint/_static/image8.png)

コント ローラーを追加するコント ローラーの追加ダイアログ ボックスで [追加] をクリックします。

![](creating-an-odata-endpoint/_static/image9.png)

注: エラー メッセージが表示することを知らせる&quot;型を取得中にエラーが発生しました.&quot;、Product クラスを追加した後は、Visual Studio プロジェクトをビルドすることを確認してください。 スキャフォールディングでは、リフレクションを使用して、クラスを検索します。

![](creating-an-odata-endpoint/_static/image10.png)

スキャフォールディングでは、プロジェクトに 2 つのコード ファイルを追加します。

- Products.cs では、OData エンドポイントを実装する Web API コント ローラーを定義します。
- ProductServiceContext.cs は、Entity Framework を使用して、基になるデータベースをクエリするメソッドを提供します。

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>EDM およびルートを追加します。

ソリューション エクスプ ローラーで、アプリを展開して\_フォルダーを起動し、WebApiConfig.cs をという名前のファイルを開きます。 このクラスは、Web API の構成コードを保持します。 このコードを次のように置き換えます。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

このコードでは、2 つの処理が行われます。

- OData エンドポイントのエンティティ データ モデル (EDM) を作成します。
- エンドポイントのルートを追加します。

EDM は、データの抽象モデルです。 EDM は、メタデータ ドキュメントを作成し、サービスの Uri を定義に使用されます。 **ODataConventionModelBuilder** EDM 既定の名前付け規則のセットを使用して、EDM を作成します。 このアプローチには、最小限のコードが必要です。 EDM より詳細に制御する場合を使えば、**ため**プロパティ、キー、およびナビゲーションのプロパティを明示的に追加することによって、EDM を作成するクラス。

**EntitySet**メソッドは、EDM に設定エンティティを追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

文字列"Products"では、エンティティ セットの名前を定義します。 コント ローラーの名前は、エンティティ セットの名前と一致する必要があります。 エンティティ セットでこのチュートリアルでは、"Products など"の名前は、コント ローラーの名前は`ProductsController`します。 エンティティ セット"ProductSet"という名前を付けた場合、コント ローラーの名前を付けますが`ProductSetController`です。 エンドポイントが複数のエンティティ セットを持てることに注意してください。 呼び出す**EntitySet&lt;T&gt;** の各エンティティ セットにし、対応するコント ローラーを定義します。

**MapODataRoute**メソッドは、OData エンドポイントのルートを追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

最初のパラメーターは、ルートのフレンドリ名です。 サービスのクライアントでは、この名前は表示されません。 2 番目のパラメーターは、エンドポイントの URI プレフィックスです。 このコードを指定するには、Products エンティティ セットの URI は http://<em>hostname</em>odata/製品です。 アプリケーションでは、1 つ以上の OData エンドポイントを持つことができます。 各エンドポイントでは、呼び出す<strong>MapODataRoute</strong>固有のルート名と一意の URI プレフィックスを入力します。

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>(省略可能) のデータベースをシードします。

この手順では、Entity Framework を使用して、テスト データの一部で、データベースのシードです。 この手順は省略可能ながすぐに、OData エンドポイントをテストすることができます。

**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

これは、移行とされる Configuration.cs をという名前のコード ファイルをという名前のフォルダーを追加します。

![](creating-an-odata-endpoint/_static/image12.png)

このファイルを開き、次のコードを追加、`Configuration.Seed`メソッドです。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

これらのコマンドは、データベースを作成し、そのコードを実行するコードを生成します。

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>OData エンドポイントの探索

使用して、このセクションで、 [Fiddler Web デバッグ プロキシ](http://www.fiddler2.com)をエンドポイントに要求を送信し、応答メッセージを確認します。 これは、OData エンドポイントの機能を理解するのに役立ちます。

Visual Studio で f5 キーを押してデバッグを開始します。 既定では、Visual Studio がブラウザーを開いて`http://localhost:*port*`ここで、*ポート*プロジェクト設定で構成されたポート番号です。

プロジェクト設定でポート番号を変更することができます。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。 [プロパティ] ウィンドウで選択**Web**です。 ポート番号を入力**プロジェクト Url**です。

### <a name="service-document"></a>サービス ドキュメント

*サービス ドキュメント*OData エンドポイントのエンティティ セットの一覧が含まれています。 サービス ドキュメントを取得するには、サービスのルート URI に GET 要求を送信します。

次の URI を入力して、Fiddler を使用して、 **Composer**  タブ:`http://localhost:port/odata/`ここで、*ポート*のポート番号です。

![](creating-an-odata-endpoint/_static/image13.png)

クリックして、 **Execute**ボタンをクリックします。 Fiddler は、アプリケーションに HTTP GET 要求を送信します。 Web セッションの一覧で、応答が表示されます。 すべてが動作している場合、状態コードは 200 になります。

![](creating-an-odata-endpoint/_static/image14.png)

インスペクター タブで、応答メッセージの詳細を表示する Web セッションの一覧で、応答をダブルクリックします。

![](creating-an-odata-endpoint/_static/image15.png)

生の HTTP 応答メッセージは、次のようになります。

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

既定では、Web API は、AtomPub 形式でサービス ドキュメントを返します。 JSON を要求するには、HTTP 要求に次のヘッダーを追加します。

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

これで、HTTP 応答には、JSON ペイロードが含まれています。

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>サービス メタデータ ドキュメント

*サービス メタデータ ドキュメント*概念スキーマ定義言語 (CSDL) と呼ばれる XML 言語を使用して、サービスのデータ モデルについて説明します。 メタデータ ドキュメントでは、サービスでは、データの構造を示していて、クライアント コードを生成するために使用できます。

メタデータ ドキュメントを取得する GET 要求を送信`http://localhost:port/odata/$metadata`です。 このチュートリアルで示すようにエンドポイントのメタデータを次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>エンティティ セット

Products エンティティ セットを取得する GET 要求を送信`http://localhost:port/odata/Products`です。

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>エンティティ

個々 の製品を入手するに GET 要求を送信`http://localhost:port/odata/Products(1)`ここで、「1」は、製品の id です。

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData のシリアル化形式

OData には、いくつかのシリアル化形式がサポートされています。

- Atom Pub (XML)
- JSON"light"(OData v3 で導入されました)
- JSON"verbose"(OData v2)

既定では、Web API は、AtomPubJSON"light"形式を使用します。 

AtomPub 形式を取得するには、「アプリケーションおよび atom + xml」に Accept ヘッダーを設定します。 応答本文の例を次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Atom 形式の 1 つの明らかな欠点を確認できます: JSON light よりもずっと詳細なであります。 ただし、AtomPub を理解しているクライアントがあれば、クライアント方がよい形式で JSON 上。

次に、同じエンティティの JSON の軽量バージョンを示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

JSON light 形式は、OData プロトコルの version 3 で導入されました。 旧バージョンとの互換性のため、クライアントは、"verbose"古い JSON 形式を要求できます。 詳細な JSON を要求する Accept ヘッダーを設定`application/json;odata=verbose`です。 詳細なバージョンを次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

この形式は、セッション全体にかなりのオーバーヘッドを追加できる応答本文でより多くのメタデータを伝達します。 また、"d"をという名前のプロパティで、オブジェクトをラップすることによって、レベルの間接参照を追加します。

## <a name="next-steps"></a>次の手順

- [エンティティのリレーションシップを追加します。](working-with-entity-relations.md)
- [OData アクションを追加します。](odata-actions.md)
- [.NET クライアントから OData サービスを呼び出す](calling-an-odata-service-from-a-net-client.md)
