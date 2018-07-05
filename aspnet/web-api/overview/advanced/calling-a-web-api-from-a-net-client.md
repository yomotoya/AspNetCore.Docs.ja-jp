---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: .NET クライアント (c#) から Web API を呼び出す |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 434515692a53c0939652b643b080cea9f2102564
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370919"
---
<a name="call-a-web-api-from-a-net-client-c"></a>.NET クライアント (c#) から Web API を呼び出す
====================
作成者[Mike Wasson](https://github.com/MikeWasson)および[Rick Anderson](https://twitter.com/RickAndMSFT)

[完成したプロジェクトをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)します。 [ダウンロードの方法はこちらをご覧ください。](/aspnet/core/tutorials/#how-to-download-a-sample) 

このチュートリアルでは [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) を使用して .NET アプリケーションから web API を呼び出す方法を説明します。

このチュートリアルでは、次の web API を使用するクライアント アプリケーションについて書かれています。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| ID によって製品を取得します。 | GET | /api/products/*id* |
| 新しい成果物を作成します。 | POST | /api/products |
| 製品を更新します。 | PUT | /api/products/*id* |
| 製品を削除します。 | Del | /api/products/*id* |

ASP.NET Web API を使用して、この API を実装する方法については [CRUD 操作をサポートする Web API の作成](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)を参照してください。

簡潔に示す目的のため、このチュートリアルではクライアント アプリケーションとして Windows コンソール アプリケーションを使用します。 **HttpClient** は Windows Phone や Windows ストア アプリでも同様にサポートされています。 より詳しい情報については [複数のプラットフォームを使用してポータブル ライブラリの Web API クライアント コードの記述。](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx) を参照してください。

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>コンソール アプリケーションを作成します。

Visual Studio で、**HttpClientSample** という名前の新しい Windows コンソール アプリを作成し、次のコードに貼り付けます。


[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

上記のコードでは、完全なクライアント アプリです。

`RunAsync` が実行され、完了するまでブロックされます。 **HttpClient**メソッドはネットワーク I/O として振る舞うため、多くの場合、非同期です。 すべての非同期タスクは `RunAsync` 内で完了します。 通常、アプリは、メイン スレッドをブロックしませんが、このアプリはユーザーとの対話を許可しません。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Web API クライアント ライブラリをインストールします。

NuGet パッケージ マネージャーを使用して、Web API クライアント ライブラリのパッケージをインストールします。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。 パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.Client`

上記のコマンドは、プロジェクトに次の NuGet パッケージを追加します。

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET は、.NET 用の人気のある高パフォーマンス JSON フレームワークです。

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>モデル クラスを追加します。

確認、`Product`クラス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

このクラスでは、web API によって使用されるデータ モデルと一致します。 アプリは **HttpClient** を使用して HTTP レスポンスから `Product` インスタンスを読み取ることができます。 アプリで逆シリアル化コードを記述する必要はありません。

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>作成し、HttpClient の初期化

静的なを調べる**HttpClient**プロパティ。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient**は 1 回インスタンス化するためのものであり、アプリケーションの有効期間にわたって再利用します。 次の条件により**SocketException**エラー。

* 新しいを作成する**HttpClient**要求ごとのインスタンス。
* 負荷の高いサーバー。

新しいを作成する**HttpClient**要求ごとにインスタンスが使用可能なソケットを使い果たしてしまうことができます。

次のコードを初期化します、 **HttpClient**インスタンス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

上のコードでは以下の操作が行われます。

* HTTP 要求のベース URI を設定します。 サーバー アプリで使用されるポートにポート番号を変更します。 サーバー アプリ用にポートを使用しない場合、アプリは動作しません。
* "Application/json"に Accept ヘッダーを設定します。 このヘッダーを設定するには、JSON 形式でデータを送信するサーバーがように指示します。

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>リソースを取得する GET 要求を送信します。

次のコードでは、製品の GET 要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync**メソッドは、HTTP GET 要求を送信します。 ときに、メソッドが完了したら、それを返します、 **HttpResponseMessage** HTTP 応答を格納しています。 応答にステータス コードが成功コードの場合は、応答本文には、製品の JSON 表現が含まれています。 呼び出す**ReadAsAsync** JSON ペイロードを逆シリアル化する、`Product`インスタンス。 **ReadAsAsync**応答本文は、任意の大きさができるため、メソッドは非同期です。

**HttpClient** HTTP 応答には、エラー コードが含まれている場合、例外はスローされません。 代わりに、 **IsSuccessStatusCode**プロパティは**false**状態がエラー コードの場合。 HTTP エラー コードを例外として処理する場合は、呼び出す[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx)応答オブジェクト。 `EnsureSuccessStatusCode` 状態コードが 200 の範囲外になった場合に例外をスローします&ndash;299 します。 なお**HttpClient**の他の理由から例外をスローすることができます&mdash;の例では、要求がタイムアウトします。

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>逆シリアル化するメディア タイプ フォーマッタ

ときに**ReadAsAsync**と呼ばれるは、パラメーターなしで、既定のセットを使用して*メディア フォーマッタ*応答本体を読み取る。 既定のフォーマッタは、JSON、XML、およびフォームの url エンコード データをサポートします。

既定のフォーマッタを使用する代わりに、フォーマッタの一覧を行うことができます、 **ReadAsAsync**メソッド。  フォーマッタの一覧を使用しては、カスタム メディアの種類のフォーマッタがある場合に便利です。

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

詳細については、次を参照してください[ASP.NET Web API 2 のメディア フォーマッタ。](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>リソースを作成する POST 要求を送信します。

次のコードを含む POST 要求を送信する、 `Product` JSON 形式でインスタンス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync**メソッド。

* オブジェクトを JSON にシリアル化します。
* POST 要求では、JSON ペイロードを送信します。

要求が成功するとします。

* 201 (Created) 応答が返されます。
* 応答は、Location ヘッダーで、作成したリソースの URL を含める必要があります。

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>リソースを更新する PUT 要求を送信します。

次のコードでは、製品を更新する PUT 要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync**メソッドのように機能**PostAsJsonAsync**POST の代わりに PUT 要求を送信する点を除いて、します。

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>リソースを削除する DELETE 要求を送信します。

次のコードでは、製品を削除する DELETE 要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

など、GET、DELETE 要求に要求本文はありません。 DELETE で JSON または XML 形式を指定する必要はありません。

## <a name="test-the-sample"></a>サンプルをテストします。

クライアント アプリをテストします。

1. [ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)サーバー アプリを実行します。 [ダウンロードの方法はこちらをご覧ください。](/aspnet/core/tutorials/#how-to-download-a-sample) サーバー アプリが動作を確認します。 Exaxmple の`http://localhost:64195/api/products`製品の一覧を返す必要があります。
2. HTTP 要求のベース URI を設定します。 サーバー アプリで使用されるポートにポート番号を変更します。
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. クライアント アプリを実行します。 次の出力が生成されます。

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
