---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Web API 2 OData v3 エンドポイントの作成 |Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) は、web 用のデータ アクセス プロトコルです。 OData では、データを構造体、データのクエリをおよびデータを操作する方法を統一を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: bcf089003cfb4233fd2aa70cf669972de46c27b1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362591"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Web API 2 OData v3 エンドポイントの作成
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Open Data Protocol](http://www.odata.org/) (OData) は、web のデータ アクセス プロトコル。 OData データ構造体、データを照会および CRUD 操作を通じてデータ セットを操作する一貫した方法を提供します (作成、読み取り、更新、および削除) します。 OData では、AtomPub (XML) と JSON 形式の両方をサポートします。 OData では、データに関するメタデータを公開する方法も定義します。 クライアントは、型情報と、データ セットの関係を検出するのにメタデータを使用できます。
> 
> ASP.NET Web API では、簡単にデータ セットの OData エンドポイントを作成できます。 OData 操作正確にエンドポイントがサポートを制御できます。 非 OData エンドポイントと共に、複数の OData エンドポイントをホストすることができます。 完全に制御、データ モデル、バック エンドのビジネス ロジック、およびデータ層があります。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6
> - [Fiddler の Web デバッグ プロキシ (省略可能)](http://www.fiddler2.com)
> 
> Web API OData のサポートが追加された[ASP.NET および Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)します。 ただし、このチュートリアルでは、Visual Studio 2013 で追加されたスキャフォールディングを使用します。


このチュートリアルでは、クライアントを照会できる単純な OData エンドポイントを作成します。 C# クライアント エンドポイントも作成します。 このチュートリアルを完了すると、次の一連のチュートリアルは、エンティティ関係、アクションを含む、多くの機能を追加する方法を示すし、$展開/$ を選択します。

- [Visual Studio プロジェクトを作成します。](#create-project)
- [エンティティ モデルを追加します。](#add-model)
- [OData コント ローラーを追加します。](#add-controller)
- [EDM およびルートを追加します。](#edm)
- [(省略可能) データベースをシードします。](#seed-db)
- [OData エンドポイントの探索](#explore)
- [OData シリアル化の形式](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

このチュートリアルでは、基本的な CRUD 操作をサポートする OData エンドポイントを作成します。 エンドポイントでは、製品の一覧の 1 つのリソースを公開します。 以降のチュートリアルより多くの機能を追加します。

Visual Studio を起動し、選択**新しいプロジェクト**スタート ページから。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**Visual c# ノードを展開します。 **Visual c#**、 **Web**します。 選択 **、ASP.NET Web アプリケーション**テンプレート。

![](creating-an-odata-endpoint/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。 &quot;フォルダーを追加し、コアを参照しています.&quot;、確認**Web API**します。 **[OK]** をクリックします。

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>エンティティ モデルを追加します。

*モデル*は、アプリケーションのデータを表すオブジェクトです。 このチュートリアルでは、製品を表すモデルが必要です。 モデルは、OData エンティティ型に対応します。

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 コンテキスト メニューでは、次のように選択します。**追加**選び**クラス**します。

![](creating-an-odata-endpoint/_static/image3.png)

**新規追加**ダイアログの項目、クラスの名前を付けて&quot;製品&quot;します。

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> 慣例により、モデル クラスは、Models フォルダーに配置されます。 独自のプロジェクトでこの規則に従う必要はありませんが、このチュートリアルを使用します。


Product.cs ファイルでは、次のクラス定義を追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID プロパティは、エンティティ キーになります。 クライアントは ID によって製品を照会できます。 このフィールドは、バックエンド データベースの主キーもなります。

これでプロジェクトをビルドします。 次の手順では、リフレクションを使用して、製品の種類を検索するいくつかの Visual Studio スキャフォールディングを使用します。

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>OData コント ローラーを追加します。

A*コント ローラー*は HTTP 要求を処理するクラスです。 各エンティティの OData サービスのセットを別のコント ローラーを定義します。 このチュートリアルでは、単一のコント ローラーを作成します。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**選び**コント ローラー**します。

![](creating-an-odata-endpoint/_static/image5.png)

**スキャフォールディングの追加**ダイアログ ボックスで、 &quot;Web API 2 OData コント ローラー アクション、Entity Framework を使用して&quot;します。

![](creating-an-odata-endpoint/_static/image6.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー"ProductsController"。 選択、&quot;非同期コント ローラー アクションを使用して、&quot;チェック ボックスをオンします。 **モデル**ドロップダウン リストで、Product クラスを選択します。

![](creating-an-odata-endpoint/_static/image7.png)

をクリックして、**新しいデータ コンテキスト.** ボタンをクリックします。 データ コンテキスト型の既定の名前のままにし、をクリックして**追加**します。

![](creating-an-odata-endpoint/_static/image8.png)

コント ローラーを追加するコント ローラーの追加ダイアログ ボックスで [追加] をクリックします。

![](creating-an-odata-endpoint/_static/image9.png)

注: いるというエラー メッセージを取得するかどうかは&quot;型を取得中にエラーが発生しました.&quot;、Product クラスを追加した後、Visual Studio プロジェクトをビルドしたことを確認します。 スキャフォールディングでは、リフレクションを使用して、クラスを検索します。

![](creating-an-odata-endpoint/_static/image10.png)

スキャフォールディングは、プロジェクトに 2 つのコード ファイルを追加します。

- Products.cs では、OData エンドポイントを実装する Web API コント ローラーを定義します。
- ProductServiceContext.cs は、Entity Framework を使用して、基になるデータベースを照会するメソッドを提供します。

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>EDM およびルートを追加します。

ソリューション エクスプ ローラーでアプリケーションを拡張して\_フォルダーを起動し、WebApiConfig.cs という名前のファイルを開きます。 このクラスは、Web API の構成コードを保持します。 このコードを次のように置き換えます。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

このコードは 2 つの処理を行います。

- OData エンドポイントのエンティティ データ モデル (EDM) を作成します。
- エンドポイントのルートを追加します。

EDM は、データの抽象モデルです。 EDM は、メタデータ ドキュメントを作成し、サービスの Uri の定義に使用されます。 **ODataConventionModelBuilder** EDM 既定の名前付け規則のセットを使用して EDM を作成します。 このアプローチでは、最小限のコードが必要です。 使用することができます、EDM より詳細に制御する場合、**に**プロパティ、キー、およびナビゲーション プロパティを明示的に追加することで、EDM を作成するクラス。

**EntitySet**メソッドは、EDM に設定エンティティを追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

文字列「製品」は、エンティティ セットの名前を定義します。 コント ローラーの名前は、エンティティ セットの名前と一致する必要があります。 エンティティ セットでこのチュートリアルでは、"Products"の名前は、コント ローラーの名前は`ProductsController`します。 エンティティ セットの"ProductSet"という名前を付けた場合は、名前、コント ローラー`ProductSetController`します。 エンドポイントは複数のエンティティ セットであることに注意してください。 呼び出す**EntitySet&lt;T&gt;** エンティティごとに設定すると、および対応するコント ローラーを定義します。

**MapODataRoute**メソッドは、OData エンドポイントのルートを追加します。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

最初のパラメーターは、ルートのフレンドリ名です。 サービスのクライアントでは、この名前は表示されません。 2 番目のパラメーターは、エンドポイントの URI プレフィックスです。 このコードを指定するには、Products エンティティ セットの URI は http://<em>hostname</em>  /odata/製品です。 アプリケーションでは、1 つ以上の OData エンドポイントを持つことができます。 各エンドポイントでは、呼び出す<strong>MapODataRoute</strong>一意のルート名と一意の URI プレフィックスを入力します。

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>(省略可能) データベースをシードします。

この手順では、何らかのテスト データでデータベースをシードするのに Entity Framework を使用します。 この手順は省略可能でも、すぐ OData エンドポイントをテストできます。

**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

これには、移行とという Configuration.cs コード ファイルをという名前のフォルダーが追加されます。

![](creating-an-odata-endpoint/_static/image12.png)

このファイルを開き、次のコードを追加、`Configuration.Seed`メソッド。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

これらのコマンドは、データベースを作成し、そのコードを実行するコードを生成します。

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>OData エンドポイントの探索

このセクションで使用します、 [Fiddler Web デバッグ プロキシ](http://www.fiddler2.com)エンドポイントに要求を送信し、応答メッセージを確認します。 これは、OData エンドポイントの機能を理解するのに役立ちます。

Visual Studio で f5 キーを押してデバッグを開始します。 既定では、Visual Studio がブラウザーを開いて`http://localhost:*port*`ここで、*ポート*はプロジェクトの設定で構成されているポート番号です。

プロジェクトの設定でポート番号を変更することができます。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。 [プロパティ] ウィンドウで次のように選択します。 **Web**します。 ポート番号を入力**プロジェクト Url**します。

### <a name="service-document"></a>サービス ドキュメント

*サービス ドキュメント*OData エンドポイントのエンティティ セットの一覧が含まれています。 サービス ドキュメントを取得するには、ルート サービスの URI に GET 要求を送信します。

次の URI を入力して、Fiddler を使用して、 **Composer**  タブ:`http://localhost:port/odata/`ここで、*ポート*のポート番号です。

![](creating-an-odata-endpoint/_static/image13.png)

をクリックして、 **Execute**ボタンをクリックします。 Fiddler では、アプリケーションに HTTP GET 要求を送信します。 Web セッションの一覧で応答が表示されます。 すべてが動作している場合、状態コードが 200 になります。

![](creating-an-odata-endpoint/_static/image14.png)

[インスペクター] タブで、応答メッセージの詳細を表示する Web セッションの一覧で、応答をダブルクリックします。

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

メタデータ ドキュメントを取得する GET 要求を送信`http://localhost:port/odata/$metadata`します。 このチュートリアルで示すようにエンドポイントのメタデータを次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>エンティティ セット

Products エンティティ セットを取得する GET 要求を送信`http://localhost:port/odata/Products`します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>エンティティ

個別の製品を取得する GET 要求を送信`http://localhost:port/odata/Products(1)`ここで、「1」は製品の id。

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData シリアル化の形式

OData は、いくつかのシリアル化形式をサポートしています。

- Atom Pub (XML)
- JSON (OData v3 で導入) の"light"
- JSON の"verbose"(OData v2)

既定では、Web API は、AtomPubJSON"light"の形式を使用します。 

AtomPub 形式を取得するには、「アプリケーション フィードおよび atom + xml」に Accept ヘッダーを設定します。 応答本文の例を次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Atom 形式の 1 つの明らかなメリットを確認できます。 JSON light よりもはるかに詳細になります。 ただし、AtomPub を認識するクライアントがある場合、クライアントがその形式を JSON に対するに優先する可能性があります。

次に、同じエンティティの JSON の軽量バージョンを示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

JSON light 形式は、OData プロトコルのバージョン 3 で導入されました。 旧バージョンと互換性のため、クライアントは、古い"verbose"の JSON 形式を要求できます。 詳細な JSON を要求に Accept ヘッダーを設定します。`application/json;odata=verbose`します。 詳細なバージョンを次に示します。

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

この形式は、セッション全体にかなりのオーバーヘッドを追加できる応答の本体でより多くのメタデータを伝達します。 また、"d"をという名前のプロパティで、オブジェクトをラップすることによって、レベルの間接参照を追加します。

## <a name="next-steps"></a>次の手順

- [エンティティのリレーションシップを追加します。](working-with-entity-relations.md)
- [OData アクションを追加します。](odata-actions.md)
- [.NET クライアントから OData サービスを呼び出す](calling-an-odata-service-from-a-net-client.md)
