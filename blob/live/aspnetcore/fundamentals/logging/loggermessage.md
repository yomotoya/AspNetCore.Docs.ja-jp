---
title: "ASP.NET Core で LoggerMessage による高パフォーマンスのログ"
author: guardrex
description: "LoggerMessage 機能を使用して、高パフォーマンスのログ記録シナリオをロガーの拡張メソッドよりも少ないオブジェクトの割り当てを必要とするキャッシュ可能なデリゲートを作成する方法を説明します。"
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="2bee3-103">ASP.NET Core で LoggerMessage による高パフォーマンスのログ</span><span class="sxs-lookup"><span data-stu-id="2bee3-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="2bee3-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2bee3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2bee3-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)機能をより少ないオブジェクトの割り当てを必要としより計算オーバーヘッドの削減キャッシュのデリゲートを作成する[ロガーの拡張メソッド](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions)など`LogInformation`、 `LogDebug`、および`LogError`.</span><span class="sxs-lookup"><span data-stu-id="2bee3-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="2bee3-106">高パフォーマンスのログ記録シナリオを使用して、`LoggerMessage`パターン。</span><span class="sxs-lookup"><span data-stu-id="2bee3-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="2bee3-107">`LoggerMessage`ロガーの拡張メソッドを次のパフォーマンスの利点があります。</span><span class="sxs-lookup"><span data-stu-id="2bee3-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="2bee3-108">ロガーの拡張メソッドなど、「ボックス化」(変換) 値の型を必要`int`に`object`です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="2bee3-109">`LoggerMessage`パターンは、静的なを使用して、ボックス化を回避できます`Action`フィールドと厳密に型指定されたパラメーターを持つ拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="2bee3-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="2bee3-110">ロガーの拡張メソッドは、ログ メッセージが書き込まれるたびにメッセージのテンプレート (名前付きの書式指定文字列) を解析する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bee3-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="2bee3-111">`LoggerMessage`のみ、メッセージが定義されている場合、テンプレートを 1 回解析が必要です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="2bee3-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2bee3-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2bee3-113">サンプル アプリを示しています`LoggerMessage`追跡システムの基本的な見積もりで機能します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="2bee3-114">アプリでは、追加し、メモリ内のデータベースを使用する引用符を削除します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="2bee3-115">使用してログのメッセージを生成するようにこれらの操作が発生すると、`LoggerMessage`パターン。</span><span class="sxs-lookup"><span data-stu-id="2bee3-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="2bee3-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="2bee3-116">LoggerMessage.Define</span></span>

<span data-ttu-id="2bee3-117">[(LogLevel、EventId には、文字列) を定義する](/dotnet/api/microsoft.extensions.logging.loggermessage.define)を作成、`Action`メッセージをログ記録するために委任します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="2bee3-118">`Define`オーバー ロードでは、名前付きの書式指定文字列 (テンプレート) に最大 6 つの型パラメーターを渡すことを許可します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="2bee3-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="2bee3-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="2bee3-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)を作成、`Func`デリゲートを定義するため、[ログ スコープ](xref:fundamentals/logging/index#log-scopes)です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="2bee3-121">`DefineScope`オーバー ロードでは、名前付きの書式指定文字列 (テンプレート) に最大 3 つの型パラメーターを渡すことを許可します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="2bee3-122">メッセージのテンプレート (書式指定文字列を名前付き)</span><span class="sxs-lookup"><span data-stu-id="2bee3-122">Message template (named format string)</span></span>

<span data-ttu-id="2bee3-123">指定された文字列、`Define`と`DefineScope`メソッドは、テンプレートと、補間文字列ではありません。</span><span class="sxs-lookup"><span data-stu-id="2bee3-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="2bee3-124">プレース ホルダーは、種類が指定されている順に入力されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="2bee3-125">テンプレートのプレース ホルダー名では、テンプレート間でわかりやすい名前と一貫性をする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bee3-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="2bee3-126">構造化されたログ データ内のプロパティ名として使用され、します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="2bee3-127">お勧め[Pascal 文字種](/dotnet/standard/design-guidelines/capitalization-conventions)プレース ホルダー名のです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="2bee3-128">たとえば、 `{Count}`、`{FirstName}`です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="2bee3-129">LoggerMessage.Define を実装します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="2bee3-130">各ログ メッセージは、`Action`によって作成された静的フィールドでは、保持`LoggerMessage.Define`です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="2bee3-131">たとえば、サンプル アプリがインデックス ページの取得要求のログ メッセージを記述するフィールドを作成 (*Internal/LoggerExtensions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="2bee3-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="2bee3-132">`Action`を指定します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="2bee3-133">ログ レベルです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-133">The log level.</span></span>
* <span data-ttu-id="2bee3-134">一意のイベントの識別子 ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) 静的拡張メソッドの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="2bee3-135">(書式指定文字列を名前付き) メッセージ テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-135">The message template (named format string).</span></span> 

<span data-ttu-id="2bee3-136">サンプル アプリのセットのインデックス ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="2bee3-137">ログ レベルを`Information`です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-137">Log level to `Information`.</span></span>
* <span data-ttu-id="2bee3-138">イベント id`1`の名前を持つ、`IndexPageRequested`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="2bee3-139">メッセージ (という名前のテンプレート書式指定文字列) を文字列にします。</span><span class="sxs-lookup"><span data-stu-id="2bee3-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="2bee3-140">構造化されたログ ストアは、イベント id をログ記録を強化することが指定されると、イベント名を使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2bee3-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="2bee3-141">たとえば、 [Serilog](https://github.com/serilog/serilog-extensions-logging)はイベント名を使用します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="2bee3-142">`Action`を通じて、厳密に型指定された拡張メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="2bee3-143">`IndexPageRequested`メソッドは、サンプル アプリで、インデックス ページの取得要求のメッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="2bee3-144">`IndexPageRequested`ロガーで呼び出されると、`OnGetAsync`メソッド*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2bee3-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="2bee3-145">アプリのコンソール出力を検査します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="2bee3-146">ログ メッセージにパラメーターを渡すには、静的フィールドを作成するときに最大 6 つの型を定義します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="2bee3-147">サンプル アプリは、定義することで、引用符を追加するときに、文字列をログ、`string`の入力、`Action`フィールド。</span><span class="sxs-lookup"><span data-stu-id="2bee3-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="2bee3-148">デリゲートのログ メッセージのテンプレートは、提供される型からそのプレース ホルダーの値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2bee3-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="2bee3-149">サンプル アプリは、見積もりパラメーターは、引用符を追加するため、デリゲートを定義、 `string`:</span><span class="sxs-lookup"><span data-stu-id="2bee3-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="2bee3-150">引用符を追加するための静的な拡張メソッド`QuoteAdded`見積もり引数の値を受け取りに渡します、`Action`を委任します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="2bee3-151">インデックス ページの分離コード ファイル内 (*Pages/Index.cshtml.cs*)、`QuoteAdded`メッセージを記録するために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="2bee3-152">アプリのコンソール出力を検査します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="2bee3-153">サンプル アプリを実装する`try` &ndash; `catch`見積もり削除のパターン。</span><span class="sxs-lookup"><span data-stu-id="2bee3-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="2bee3-154">情報メッセージが正常に削除操作の記録されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="2bee3-155">例外がスローされたときに、削除操作のエラー メッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="2bee3-156">ログ メッセージが含まれています、例外スタック トレースには削除操作の失敗の (*Internal/LoggerExtensions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="2bee3-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="2bee3-157">内のデリゲートに渡された例外に注意してください`QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="2bee3-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="2bee3-158">インデックス ページの分離コード、成功した見積もり削除の呼び出し、`QuoteDeleted`ロガーのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="2bee3-159">削除対象として、引用符が見つからないときに、`ArgumentNullException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="2bee3-160">例外をトラップ、 `try` &ndash; `catch`ステートメントを呼び出してログに記録し、`QuoteDeleteFailed`で logger にメソッド、`catch`ブロック (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="2bee3-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="2bee3-161">見積もりが正常に削除されると、アプリのコンソール出力を検査します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="2bee3-162">見積もりの削除に失敗した場合は、アプリのコンソール出力を調べてください。</span><span class="sxs-lookup"><span data-stu-id="2bee3-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="2bee3-163">ログ メッセージで例外が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2bee3-163">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="2bee3-164">LoggerMessage.DefineScope を実装します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="2bee3-165">定義、[ログ スコープ](xref:fundamentals/logging/index#log-scopes)を使用してログ メッセージの系列に適用する、 [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="2bee3-166">サンプル アプリには、**すべてクリア**のすべてのデータベース内の引用符を削除するためのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2bee3-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="2bee3-167">1 つ削除では、引用符は削除、時にします。</span><span class="sxs-lookup"><span data-stu-id="2bee3-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="2bee3-168">引用符が削除されるたびに、`QuoteDeleted`ロガーにメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="2bee3-169">ログのスコープは、これらのログ メッセージに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="2bee3-170">有効にする`IncludeScopes`コンソール ロガー オプション。</span><span class="sxs-lookup"><span data-stu-id="2bee3-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="2bee3-171">設定`IncludeScopes`ASP.NET Core 2.0 アプリでログのスコープを有効にするために必要なです。</span><span class="sxs-lookup"><span data-stu-id="2bee3-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="2bee3-172">設定`IncludeScopes`を介して*appsettings*構成ファイルは、ASP.NET Core 2.1 リリースが予定されている機能です。</span><span class="sxs-lookup"><span data-stu-id="2bee3-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="2bee3-173">サンプル アプリでは、その他のプロバイダーを削除し、ログ出力を削減するためにフィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="2bee3-174">これにより、具体的に示すサンプルのログ メッセージをわかりやすく`LoggerMessage`機能します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="2bee3-175">ログのスコープを作成するには、保持するためにフィールドを追加、`Func`スコープを委任します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="2bee3-176">フィールドを作成するサンプル アプリ`_allQuotesDeletedScope`(*Internal/LoggerExtensions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="2bee3-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="2bee3-177">使用して`DefineScope`デリゲートを作成します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="2bee3-178">次の 3 種類まで指定できます用にテンプレート引数としてデリゲートが呼び出されたときに。</span><span class="sxs-lookup"><span data-stu-id="2bee3-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="2bee3-179">サンプル アプリで使用するメッセージを含むテンプレートを削除した引用符の数 (、`int`型)。</span><span class="sxs-lookup"><span data-stu-id="2bee3-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="2bee3-180">ログ メッセージ用の静的な拡張メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="2bee3-181">メッセージ テンプレートに表示される名前付きのプロパティの型パラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="2bee3-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="2bee3-182">サンプル アプリは、`count`引用符を削除し、返します`_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="2bee3-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="2bee3-183">ログの拡張機能の呼び出しのスコープ ラップ、`using`ブロック。</span><span class="sxs-lookup"><span data-stu-id="2bee3-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="2bee3-184">アプリのコンソール出力のログ メッセージを検査します。</span><span class="sxs-lookup"><span data-stu-id="2bee3-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="2bee3-185">次の結果は、次の 3 つの引用符が含まれるログ スコープのメッセージが削除を示しています。</span><span class="sxs-lookup"><span data-stu-id="2bee3-185">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a><span data-ttu-id="2bee3-186">関連項目</span><span class="sxs-lookup"><span data-stu-id="2bee3-186">See also</span></span>

* [<span data-ttu-id="2bee3-187">ログ</span><span class="sxs-lookup"><span data-stu-id="2bee3-187">Logging</span></span>](xref:fundamentals/logging/index)
