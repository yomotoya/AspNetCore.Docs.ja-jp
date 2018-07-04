---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: .NET クライアント (c#) から OData サービスを呼び出す |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。 チュートリアルの Visual Studio 2013 (Visual S. 連動.. で使用されているソフトウェア バージョン
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369784"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>.NET クライアント (c#) から OData サービスを呼び出す
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 で動作)
> - [WCF Data Services クライアント ライブラリ](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (この OData サービスの例は Web API 2 を使用して構築していますが、クライアント アプリケーションは Web API に依存しません)。


このチュートリアルで説明します、OData サービスを呼び出すクライアント アプリケーションを作成します。 OData サービスでは、次のエンティティを公開します。

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

次の記事では、Web api OData サービスを実装する方法について説明します。 (ただし、このチュートリアルを理解しておくことを読み取るする必要はありません)。

- [Web API 2 OData エンドポイントの作成](creating-an-odata-endpoint.md)
- [Web API 2 OData エンティティ関係](working-with-entity-relations.md)
- [Web API 2 の OData アクション](odata-actions.md)

## <a name="generate-the-service-proxy"></a>サービス プロキシを生成する

最初の手順では、サービス プロキシを生成します。 サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。 プロキシでは、メソッド呼び出しの HTTP 要求に変換します。

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Visual Studio での OData サービスのプロジェクトを開くことで開始します。 IIS Express でローカルでサービスを実行するには、CTRL + F5 キーを押します。 Visual Studio による割り当てポート番号を含む、ローカル アドレスに注意してください。 このアドレスは、プロキシを作成するときにする必要があります。

次に、Visual Studio の別のインスタンスを開き、コンソール アプリケーション プロジェクトを作成します。 コンソール アプリケーションは、OData クライアント アプリケーションになります。 (追加することできますも、プロジェクト、サービスと同じソリューションにします。)

> [!NOTE]
> 残りの手順は、コンソール プロジェクトを参照してください。


ソリューション エクスプ ローラーで右クリックして**参照**選択**サービス参照の追加**します。

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

**サービス参照の追加**ダイアログ ボックスで、OData サービスのアドレスを入力します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

場所*ポート*のポート番号です。

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

**Namespace**、"ProductService"を入力します。 このオプションは、プロキシ クラスの名前空間を定義します。

**[検索]** をクリックします。 Visual Studio では、サービス内のエンティティを検出する OData メタデータ ドキュメントを読み取ります。

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

クリックして**OK**をプロジェクトにプロキシ クラスを追加します。

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>サービスのプロキシ クラスのインスタンスを作成します。

内で、`Main`メソッドでは、次のように、プロキシ クラスの新しいインスタンスを作成します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

ここでも、サービスが実行されているを実際のポート番号に使用します。 サービスをデプロイするときに、ライブ サービスの URI を使用します。 プロキシを更新する必要はありません。

次のコードは、コンソール ウィンドウへの要求 Uri を出力するイベント ハンドラーを追加します。 この手順は必須では、興味深いことに、各クエリの Uri。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>サービスをクエリします。

次のコードでは、OData サービスから製品の一覧を取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

HTTP 要求を送信または応答を解析するコードを記述する必要があることに注意してください。 プロキシ クラスが列挙するときは自動的にこの、`Container.Products`内のコレクション、 **foreach**ループします。

アプリケーションを実行すると、出力は次のようになります。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

ID でエンティティを取得する、`where`句。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

このトピックの残りの部分全体を見せしません`Main`関数は、サービスの呼び出しに必要なコードだけです。

## <a name="apply-query-options"></a>クエリ オプションを適用します。

OData 定義[クエリ オプション](../supporting-odata-query-options.md)フィルター、並べ替え、ページのデータなどに使用できます。 サービス プロキシには、さまざまな LINQ 式を使用してこれらのオプションを適用できます。

このセクションでは、簡単な例を紹介します。 詳細については、トピックを参照してください。 [LINQ に関する留意点 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) msdn です。

### <a name="filtering-filter"></a>フィルター処理 ($filter)

フィルターを使用して、`where`句。 製品カテゴリで次の例のフィルター。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

このコードは、次の OData クエリに対応します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

プロキシに変換しますが、 `where` OData 句`$filter`式。

### <a name="sorting-orderby"></a>並べ替え ($orderby)

並べ替えるには、使用、`orderby`句。 次の例は、高いものからの価格で並べ替えます。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

対応する OData 要求を次に示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>クライアント側のページング ($skip および $top)

大きなエンティティ セットの場合は、クライアントは、結果の数を制限する可能性があります。 たとえば、クライアントは、一度に 10 個のエントリを表示する場合があります。 これは呼び出されます*クライアント側のページング*します。 (も[サーバー側ページング](../supporting-odata-query-options.md#server-paging)サーバーが結果の数を制限します)。クライアント側のページングを実行するには、LINQ を使用して**スキップ**と**かかる**メソッド。 次の例では、40 の結果をスキップし、[次へ] の 10 を受け取る。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

対応する OData 要求を次に示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>($Select) を選択し、[展開] (展開 $)

関連エンティティを含めるには使用、`DataServiceQuery<t>.Expand`メソッド。 などの`Supplier`各`Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

対応する OData 要求を次に示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

応答の形状を変更するには、LINQ を使用して、**選択**句。 次の例では、その他のプロパティを持たない、各製品の名前だけを取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

対応する OData 要求を次に示します。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Select 句では、関連エンティティを含めることができます。 その場合、呼び出さない**展開**; プロキシで拡張をここでは、自動的にします。 次の例では、各製品の提供元と名前を取得します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

対応する OData 要求を次に示します。 それに含まれる通知、 **$展開**オプション。

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

$Select および $expand の詳細については、展開しを参照してください[$select を使用して、$ の展開、および Web API 2 で $value](../using-select-expand-and-value.md)します。

## <a name="add-a-new-entity"></a>新しいエンティティを追加します。

エンティティ セットには、新しいエンティティを追加するには、呼び出す`AddToEntitySet`ここで、 *EntitySet*エンティティ セットの名前を指定します。 たとえば、`AddToProducts`新しい`Product`を`Products`エンティティ セット。 プロキシを生成するときに WCF Data Services が自動的に作成これら厳密に型指定された**AddTo**メソッド。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

2 つのエンティティ間のリンクを追加するには、使用、 **AddLink**と**SetLink**メソッド。 次のコードでは、新しい仕入先と新しい製品を追加し、し、それらの間のリンクを作成します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

使用**AddLink**ナビゲーション プロパティは、コレクションです。 この例で追加する製品、`Products`サプライヤーのコレクション。

使用**SetLink**ナビゲーション プロパティが 1 つのエンティティ。 この例では設定、`Supplier`製品のプロパティ。

## <a name="update--patch"></a>更新/パッチ

エンティティを更新するには、呼び出し、 **UpdateObject**メソッド。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

呼び出すときに、更新プログラムが実行される**SaveChanges**します。 既定では、WCF は、HTTP MERGE 要求を送信します。 **PatchOnUpdate**オプションを代わりに、HTTP PATCH を送信する WCF に指示します。

> [!NOTE]
> マージと PATCH なぜでしょうか。 元の HTTP 1.1 の仕様 ([RCF 2616](http://tools.ietf.org/html/rfc2616))「部分更新」のセマンティクスを持つ任意の HTTP メソッドを定義しませんでした。 部分的な更新をサポートするためには、OData 仕様には、MERGE メソッドが定義されています。 2010 では、 [RFC 5789](http://tools.ietf.org/html/rfc5789)部分的な更新プログラムの PATCH メソッドを定義します。 この履歴の一部を読み取ることができます[ブログの投稿](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF Data Services のブログにします。 今日では、修正プログラムがマージに適しています。 Web API のスキャフォールディングによって作成された OData コント ローラーには、両方の方法がサポートしています。


エンティティ (PUT セマンティクス) 全体を置換する場合は、指定、 **ReplaceOnUpdate**オプション。 これにより、HTTP PUT 要求を送信する WCF です。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>エンティティを削除します。

エンティティを削除する**DeleteObject**します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>OData アクションを呼び出す

Odata では、[アクション](odata-actions.md)エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。

アクションの説明が、OData メタデータ ドキュメントをそれらのプロキシ クラス上で、厳密に型指定されたメソッドは作成されません。 ジェネリックを使用して OData アクションを呼び出すことができますも**Execute**メソッド。 ただし、パラメーターと戻り値のデータ型を把握する必要があります。

たとえば、`RateProduct`アクションは、型の「評価」という名前のパラメーターを受け取る`Int32`を返します、`double`します。 次のコードでは、この操作を呼び出す方法を示します。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

詳細については、次を参照してください。[呼び出すサービス操作とアクション](https://msdn.microsoft.com/library/hh230677.aspx)します。

1 つのオプションは、拡張する、**コンテナー**処理を実行する厳密に型指定されたメソッドを提供するクラス。

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
