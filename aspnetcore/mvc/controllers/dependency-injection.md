---
title: "コント ローラーに依存関係の挿入"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: ff0a1a34ee6b025be6312a81f1a0bcdd07026adb
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="dependency-injection-into-controllers"></a>コント ローラーに依存関係の挿入

<a name="dependency-injection-controllers"></a>

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core の MVC コント ローラーには、そのコンス トラクターを使用して明示的にその依存関係を要求する必要があります。 場合によっては、個々 のコント ローラーのアクションは、サービスを必要があり、ことはできません、コント ローラー レベルを要求する合理的です。 この場合、アクション メソッドのパラメーターとしてサービスを挿入することもできます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="dependency-injection"></a>依存関係の挿入

依存関係の挿入がこれに続く手法、[依存関係の逆転原則](http://deviq.com/dependency-inversion-principle/)、疎結合モジュールで構成されるアプリケーションにすることができます。 ASP.NET Core はの組み込みサポート[依存性の注入](../../fundamentals/dependency-injection.md)を簡単にアプリケーションをテストし、維持します。

## <a name="constructor-injection"></a>コンス トラクター インジェクション

MVC コント ローラーにコンス トラクターに基づく依存関係の挿入の ASP.NET Core の組み込みサポートを拡張します。 単にサービスの種類を追加すると、コンス トラクター パラメーターとして、コント ローラーに、ASP.NET Core をサービス コンテナーでそのビルドを使用してその型の解決を試みます。 サービスは、常にではありませんが、通常、定義のインターフェイスを使用します。 たとえば、アプリケーションに現在の時刻に依存するビジネス ロジックがある場合、テストの設定された時刻を使用する実装を渡すことを許可する時間 (代わりにハードコーディングする)、サービスを挿入できます。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


単純では実行時に、システム クロックを使用するように、このようなインターフェイスを実装します。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


こうすると、サービス、コント ローラーに使用できます。 いくつかのロジックを追加しましたここで、 `HomeController` `Index`時刻に基づいて、ユーザーにするとあいさつ文を表示するメソッド。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

今すぐアプリケーションを実行する場合ほとんどの場合、エラーが発生します。

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

このエラーでサービスが構成されていないときに発生、`ConfigureServices`メソッドで、`Startup`クラスです。 要求を指定する`IDateTime`のインスタンスを使用して解決する必要がある`SystemDateTime`には、以下のリストで強調表示された行を追加、`ConfigureServices`メソッド。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> この特定のサービスは、さまざまな有効期間オプションがいくつかのいずれかを使用して実装することも (`Transient`、 `Scoped`、または`Singleton`)。 参照してください[依存性の注入](../../fundamentals/dependency-injection.md)方法、サービスの動作に影響はこれらの各スコープ オプションを理解します。

サービスが構成されると、アプリケーションを実行し、ホーム ページに移動する必要がありますメッセージを表示、時間ベース期待どおりに。

![サーバーの応答メッセージ](dependency-injection/_static/server-greeting.png)

>[!TIP]
> 参照してください[テスト コント ローラー ロジック](testing.md)の依存関係を明示的に要求する方法について[http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)コント ローラーに簡単にコードをテストします。

ASP.NET Core の組み込みの依存関係の挿入は、サービスを要求するクラスの 1 つのコンス トラクターのみを含めることがサポートされます。 複数のコンス トラクターを使っている場合、例外を示すを取得する可能性があります。

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

エラー メッセージが示しているとは、コンス トラクターは 1 つだけを持つこの問題を修正することができます。 こともできます[サード パーティ製の実装で既定の依存関係の挿入のサポートを置き換えます](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)、多くの複数のコンス トラクターをサポートします。

## <a name="action-injection-with-fromservices"></a>FromServices による操作性の注入

場合があります、コント ローラー内のサービスを 1 つ以上のアクションは必要ありません。 この場合が賢明をアクション メソッドにパラメーターとして、サービスを挿入します。 これは、属性を持つパラメーターをマークすることで`[FromServices]`次に示すようにします。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>コント ローラーから設定へのアクセス

コント ローラー内からアプリケーションまたは構成の設定へのアクセスは、一般的なパターンです。 このアクセスで説明されているオプション パターンを使用する必要があります[構成](../../fundamentals/configuration.md)です。 一般にする必要がありますいないを要求する設定の依存関係の挿入を使用して、コント ローラーから直接。 より適切な方法は、要求、`IOptions<T>`インスタンス、場所`T`必要がある構成クラスです。

オプションのパターンを使用するには、ように、オプションを表すクラスを作成する必要があります。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

オプションのモデルを使用し、構成、クラス、サービスでコレクションに追加するアプリケーションを構成する必要があります`ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> 上記の一覧で、アプリケーションが JSON 形式のファイルから設定を読み取る構成を行っています。 上記のコメントが付けられたコードに示すようには、コードで完全設定を構成することもできます。 参照してください[構成](../../fundamentals/configuration.md)詳細構成オプションを選択します。

厳密に型指定された構成オブジェクトを指定すると、(この場合、 `SampleWebSettings`) され、追加サービスのコレクションに要求できますが、コント ローラーまたはアクション メソッドからのインスタンスを要求することによって`IOptions<T>`(この場合、 `IOptions<SampleWebSettings>`). 次のコードは、コント ローラーから、設定を要求 1 つの方法を示しています。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

設定と構成、互いから分離するのには、オプションのパターンに従うと、コント ローラーは、次のことを確認[関心の分離](http://deviq.com/separation-of-concerns/)方法や場所を知る必要があるため、設定が見つかりません情報です。 また、コント ローラーの単体テストを簡単に[テスト コント ローラー ロジック](testing.md)があるため、ありません[静的窓](http://deviq.com/static-cling/)またはコント ローラー クラス内で設定クラスの直接インスタンス化します。
