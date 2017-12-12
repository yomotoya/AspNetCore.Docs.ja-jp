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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>ASP.NET Core で LoggerMessage による高パフォーマンスのログ

作成者: [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)機能をより少ないオブジェクトの割り当てを必要としより計算オーバーヘッドの削減キャッシュのデリゲートを作成する[ロガーの拡張メソッド](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions)など`LogInformation`、 `LogDebug`、および`LogError`. 高パフォーマンスのログ記録シナリオを使用して、`LoggerMessage`パターン。

`LoggerMessage`ロガーの拡張メソッドを次のパフォーマンスの利点があります。

* ロガーの拡張メソッドなど、「ボックス化」(変換) 値の型を必要`int`に`object`です。 `LoggerMessage`パターンは、静的なを使用して、ボックス化を回避できます`Action`フィールドと厳密に型指定されたパラメーターを持つ拡張メソッド。
* ロガーの拡張メソッドは、ログ メッセージが書き込まれるたびにメッセージのテンプレート (名前付きの書式指定文字列) を解析する必要があります。 `LoggerMessage`のみ、メッセージが定義されている場合、テンプレートを 1 回解析が必要です。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

サンプル アプリを示しています`LoggerMessage`追跡システムの基本的な見積もりで機能します。 アプリでは、追加し、メモリ内のデータベースを使用する引用符を削除します。 使用してログのメッセージを生成するようにこれらの操作が発生すると、`LoggerMessage`パターン。

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[(LogLevel、EventId には、文字列) を定義する](/dotnet/api/microsoft.extensions.logging.loggermessage.define)を作成、`Action`メッセージをログ記録するために委任します。 `Define`オーバー ロードでは、名前付きの書式指定文字列 (テンプレート) に最大 6 つの型パラメーターを渡すことを許可します。

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)を作成、`Func`デリゲートを定義するため、[ログ スコープ](xref:fundamentals/logging/index#log-scopes)です。 `DefineScope`オーバー ロードでは、名前付きの書式指定文字列 (テンプレート) に最大 3 つの型パラメーターを渡すことを許可します。

## <a name="message-template-named-format-string"></a>メッセージのテンプレート (書式指定文字列を名前付き)

指定された文字列、`Define`と`DefineScope`メソッドは、テンプレートと、補間文字列ではありません。 プレース ホルダーは、種類が指定されている順に入力されます。 テンプレートのプレース ホルダー名では、テンプレート間でわかりやすい名前と一貫性をする必要があります。 構造化されたログ データ内のプロパティ名として使用され、します。 お勧め[Pascal 文字種](/dotnet/standard/design-guidelines/capitalization-conventions)プレース ホルダー名のです。 たとえば、 `{Count}`、`{FirstName}`です。

## <a name="implementing-loggermessagedefine"></a>LoggerMessage.Define を実装します。

各ログ メッセージは、`Action`によって作成された静的フィールドでは、保持`LoggerMessage.Define`です。 たとえば、サンプル アプリがインデックス ページの取得要求のログ メッセージを記述するフィールドを作成 (*Internal/LoggerExtensions.cs*)。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

`Action`を指定します。

* ログ レベルです。
* 一意のイベントの識別子 ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) 静的拡張メソッドの名前に置き換えます。
* (書式指定文字列を名前付き) メッセージ テンプレートです。 

サンプル アプリのセットのインデックス ページを要求します。

* ログ レベルを`Information`です。
* イベント id`1`の名前を持つ、`IndexPageRequested`メソッドです。
* メッセージ (という名前のテンプレート書式指定文字列) を文字列にします。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

構造化されたログ ストアは、イベント id をログ記録を強化することが指定されると、イベント名を使用可能性があります。 たとえば、 [Serilog](https://github.com/serilog/serilog-extensions-logging)はイベント名を使用します。

`Action`を通じて、厳密に型指定された拡張メソッドが呼び出されます。 `IndexPageRequested`メソッドは、サンプル アプリで、インデックス ページの取得要求のメッセージを記録します。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`ロガーで呼び出されると、`OnGetAsync`メソッド*Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

アプリのコンソール出力を検査します。

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

ログ メッセージにパラメーターを渡すには、静的フィールドを作成するときに最大 6 つの型を定義します。 サンプル アプリは、定義することで、引用符を追加するときに、文字列をログ、`string`の入力、`Action`フィールド。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

デリゲートのログ メッセージのテンプレートは、提供される型からそのプレース ホルダーの値を受け取ります。 サンプル アプリは、見積もりパラメーターは、引用符を追加するため、デリゲートを定義、 `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

引用符を追加するための静的な拡張メソッド`QuoteAdded`見積もり引数の値を受け取りに渡します、`Action`を委任します。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

インデックス ページの分離コード ファイル内 (*Pages/Index.cshtml.cs*)、`QuoteAdded`メッセージを記録するために呼び出されます。

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

アプリのコンソール出力を検査します。

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

サンプル アプリを実装する`try` &ndash; `catch`見積もり削除のパターン。 情報メッセージが正常に削除操作の記録されます。 例外がスローされたときに、削除操作のエラー メッセージが記録されます。 ログ メッセージが含まれています、例外スタック トレースには削除操作の失敗の (*Internal/LoggerExtensions.cs*)。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

内のデリゲートに渡された例外に注意してください`QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

インデックス ページの分離コード、成功した見積もり削除の呼び出し、`QuoteDeleted`ロガーのメソッドです。 削除対象として、引用符が見つからないときに、`ArgumentNullException`がスローされます。 例外をトラップ、 `try` &ndash; `catch`ステートメントを呼び出してログに記録し、`QuoteDeleteFailed`で logger にメソッド、`catch`ブロック (*Pages/Index.cshtml.cs*)。

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

見積もりが正常に削除されると、アプリのコンソール出力を検査します。

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

見積もりの削除に失敗した場合は、アプリのコンソール出力を調べてください。 ログ メッセージで例外が含まれることに注意してください。

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

## <a name="implementing-loggermessagedefinescope"></a>LoggerMessage.DefineScope を実装します。

定義、[ログ スコープ](xref:fundamentals/logging/index#log-scopes)を使用してログ メッセージの系列に適用する、 [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)メソッドです。

サンプル アプリには、**すべてクリア**のすべてのデータベース内の引用符を削除するためのボタンをクリックします。 1 つ削除では、引用符は削除、時にします。 引用符が削除されるたびに、`QuoteDeleted`ロガーにメソッドが呼び出されます。 ログのスコープは、これらのログ メッセージに追加されます。

有効にする`IncludeScopes`コンソール ロガー オプション。

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

設定`IncludeScopes`ASP.NET Core 2.0 アプリでログのスコープを有効にするために必要なです。 設定`IncludeScopes`を介して*appsettings*構成ファイルは、ASP.NET Core 2.1 リリースが予定されている機能です。

サンプル アプリでは、その他のプロバイダーを削除し、ログ出力を削減するためにフィルターを追加します。 これにより、具体的に示すサンプルのログ メッセージをわかりやすく`LoggerMessage`機能します。

ログのスコープを作成するには、保持するためにフィールドを追加、`Func`スコープを委任します。 フィールドを作成するサンプル アプリ`_allQuotesDeletedScope`(*Internal/LoggerExtensions.cs*)。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

使用して`DefineScope`デリゲートを作成します。 次の 3 種類まで指定できます用にテンプレート引数としてデリゲートが呼び出されたときに。 サンプル アプリで使用するメッセージを含むテンプレートを削除した引用符の数 (、`int`型)。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

ログ メッセージ用の静的な拡張メソッドを提供します。 メッセージ テンプレートに表示される名前付きのプロパティの型パラメーターを含めます。 サンプル アプリは、`count`引用符を削除し、返します`_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

ログの拡張機能の呼び出しのスコープ ラップ、`using`ブロック。

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

アプリのコンソール出力のログ メッセージを検査します。 次の結果は、次の 3 つの引用符が含まれるログ スコープのメッセージが削除を示しています。

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

## <a name="see-also"></a>関連項目

* [ログ](xref:fundamentals/logging/index)
