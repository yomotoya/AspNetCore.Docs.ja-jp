---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: ".NET クライアント (c#) から Web API を呼び出す |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 44e02888b53ee372ab93db5f90acb691f26b7519
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a>.NET クライアント (c#) から Web API を呼び出す
====================
作成者 [Mike Wasson](https://github.com/MikeWasson) および [Rick Anderson](https://twitter.com/RickAndMSFT)

[完成したプロジェクトのダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

このチュートリアルでは [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) を使用して .NET アプリケーションから web API を呼び出す方法を説明します。

このチュートリアルでは、次の web API を使用するクライアント アプリケーションについて書かれています。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| ID によって製品を取得します。 | GET | /api/products/*id* |
| 新しい製品を作成します。 | POST | /api/products |
| 製品を更新します。 | PUT | /api/products/*id* |
| 製品を削除します。 | Del | /api/products/*id* |

ASP.NET Web API を使用して、この API を実装する方法については [CRUD 操作をサポートする Web API を作成する](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
) を参照してください。

説明をシンプルに保つ目的から、このチュートリアルでは Windows コンソール アプリケーションをクライアント アプリケーションとして使用します。 **HttpClient** は Windows Phone や Windows ストア アプリでも同様にサポートされています。 詳しい情報は「[Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)」(ポータブル ライブラリを使用して複数のプラットフォームの Web API クライアント コードを記述する) を参照してください。

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>コンソール アプリケーションを作成する

Visual Studio で **HttpClientSample** という名前の新しい Windows コンソール アプリを作成し、次のコードに貼り付けます:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

上記のコードは、完全なクライアント アプリケーションです。

`RunAsync` が起動し、完了するまでブロックされます。 **HttpClient** の多くのメソッドはネットワーク I/O として振る舞うため、非同期です。すべての非同期タスクは `RunAsync` の内部で完結します。 通常、アプリは、メイン スレッドをブロックしませんが、このアプリはユーザーとの対話を許可しません。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Web API のクライアント ライブラリをインストールする

NuGet パッケージ マネージャーを使用して、Web API Client Libraries パッケージをインストールします。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。 パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.Client`

上記のコマンドは、プロジェクトに次の NuGet パッケージを追加します。

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET は、.NET の人気のある高パフォーマンス JSON フレームワークです。

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>モデル クラスを追加します。

確認、`Product`クラス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

このクラスでは、web API によって使用されるデータ モデルと一致します。 アプリが使用できる**HttpClient**を読み取る、 `Product` HTTP 応答からインスタンス。 逆シリアル化コードを記述する、アプリはありません。

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>作成および HttpClient の初期化

静的なを調べて**HttpClient**プロパティ。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient**は 1 回インスタンス化するためのものであり、アプリケーションの有効期間全体で再利用します。 次の条件が**SocketException**エラー。

* 新たに作成する**HttpClient**要求ごとのインスタンス。
* 負荷の高いサーバーです。

新たに作成する**HttpClient**要求ごとにインスタンスが使用可能な sockets を使い果たしてしまうことができます。

次のコードを初期化します、 **HttpClient**インスタンス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

上のコードでは以下の操作が行われます。

* HTTP 要求のベース URI を設定します。 サーバー アプリケーションで使用されるポートにポート番号を変更します。 サーバー アプリケーション用のポートを使用しない場合、アプリは機能しません。
* "Application/json"に Accept ヘッダーを設定します。 このヘッダーを設定するには、JSON 形式でデータを送信するサーバーがように指示します。

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>リソースを取得する GET 要求を送信します。

次のコードは、製品の GET 要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**されます**メソッドは、HTTP GET 要求を送信します。 完了時のメソッドを返します、 **HttpResponseMessage** HTTP 応答を格納しています。 応答にステータス コードが成功コードの場合は、応答本文には、製品の JSON 表現が含まれています。 呼び出す**ReadAsAsync** JSON ペイロードを逆シリアル化する、`Product`インスタンス。 **ReadAsAsync**メソッドでは非同期応答の本体は任意の大きさを指定できます。

**HttpClient** HTTP 応答には、エラー コードが含まれている場合、例外はスローされません。 代わりに、 **IsSuccessStatusCode**プロパティは**false**状態がエラー コードの場合。 HTTP エラー コードを例外として処理する場合は、呼び出す[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) response オブジェクトにします。 `EnsureSuccessStatusCode`ステータス コードが 200 の範囲外になった場合に例外をスロー&ndash;299 です。 注意してください**HttpClient**の他の理由から例外をスローできます&mdash;要求がタイムアウトになる場合などです。

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>逆シリアル化するメディア タイプ フォーマッタ

ときに**ReadAsAsync**が呼び出されたの既定のセットを使用して、パラメーターなしで*メディア フォーマッタ*応答本体を読み取る。 既定のフォーマッタは、JSON、XML、およびフォームの url エンコード データをサポートします。

既定のフォーマッタを使用する代わりに、フォーマッタの一覧を指定することができます、 **ReadAsAsync**メソッドです。  フォーマッタのリストを使用して、カスタムのメディアの種類のフォーマッタがある場合に便利です。

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

詳細については、次を参照してください[ASP.NET Web API 2 に、メディア フォーマッタ。](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>リソースを作成する POST 要求を送信します。

次のコードを含む POST 要求を送信する、 `Product` JSON 形式でインスタンス。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync**メソッド。

* オブジェクトを JSON にシリアル化します。
* POST 要求で JSON ペイロードを送信します。

要求が成功するとします。

* 201 (Created) 応答が返されます。
* 応答は、Location ヘッダーの作成済みのリソースの URL を含める必要があります。

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>リソースを更新する PUT 要求を送信します。

次のコードでは、製品を更新する PUT 要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync**のような方法は、機能**PostAsJsonAsync**POST の代わりに PUT 要求を送信する点を除いて、します。

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>リソースを削除する削除要求を送信します。

次のコードは、製品を削除する削除要求を送信します。

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

GET と同様に DELETE 要求しても、要求本文はありません。 DELETE で JSON または XML の形式を指定する必要はありません。

## <a name="test-the-sample"></a>このサンプルをテストします。

クライアント アプリをテストします。

1. [ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)server アプリを実行します。 [ダウンロード命令の](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample)します。 サーバー アプリが動作を確認します。 Exaxmple の`http://localhost:64195/api/products`製品の一覧を返す必要があります。
2. HTTP 要求のベース URI を設定します。 サーバー アプリケーションで使用されるポートにポート番号を変更します。
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. クライアント アプリを実行します。 次の出力が生成されます。

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
