---
title: "ASP.NET Core でのアプリケーション部分"
author: ardalis
description: "アプリのリソースに対する abstrations は、アプリケーションの構成要素を使用して、検出またはアセンブリからの機能の読み込みを避けるためにアプリを構成する方法を説明します。"
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 702d7773374f331b25489060b18f752186d7acea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="application-parts-in-aspnet-core"></a>ASP.NET Core でのアプリケーション部分

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

*アプリケーション パーツ*MVC のコント ローラー、コンポーネントの表示と同様に機能元となる、アプリケーションのリソースを抽象化は、またはタグ ヘルパーを検出することがあります。 アプリケーション パーツの 1 つの例は、アセンブリ参照と公開型およびコンパイルの参照をカプセル化するの AssemblyPart です。 *機能のプロバイダー* ASP.NET Core MVC アプリの機能を設定するアプリケーション部分と連携します。 アプリケーション パーツの主なユース ケースが検出 (または読み込みを回避する) にアプリケーションを構成できるようにするアセンブリから MVC 機能します。

## <a name="introducing-application-parts"></a>アプリケーション パーツの概要

MVC アプリからその機能を読み込む[アプリケーション パーツ](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)です。 具体的には、 [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)クラスがアセンブリで補助されているアプリケーションの部分を表します。 検出し、コント ローラー、コンポーネントの表示、タグ ヘルパーの場合は、razor コンパイル ソースなどの MVC 機能を読み込むには、これらのクラスを使用できます。 [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) MVC アプリに、アプリケーションの部分と使用可能な機能のプロバイダーを追跡します。 操作できますが、`ApplicationPartManager`で`Startup`MVC を構成する場合。

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

既定では、MVC は依存関係ツリーを検索および (でも他のアセンブリ) コント ローラーを検索します。 (たとえば、コンパイル時に参照されていないプラグイン) から任意のアセンブリを読み込むには、アプリケーションの部分を使用することができます。

アプリケーションの部分で構成を使用することができます*回避*コント ローラーで、特定のアセンブリまたは場所を検索します。 部分 (またはアセンブリ) を利用できますがアプリに変更することによってを制御する、`ApplicationParts`のコレクション、`ApplicationPartManager`です。 内のエントリの順序、`ApplicationParts`コレクションが重要ではありません。 完全に構成することが重要、`ApplicationPartManager`コンテナー内のサービスの構成を使用する前にします。 たとえば、完全に設定します、`ApplicationPartManager`を呼び出す前に`AddControllersAsServices`です。 ためには、失敗していることを意味ことコント ローラー アプリケーション パーツの追加後に、メソッドの呼び出しに影響するされません (サービスとして登録しない)、アプリケーションの不適切な bevavior する可能性があります。

使用したくないコント ローラーを含むアセンブリがあればを削除してから、 `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

プロジェクトのアセンブリとその依存アセンブリに加えて、`ApplicationPartManager`のパーツが含まれます`Microsoft.AspNetCore.Mvc.TagHelpers`と`Microsoft.AspNetCore.Mvc.Razor`既定です。

## <a name="application-feature-providers"></a>アプリケーション機能のプロバイダー

アプリケーション機能のプロバイダーは、アプリケーション部分を調べるし、これらのパーツの機能を提供します。 次の MVC 機能の組み込み機能のプロバイダーがあります。

* [コントローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [メタデータの参照](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [タグ ヘルパー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [コンポーネントの表示](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

機能プロバイダーを継承`IApplicationFeatureProvider<T>`ここで、`T`機能の種類です。 MVC の機能の種類のいずれかのプロバイダーが上に示した独自の機能を実装することができます。 機能プロバイダーでの順序、`ApplicationPartManager.FeatureProviders`以降のプロバイダーは、以前のプロバイダーによって実行されたアクションに対処できるため、コレクションが重要ですが、できます。

### <a name="sample-generic-controller-feature"></a>例: 汎用コント ローラーの機能

既定では、ASP.NET Core MVC には、汎用的なコント ローラーが無視されます (たとえば、 `SomeController<T>`)。 このサンプルは、既定のプロバイダーの後に実行し、型の指定されたリストの汎用的なコント ローラー インスタンスを追加するコント ローラー機能プロバイダーを使用して (で定義されている`EntityTypes.Types`)。

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

エンティティの種類:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

機能プロバイダーを追加`Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

既定では、ルーティングに使用されるジェネリック コント ローラー名と形式にする*GenericController'1 [ウィジェット]*の代わりに*ウィジェット*です。 次の属性を使用して、コント ローラーによって使用されるジェネリック型に対応する名前を変更します。

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController`クラス。

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

結果、一致するルートが要求されたときに:

![サンプル アプリからの出力例を読み取ると、'こんにちはジェネリック Sproket コント ローラーからです '](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>例: 表示の使用可能な機能

反復処理できる使用可能なデータが設定された機能をアプリに要求することによって、`ApplicationPartManager`を通じて[依存性の注入](../../fundamentals/dependency-injection.md)と適切な機能のインスタンスを作成使用します。

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

出力例:

![サンプル アプリからの出力例](app-parts/_static/available-features.png)
