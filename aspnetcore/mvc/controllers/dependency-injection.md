---
title: "コントローラーへの依存関係の挿入"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a>コントローラーへの依存関係の挿入

<a name="dependency-injection-controllers"></a>

作成者: [Steve Smith](https://ardalis.com/)

ASP.NET Core の MVC コントローラーは、それらのコンストラクターを使用して明示的にそれらの依存関係を要求する必要があります。 場合によっては、個々のコントローラーのアクションがサービスを必要とし、コントローラー レベルで要求しても意味がないことがあります。 この場合、アクション メソッドのパラメーターとしてサービスを挿入することもできます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="dependency-injection"></a>依存関係の挿入

依存関係の挿入は、[依存関係逆転の原則](http://deviq.com/dependency-inversion-principle/)に従う手法であり、弱く結合されたモジュールでアプリケーションを構成できるようにします。 ASP.NET Core には[依存関係の挿入](../../fundamentals/dependency-injection.md)の組み込みのサポートがあり、アプリケーションを簡単にテストして維持することができます。

## <a name="constructor-injection"></a>コンストラクターの挿入

ASP.NET Core のコンストラクターに基づく依存関係の挿入の組み込みサポートは、MVC コントローラーを拡張します。 単にサービスの種類をコンストラクター パラメーターとしてコントローラーに追加することで、ASP.NET Core は組み込みのサービス コンテナーを使用してその種類の解決を試みます。 サービスの定義には、常にではありませんが、一般的にインターフェイスを使用します。 たとえば、アプリケーションに現在の時刻に依存するビジネス ロジックがある場合、時刻を取得するサービスを (ハードコーディングする代わりに) 挿入することができます。これにより設定された時刻を使用する実装でテストに合格させることができます。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


実行時にシステム クロックを使用するように、このようなインターフェイスを実装するのは簡単です。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


こうすると、コントローラーでサービスを使用できます。 このケースでは、いくつかのロジックを `HomeController` `Index` メソッドに追加し、時刻に基づいてユーザーにあいさつを表示します。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

アプリケーションを今すぐ実行した場合、エラーが発生する可能性が高くなります。

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

このエラーは、`Startup` クラスの `ConfigureServices` メソッドでサービスを構成していないときに発生します。 `IDateTime` の要求が `SystemDateTime` のインスタンスを使用して解決されるように指定するには、下のリストで強調表示された行を `ConfigureServices` メソッドに追加します。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> この特定のサービスは、いくつかの異なる有効期間オプション (`Transient`、`Scoped`、または`Singleton`) のいずれかを使用して実装することもできます。 これらの範囲オプションがサービスの動作にどのように影響するかを理解するには、「[依存関係の挿入](../../fundamentals/dependency-injection.md)」を参照してください。

サービスが構成された後に、アプリケーションを実行して、ホーム ページに移動すると、予想どおりに時刻ベースのメッセージが表示されます。

![サーバーの応答メッセージ](dependency-injection/_static/server-greeting.png)

>[!TIP]
> コントローラーで依存関係を明示的に要求することによってコードをテストしやすくする方法については、「[Testing Controller Logic](testing.md)」(コントローラー ロジックのテスト) ([http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)) を参照してください。

ASP.NET Core の組み込みの依存関係の挿入は、サービスを要求するクラスの 1 つのコンストラクターのみの使用をサポートします。 複数のコンストラクターを使用している場合、次のような例外が表示される可能性があります。

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

エラー メッセージが示しているように、コンストラクターを 1 つだけにすることでこの問題を修正することができます。 [既定の依存関係の挿入のサポートをサード パーティ製の実装に置き換えることもできます](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)。それらの多くは複数のコンストラクターをサポートします。

## <a name="action-injection-with-fromservices"></a>FromServices によるアクションの挿入

コントローラー内の複数のアクションのサービスが必要ない場合があります。 この場合、アクション メソッドにパラメーターとしてサービスを挿入するのが賢明です。 次に示すように、これは属性 `[FromServices]` でパラメーターをマークすることで行います。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>コントローラーから設定へのアクセス

コントローラー内からアプリケーションまたは構成の設定へのアクセスが一般的なパターンです。 このアクセスでは、[構成](xref:fundamentals/configuration/index)に関するページで説明されているオプション パターンを使用する必要があります。 一般的に、依存関係の挿入を使用してコントローラーから直接設定を要求しないようにする必要があります。 `IOptions<T>` インスタンスを要求する方法がより適切です。`T` は必要な構成クラスです。

オプションのパターンを使用するには、次のようなオプションを表すクラスを作成する必要があります。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

その後で、オプションのモデルを使用するようにアプリケーションを構成し、構成クラスを `ConfigureServices` でサービス コレクションに追加します。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> 上記のリストで、JSON 形式のファイルから設定を読み取るようにアプリケーションを構成しています。 上記のコメントが付けられたコードに示すように、コードで設定全体を構成することもできます。 その他の構成オプションについては、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

厳密に型指定された構成オブジェクト (このケースでは `SampleWebSettings`) を指定し、それをサービスのコレクションに追加したら、`IOptions<T>` のインスタンス (このケースでは `IOptions<SampleWebSettings>`) を要求することによって、任意のコントローラーまたはアクション メソッドからそれを要求することができます。 次のコードは、コントローラーから設定を要求する方法を示しています。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

オプションのパターンに従うと、設定や構成を相互に分離し、さらにコントローラーが設定情報を見つける方法または場所を知る必要がないので、コントローラーを[関心の分離](http://deviq.com/separation-of-concerns/)に確実に従わせることができます。 また、コントローラー クラス内での[静的な結合](http://deviq.com/static-cling/)または設定クラスの直接のインスタンス化がないので、コントローラーの単体テスト ([コントローラー ロジックのテスト](testing.md)) が容易になります。
