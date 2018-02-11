---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: ".NET クライアント (c#) から OData サービスを呼び出す |Microsoft ドキュメント"
author: MikeWasson
description: "このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。 チュートリアルの Visual Studio 2013 (Visual S. と連携.. で使用されているソフトウェアのバージョン"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>.NET クライアント (c#) から OData サービスを呼び出す
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 で動作します)
> - [WCF Data Services クライアント ライブラリ](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (Web API 2 を使用してビルドした OData サービスの使用例、しかしクライアント アプリケーションは Web API に依存しない)。


このチュートリアルでは、OData サービスを呼び出すクライアント アプリケーションの作成について詳しく説明します。 OData サービスは、次のエンティティを公開します。

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

次の記事では、Web API の OData サービスを実装する方法について説明します。 (必要はありませんただしこのチュートリアルでは、理解しておくことを読み取れません。)

- [Web API 2 OData エンドポイントを作成します。](creating-an-odata-endpoint.md)
- [Web API 2 OData エンティティ関係](working-with-entity-relations.md)
- [Web API 2 の OData アクション](odata-actions.md)

## <a name="generate-the-service-proxy"></a>サービス プロキシを生成します。

最初の手順では、サービス プロキシを生成します。 サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。 プロキシは、HTTP 要求にメソッド呼び出しを変換します。

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Visual Studio での OData サービス プロジェクトを開くことで開始します。 CTRL + f5 キーを押してサービスを IIS Express でローカルに実行します。 Visual Studio を代入するポート番号を含む、ローカル アドレスに注意してください。 このアドレスは、プロキシを作成するときにする必要があります。

次に、Visual Studio の別のインスタンスを開き、コンソール アプリケーション プロジェクトを作成します。 コンソール アプリケーションは、OData クライアント アプリケーションになります。 (しても、プロジェクトを追加できますサービスと同じソリューションです。)

> [!NOTE]
> 残りの手順には、コンソール プロジェクトが参照してください。


ソリューション エクスプ ローラーで右クリック**参照**選択**サービス参照の追加**です。

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

**サービス参照の追加**ダイアログ ボックスで、OData サービスのアドレスを入力します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

ここで*ポート*のポート番号です。

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

**Namespace**、"ProductService"を入力します。 このオプションは、プロキシ クラスの名前空間を定義します。

**[検索]**をクリックします。 Visual Studio では、サービス内のエンティティを検出する OData メタデータ ドキュメントを読み取ります。

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

をクリックして**OK**プロキシ クラスをプロジェクトに追加します。

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>サービスのプロキシ クラスのインスタンスを作成します。

内部、`Main`メソッド、次のように、プロキシ クラスの新しいインスタンスを作成します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

もう一度、使用して実際のポート番号、サービスが実行されています。 サービスをデプロイするときに、ライブのサービスの URI を使用します。 プロキシを更新する必要はありません。

次のコードでは、コンソール ウィンドウに、要求の Uri を出力するイベント ハンドラーを追加します。 この手順は必須では、興味深いことに、各クエリの Uri。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>サービスをクエリします。

次のコードは、OData サービスから製品の一覧を取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

HTTP 要求を送信または応答を解析するためのコードを記述する必要があることに注意してください。 プロキシ クラスは、この自動的に列挙するとき、`Container.Products`内のコレクション、 **foreach**ループします。

アプリケーションを実行するときに、出力は次のようになります。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

ID でエンティティを取得する、`where`句。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

このトピックの残りの部分の全体を表示しない`Main`関数は、サービスの呼び出しに必要なコードだけです。

## <a name="apply-query-options"></a>クエリ オプションを適用します。

OData 定義[クエリ オプション](../supporting-odata-query-options.md)フィルター、並べ替え、ページ データなどに使用できます。 サービス プロキシでは、さまざまな LINQ 式を使用してこれらのオプションを適用できます。

このセクションでは、簡単な例を示します。 詳細については、トピックを参照してください。 [LINQ に関する留意点 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) msdn です。

### <a name="filtering-filter"></a>フィルター処理 ($filter)

フィルターを使用して、`where`句。 製品カテゴリで次の例をフィルターします。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

このコードは、次の OData クエリに対応します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

プロキシに変換される、 `where` OData に句`$filter`式。

### <a name="sorting-orderby"></a>並べ替え ($orderby)

並べ替えるを使用して、`orderby`句。 次の例は、価格、最上位から最下位から並べ替えます。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

次に、対応する OData 要求を示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>クライアント側のページング ($skip と $top)

大きなエンティティ セットに対して、クライアントで結果の数を制限する必要が場合があります。 たとえば、クライアントは、一度に 10 個のエントリを表示する可能性があります。 これと呼ばれる*クライアント側のページング*です。 (も[サーバー側のページング](../supporting-odata-query-options.md#server-paging)サーバーが結果の数を制限します)。クライアント側のページングを実行するには、LINQ を使用して**Skip**と**かかる**メソッドです。 次の例では、最初の 40 の結果をスキップし、[次へ] の 10 を受け取る。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

次に、対応する OData 要求を示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>($Select) を選択し、[展開] ($展開)

関連エンティティを含めるを使用して、`DataServiceQuery<t>.Expand`メソッドです。 例については、含める、`Supplier`各`Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

次に、対応する OData 要求を示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

応答の形状を変更するには、LINQ を使用**選択**句。 次の例では、その他のプロパティがないと、各製品の名前だけを取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

次に、対応する OData 要求を示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Select 句では、関連エンティティを含めることができます。 その場合は、呼び出す必要はありません**展開**; プロキシの拡張をここで含む自動的にします。 次の例では、各製品の仕入先と名前を取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

次に、対応する OData 要求を示します。 含まれていることを確認、 **$展開**オプション。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

$Select および $expand の詳細については、展開しを参照してください[$ $select を使用して、展開、および Web API 2 で $value](../using-select-expand-and-value.md)です。

## <a name="add-a-new-entity"></a>新しいエンティティを追加します。

エンティティ セットに新しいエンティティを追加するには、呼び出す`AddToEntitySet`ここで、 *EntitySet*エンティティ セットの名前を指定します。 たとえば、`AddToProducts`新しい`Product`を`Products`エンティティ セット。 プロキシを生成するときに WCF Data Services が自動的に作成これら厳密に型指定された**AddTo**メソッドです。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

2 つのエンティティ間のリンクを追加するには、使用、 **AddLink**と**SetLink**メソッドです。 次のコードでは、新しい仕入先と、新しい製品を追加し、それらの間のリンクを作成します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

使用して**AddLink**ナビゲーション プロパティがコレクションの場合。 製品を追加するこの例では、`Products`サプライヤーのコレクション。

使用して**SetLink**ナビゲーション プロパティが 1 つのエンティティの場合。 この例では設定するか、`Supplier`製品のプロパティです。

## <a name="update--patch"></a>更新/パッチ

エンティティを更新するには、呼び出し、 **UpdateObject**メソッドです。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

呼び出すときに、更新プログラムが実行される**SaveChanges**です。 既定では、WCF は、HTTP MERGE 要求を送信します。 **PatchOnUpdate**オプションは、HTTP PATCH を代わりに送信する WCF に指示します。

> [!NOTE]
> なぜマージとパッチを適用しますか。 元の HTTP 1.1 の仕様 ([RCF 2616](http://tools.ietf.org/html/rfc2616))「部分的な更新」のセマンティクスを持つ任意の HTTP メソッドを定義しませんでした。 部分的な更新をサポートするためには、OData の仕様は、マージ メソッドを定義します。 2010 では、 [RFC 5789](http://tools.ietf.org/html/rfc5789)部分的な更新プログラムの更新プログラムの方法を定義します。 この履歴の一部を読み取ることができる[ブログの投稿](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF データ サービス ブログ。 今日では、修正プログラムはマージ経由で推奨されます。 Web API のスキャフォールディングによって作成された OData コント ローラーには、両方の方法がサポートしています。


エンティティ (PUT のセマンティクス) 全体を置換する場合は、指定、 **ReplaceOnUpdate**オプション。 これにより、HTTP PUT 要求を送信する WCF です。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>エンティティを削除します。

エンティティを削除するには、呼び出す**DeleteObject**です。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>OData アクションを呼び出す

OData では、[アクション](odata-actions.md)エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法は、します。

OData メタデータ ドキュメントでは、操作を説明をそれらのプロキシ クラス上で厳密に型指定されたメソッドを一切は作成されません。 ジェネリックを使用して、OData アクションを呼び出すことができますも**Execute**メソッドです。 ただし、パラメーターと戻り値のデータ型を知っておく必要があります。

たとえば、`RateProduct`アクションは、型の「評価」をという名前のパラメーターを受け取る`Int32`を返します、`double`です。 次のコードでは、このアクションを呼び出す方法を示します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

詳細については、次を参照してください。[サービス操作の呼び出しとアクション](https://msdn.microsoft.com/library/hh230677.aspx)です。

1 つのオプションは拡張を**コンテナー**アクションを起動する厳密に型指定されたメソッドを提供するクラス。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
