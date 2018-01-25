---
title: "ASP.NET Core の依存関係の挿入"
author: ardalis
description: "ASP.NET Core が依存関係の挿入を実装する方法とその使用方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a5a0991694b2c7caa79dbc09f6471d614f67dac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>ASP.NET Core の依存関係の挿入の概要

<a name="fundamentals-dependency-injection"></a>

によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)

ASP.NET Core は、まったく新たに設計をサポートし、依存関係の挿入を活用します。 ASP.NET Core アプリケーションは、スタートアップ クラス内のメソッドを挿入することによってサービスを組み込みフレームワークを利用でき、挿入もアプリケーション サービスを構成することができます。 ASP.NET Core によって提供される既定のサービス コンテナーは、最小限の機能が設定され、その他のコンテナーを置換するためのものではありませんを提供します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-dependency-injection"></a>依存関係の挿入とは何ですか。

依存性の注入 (DI) は、オブジェクトとの共同作業者、または依存関係の間の疎結合を実現するための手法です。 、の共同作業者を直接インスタンス化または静的参照を使用して、ではなく、クラスは、そのアクションを実行する必要があるオブジェクトは、なんらかの方法でクラスに提供されます。 クラスに従うように、コンス トラクターを使用して、その依存関係を宣言する、ほとんどの場合、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。 この方法は、「コンス トラクター インジェクション」と呼ばれます。

クラスは、注意 DI でデザインされます、ときにしているより疎結合された共同作業者に直接、ハード コーディングされた依存関係がないためです。 次のように、[依存関係の逆転原則](http://deviq.com/dependency-inversion-principle/)、ことを示している*"高レベルのモジュールは、低レベルのモジュールに依存しないでください抽象化の両方を利用する必要があります。"。* クラスが抽象化を要求する特定の実装を参照するには、代わりに (通常`interfaces`) クラスを作成するときに提供されます。 インターフェイスへの依存関係を抽出し、これらのインターフェイスのパラメーターとして実装することはの例ではまた、[戦略の設計パターン](http://deviq.com/strategy-design-pattern/)です。

DI を使用する、システムの設計時に、コンス トラクター (またはプロパティ) を使用して、その依存関係を要求する多くのクラスが関連付けられている依存関係とこれらのクラスを作成するのには専用のクラスをするおくと便利です。 これらのクラスと呼びます*コンテナー*、または具体的には、[制御の反転 (IoC)](http://deviq.com/inversion-of-control/)や依存関係の挿入 (DI) コンテナーのコンテナーです。 コンテナーとは、そこから要求された型のインスタンスを提供するファクトリを本質的にします。 場合は、指定された型は、依存関係があるし、依存関係の種類を提供するコンテナーが構成されていることを宣言しましたが、要求されたインスタンスの作成の一環として依存関係が作成されます。 これにより、複雑な依存関係グラフは、ハード コーディングされたオブジェクトの構築が必要ないクラスに提供できます。 オブジェクトをその依存関係を作成するだけでなくコンテナーは通常、アプリケーション内でオブジェクトの有効期間を管理します。

ASP.NET Core に簡単なビルトイン コンテナーが含まれています (によって表される、`IServiceProvider`インターフェイス) 既定では、コンス トラクターの挿入をサポートして、ASP.NET により、特定のサービスが DI を介して使用できます。 ASP です。NET のコンテナーとして管理されます。 その型を指す*services*です。 この記事の残りの部分全体にわたって*services*は ASP.NET Core IoC コンテナーで管理されている型を参照してください。 組み込みのコンテナーのサービスを構成する、`ConfigureServices`で、アプリケーションの`Startup`クラスです。

> [!NOTE]
> Martin ファウラー上の広範な資料が書き込ま[逆転のコントロール コンテナーとの依存関係挿入パターン](https://www.martinfowler.com/articles/injection.html)です。 最適な説明も Microsoft Patterns and Practices[依存性の注入](https://msdn.microsoft.com/library/hh323705.aspx)です。

> [!NOTE]
> この記事では、すべての ASP.NET アプリケーションに適用される、依存関係の挿入がについて説明します。 MVC コント ローラー内の依存関係の挿入は、「[依存関係挿入とコント ローラー](../mvc/controllers/dependency-injection.md)です。

### <a name="constructor-injection-behavior"></a>コンス トラクター インジェクションの動作

コンス トラクターの挿入は、対象のコンス トラクターがある必要があります*パブリック*です。 それ以外の場合、アプリがスローされます、 `InvalidOperationException`:

> 型 '' の適切なコンス トラクターが見つかりませんでした。 具体的な型とサービスに登録されて、パブリック コンス トラクターのすべてのパラメーターを確認します。


コンス トラクターの挿入では、その 1 つだけ適用可能なコンス トラクターの存在が必要です。 コンス トラクター オーバー ロードがサポートされますが、引数のすべてで満足できる依存関係の挿入のみ 1 つのオーバー ロードが存在できます。 1 つ以上存在する場合、アプリがスローされます、 `InvalidOperationException`:

> 型 '' には、すべての指定した引数型を受け付ける複数のコンス トラクターが検出されました。 適用可能なコンス トラクターの 1 つだけあります。

コンス トラクターは、依存関係の挿入では提供されない引数を受け入れることができますが、これらの既定値をサポートする必要があります。 例:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Framework が提供するサービスを使用します。

`ConfigureServices`メソッドで、`Startup`クラスは Entity Framework Core および ASP.NET Core MVC などのプラットフォーム機能を含む、アプリケーションで使用するサービスの定義を担当します。 最初に、`IServiceCollection`に提供される`ConfigureServices`が定義されている次のサービス (に応じて[ホストの構成方法](xref:fundamentals/hosting))。

| サービスの種類 | 有効期間 |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | シングルトン |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | シングルトン |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | シングルトン |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | 一時的なもの |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | 一時的なもの |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | シングルトン |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | シングルトン |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | 一時的なもの |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | シングルトン |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | 一時的なもの |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | シングルトン |

以下のように拡張メソッドの番号を使用してコンテナーに追加のサービスを追加する方法の例は、 `AddDbContext`、 `AddIdentity`、および`AddMvc`です。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

機能と ASP.NET、MVC などによって提供されるミドルウェアを 1 つの追加を使用する規則に従って*ServiceName*その機能に必要なサービスをすべて登録する拡張メソッド。

>[!TIP]
> 内の特定のフレームワークに用意されているサービスを要求する`Startup`パラメーター リストで使用してメソッドを参照してください[アプリケーションの起動](startup.md)詳細についてはします。

## <a name="registering-your-own-services"></a>独自のサービスを登録します。

次のように、独自のアプリケーション サービスを登録できます。 最初のジェネリック型では、コンテナーから要求される型 (通常はインターフェイス) を表します。 2 番目のジェネリック型では、コンテナーによってインスタンス化してこのような要求を満たすために使用される具体的な型を表します。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> 各`services.Add<ServiceName>`拡張メソッドを追加 (および可能性のある構成) サービス。 たとえば、 `services.AddMvc()` MVC が必要とするサービスを追加します。 拡張メソッドを配置することをこの規則に従うお勧め、`Microsoft.Extensions.DependencyInjection`名前空間には、サービス登録のグループをカプセル化します。

`AddTransient`抽象型を必要とするすべてのオブジェクトとは別にインスタンス化される具体的なサービスにマップするメソッドを使用します。 これは、サービスと呼ばれます*有効期間*、追加の有効期間オプションは次に説明します。 ことが重要、各サービスを登録するための適切な有効期間を選択します。 それを要求する各クラスに、サービスの新しいインスタンスを提供しますか。 指定された web 要求全体で、1 つのインスタンスを使用する必要がありますか。 または、アプリケーションの有効期間の 1 つのインスタンスを使用します必要がありますか。

この記事のサンプルと呼ばれる文字名を表示する単純なコント ローラーが`CharactersController`です。 その`Index`メソッドは、現在のアプリケーションで格納されている文字の一覧を表示しが存在しない場合に、いくつかの文字を使用して、コレクションを初期化します。 このアプリケーションは、Entity Framework のコアを使用して、`ApplicationDbContext`のいずれも、永続化のためのクラス、コント ローラーで明らかです。 特定のデータ アクセスのメカニズムが代わりに、インターフェイスの背後に抽象化されて`ICharacterRepository`、どの次のように、[リポジトリ パターン](http://deviq.com/repository-pattern/)です。 インスタンス`ICharacterRepository`コンス トラクターを使用して要求し、必要に応じて文字へのアクセスを使用して、プライベート フィールドに割り当てられます。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository`コント ローラーを使用する必要がある 2 つのメソッドを定義`Character`インスタンス。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

このインターフェイスは、具象型で実装に`CharacterRepository`、実行時に使用されます。

> [!NOTE]
> DI 方法を使用、`CharacterRepository`クラスは、すべての「リポジトリ」] または [データ アクセス クラスではなく、アプリケーション サービスを行うことができる汎用モデル。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

なお`CharacterRepository`要求、`ApplicationDbContext`のコンス トラクターにします。 連鎖的に、次のように要求された依存関係は、独自の依存関係をさらに要求するごとに使用する依存関係の挿入の異常なことはできません。 コンテナーは、すべてのグラフ内の依存関係を解決し、完全に解決済みのサービスを返すことを担当します。

> [!NOTE]
> 要求されたオブジェクトとすべてのオブジェクトを必要としてすべての必要なものは、オブジェクトの作成とも呼ば、*オブジェクト グラフ*です。 同様に、解決する必要がある依存関係の集合を通常呼びます、*依存関係ツリー*または*依存関係グラフ*です。

この場合、両方`ICharacterRepository`さらに`ApplicationDbContext`でサービス コンテナーに登録されている必要があります`ConfigureServices`で`Startup`です。 `ApplicationDbContext`拡張メソッドの呼び出しで構成されている`AddDbContext<T>`です。 次のコードの登録を示しています、`CharacterRepository`型です。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework コンテキストは、サービスを使用してコンテナーに追加するか、`Scoped`有効期間。 これは、処理の完了に自動的に上記のように、これらのヘルパー メソッドを使用する場合。 Entity Framework を使用すると、リポジトリは、同じ有効期間を使用してください。

>[!WARNING]
> 注意しなければならないメイン危険性を解決する、`Scoped`単一サービスです。 このような場合は後続の要求を処理するときに、サービスが正しくない状態を持つこと可能性があります。

依存関係のあるサービスをコンテナーに登録する必要があります。 かどうか、サービスのコンス トラクターが必要です、プリミティブなど、`string`を使用してこれを挿入できます[構成](xref:fundamentals/configuration/index)と[オプション パターン](xref:fundamentals/configuration/options)です。

## <a name="service-lifetimes-and-registration-options"></a>サービスの有効期間と登録オプション

ASP.NET サービスは、次の有効期間で構成できます。

**一時的なもの**

一時的な有効期間サービスは、要求されている各時に作成されます。 この有効期間は軽量のステートレスなサービスに最適です。

**スコープ**

有効期間のスコープを持つサービスは、要求あたり 1 回作成されます。

**シングルトン**

シングルトン有効期間サービスが要求されている最初の時に作成されます (または`ConfigureServices`インスタンスがありますを指定する場合に実行) し、すべての後続の要求が同じインスタンスを使用します。 アプリケーションは、シングルトン動作を必要とする場合は、シングルトン デザイン パターンを実装して、自分でクラスのオブジェクトの有効期間を管理するのではなく勧めサービス コンテナー サービスの有効期間を管理できるようにします。

サービスは、いくつかの方法で、コンテナーに登録することができます。 使用する具体的な種類を指定して型を指定して、サービスの実装を登録する方法が既にあります。 さらに、ファクトリを指定できます、し、要求時にインスタンスの作成に使用されます。 3 番目のアプローチでは、直接使用するには型のインスタンスをコンテナーの場合は、インスタンスを作成することはありません試みます (も、インスタンスが破棄されます) を指定します。

これらの有効期間と登録のオプションの違いを示すためには、1 つまたは複数のタスクを表す単純なインターフェイスを検討してください、*操作*一意の識別子を持つ`OperationId`します。 このサービスの有効期間を構成して方法に応じて、コンテナーは要求元のクラスにサービスの同じまたは別のインスタンスを提供します。 オフにする有効期間が要求されているように、お lifetime オプションごとに 1 つの型が作成されます。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

1 つのクラスを使用してこれらのインターフェイスを実装して`Operation`を受け入れる、`Guid`コンス トラクター、または使用して、新しい`Guid`がない場合。

次に、 `ConfigureServices`、各種類は、名前付きの有効期間に従って、コンテナーに追加します。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

なお、`IOperationSingletonInstance`サービスが特定のインスタンスを使用して既知の ID を持つ`Guid.Empty`のため、この型を (その Guid はすべて 0 になります) を使用するとき、明確になります。 登録されていること、`OperationService`他のそれぞれに依存する`Operation`ことができるように、要求内でチェック ボックスをオフこのサービスが、コント ローラー、または操作の種類ごとに、新しいものと同じインスタンスを取得するかどうかを入力します。 このサービスはすべて、ビューが表示されるように、その依存関係をプロパティとして公開します。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

内でのアプリケーションに個別の個々 の要求間およびオブジェクトの有効期間を示すためには、サンプルが含まれています、`OperationsController`各種類の要求を`IOperation`型だけでなく、`OperationService`です。 `Index`すべてのコント ローラーのおよびサービスのアクションで、表示`OperationId`値。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

今すぐこのコント ローラーのアクションを次の 2 つの個別の要求が行われます。

![一時的なスコープ ベース、シングルトン、およびインスタンスのコント ローラーおよび最初の要求でサービス操作の操作の操作の ID 値 (GUID) を示す Microsoft Edge で実行されている依存関係の挿入のサンプル web アプリケーションの操作ビューです。](dependency-injection/_static/lifetimes_request1.png)

![操作は、2 番目の要求の操作の ID 値が表示を表示します。](dependency-injection/_static/lifetimes_request2.png)

確認のうち、`OperationId`および要求間で、要求内に値が異なります。

* *一時的な*オブジェクトが異なる常に以外の新しいインスタンスはすべてのコント ローラーおよびすべてのサービスに提供します。

* *スコープ*オブジェクトは、異なる要求間で異なるが、要求内で同じです。

* *シングルトン*オブジェクトは、すべてのオブジェクトとすべての要求で同じ (インスタンスがで提供されるかどうかに関係なく`ConfigureServices`)

## <a name="request-services"></a>サービスを要求します。

ASP.NET 内で使用可能なサービスを要求`HttpContext`を通じて公開される、`RequestServices`コレクション。

![HttpContext 要求サービス Intellisense 要求サービスを取得または要求のサービス コンテナーへのアクセスを提供する IServiceProvider を設定することを示すダイアログ コンテキスト。](dependency-injection/_static/request-services.png)

サービスを要求では、サービスを構成して、アプリケーションの一部として要求を表します。 オブジェクトは、依存関係を指定するときに、これらがで検出された型で満たされます。`RequestServices`ではなく、`ApplicationServices`です。

一般に、クラスのコンス トラクターを使用して必要なクラス型を要求することで、フレームワークのこれらの依存関係を挿入できるようにすることは、直接、これらのプロパティを使用しないでください。 簡単にテストされるクラスが生成されます。 (を参照してください[テスト](../testing/index.md)) より疎結合されたとします。

> [!NOTE]
> アクセスにコンス トラクターのパラメーターとしての依存関係の要求を必要に応じて、`RequestServices`コレクション。

## <a name="designing-your-services-for-dependency-injection"></a>依存関係の挿入で、サービスの設計

共同作業者を取得する依存関係の挿入を使用するサービスを設計する必要があります。 つまり、ステートフルな静的メソッドの呼び出しは使用しない (と呼ばれるコード匂いが発生する[静的窓](http://deviq.com/static-cling/)) と、サービス内の依存クラスを直接インスタンス化します。 やすくなり、語句を注意してください[は、New はグルー](https://ardalis.com/new-is-glue)型のインスタンスを作成するか、依存関係の挿入を使用して要求するかを選択するときに、します。 に従って、[実線の基本原則のオブジェクト指向設計](http://deviq.com/solid/)クラスが必然的に小さく、十分に考慮された、簡単にテストする傾向があります。

場合、クラスが多すぎる依存関係が挿入されていることになる傾向がありますを検索しますか。 これは、クラスが多すぎる、実行しようとし、SRP - に違反する可能性がありますが、サインインでは、通常、[単一責任の原則](http://deviq.com/single-responsibility-principle/)です。 新しいクラスにその負荷の一部を移動することによって、クラスをリファクタリングすることができるかどうかを参照してください。 注意してください、`Controller`クラス必要があるに重点を置いて UI の問題、これらに適切なクラスにビジネス ルールとデータ アクセスの実装の詳細を保持する必要がありますので[に関する注意事項を区切る](http://deviq.com/separation-of-concerns/)です。

データ アクセスに関する具体的には、挿入することできます、 `DbContext` 、コント ローラーに (と仮定した場合にある services コンテナに追加した EF `ConfigureServices`)。 一部の開発者がデータベースへのリポジトリ インターフェイスの使用を好むを挿入することではなく、`DbContext`直接です。 1 か所でアクセス ロジックのデータをカプセル化するインターフェイスを使用して、データベースが変更されたときに変更する必要がどのようにさまざまな場所が最小化できます。

### <a name="disposing-of-services"></a>サービスの破棄

コンテナーが呼び出す`Dispose`の`IDisposable`型が作成されます。 ただし場合は、自分自身を追加するインスタンスのコンテナーに、これは破棄されません。

例:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> バージョン 1.0 の場合で、コンテナーはで dispose を呼び出す*すべて* `IDisposable` 、それを作成していないものを含むオブジェクト。

## <a name="replacing-the-default-services-container"></a>既定のサービス コンテナーを置き換える

フレームワークと上に構築されたほとんどのコンシューマー アプリケーションの基本的なニーズに対応するものでは、組み込みのサービス コンテナー。 ただし、開発者は、推奨されるコンテナーで組み込みのコンテナーを置き換えることができます。 `ConfigureServices`メソッドは通常を返します`void`を返すシグネチャが変更された場合、 `IServiceProvider`、別のコンテナーを構成し、返されます。 多くの IOC コンテナーは .NET を使用できます。 この例では、 [Autofac](https://autofac.org/)パッケージを使用します。

最初に、適切なコンテナーのパッケージをインストールします。

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

次に、構成内のコンテナー`ConfigureServices`を返すと、 `IServiceProvider`:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> サード パーティ製 DI コンテナーを使用して、変更する必要あります`ConfigureServices`を返すように`IServiceProvider`の代わりに`void`です。

通常どおり Autofac を最後に、構成`DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

実行時に、Autofac 型を解決して、依存関係の挿入に使用されます。 [Autofac および ASP.NET Core の使用に関する詳細については](http://docs.autofac.org/en/latest/integration/aspnetcore.html)します。

### <a name="thread-safety"></a>スレッド セーフ

シングルトン サービスは、スレッド セーフである必要があります。 一時的なサービスのシングルトン サービスに依存した場合、一時的なサービスもスレッドによって、単一の使用方法に応じてセーフである必要があります。

## <a name="recommendations"></a>推奨事項

依存関係の挿入を使用する場合は、次の推奨事項に留意してください。

* DI では、複雑な依存関係のあるオブジェクト用です。 コント ローラー、サービス、アダプター、およびリポジトリ di が追加されるオブジェクトのすべての例に示します。

* DI で直接データと構成を保存しないでください。 たとえば、ユーザーのショッピング カートは、通常サービス コンテナーに追加べきではありません。 構成を使用する必要があります、[オプション パターン](xref:fundamentals/configuration/options)です。 同様に、その他のオブジェクトへのアクセスを許可するだけに存在する「データ ホルダー」オブジェクトは避けてください。 可能であれば、DI、経由で必要な実際のアイテムを要求することをお勧めします。

* 静的なサービスにアクセスしないようにします。

* アプリケーション コードでサービスの場所は避けてください。

* 静的なアクセスを防ぐ`HttpContext`です。

> [!NOTE]
> 推奨事項のすべてのセットと同様に 1 つを無視する必要がある状況が発生する可能性があります。 例外はまれで--framework 自体内のほとんどの場合、非常に特殊なケースが見つかりました。

依存関係の挿入は、*代替*静的/グローバル オブジェクト アクセスのパターンにします。 静的オブジェクトのアクセス権を持つ混在している場合は、DI のメリットを実現することはできません。

## <a name="additional-resources"></a>その他のリソース

* [アプリケーションの起動](startup.md)

* [テスト](../testing/index.md)

* [依存関係の挿入 (MSDN) での ASP.NET Core でクリーンなコードの記述](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [コンテナー管理アプリケーションの設計、前段階: が属しているコンテナーしますか。](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)

* [コントロール コンテナーとの依存関係挿入パターンの反転](https://www.martinfowler.com/articles/injection.html)(ファウラー)
