---
title: "ASP.NET Core でのログ記録"
author: ardalis
description: "ASP.NET Core でのログ記録フレームワークについて説明します。 組み込みのログ プロバイダーと、サードパーティ製の一般的なプロバイダーについて説明します。"
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: c8152b94311acb672e9810828b634c744cb46eae
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a>ASP.NET Core でのログ記録の概要

執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。 組み込みのプロバイダーを使用すると、1 つ以上の宛先にログを送信できます。また、サードパーティ製のログ記録フレームワークをプラグインとして使用できます。 この記事では、組み込みのログ記録 API とプロバイダーをコードで使用する方法について説明します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

---

## <a name="how-to-create-logs"></a>ログを作成する方法

ログを作成するには、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーから `ILogger` オブジェクトを取得します。

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

次に、そのロガー オブジェクトに対してログ記録メソッドを呼び出します。

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

この例では、*カテゴリ*として `TodoController` クラスを使用してログを作成しています。 カテゴリについては、[この記事で後ほど](#log-category)説明します。

ログ記録はすばやく行う必要があり、非同期を使用するコストに見合わないため、ASP.NET Core には非同期のロガー メソッドが用意されていません。 非同期のロガー メソッドが必要な場合は、ログ記録の方法を変更することをお勧めします。 データ ストアが低速な場合は、まずログ メッセージを高速なストアに書き込んでから、後で低速なストアに移動します。 たとえば、メッセージ キューにログを記録し、別のプロセスで読み取り、低速なストレージに保存します。

## <a name="how-to-add-providers"></a>プロバイダーを追加する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。 たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。

プロバイダーを使用するには、*Program.cs* でプロバイダーの `Add<ProviderName>` 拡張メソッドを呼び出します。

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

既定のプロジェクト テンプレートを使用すると、[CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) メソッドを使用したログ記録が有効になります。

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。 たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。

プロバイダーを使用するには、NuGet パッケージをインストールし、次の例のように `ILoggerFactory` のインスタンスに対してプロバイダーの拡張メソッドを呼び出します。

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core の[依存関係の挿入](xref:fundamentals/dependency-injection) (DI) には、`ILoggerFactory` インスタンスがあります。 `AddConsole` および `AddDebug` 拡張メソッドは、[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) パッケージと [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) パッケージで定義されています。 各拡張メソッドで `ILoggerFactory.AddProvider` メソッドを呼び出し、プロバイダーのインスタンスで渡します。 

> [!NOTE]
> この記事のサンプル アプリケーションでは、`Startup` クラスの `Configure` メソッドでログ プロバイダーを追加します。 前の手順で実行したコードのログ出力を取得するには、代わりに `Startup` クラス コンストラクターにログ プロバイダーを追加します。 

---

各[組み込みログ プロバイダー](#built-in-logging-providers)と[サードパーティ製ログ プロバイダー](#third-party-logging-providers)のリンクについては、この記事で後ほど説明します。

## <a name="sample-logging-output"></a>サンプルのログ記録の出力

前のセクションで紹介したサンプル コードをコマンド ラインから実行すると、コンソールにログが表示されます。 コンソールの出力例は次のとおりです。

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
 
`http://localhost:5000/api/todo/0` にアクセスし、前のセクションで紹介した `ILogger` の呼び出しの実行が両方トリガーされることで、これらのログは作成されました。

Visual Studio でサンプル アプリケーションを実行すると [デバッグ] ウィンドウに表示されるログと同じログの例を次に示します。

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

前のセクションで紹介した `ILogger` の呼び出しで作成されるログは、"TodoApi.Controllers.TodoController" から始まります。 "Microsoft" カテゴリから始まるログは、ASP.NET Core のログです。 ASP.NET Core 自体とアプリケーション コードは、同じログ記録 API と同じログ プロバイダーを使用しています。

以降、この記事では、ログ記録の詳細とオプションについて説明します。

## <a name="nuget-packages"></a>NuGet パッケージ

`ILogger` および `ILoggerFactory` インターフェイスは、[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 内にあり、それらの既定の実装は [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) 内にあります。

## <a name="log-category"></a>ログのカテゴリ

作成する各ログには、*カテゴリ*が含まれています。 `ILogger` オブジェクトを作成するときにカテゴリを指定します。 カテゴリには任意の文字列を指定できますが、ログを書き込むクラスの完全修飾名を使用することが規則です。 たとえば、"TodoApi.Controllers.TodoController" と指定します。

文字列としてカテゴリを指定するか、型からカテゴリを派生させる拡張メソッドを使用することができます。 文字列としてカテゴリを指定するには、次のように `ILoggerFactory` インスタンスに対して `CreateLogger` を呼び出します。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

ほとんどの場合、次の例のように `ILogger<T>` を使用する方が簡単です。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

これは、`T` の完全修飾型名を使用した `CreateLogger` の呼び出しと同じです。

## <a name="log-level"></a>ログ レベル

ログを書き込むたびに、[LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel) を指定します。 ログ レベルは、重大度または重要度を示します。 たとえば、メソッドが通常どおりに終了したときには `Information` ログ、メソッドから 404 リターン コードが返されたときには `Warning` ログ、予期しない例外をキャッチしたときには `Error` ログを書き込むことができます。

次のコード例では、メソッドの名前 (たとえば `LogWarning`) でログ レベルを指定します。 最初のパラメーターは[ログ イベント ID](#log-event-id) です。 2 つ目のパラメーターは、他のメソッド パラメーターに提供される引数値のプレースホルダーを含む[メッセージ テンプレート](#log-message-template)です。 メソッド パラメーターの詳細については、この記事で後ほど説明します。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

メソッド名にレベルを含むログ メソッドは、[ILogger の拡張メソッド](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)です。 これらのメソッドは、背後で `LogLevel` パラメーターを受け取る `Log` メソッドを呼び出します。 これらの拡張メソッドのいずれかではなく、`Log` メソッドを直接呼び出すことができますが、構文は比較的複雑です。 詳細については、[ILogger インターフェイス](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)と[ロガー拡張ソース コード](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)に関するページを参照してください。

ASP.NET Core には、次の[ログ レベル](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)が定義されています (重大度の低いレベルから高いレベルの順)。

* Trace = 0

  問題をデバッグする開発者にのみ重要な情報の場合。 これらのメッセージには機密性の高いアプリケーション データが含まれる可能性があるため、運用環境は有効にしないことをお勧めします。 *既定で無効です。* 例 : `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debug = 1

  開発およびデバッグ中に短期的に実用性がある情報の場合。 例: `Entering method Configure with flag set to true.` `Debug` レベルのログはログのサイズが大きくなるため、通常、トラブルシューティングのためではない限り、運用環境では有効にしません。

* Information = 2

  アプリケーションの一般的なフローを追跡する場合。 通常、これらのログには、長期的な値があります。 例 : `Request received for path /api/todo`

* Warning = 3

  アプリケーション フローの異常なイベントまたは予期しないイベントの場合。 たとえば、アプリケーションは停止しなくても、調査が必要な可能性があるエラーや他の条件が含まれます。 `Warning` ログ レベルが使用される一般的な場所として、例外の処理があります。 例 : `FileNotFoundException for file quotes.txt.`

* Error = 4

  処理できないエラーと例外の場合。 これらのメッセージは、アプリケーション全体のエラーではなく、現在のアクティビティまたは操作 (現在の HTTP 要求など) のエラーを示します。 ログ メッセージの例: `Cannot insert record due to duplicate key violation.`

* Critical = 5

  即時の注意が必要なエラーの場合。 例: データ損失のシナリオ、ディスク領域不足。

ログ レベルを使用して、特定のストレージ メディアまたは表示ウィンドウに書き込むログの出力量を制御できます。 たとえば、運用環境で、`Information` レベル以下のすべてのログをボリューム データ ストアに、`Warning` 以上のすべてのログを値のデータ ストアに書き込むことができます。 開発中は、通常、`Warning` 以上の重大度のログをコンソールに送信することがあります。 トラブルシューティングが必要になったら、`Debug` レベルを追加できます。 この記事で後述する「[ログのフィルター処理](#log-filtering)」セクションでは、プロバイダーで処理するログ レベルの制御方法について説明します。

ASP.NET Core フレームワークは、フレームワーク イベントに対して `Debug` ログ レベルを書き込みます。 この記事で前述したログの例では、`Information` レベル以下のログを除外したため、`Debug` ログ レベルは表示されていません。 Console プロバイダーに関する `Debug` 以上のログを表示するように構成されたサンプル アプリケーションを実行した場合のコンソール ログ例を次に示します。

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

## <a name="log-event-id"></a>ログ イベント ID

ログを書き込むたびに、*イベント ID* を指定します。 サンプル アプリでは、この処理にローカルで定義された `LoggingEvents` クラスを使用します。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

イベント ID は、ログに記録された一連のイベントを関連付けるために使用できる整数値です。 たとえば、項目をショッピング カートに追加する処理のログをイベント ID 1000 にしたり、購入を完了する処理のログをイベント ID 1001 にしたりすることができます。

ログ記録の出力のイベント ID は、プロバイダーに応じてフィールドに保存されるか、テキスト メッセージに含まれることがあります。 Debug プロバイダーではイベント ID が表示されませんが、Console プロバイダーではカテゴリの後に角かっこに囲まれたイベント ID が表示されます。

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>ログ メッセージ テンプレート

ログ メッセージを書き込むたびに、メッセージ テンプレートを指定します。 メッセージ テンプレートは文字列にするか、引数値を配置する名前付きプレースホルダーを含めることができます。 テンプレートは書式設定文字列ではありません。プレースホルダーは番号ではなく名前付きにする必要があります。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

プレースホルダーの名前ではなく、プレースホルダーの順序によって、値の指定に使用されるパラメーターが決まります。 次のようなコードがあるとします。

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

結果のログ メッセージは次のようになります。

```
Parameter values: parm1, parm2
```

ログ記録フレームワークは、ログ プロバイダーが [セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実装できるような方法でメッセージの書式設定を実行します。 書式設定されたメッセージ テンプレートだけでなく、引数自体がログ記録システムに渡されるので、ログ プロバイダーはメッセージ テンプレートに加えてフィールドとしてもパラメーター値を保存できます。 ログの出力先を Azure Table Storage にすると、ロガー メソッドの呼び出しは次のようになります。

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

各 Azure Table エンティティに `ID` および `RequestTime` プロパティを指定し、ログ データのクエリを簡略化することができます。 指定した `RequestTime` の範囲内のすべてのログを検索できます。テキスト メッセージから時間を解析する必要はありません。

## <a name="logging-exceptions"></a>ログ記録の例外

ロガー メソッドには、次の例のように、例外で渡すことができるオーバーロードがあります。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

プロバイダーごとに、例外情報の処理方法は異なります。 前述のコードの Debug プロバイダーの出力の例を次に示します。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>ログのフィルター処理

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

特定のプロバイダーとカテゴリ、またはすべてのプロバイダーまたはすべてのカテゴリに最小ログ レベルを指定できます。 最小レベルを下回るログは、そのプロバイダーに渡されないので、表示または保存されません。 

すべてのログを抑制する場合は、最小ログ レベルに `LogLevel.None` を指定できます。 `LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。

**構成にフィルター規則を作成する**

プロジェクト テンプレートは、`CreateDefaultBuilder` を呼び出して、Console プロバイダーと Debug プロバイダーのログ記録を設定するコードを作成します。 `CreateDefaultBuilder` メソッドは、次のようなコードを使用して、`Logging` セクションの構成を検索するログ記録も設定します。

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

次の例のように、構成データでは、プロバイダーとカテゴリごとに最小ログ レベルを指定します。

[!code-json[](index/sample2/appsettings.json)]

この JSON では、6 個のフィルター規則を作成します。1 つは Debug プロバイダー用、4 つは Console プロバイダー用、1 つはすべてのプロバイダーに適用されるフィルターです。 `ILogger` オブジェクトの作成時に、各プロバイダーにこれらの規則のうち 1 つのみを選択する方法を後で説明します。

**コードのフィルター規則**

次の例のように、コードでフィルター規則を登録できます。

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

2 つ目の `AddFilter` では、プロバイダーの種類名を使用して Debug プロバイダーを指定します。 1 つ目の `AddFilter` は、プロバイダーの種類を指定していないため、すべてのプロバイダーに適用されます。

**フィルター規則を適用する方法**

前の例の構成データと `AddFilter` コードでは、次の表に示す規則を作成します。 最初の 6 つは構成例、最後の 2 つはコード例のものです。

| 数値 | プロバイダー      | 以下から始まるカテゴリ          | 最小ログ レベル |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | デバッグ         | すべてのカテゴリ                          | 情報       |
| 2      | コンソール       | Microsoft.AspNetCore.Mvc.Razor.Internal | 警告           |
| 3      | コンソール       | Microsoft.AspNetCore.Mvc.Razor.Razor    | デバッグ             |
| 4      | コンソール       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | コンソール       | すべてのカテゴリ                          | 情報       |
| 6      | すべてのプロバイダー | すべてのカテゴリ                          | デバッグ             |
| 7      | すべてのプロバイダー | システム                                  | デバッグ             |
| 8      | デバッグ         | Microsoft                               | トレース             |

ログの書き込みに使用する `ILogger` オブジェクトを作成すると、`ILoggerFactory` オブジェクトでそのロガーに適用されるプロバイダーごとの 1 つの規則が選択されます。 その `ILogger` オブジェクトで書き込まれるすべてのメッセージは、選択した規則に基づいてフィルター処理されます。 使用できる規則から、各プロバイダーとカテゴリのペアごとに該当する最も限定的な規則が選択されます。

特定のカテゴリに `ILogger` が作成されるときに、各プロバイダーに次のアルゴリズムが使用されます。

* プロバイダーとそのエイリアスと一致するすべての規則が選択されます。 何も見つからない場合は、空のプロバイダーですべての規則が選択されます。
* 前の手順の結果、最も長いカテゴリのプレフィックスが一致する規則が選択されます。 何も見つからない場合は、カテゴリを指定しないすべての規則が選択されます。
* 複数の規則が選択されている場合は、**最後**の 1 つが使用されます。
* 規則が選択されていない場合は、`MinimumLevel` が使用されます。
 
たとえば、前の規則一覧があり、カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" に `ILogger` オブジェクトを作成するとします。

* Debug プロバイダーの場合、規則 1、6、8 が適用されます。 規則 8 が最も限定的なので、規則 8 が選択されます。
* Console プロバイダーの場合、規則 3、4、5、6 が適用されます。 規則 3 が最も限定的です。

カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" の `ILogger` を含むログを作成すると、`Trace` レベル以上のログは Debug プロバイダーに送信され、`Debug` レベル以上のログは Console プロバイダーに送信されます。

**プロバイダーのエイリアス**

構成では種類の名前を使用してプロバイダーを指定できますが、各プロバイダーには、使いやすい短い*エイリアス*が定義されています。 組み込みのプロバイダーの場合は、次のエイリアスを使用してください。

- コンソール
- デバッグ
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**既定の最小レベル**

指定したプロバイダーとカテゴリに適用される構成またはコードの規則がない場合にのみ反映される最小レベルの設定があります。 最小レベルを設定する方法を次の例に示します。

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

最小レベルを明示的に設定しない場合、既定値は `Information` です。これは、`Trace` および `Debug` ログが無視されることを意味します。

**フィルター関数**

フィルター関数には、フィルター規則を適用するコードを記述できます。 フィルター関数は、構成またはコードによって規則が割り当てられていないすべてのプロバイダーとカテゴリについて呼び出されます。 この関数内のコードは、プロバイダーの種類、カテゴリ、およびログ レベルにアクセスし、メッセージをログに記録するかどうかを決定することができます。 例:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

一部のログ プロバイダーでは、ログのレベルとカテゴリに基づいてログをストレージ メディアに書き込む場合や無視する場合を指定できます。

`AddConsole` および `AddDebug` 拡張メソッドには、フィルター条件で渡すことができるオーバーロードが用意されています。 次のサンプル コードを実行すると、Console プロバイダーは `Warning` レベル未満のログを無視し、Debug プロバイダーはフレームワークで作成されたログを無視します。

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` メソッドには、`EventLogSettings` インスタンスを受け取るオーバーロードがあります。このインスタンスの `Filter` プロパティにはフィルター関数を含めることができます。 TraceSource プロバイダーには、これらのオーバーロードのいずれも用意されていません。これは、ログ レベルと他のパラメーターが、使用される `SourceSwitch` と `TraceListener` に基づいているためです。

`WithFilter` 拡張メソッドを使用して、`ILoggerFactory` インスタンスに登録されているすべてのプロバイダーにフィルター規則を設定できます。 以下の例は、フレームワーク ログを警告に制限しますが (カテゴリは "Microsoft" または "System" で始まります)、アプリではデバッグ レベルでログを記録できます。

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

フィルター処理を使用して、特定のカテゴリに関するすべてのログが書き込まれないようにするには、そのカテゴリの最小ログ レベルとして `LogLevel.None` を指定できます。 `LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。

`WithFilter` 拡張メソッドは [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージで提供されます。 このメソッドは、登録されているすべてのログ プロバイダーに渡されたログ メッセージをフィルター処理する新しい `ILoggerFactory` インスタンスを返します。 これは、元の `ILoggerFactory` インスタンスを含め、他の `ILoggerFactory` インスタンスには影響がありません。

---

## <a name="log-scopes"></a>ログのスコープ

論理操作セットの一部として作成される各ログに同じデータをアタッチするために、*スコープ*内の論理操作セットをグループ化することができます。 たとえば、状況によっては、トランザクションの処理の一部として作成されるすべてのログにトランザクション ID を含める必要があります。

スコープは `ILogger.BeginScope<TState>` メソッドから返される `IDisposable` の種類であり、破棄されるまで継続します。 スコープを使用するには、次のようにロガーの呼び出しを `using` ブロックでラップします。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

次のコードでは、Console プロバイダーのスコープを有効にしています。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Program.cs* の場合:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。 *appsettings* 構成ファイルを使用する `IncludeScopes` の構成は、ASP.NET Core 2.1 のリリースで使用できます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Startup.cs* の場合:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

各ログ メッセージには、スコープ内の情報が含まれます。

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>組み込みのログ プロバイダー

ASP.NET Core には次のプロバイダーが付属しています。

* [コンソール](#console)
* [デバッグ](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>Console プロバイダー

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) プロバイダー パッケージは、ログ出力をコンソールに送信します。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole オーバーロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)を使用すると、最小ログ レベル、フィルター関数、スコープがサポートされているかどうかを示すブール値を渡すことができます。 もう 1 つの選択肢として、`IConfiguration` オブジェクトを渡す方法があります。この場合、スコープのサポートとログ レベルを指定できます。 

運用環境で使用する Console プロバイダーを検討している場合は、パフォーマンスに重大な影響がある点に注意してください。

Visual Studio で新しいプロジェクトを作成する場合、`AddConsole` メソッドは次のようになります。

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

このコードは、*appSettings.json* ファイルの `Logging` セクションを参照しています。

[!code-json[](index/sample//appsettings.json)]

この設定ではフレームワークのログを警告に制限していますが、「[ログのフィルター処理](#log-filtering)」セクションで説明したように、アプリではデバッグ レベルでログに記録することができます。 詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Debug プロバイダー

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) プロバイダー パッケージは、[System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) クラス (`Debug.WriteLine` メソッドの呼び出し) を使用してログの出力を書き込みます。

Linux では、このプロバイダーから */var/log/message* にログが書き込まれます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug オーバーロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)を使用すると、最小ログ レベルまたはフィルター関数を渡すことができます。

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource プロバイダー

ASP.NET Core 1.1.0 以降をターゲットとするアプリの場合、[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) プロバイダー パッケージは、イベントのトレースを実装できます。 Windows では、[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803) を使用します。 プロバイダーはクロスプラットフォームですが、Linux または macOS 用のイベント コレクションはまだ存在せず、ツールは表示されません。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

ログの収集と表示には、[PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567) を使用することをお勧めします。 ETW ログを表示できる他のツールはありますが、ASP.NET から出力される ETW イベントを操作する場合、PerfView は最適なエクスペリエンスを提供します。 

このプロバイダーでログに記録されるイベントを収集するように PerfView を構成するには、**[追加プロバイダー]** の一覧に文字列 `*Microsoft-Extensions-Logging` を追加します  (文字列の先頭に忘れずにアスタリスクを付けてください)。

![Perfview の追加プロバイダー](index/_static/perfview-additional-providers.png)

Nano Server で発生したイベントをキャプチャするには、追加の設定がいくつか必要です。

* PowerShell リモート処理を Nano Server に接続します。

  ```powershell
  Enter-PSSession [name]
  ```

* ETW セッションを作成します。

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* 必要に応じて、[CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers)、ASP.NET Core などの ETW プロバイダーを追加します。 ASP.NET Core プロバイダーの GUID は `3ac73b97-af73-50e9-0822-5da4367920d0` です。 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* サイトを実行し、トレース情報が必要なすべてのアクションを実行します。

* 完了したらトレース セッションを停止します。

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

他の Windows エディションと同様に、結果の *C:\trace.etl* ファイルは PerfView で分析できます。

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows EventLog プロバイダー

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) プロバイダー パッケージは、ログ出力を Windows イベント ログに送信します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog オーバーロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)を使用すると、`EventLogSettings` または最小ログ レベルを渡すことができます。

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource プロバイダー

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) プロバイダー パッケージは、[System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) のライブラリとプロバイダーを使用します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource オーバーロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)を使用すると、ソース スイッチとトレース リスナーを渡すことができます。

このプロバイダーを使用するには、アプリケーションを (.NET Core ではなく) .NET Framework 上で実行する必要があります。 このプロバイダーを使用すると、サンプル アプリケーションで使用されている [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) など、多様な[リスナー](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners)にメッセージをルーティングすることができます。

次の例では、`Warning` 以上のメッセージのログをコンソール ウィンドウに出力する `TraceSource` プロバイダーを構成します。

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure App Service プロバイダー

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) プロバイダー パッケージは、Azure App Service アプリのファイル システムのテキスト ファイルと、Azure Storage アカウントの [BLOB ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)にログを書き込みます。 このプロバイダーは、ASP.NET Core 1.1.0 以降をターゲットとするアプリでのみ使用できます。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

.NET Core を対象にする場合は、プロバイダー パッケージをインストールしたり、`AddAzureWebAppDiagnostics` を明示的に呼び出したりする必要はありません。 アプリを Azure App Service にデプロイすると、プロバイダーはアプリで自動的に使用できるようになります。

.NET Framework を対象にする場合は、プロバイダー パッケージをプロジェクトに追加して、`AddAzureWebAppDiagnostics` を呼び出します。

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` オーバーロードを使用すると、ログ記録出力テンプレート、BLOB 名、ファイル サイズの制限など、既定の設定をオーバーライドできる [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) を渡すことができます  (*出力テンプレート*は、`ILogger` メソッドを呼び出すときに指定するテンプレートに加え、すべてのログに適用されるメッセージ テンプレートです)。

---

App Service アプリにデプロイすると、アプリケーションは Azure Portal の **[App Service]** ページの [[診断ログ]](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) に指定された設定を適用します。 これらの設定を変更すると、変更は即時に反映されます。アプリの再起動や、コードの再デプロイは不要です。 

![Azure のログ設定](index/_static/azure-logging-settings.png)

ログ ファイルの既定の場所は、*D:\\home\\LogFiles\\Application* です。既定のファイル名は *diagnostics-yyyymmdd.txt* です。 既定のファイル サイズ制限は 10 MB です。保持されるファイルの既定の最大数は 2 です。 既定の BLOB 名は *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt* です。 既定の動作の詳細については、[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) を参照してください。

このプロバイダーは、プロジェクトが Azure 環境で実行される場合にのみ機能します。 BLOB のローカル ファイルまたはローカル開発ストレージに書き込まない &mdash; をローカルで実行しても、効果はありません。

## <a name="third-party-logging-providers"></a>サードパーティ製のログ プロバイダー

ここでは、ASP.NET Core で使用できるサードパーティ製のログ記録フレームワークをいくつか紹介します。

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - Elmah.Io サービスのプロバイダー

* [JSNLog](http://jsnlog.com) - JavaScript の例外と他のクライアント側イベントをサーバー側のログに記録します。

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - Loggr サービスのプロバイダー

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) - NLog ライブラリのプロバイダー

* [Serilog](https://github.com/serilog/serilog-extensions-logging) - Serilog ライブラリのプロバイダー

一部のサードパーティ製フレームワークは、[セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実行できます。

サードパーティ製フレームワークの使用方法は、組み込みのプロバイダーのいずれかを使用する方法と似ています。NuGet パッケージをプロジェクトに追加し、`ILoggerFactory` に対して拡張メソッドを呼び出します。 詳細については、各フレームワークのドキュメントを参照してください。

独自のカスタム プロバイダーを作成して、他のログ記録フレームワークや独自のログ記録要件に対応することもできます。

## <a name="azure-log-streaming"></a>Azure ログのストリーミング

Azure ログのストリーミングを使用すると、リアル タイムで以下のログ アクティビティを確認できます。 

* アプリケーション サーバー 
* Web サーバー
* 失敗した要求のトレース 

Azure ログのストリーミングを構成するには 

* アプリケーションのポータル ページから **[診断ログ]** ページに移動します。
* **[アプリケーション ログ (ファイル システム)]** を [オン] に設定します。 

![Azure Portal の [診断ログ] ページ](index/_static/azure-diagnostic-logs.png)

**[ログ ストリーミング]** ページに移動して、アプリケーション メッセージを確認します。 これらのメッセージは、アプリケーションで `ILogger` インターフェイスを介してログに記録されます。 

![Azure Portal のアプリケーション ログのストリーミング](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>関連項目

[LoggerMessage による高パフォーマンスのログ記録](xref:fundamentals/logging/loggermessage)
