---
title: "ASP.NET Core でのログ記録"
author: ardalis
description: "ASP.NET Core のログ記録フレームワークが導入されています。 いくつかの一般的なサード パーティ プロバイダーへのリンクと各組み込みのログ プロバイダーのセクションが含まれています。"
keywords: "ASP.NET Core、ログ、ログ プロバイダー、Microsoft.Extensions.Logging ILogger、iloggerfactory を提供、LogLevel、WithFilter、TraceSource、EventLog EventSource、スコープします。"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e5c97ce868e281310aa75c16e73298e2aaa0d9d
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>ASP.NET Core でのログ記録の概要

によって[Steve Smith](http://ardalis.com)と[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core は、さまざまなログ プロバイダーと連携するログ記録 API をサポートします。 組み込みのプロバイダーを使用する 1 つまたは複数の送信先にログを送信してとサード パーティ製のログ記録のフレームワークにプラグインできます。 この記事では、コードで組み込みのログ記録 API とプロバイダーを使用する方法を示します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a>ログを作成する方法

ログを作成するには、取得、`ILogger`オブジェクトから、[依存性の注入](dependency-injection.md)コンテナー。

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

その後そのロガー オブジェクト上ログ メソッドを呼び出します。

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

この例を使用してログを作成、`TodoController`クラスの*カテゴリ*です。  カテゴリが説明されている[この記事で後述](#log-category)です。

ASP.NET Core メソッドは提供しません async ロガーのログ記録は、非同期にかかるコストの価値があることができない高速に動作する必要があります。 True でないそのような状況での場合は、ログに記録する方法の変更を検討してください。  データ ストアが低速の場合は、最初に、高速なストアに、ログ メッセージを記述し、後で、低速なストアに移動します。 たとえば、読み取りおよび別のプロセスによって低速のストレージに保存されるメッセージ キューにログインします。

## <a name="how-to-add-providers"></a>プロバイダーを追加する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

ログ プロバイダーが使用して作成したメッセージを取得、`ILogger`オブジェクトで表示されるかに保存します。 たとえば、コンソール プロバイダーでは、コンソールで、メッセージが表示され、Azure アプリのサービス プロバイダーが Azure blob ストレージに格納できます。

プロバイダーを使用するには、プロバイダーを呼び出す`Add<ProviderName>`でも拡張メソッド*Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

既定のプロジェクト テンプレートが、前のコードに表示する方法のログ記録の設定が、`ConfigureLogging`呼び出しを行う、`CreateDefaultBuilder`メソッドです。 次のコードは*Program.cs*プロジェクト テンプレートによって作成します。

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ログ プロバイダーが使用して作成したメッセージを取得、`ILogger`オブジェクトで表示されるかに保存します。 たとえば、コンソール プロバイダーでは、コンソールで、メッセージが表示され、Azure アプリのサービス プロバイダーが Azure blob ストレージに格納できます。

プロバイダーを使用するその NuGet パッケージをインストールし、インスタンス上のプロバイダーの拡張メソッドを呼び出す`ILoggerFactory`の次の例に示すようにします。

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core[依存性の注入](dependency-injection.md)(DI) の提供、`ILoggerFactory`インスタンス。 `AddConsole`と`AddDebug`で拡張メソッドが定義されている、 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)と[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)パッケージです。 各拡張メソッドを呼び出す、`ILoggerFactory.AddProvider`メソッド、プロバイダーのインスタンスに渡します。 

> [!NOTE]
> この記事のサンプル アプリケーションでのログ プロバイダーの追加、`Configure`のメソッド、`Startup`クラスです。 以前に実行されるコードからログ出力を取得する場合は、追加のログ プロバイダー、`Startup`クラス コンス トラクターを代わりにします。 

---

各に関する情報を参照する[組み込みのログ プロバイダー](#built-in-logging-providers)へのリンク[サード パーティ製のログ記録プロバイダー](#third-party-logging-providers)記事で後述します。

## <a name="sample-logging-output"></a>サンプルのログ出力

サンプル コードは、前のセクションに示すように、コマンドラインから実行すると、コンソールにログが表示されます。 コンソール出力の例を次に示します。

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
これらのログに移動して作成された`http://localhost:5000/api/todo/0`、両方の実行をトリガーする`ILogger`呼び出しの前のセクションに示すようにします。

Visual Studio でサンプル アプリケーションを実行すると、デバッグ ウィンドウに表示されるように、同じログの例を次に示します。

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

によって作成されたログ、 `ILogger` "TodoApi.Controllers.TodoController"で、前のセクションで示した呼び出しを開始します。 カテゴリの"Microsoft"で始まるログから ASP.NET Core されています。 ASP.NET Core 自体と、アプリケーション コードには、同じログ記録 API と同じログ プロバイダーが使用しています。

この記事の残りの部分は、いくつかの詳細とログ記録のオプションについて説明します。

## <a name="nuget-packages"></a>NuGet パッケージ

`ILogger`と`ILoggerFactory`インターフェイスが、 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)、およびそれらの既定の実装を[Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)です。

## <a name="log-category"></a>ログのカテゴリ

A*カテゴリ*を作成する各ログに含まれています。  作成するときに、カテゴリを指定する、`ILogger`オブジェクト。 カテゴリできますが、任意の文字列は、規則は、ログの書き込み元となるクラスの完全修飾名を使用します。  例:"TodoApi.Controllers.TodoController"です。

文字列として指定したり、カテゴリの型から派生する拡張メソッドを使用できます。 カテゴリを文字列として指定するには、呼び出す`CreateLogger`上、`ILoggerFactory`インスタンス、次のようにします。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

ほとんどの場合、なりますを使いやすくする`ILogger<T>`、次の例のようにします。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

これは、呼び出すことと同じ`CreateLogger`の完全修飾型名で`T`です。

## <a name="log-level"></a>ログ レベル

たびに、指定したログを記述すると、その[LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)です。 ログ レベルでは、重大度または重要性の度合いを示します。  たとえば、記述する場合があります、`Information`メソッドは通常、終了時にログに記録、`Warning`メソッドから返された 404 リターン コード、および、ログに記録`Error`予期しない例外をキャッチするときにログに記録します。

次のコード例で、メソッドの名前 (たとえば、 `LogWarning`) ログ レベルを指定します。  最初のパラメーターは、[ログ イベント ID](#log-event-id) (この記事の後半で説明します)。  残りのパラメーターは、ログのメッセージ文字列を構築します。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

メソッド名に、レベルが含まれるログのメソッドは[ILogger の拡張メソッドを](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)です。 背後では、これらのメソッドを呼び出す、`Log`を受け取るメソッド、`LogLevel`パラメーター。 呼び出すことができます、`Log`これらの拡張メソッドが、構文のいずれかではなく、メソッドを直接は比較的複雑になります。 詳細については、次を参照してください。、 [ILogger インターフェイス](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)と[ロガーの拡張機能のソース コード](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)です。

ASP.NET Core は、次を定義[ログ レベル](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)、ここに最高の重大度が最もから注文します。

* トレース = 0

  開発者にのみ重要な情報の問題をデバッグします。 これらのメッセージは機密性の高いアプリケーションのデータを含む可能性およびようにする必要がありますいないで有効にする、実稼働環境。 *既定では無効です。* 例 : `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* デバッグ = 1

  については、開発およびデバッグ中に短期的な実用性を持ちます。 例:`Entering method Configure with flag set to true.`通常は有効にしない`Debug`大量のログのため、トラブルシューティングする場合を除き、実稼働環境でレベルをログに記録します。

* 情報 = 2

  アプリケーションの一般的なフローを追跡します。 これらのログは、通常、いくつかの長期的な値を持ちます。 例 : `Request received for path /api/todo`

* 警告 3 を =

  アプリケーションのフローで異常なまたは予期しないイベントのします。 エラーまたは停止するには、アプリケーションが発生しないことが調査する必要がありますが、その他の条件が含まれます。 処理済みの例外は、共通の場所を使用する、`Warning`ログ レベルです。 例 : `FileNotFoundException for file quotes.txt.`

* エラー = 4

  エラーと例外を処理できません。 これらのメッセージは、現在のアクティビティまたは (現在の HTTP 要求) などの操作のエラー、アプリケーション全体にわたる障害ではありませんを示します。 ログのメッセージの例:`Cannot insert record due to duplicate key violation.`

* 重要な 5 を =

  エラーは、早急に措置を必要とします。 例: データ損失シナリオ、ディスク領域が不足しています。

ログ レベルを使用して、ログ出力の量が特定の記憶域メディアに書き込まれる制御またはウィンドウを表示することができます。 たとえば、実稼働環境でことができますのすべてのログ`Information`レベルとボリュームのデータ ストアとのすべてのログに移動して`Warning`レベルと値のデータ ストアに移動するより高い。 開発中はのログを送信することが通常`Warning`またはコンソールに重大度が高いです。 追加することができますをトラブルシューティングする必要がある場合、`Debug`レベル。 [ログ フィルター](#log-filtering)この記事の後半で、プロバイダーを処理するログ レベルを制御する方法について説明します。

ASP.NET Core framework 書き込みます`Debug`framework のイベント ログのレベルです。 ログの例は、前の「除外以下のログ`Information`レベル、ため`Debug`レベル ログが表示されていました。 サンプル アプリケーションを表示するように構成を実行する場合に、コンソール ログの例を次に示します`Debug`とコンソールのプロバイダーの高いログ。

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>ログのイベント ID

たびにログを記述することができますを指定する、*イベント ID*です。 サンプル アプリはローカルに定義を使用することで`LoggingEvents`クラス。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

イベント ID は、ログに記録されたイベントのセットを関連付ける互いに使用できる整数値です。 たとえば、ショッピング カートにアイテムを追加するためのログはイベント ID 1000 と購入を完了するためのログはイベント ID 1001 である可能性があります。

ログ出力でイベント ID フィールドに格納されているまたはプロバイダーによって、テキスト メッセージに含まれます。  デバッグ プロバイダーは、イベント Id を表示しませんが、コンソールのプロバイダーを示す角かっこでカテゴリ後。

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>ログ メッセージの書式指定文字列

テキスト メッセージを提供するたびに、ログを記述します。 メッセージ文字列は、どちらの引数に値を配置している次の例のように、名前付きプレース ホルダーを含めることができます。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

その名前のプレース ホルダーの順序は、どのパラメーターはそれらの使用を決定します。 たとえば、次のようなコードがあるとします。

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

結果のログ メッセージは、次のようになります。

```
Parameter values: parm1, parm2
```

ログ フレームワークはメッセージのログ プロバイダーを実装するを可能にするには、この方法で書式設定[セマンティック ログ記録、構造化されたログ記録とも呼ばれる](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)です。 引数そのものが形式のメッセージ文字列だけでなく、ログ システムに渡されるために、ログ プロバイダーは、メッセージ文字列だけでなくフィールドとしてパラメーターの値を格納できます。 たとえば、誘導している場合、ログが Azure テーブル ストレージに出力し、ロガー メソッドの呼び出しが次のよう。

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

各 Azure テーブル エンティティがある可能性があります`ID`と`RequestTime`プロパティで、ログ データにクエリが簡素化します。 特定の内のすべてのログを検索できます`RequestTime`範囲、テキスト メッセージのタイムアウトを解析する必要はありません。

## <a name="logging-exceptions"></a>ログの例外

ロガー メソッドでは、次の例のように、例外を渡すことのできるオーバー ロードがあります。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

異なるプロバイダーでは、さまざまな方法で、例外情報を処理します。 上記のコードからのデバッグ、プロバイダーの出力の例を次に示します。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>ログのフィルター選択

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

特定のプロバイダーとカテゴリまたはすべてのプロバイダーまたはすべてのカテゴリでは、最小のログ レベルを指定することができます。  最小レベルの下の任意のログはいない表示または保存されている取得しないように、そのプロバイダーに渡されます。 

すべてのログを抑制するかどうかを指定できます`LogLevel.None`最小のログ レベルとします。 整数値`LogLevel.None`6、これはより高い`LogLevel.Critical`(5)。

**構成でのフィルタの規則を作成します。**

呼び出すコードを作成するプロジェクト テンプレート`CreateDefaultBuilder`コンソールとデバッグのプロバイダーのログ記録を設定します。 `CreateDefaultBuilder`メソッドもログ記録を構成で検索する設定、`Logging`セクションで、次のようにコードを使用します。

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

構成データは、プロバイダーと次の例のように、カテゴリによって最小のログ レベルを指定します。

[!code-json[](logging/sample2/AppSettings.json)]

この JSON では、デバッグ プロバイダーの 1 つ、4 つのコンソール プロバイダーとすべてのプロバイダーに適用される 1 つ、6 つのフィルター規則を作成します。 これらの規則を 1 つだけかは、後ほどが各プロバイダーの選択が表示されるときに、`ILogger`オブジェクトを作成します。

**コードでのフィルタの規則**

次の例で示すように、コードでは、フィルタの規則を登録できます。

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

2 番目`AddFilter`型の名前を使用して、デバッグ プロバイダーを指定します。 最初の`AddFilter`プロバイダーの種類を指定されていないために、すべてのプロバイダーに適用されます。

**フィルターの規則が適用されます。**

構成データと`AddFilter`前の例に示すコードは、次の表に示すようにルールを作成します。 最初の 6 個が構成の例から取り出され、最後の 2 つのコード例に由来します。

数値|プロバイダー|始まるカテゴリ|最小のログ レベル|
------|--------|--------------------------|-----------------|
1|デバッグ|すべてのカテゴリ|情報|
2|コンソール|Microsoft.AspNetCore.Mvc.Razor.Internal|警告|
3|コンソール|Microsoft.AspNetCore.Mvc.Razor.Razor|デバッグ|
4|コンソール|Microsoft.AspNetCore.Mvc.Razor|エラー|
5|コンソール|すべてのカテゴリ|情報|
6|すべてのプロバイダー|すべてのカテゴリ|警告
7|すべてのプロバイダー|システム|デバッグ
8|デバッグ|Microsoft|トレース

作成するときに、`ILogger`を使用してログに書き込むオブジェクト、`ILoggerFactory`オブジェクトはそのロガーに適用するプロバイダーごとに 1 つの規則を選択します。 によって書き込まれたすべてのメッセージ`ILogger`オブジェクトは、選択したルールに基づいてフィルター処理します。 プロバイダーとカテゴリのペアごとに可能な最も固有のルールは、利用可能な規則から選択されます。

プロバイダーごとに、次のアルゴリズムが使用されるときに、`ILogger`が 1 つのカテゴリの作成します。

* プロバイダーまたはそのエイリアスと一致するすべてのルールを選択します。  、何も見つからない場合は、空のプロバイダーとすべてのルールを選択します。
* 前の手順の結果からは、時間が最長を含む選択ルールは、カテゴリ プレフィックスを照合します。 何も見つからない場合は、カテゴリを指定しないすべてのルールを選択します。
* 複数の規則が選択されている場合、**最後**いずれか。
* ルールが選択されていない場合に使用して`MinimumLevel`です。
 
たとえば、上記の規則のリストがあり、作成する、`ILogger`カテゴリ"Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"のオブジェクト。

* デバッグ プロバイダーの場合は、1、6、および 8 規則が適用されます。 ルール 8 は、選択したものになるように最も固有です。
* コンソール プロバイダーの場合は、3、4、5、6 および規則が適用されます。 規則 3 は、最も固有です。

使用してログを作成する場合、 `ILogger` "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"のカテゴリのログ`Trace`レベルし、上記のログとデバッグ プロバイダーに移動する`Debug`レベルし、以降は、コンソールのプロバイダーに移動します。

**プロバイダーのエイリアス**

構成では、プロバイダーを指定して、型名を使用することができますが、各プロバイダー定義、短い*エイリアス*を使いやすくすることができます。 組み込みのプロバイダーでは、次のエイリアスを使用します。

- コンソール
- デバッグ
- イベント ログ
- AzureAppServices
- TraceSource
- EventSource

**既定の最小レベル**

指定されたプロバイダーおよびカテゴリの構成またはコードからのルールが適用されない場合にのみ有効になりますの最小レベルの設定があります。 次の例では、最低レベルを設定する方法を示します。

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

最小レベルを明示的に設定しない場合、既定値は`Information`、つまり`Trace`と`Debug`ログは無視されます。

**フィルター関数**

フィルタ リング規則を適用するフィルター関数では、コードを記述することができます。 フィルター関数は、すべてのプロバイダーおよび構成またはコードによって、それらに割り当てられている規則がないカテゴリに対して呼び出されます。 関数のコードでは、メッセージをログに記録するかどうかを決定するには、プロバイダーの種類、カテゴリ、およびログ レベルへのアクセスを持っています。 例:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

一部のログ プロバイダーを指定できますログをストレージ メディアに書き込まれるまたは無視する場合のログ レベルとカテゴリに基づいています。

`AddConsole`と`AddDebug`拡張メソッドは、フィルター条件に渡すことが許可されるオーバー ロードを提供します。 次のサンプル コードの原因を以下のログを無視するコンソール プロバイダー`Warning`レベル中、デバッグのプロバイダー フレームワークによって作成されるログは無視されます。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog`メソッドを受け取るオーバー ロードには、`EventLogSettings`でフィルター処理関数を含むことができるインスタンスは、その`Filter`プロパティです。 ログ記録レベルとその他のパラメーターに基づいているため TraceSource プロバイダーがこれらのオーバー ロードのいずれかを指定できません、`SourceSwitch`と`TraceListener`を使用します。

すべてのプロバイダーに登録されているフィルタ リング規則を設定することができます、`ILoggerFactory`インスタンスを使用して、`WithFilter`拡張メソッド。 次の例は、デバッグ レベルでのアプリケーション ログをながら framework ログ (カテゴリは、"Microsoft"または"System"で始まる) の警告を制限します。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

フィルター処理をすべてのログが特定のカテゴリ用に記述されていることを防ぐために使用するかどうかを指定できます`LogLevel.None`そのカテゴリの最小のログ レベルとします。 整数値`LogLevel.None`6、これはより高い`LogLevel.Critical`(5)。

`WithFilter`によって拡張メソッドが提供される、 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージです。 メソッドは、新しい返します`ILoggerFactory`ことに登録されているすべてのログ プロバイダーに渡されたログ メッセージがフィルター処理するインスタンス。 影響しません、他の`ILoggerFactory`元を含むインスタンス`ILoggerFactory`インスタンス。

---

## <a name="log-scopes"></a>ログのスコープ

内の論理操作のセットをグループ化することができます、*スコープ*そのセットの一部として作成される各ログに、同じデータをアタッチするためにします。  たとえば、トランザクション ID は、トランザクションの処理の一部として作成されたすべてのログをする可能性があります。

スコープは、`IDisposable`によって返される型、`ILogger.BeginScope<TState>`メソッドおよびが破棄されるまで継続します。 スコープを使用して、ロガーの呼び出しでラップして、`using`ブロック、次のように。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

次のコードは、コンソールのプロバイダーのスコープを有効にします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

*Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

各ログ メッセージには、スコープ対象の情報が含まれています。

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>組み込みのログ プロバイダー

ASP.NET Core には、次のプロバイダーが付属します。

* [コンソール](#console)
* [デバッグ](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>コンソール プロバイダー

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)プロバイダーのパッケージは、コンソールにログ出力を送信します。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)を渡すことができます、最小のログ レベル、フィルター関数の場合、およびスコープがサポートされているかどうかを示すブール値。  渡すこともできます、`IConfiguration`オブジェクトで、スコープのサポートとログ記録レベルを指定できます。 

実稼働環境で使用するためのコンソール プロバイダーを検討している場合のパフォーマンスに大きな影響があることに注意してください。

Visual Studio で、新しいプロジェクトを作成するときに、`AddConsole`メソッドは、次のようになります。

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

このコードを参照して、`Logging`のセクションで、*される appSettings.json*ファイル。

[!code-json[](logging/sample//appsettings.json)]

説明するようデバッグ レベルでログに記録する一方で、アプリ制限 framework ログ警告が表示設定、[ログ フィルター](#log-filtering)セクションです。 詳細については、次を参照してください。[構成](configuration.md)です。

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>デバッグ プロバイダー

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)プロバイダーのパッケージを使用してログの出力を書き込みます、 [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug)クラス (`Debug.WriteLine`メソッドの呼び出し)。

Linux では、このプロバイダーはログを書き込む*/var/log/message*です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)最小のログ レベル、またはフィルター関数に渡すことができます。

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource プロバイダー

ASP.NET Core 1.1.0 を対象とするアプリの高い、または、 [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)プロバイダーのパッケージは、イベントのトレースを実装できます。 Windows では、使用して[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)です。 プロバイダーは、クロスプラット フォームが存在しないイベント for Linux または macOS まだ収集および表示するツールです。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

収集し、ログを表示することをお勧めは使用する、 [PerfView ユーティリティ](https://www.microsoft.com/download/details.aspx?id=28567)です。 ETW ログを表示するには、その他のツールがありますが、PerfView は、ASP.NET によって出力される ETW イベントを操作するための最適なエクスペリエンスを提供します。 

このプロバイダーによって記録されたイベントを収集するため PerfView を構成するには、文字列を追加`*Microsoft-Extensions-Logging`を**追加のプロバイダー**  ボックスの一覧です。 (見逃さない文字列の先頭にアスタリスク。)

![Perfview プロバイダーの追加](logging/_static/perfview-additional-providers.png)

Nano Server でのイベントのキャプチャには、いくつか追加の設定が必要です。

* PowerShell リモート処理を Nano Server に接続します。

  ```powershell
  Enter-PSSession [name]
  ```

* ETW セッションを作成します。

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* ETW プロバイダーを追加[CLR](https://msdn.microsoft.com/library/ff357718)、ASP.NET Core および必要に応じて他のユーザーです。 GUID は、ASP.NET Core プロバイダー`3ac73b97-af73-50e9-0822-5da4367920d0`です。 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* サイトを実行しのトレース情報が必要な任意のアクションを実行します。

* 完了したらトレース セッションを停止します。

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

その結果、 *C:\trace.etl*ファイルは、Windows の他のエディションで、PerfView で分析できます。

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows イベント ログ プロバイダー

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)プロバイダーのパッケージは、Windows イベント ログにログ出力を送信します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)を渡すように`EventLogSettings`または最小のログ レベルです。

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource プロバイダー

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource)プロバイダーのパッケージを使用して、 [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx)ライブラリとプロバイダー。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)ソース スイッチと、トレース リスナーを渡すことができます。

このプロバイダーを使用するのには、アプリケーションは、.NET Framework (ではなく .NET Core) で実行するがします。 このプロバイダーでのさまざまなにメッセージをルーティングする[リスナー](https://msdn.microsoft.com/library/4y5y10s7)など、 [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener)サンプル アプリケーションで使用します。

次の例では、構成、`TraceSource`ログに記録するプロバイダー`Warning`とコンソール ウィンドウに高いメッセージです。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure App Service provider

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices)プロバイダーのパッケージは、Azure App Service アプリのファイル システム内とテキスト ファイルにログを書き込みます[blob ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)Azure ストレージ アカウントにします。 プロバイダーは、ASP.NET Core 1.1.0 を対象とするアプリに対してのみ使用可能な以上です。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

> [!NOTE]
> ASP.NET Core 2.0 はプレビュー段階です。  Azure App Service に配置されたときに、最新のプレビュー リリースで作成されたアプリは実行されない可能性があります。 Azure App Service が 2.0 を実行する ASP.NET Core 2.0 が解放されると、アプリ、および Azure App Service をここに記載されているプロバイダーは機能します。

プロバイダーのパッケージまたは呼び出しをインストールする必要はありません、`AddAzureWebAppDiagnostics`拡張メソッド。  プロバイダーは、Azure App Service にアプリを展開するとき、アプリを自動的に利用可能です。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics`を渡すことができます、オーバー ロードの[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)、使用するには、ログ出力のテンプレート、blob の名前、およびファイル サイズの制限などの既定の設定をオーバーライドできます。 (*出力テンプレート*メッセージの書式指定文字列を呼び出すときに提供する 1 つ上のすべてのログに適用されるは、`ILogger`メソッドです)。  

---

アプリケーションでの設定は、App Service アプリを展開するときに、[診断ログ](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)のセクションで、 **App Service** Azure ポータルのページです。 これらの設定を変更するときに変更を有効すぐにアプリを再起動するか、コードを再展開を必要とせずします。 

![Azure のログ記録の設定](logging/_static/azure-logging-settings.png)

ログ ファイルの既定の場所は、 *d:\\ホーム\\LogFiles\\アプリケーション*フォルダー、および既定のファイル名は*診断 yyyymmdd.txt*です。 既定のファイル サイズの制限は 10 MB であり、保持ファイルの既定の最大数は 2 です。 既定の blob 名は*{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*です。 既定の動作の詳細については、次を参照してください。 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)です。

プロバイダーは、プロジェクトは、Azure 環境で実行するときにのみ機能します。  影響を与えませんローカルで実行するときに&mdash;ローカル ファイルまたは blob のローカル開発ストレージに書き込まれません。

## <a name="third-party-logging-providers"></a>サード パーティ製のログ プロバイダー

ASP.NET Core で動作する一部のサード パーティ製のログ記録フレームワークを次に示します。

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Elmah.Io サービス プロバイダー

* [JSNLog](http://jsnlog.com) -、サーバー側のログでの JavaScript 例外およびその他のクライアント側のイベント ログに記録します。

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Loggr サービス プロバイダー

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -NLog ライブラリのプロバイダー

* [Serilog](https://github.com/serilog/serilog-framework-logging) -Serilog ライブラリのプロバイダー

一部のサード パーティ製のフレームワークが実行できる[セマンティック ログ記録、構造化されたログ記録とも呼ばれる](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)です。

組み込みのプロバイダーのいずれかを使用してに似ていますが、サード パーティ製のフレームワークを使用します。 NuGet パッケージをプロジェクトに追加し、上の拡張メソッドを呼び出す`ILoggerFactory`です。 詳細については、各 framework のドキュメントを参照してください。

独自のカスタム プロバイダーを同様は、他のログ記録のフレームワーク、または独自のログ記録の要件をサポートするために作成できます。
