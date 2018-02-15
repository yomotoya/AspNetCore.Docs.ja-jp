---
title: "ASP.NET Core での依存関係の挿入"
author: ardalis
description: "ASP.NET Core で依存関係の挿入を実装する方法とそれを使う方法について説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: acbce5d139da0acc0870a9cf23a779bf27699a61
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>ASP.NET Core での依存関係の挿入

<a name="fundamentals-dependency-injection"></a>

作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)

ASP.NET Core は、依存関係の挿入をサポートして利用するようにまったく新しく設計されています。 ASP.NET Core アプリケーションは、組み込みフレームワーク サービスをスタートアップ クラスのメソッドに挿入することによってサービスを利用でき、アプリケーション サービスも挿入用に構成することができます。 ASP.NET Core によって提供される既定のサービス コンテナーは、最小限の機能セットを提供するものであり、他のコンテナーの置き換えは意図されていません。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-dependency-injection"></a>依存関係の挿入とは

依存関係の挿入 (DI) とは、オブジェクトとそのコラボレーター (依存関係) との間の疎結合を実現するための手法です。 クラスがそのアクションを実行するために必要なオブジェクトは、コラボレーターを直接インスタンス化したり、静的参照を使ったりするのではなく、それ以外の何らかの方法でクラスに提供されます。 ほとんどの場合、クラスはコンストラクターを使って依存関係を宣言することで、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従うことができます。 この方法は、"コンストラクターの挿入" と呼ばれます。

クラスが DI を考慮して設計されていると、コラボレーターに対して直接的なハード コーディングされた依存関係を持たないので、より弱い結合になります。 これは[依存関係の逆転の原則](http://deviq.com/dependency-inversion-principle/)に従うものです。この原則は、"*高レベルのモジュールは低レベルのモジュールに依存してはならず、どちらも抽象化に依存する必要がある*" というものです。 特定の実装を参照する代わりに、クラスはクラスの構築時に提供される抽象化 (通常は `interfaces`) に要求します。 依存関係をインターフェイスに抽出し、これらのインターフェイスの実装をパラメーターとして提供することも、[戦略設計パターン](http://deviq.com/strategy-design-pattern/)の例です。

システムが DI を使うように設計されていて、多くのクラスがコンストラクター (またはプロパティ) を使って依存関係を要求する場合は、これらのクラスとそれに関連付けられている依存関係を作成する専用のクラスを用意すると便利です。 このようなクラスは、"*コンテナー*"、あるいはさらに具体的に[制御の反転 (IoC)](http://deviq.com/inversion-of-control/) コンテナーまたは依存関係の挿入 (DI) コンテナーと呼ばれます。 コンテナーは基本的に、要求された型のインスタンスを提供するファクトリです。 特定の型が依存関係を持つものとして宣言されていて、コンテナーが依存関係の型を提供するように構成されている場合、コンテナーは要求されたインスタンスの作成の一環として依存関係を作成します。 これにより、複雑な依存関係グラフをクラスに提供でき、ハード コーディングされたオブジェクトの構築は必要ありません。 依存関係を含むオブジェクトを作成するだけでなく、コンテナーは通常、アプリケーション内でのオブジェクトの有効期間を管理します。

ASP.NET Core には既定でコンストラクターの挿入をサポートする簡単な組み込みコンテナーが含まれ (`IServiceProvider` インターフェイスによって表されます)、ASP.NET は DI によって特定のサービスを利用できるようにします。 ASP.NET のコンテナーでは、それが管理する型を "*サービス*" と呼びます。 これ以降この記事では、"*サービス*" は ASP.NET Core の IoC コンテナーによって管理される型を指します。 組み込みコンテナーのサービスは、アプリケーションの `Startup` クラスの `ConfigureServices` メソッドで構成します。

> [!NOTE]
> Martin Fowler は、「[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html)」(制御の反転コンテナーと依存関係の挿入パターン) で広範な記事を書いています。 Microsoft のパターンとプラクティスにも[依存関係の挿入](https://msdn.microsoft.com/library/hh323705.aspx)に関する優れた説明があります。

> [!NOTE]
> この記事では、すべての ASP.NET アプリケーションに適用される依存関係の挿入について説明します。 MVC コントローラーでの依存関係の挿入については、「[Dependency injection into controllers](../mvc/controllers/dependency-injection.md)」(コントローラーへの依存関係の挿入) をご覧ください。

### <a name="constructor-injection-behavior"></a>コンストラクターの挿入の動作

コンストラクターの挿入には、対象のコンストラクターが "*パブリック*" である必要があります。 そうでない場合、アプリは `InvalidOperationException` をスローします。

> A suitable constructor for type 'YourType' couldn't be located. Ensure the type is concrete and services are registered for all parameters of a public constructor. (型 'YourType' の適切なコンストラクターが見つかりませんでした。型が具象であること、およびサービスがパブリック コンストラクターのすべてのパラメーターに登録されていることを確認してください。)


コンストラクターの挿入では、該当するコンストラクターがただ 1 つだけ存在する必要があります。 コンストラクターのオーバーロードはサポートされていますが、依存関係の挿入によってすべての引数を設定できるオーバーロードは 1 つしか存在できません。 複数のコンストラクターが存在する場合、アプリは `InvalidOperationException` をスローします。

> Multiple constructors accepting all given argument types have been found in type 'YourType'. There should only be one applicable constructor. (型 'YourType' には、特定の引数の型をすべて受け付けるコンストラクターが複数存在します。該当するコンストラクターは 1 つだけでなければなりません。)

コンストラクターは、依存関係の挿入によって提供されない引数を受け入れることができますが、既定値をサポートしている必要があります。 例:

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

## <a name="using-framework-provided-services"></a>フレームワークが提供するサービスの使用

`Startup` クラスの `ConfigureServices` メソッドでは、Entity Framework Core や ASP.NET Core MVC といったプラットフォーム機能など、アプリケーションが使うサービスを定義する必要があります。 最初に、`ConfigureServices` に提供される `IServiceCollection` では、次のサービスが定義されています ([ホストの構成方法](xref:fundamentals/hosting)に依存します)。

| サービスの種類 | 有効期間 |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | シングルトン |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | シングルトン |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | シングルトン |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | 一時的 |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | 一時的 |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | シングルトン |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | シングルトン |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | 一時的 |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | シングルトン |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | 一時的 |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | シングルトン |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | シングルトン |

`AddDbContext`、`AddIdentity`、`AddMvc` などの複数の拡張メソッドを使ってコンテナーにサービスを追加する方法の例を次に示します。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

ASP.NET によって提供される機能とミドルウェア (MVC など) は、単一の Add<*サービス名*> 拡張メソッドを使って、その機能に必要なすべてのサービスを登録する規則に従います。

>[!TIP]
> `Startup` メソッドでは、パラメーター リストを使って、フレームワークによって提供される特定のサービスを要求できます。詳細については、[アプリケーションの起動](startup.md)に関するページをご覧ください。

## <a name="registering-services"></a>サービスの登録

次のようにして、独自のアプリケーション サービスを登録できます。 最初のジェネリック型は、コンテナーに要求する型 (通常はインターフェイス) を表します。 2 番目のジェネリック型は、コンテナーによってインスタンス化されてそのような要求を満たすために使われる具象型を表します。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> 各 `services.Add<ServiceName>` 拡張メソッドは、サービスを追加 (および場合によっては構成) します。 たとえば、`services.AddMvc()` は MVC が必要とするサービスを追加します。 この規則に従い、拡張メソッドを `Microsoft.Extensions.DependencyInjection` 名前空間に配置して、サービス登録のグループをカプセル化することをお勧めします。

抽象型を、それを要求するすべてのオブジェクトに対して個別にインスタンス化される具象サービスにマップするには、`AddTransient` メソッドを使います。 これは、サービスの "*有効期間*" と呼ばれます。有効期間の他のオプションについては後で説明します。 登録するサービスごとに適切な有効期間を選ぶことが重要です。 要求するクラスごとに、サービスの新しいインスタンスを提供する必要がありますか。 特定の Web 要求全体で、1 つのインスタンスを使う必要がありますか。 それとも、アプリケーションの有効期間を通して 1 つのインスタンスを使う必要がありますか。

この記事のサンプルには、文字名を表示する `CharactersController` という名前の簡単なコントローラーがあります。 その `Index` メソッドは、アプリケーションに格納されている文字の現在のリストを表示し、存在しない場合はいくつかの文字でコレクションを初期化します。 このアプリケーションは、永続化のために Entity Framework Core と `ApplicationDbContext` クラスを使いますが、いずれもコントローラーですぐにわかるようにはなっていません。 代わりに、特定のデータ アクセス メカニズムがインターフェイス `ICharacterRepository` の背後に抽象化されており、それは[リポジトリ パターン](http://deviq.com/repository-pattern/)に従います。 `ICharacterRepository` のインスタンスがコンストラクターによって要求されてプライベート フィールドに割り当てられ、必要に応じて文字へのアクセスに使われます。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` では、コントローラーが `Character` インスタンスを操作するときに必要な 2 つのメソッドが定義されています。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

このインターフェイスは、実行時に使われる具象型 `CharacterRepository` で実装されています。

> [!NOTE]
> `CharacterRepository` クラスでの DI の使い方は一般的なモデルであり、"リポジトリ" やデータ アクセス クラスだけでなく、すべてのアプリケーション サービスで従うことができます。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

`CharacterRepository` はコンストラクターで `ApplicationDbContext` を要求していることに注意してください。 このように、要求された各依存関係がさらにそれ自身の依存関係を要求するチェーン形式で依存関係の挿入が使われるのは、珍しいことではありません。 コンテナーは、グラフ内のすべての依存関係を解決し、完全に解決されたサービスを返す必要があります。

> [!NOTE]
> 要求されたオブジェクト、それが要求するすべてのオブジェクト、さらにそれらが要求するすべてのオブジェクトを作成する処理は、"*オブジェクト グラフ*" と呼ばれることがあります。 同様に、解決する必要がある依存関係の集合的なセットは、通常、"*依存関係ツリー*" または "*依存関係グラフ*" と呼ばれます。

この場合、`ICharacterRepository` と `ApplicationDbContext` の両方が、`Startup` の `ConfigureServices` でサービス コンテナーに登録されている必要があります。 `ApplicationDbContext` は、拡張メソッド `AddDbContext<T>` を呼び出して構成します。 次のコードでは、`CharacterRepository` 型の登録を示します。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework コンテキストは、`Scoped` 有効期間を使ってサービス コンテナーに追加する必要があります。 上で示したようにヘルパー メソッドを使っている場合は、これは自動的に処理されます。 Entity Framework を利用するリポジトリは、同じ有効期間を使う必要があります。

>[!WARNING]
> 注意しなければならない最大の危険は、シングルトンからの `Scoped` サービスの解決です。 このような場合、後続の要求を処理するときに、サービスが正しくない状態になっている可能性があります。

依存関係のあるサービスは、コンテナーに登録する必要があります。 サービスのコンストラクターでプリミティブ (`string` など) が必要な場合は、[構成](xref:fundamentals/configuration/index)と[オプション パターン](xref:fundamentals/configuration/options)を使ってこれを挿入できます。

## <a name="service-lifetimes-and-registration-options"></a>サービスの有効期間と登録のオプション

ASP.NET サービスは、次の有効期間で構成できます。

**一時的**

有効期間が一時的のサービスは、要求されるたびに作成されます。 この有効期間は、軽量でステートレスのサービスに最適です。

**スコープ**

有効期間がスコープのサービスは、要求ごとに 1 回作成されます。

**シングルトン**

有効期間がシングルトンのサービスは、サービスが初めて要求されたとき (または、インスタンスを指定する場合は `ConfigureServices` が実行されたとき) に作成され、後続のすべての要求で同じインスタンスが使われます。 アプリケーションでシングルトン動作が必要な場合は、シングルトン設計パターンを実装して自分でクラス内のオブジェクトの有効期間を管理するのではなく、サービス コンテナーにサービスの有効期間の管理を任せることをお勧めします。

サービスは、複数の方法でコンテナーに登録できます。 使う具象型を指定することにより特定の型でサービスの実装を登録する方法は既に示しました。 さらに、ファクトリを指定することができます。ファクトリは、必要なときにインスタンスを作成するために使われます。 3 番目の方法は、使う型のインスタンスを直接指定する方法です。この場合、コンテナーはインスタンスの作成を試みません (インスタンスの破棄も行いません)。

これらの有効期間と登録オプションの違いを示すため、1 つまたは複数のタスクを一意の識別子 `OperationId` を持つ "*操作*" として表す簡単なインターフェイスについて考えます。 このサービスの有効期間の構成方法に応じて、コンテナーはサービスの同じインスタンスまたは異なるインスタンスを要求元のクラスに提供します。 要求されている有効期間がはっきりわかるように、有効期間オプションごとに 1 つの型を作成します。

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

これらのインターフェイスを、1 つのクラス `Operation` を使って実装します。このクラスは、コンストラクターで `Guid` を受け取り、提供されない場合は新しい `Guid` を使います。

次に、`ConfigureServices` では、各型が名前で指定されている有効期間に従ってコンテナーに追加されます。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

`IOperationSingletonInstance` サービスは既知の ID `Guid.Empty` を持つ特定のインスタンスを使っているので、この型が使われているときは明確にわかります (その Guid はすべて 0 になります)。 また、他の各 `Operation` 型に依存する `OperationService` も登録してあるので、操作の種類ごとに、このサービスがコントローラーと同じインスタンスを取得しているか、または新しいインスタンスかが、要求内ではっきりわかります。 このサービスが行うことは、依存関係をビューに表示できるように、依存関係をプロパティとして公開することだけです。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

アプリケーションに対する 1 つの要求内でのオブジェクトの有効期間および異なる個別の要求間でのオブジェクトの有効期間を示すため、サンプルには、各種類の `IOperation` 型と `OperationService` を要求する `OperationsController` が含まれます。 その後、`Index` アクションは、すべてのコント ローラーおよびサービスの `OperationId` の値を表示します。

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

ここで、このコントローラー アクションに対して 2 つの異なる要求を行います。

![最初の要求での Transient、Scoped、Singleton、Instance の各コントローラーと OperationService の操作の操作 ID の値 (GUID) が表示されている、Microsoft Edge で実行する DependencyInjectionSample Web アプリケーションの操作ビュー。](dependency-injection/_static/lifetimes_request1.png)

![2 番目の要求の操作 ID の値が表示されている操作ビュー。](dependency-injection/_static/lifetimes_request2.png)

要求内および要求間で変化している `OperationId` の値を確認してください。

* *Transient* オブジェクトは常に異なります。コントローラーごとおよびサービスごとに、新しいインスタンスが提供されます。

* *Scoped* オブジェクトは、要求内では同じですが、異なる要求間では異なります

* *Singleton* オブジェクトは、インスタンスが `ConfigureServices` で提供されるかどうかに関係なく、すべてのオブジェクトとすべての要求について同じです

## <a name="request-services"></a>要求サービス

`HttpContext` からの ASP.NET 要求内で使用可能なサービスは、`RequestServices` コレクションを通じて公開されます。

![要求のサービス コンテナーへのアクセスを提供する IServiceProvider を要求サービスが取得または設定することを示す、HttpContext 要求サービスの Intellisense コンテキスト ダイアログ。](dependency-injection/_static/request-services.png)

要求サービスは、アプリケーションの一部として構成および要求するサービスを表します。 オブジェクトで依存関係を指定すると、これらは `ApplicationServices` ではなく `RequestServices` で検出された型で満たされます。

一般に、これらのプロパティを直接使ってはいけません。代わりに、クラスのコンストラクターを使ってクラスで必要な型を要求し、フレームワークにこれらの依存関係を挿入させます。 このようにすると、クラスはテストしやすくなり (「[テスト](../testing/index.md)」を参照)、より弱い結合になります。

> [!NOTE]
> コンストラクターのパラメーターとして依存関係を要求し、`RequestServices` コレクションにアクセスするようにします。

## <a name="designing-services-for-dependency-injection"></a>依存関係の挿入のためのサービスの設計

依存関係の挿入を使ってコラボレーターを取得するように、サービスを設計する必要があります。 つまり、ステートフルな静的メソッドの呼び出し ([静的な結び付き](http://deviq.com/static-cling/)として知られるコードの状態になります)、およびサービス内での依存クラスの直接インスタンス化は、使わないようにします。 型をインスタンス化するか、それとも依存関係の挿入を使って要求するか迷ったときは、"[new は密接な結び付きを生み出す](https://ardalis.com/new-is-glue)" という格言を思い出すと役に立つことがあります。 [オブジェクト指向設計の SOLID 原則](http://deviq.com/solid/)に従うことで、クラスは必然的に小さく、十分に考慮された、簡単にテストできるものになる可能性が高くなります。

クラスで挿入される依存関係が多すぎる場合はどうすればよいでしょうか。 これは一般に、クラスで行おうとしていることが多すぎるサインであり、SRP ([単一責任の原則](http://deviq.com/single-responsibility-principle/)) に違反する可能性があります。 責任の一部を新しいクラスに移動することにより、クラスをリファクタリングできるかどうか検討します。 `Controller` クラスは UI に関することに集中する必要があり、ビジネス ルールとデータ アクセスの実装の詳細は[問題の分離](http://deviq.com/separation-of-concerns/)に適したクラスに維持する必要があることに留意してください。

特にデータ アクセスに関しては、`DbContext` をコントローラーに挿入できます (`ConfigureServices` のサービス コンテナーに EF を追加した場合)。 `DbContext` を直接挿入するより、データベースへのリポジトリ インターフェイスを使う方が好まれる場合があります。 インターフェイスを使ってデータ アクセス ロジックを 1 か所にカプセル化すると、データベースを変更するときに変更する必要がある場所の数を最少にできます。

### <a name="disposing-of-services"></a>サービスの破棄

コンテナーは、作成する `IDisposable` 型の `Dispose` を呼び出します。 ただし、手動でインスタンスをコンテナーに追加した場合は、破棄されません。

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
> バージョン 1.0 のコンテナーは、コンテナーで作成しなかったものも含めて、"*すべての*" `IDisposable` に対して破棄を呼び出しました。

## <a name="replacing-the-default-services-container"></a>既定のサービス コンテナーの置き換え

組み込みのサービス コンテナーは、フレームワークと、それを基に構築されたほとんどのコンシューマー アプリケーションの、基本的なニーズに対応することを意図したものです。 ただし、開発者は、組み込みのコンテナーを好みのコンテナーで置き換えることができます。 通常、`ConfigureServices` メソッドは `void` を返しますが、`IServiceProvider` を返すようにシグネチャを変更した場合、別のコンテナーを構成して返すことができます。 .NET で使うことができる多くの IOC コンテナーがあります。 この例では、[Autofac](https://autofac.org/) パッケージを使います。

最初に、適切なコンテナー パッケージをインストールします。

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

次に、`ConfigureServices` でコンテナーを構成して、`IServiceProvider` を返します。

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
> サードパーティの DI コンテナーを使うときは、`void` ではなく `IServiceProvider` を返すように、`ConfigureServices` を変更する必要があります。

最後に、`DefaultModule` で Autofac を通常どおり構成します。

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

実行時には、Autofac を使って、型の解決と依存関係の挿入が行われます。 [Autofac と ASP.NET Core の使用については、こちらをご覧ください](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。

### <a name="thread-safety"></a>スレッド セーフ

シングルトン サービスは、スレッド セーフである必要があります。 シングルトン サービスに一時的サービスへの依存関係がある場合、シングルトンによる一時的サービスの使い方によっては、一時的サービスもスレッド セーフであることが必要な場合があります。

## <a name="recommendations"></a>推奨事項

依存関係の挿入を使うときは、次の推奨事項に留意してください。

* DI は、複雑な依存関係のあるオブジェクトのためのものです。 コントローラー、サービス、アダプター、リポジトリはすべて、DI に追加される可能性があるオブジェクトの例です。

* データと構成は、DI に直接格納しないようにします。 たとえば、通常、ユーザーのショッピング カートをサービス コンテナーに追加してはいけません。 構成では、[オプション パターン](xref:fundamentals/configuration/options)を使う必要があります。 同様に、他のオブジェクトへのアクセスを許可するためだけに存在する "データ ホルダー" オブジェクトは避ける必要があります。 可能な場合は、実際に必要なアイテムを DI 経由で要求することをお勧めします。

* サービスへの静的なアクセスを行わないようにします。

* サービスの場所をアプリケーション コード内に記述しないようにします。

* `HttpContext` への静的なアクセスを行わないようにします。

> [!NOTE]
> どのような推奨事項でも、無視する必要がある状況が発生する可能性があります。 例外はまれであることがわかっており、ほとんどはフレームワーク自体内での非常に特殊なケースです。

依存関係の挿入は静的/グローバル オブジェクト アクセス パターンの "*代替機能*" であることを憶えておいてください。 静的オブジェクト アクセスと併用した場合、DI のメリットを実現することはできません。

## <a name="additional-resources"></a>その他の技術情報

* [アプリケーションの起動](xref:fundamentals/startup)
* [テスト](xref:testing/index)
* [依存関係の挿入による ASP.NET Core でのクリーンなコードの作成 (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [コンテナー管理アプリケーションの設計、前段階: コンテナーはどこに属していますか](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)
* [制御の反転コンテナーと依存関係の挿入パターン](https://www.martinfowler.com/articles/injection.html) (Fowler)
