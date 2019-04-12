---
title: Razor のコンポーネントの依存関係の挿入
author: guardrex
description: アプリの Blazor と剃刀コンポーネントがコンポーネントに挿入することによってサービスを使用する方法を参照してください。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515486"
---
# <a name="razor-components-dependency-injection"></a>Razor のコンポーネントの依存関係の挿入

によって[Rainer Stropek](https://www.timecockpit.com)

Razor のコンポーネントをサポートしています[依存関係の注入 (DI)](xref:fundamentals/dependency-injection)します。 アプリでは、コンポーネントに挿入することによって、組み込みのサービスを使用できます。 アプリも定義し、カスタム サービスを登録し、DI を使用して、アプリ全体で使用できるように。

## <a name="dependency-injection"></a>依存関係の挿入

DI は、中央の場所に構成されているサービスにアクセスするための手法です。 これは、Razor コンポーネント アプリで役立ちます。

* 呼ばれる、多くのコンポーネント間で、サービス クラスの 1 つのインスタンスを共有する*シングルトン*サービス。
* 参照の抽象化を使用して、サービスの具象クラスからコンポーネントを分離します。 たとえば、インターフェイス`IDataAccess`アプリ内のデータにアクセスするためです。 インターフェイスを実装する具象によって`DataAccess`クラスし、アプリのサービス コンテナーにサービスとして登録します。 コンポーネントが受信する DI を使用する場合、`IDataAccess`コンポーネントの実装が具象型に結合されていません。 おそらく、実装は、単体テストでモック実装にはスワップできます。

詳細については、「 <xref:fundamentals/dependency-injection> 」を参照してください。

## <a name="add-services-to-di"></a>DI にサービスを追加します。

新しいアプリを作成すると、確認、`Startup.ConfigureServices`メソッド。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices`メソッドに渡されますが、 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>、サービスの記述子オブジェクトの一覧 (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>)。 サービスは、サービスのコレクションにサービスの記述子を提供することによって追加されます。 次の例では、概念を`IDataAccess`インターフェイスとその具体的な実装`DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

サービスは、有効期間の次の表に示すように構成できます。

| 有効期間 | 説明 |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI を作成、*単一インスタンス*のサービス。 必要とするすべてのコンポーネントを`Singleton`サービスが同じサービスのインスタンスを受信します。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | コンポーネントがのインスタンスを取得するたびに、`Transient`サービスは受信サービスのコンテナーから、*新しいインスタンス*サービスの。 |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | クライアント側 Blazor 現在 DI スコープの概念はありません。 `Scoped` ように動作`Singleton`します。 ただし、ASP.NET Core Razor のコンポーネントのサポート、`Scoped`有効期間。 Razor のコンポーネントでは、スコープ化されたサービスの登録は、接続に制限されます。 このためを使用してスコープを持つサービスの現在のユーザーにスコープする必要がありますサービス現在の目的は、クライアント側で実行する場合でも、ブラウザーでします。 |

DI システムは、ASP.NET Core で DI システムに基づきます。 詳細については、「 <xref:fundamentals/dependency-injection> 」を参照してください。

## <a name="default-services"></a>既定のサービス

既定のサービスは、アプリのサービスのコレクションに自動的に追加されます。

| サービス | 説明 |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | HTTP 要求の送信と URI (シングルトン) で識別されるリソースから HTTP 応答を受信するメソッドを提供します。 注意のこのインスタンス`HttpClient`バック グラウンドで HTTP トラフィックを処理するため、ブラウザーを使用します。 [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress)アプリのベース URI プレフィックスが自動的に設定します。 `HttpClient` クライアント側 Blazor アプリにのみ提供されます。 |
| `IJSRuntime` | JavaScript ランタイムの呼び出しをディスパッチできますのインスタンスを表します。 詳細については、「 <xref:razor-components/javascript-interop> 」を参照してください。 |
| `IUriHelper` | Uri およびナビゲーションの状態 (シングルトン) を操作するためのヘルパーが含まれています。 `IUriHelper` Blazor と剃刀コンポーネントの両方のアプリに提供されます。 |

既定のテンプレートによって追加された既定のサービス プロバイダーではなく、カスタム サービス プロバイダーを使用することになります。 カスタム サービス プロバイダーは、表に示す既定のサービスを自動的に提供しません。 カスタム サービス プロバイダーを使用して、テーブルのようにサービスを必要とする場合は、新しいサービス プロバイダーに必要なサービスを追加します。

## <a name="request-a-service-in-a-component"></a>要求では、コンポーネント サービス

サービスがサービス コレクションに追加された後を使用して、コンポーネントの Razor テンプレートに、サービスを挿入、 [ @inject ](xref:mvc/views/razor#section-4) Razor ディレクティブ。 `@inject` 2 つのパラメーターがあります。

* 型名:挿入するサービスの型。
* プロパティ名:挿入された app service の受信プロパティの名前。 プロパティが手動で作成を必要としないことに注意してください。 コンパイラは、プロパティを作成します。

詳細については、「 <xref:mvc/views/dependency-injection> 」を参照してください。

複数回使用`@inject`さまざまなサービスを挿入するステートメント。

次の例は、`@inject` を使用する方法を示しています。 実装するサービス`Services.IDataAccess`コンポーネントのプロパティには挿入`DataRepository`します。 コードがのみを使用する方法に注意してください、`IDataAccess`抽象化します。

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

内部的には、生成されたプロパティ (`DataRepository`) で修飾されたが、`InjectAttribute`属性。 通常、この属性は、直接使用されません。 基底クラスはコンポーネントに必要な場合に挿入されたプロパティは、基底クラスに必要なも`InjectAttribute`手動で追加することができます。

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

基本クラスから派生したコンポーネントで、`@inject`ディレクティブは必要ありません。 `InjectAttribute`の基本クラスで十分です。

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>サービスの依存関係の挿入

複雑なサービスには、その他のサービスが必要です。 前の例では、`DataAccess`必要があります、`HttpClient`既定のサービスです。 `@inject` (または`InjectAttribute`) はサービスで使用するために使用できません。 *コンス トラクターの挿入*代わりに使用する必要があります。 必要なサービスを追加するには、サービスのコンス トラクターにパラメーターを追加します。 依存関係の挿入は、サービスを作成するときに、コンス トラクターで必要し、するにはそれに応じて、サービスを認識します。

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

コンス トラクターの挿入の前提条件:

* 1 つのコンス トラクター引数はすべてによって満たすことが依存関係の挿入が必要があります。 既定値を指定する場合、DI でカバーされない追加のパラメーターが許可されることに注意してください。
* 該当するコンス トラクターがある必要があります*パブリック*します。
* 該当するコンス トラクターの 1 つだけあります。 DI は、あいまいさが発生した場合、例外をスローします。

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
