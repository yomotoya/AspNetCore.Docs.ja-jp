---
title: "ASP.NET から ASP.NET Core 2.0 への移行"
author: isaac2004
description: "このリファレンス ドキュメントでは、既存の ASP.NET MVC または Web API アプリケーションを ASP.NET Core 2.0 に移行するときのガイダンスを示します。"
keywords: "ASP.NET Core,MVC,移行"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/proper-to-2x/index
ms.openlocfilehash: 6e32ee64970bac5d7f09207e570f543477745b60
ms.sourcegitcommit: 8cafdd1dd409d5070d227100ba0e094c779ac47b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="a710d-104">ASP.NET から ASP.NET Core 2.0 への移行</span><span class="sxs-lookup"><span data-stu-id="a710d-104">Migrating from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="a710d-105">著者: [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="a710d-105">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="a710d-106">この記事は、ASP.NET アプリケーションを ASP.NET Core 2.0 に移行するための参考ガイドです。</span><span class="sxs-lookup"><span data-stu-id="a710d-106">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a710d-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a710d-107">Prerequisites</span></span>

* <span data-ttu-id="a710d-108">[.NET Core 2.0.0 SDK](https://dot.net/core) 以降。</span><span class="sxs-lookup"><span data-stu-id="a710d-108">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>
* <span data-ttu-id="a710d-109">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) バージョン 15.3 以降。</span><span class="sxs-lookup"><span data-stu-id="a710d-109">[Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) version 15.3 or later with the **ASP.NET and web development** workload.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="a710d-110">ターゲット フレームワーク</span><span class="sxs-lookup"><span data-stu-id="a710d-110">Target Frameworks</span></span>
<span data-ttu-id="a710d-111">ASP.NET Core 2.0 プロジェクトを使うと、開発者は、.NET Core と .NET Framework のどちらか一方または両方を対象にして柔軟に開発できます。</span><span class="sxs-lookup"><span data-stu-id="a710d-111">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="a710d-112">最も適切なターゲット フレームワークの決定については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-112">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="a710d-113">.NET Framework を対象にする場合は、プロジェクトで個々の NuGet パッケージを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a710d-113">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="a710d-114">.NET Core を対象にすると、ASP.NET Core 2.0 [メタパッケージ](xref:fundamentals/metapackage)のおかげで、さまざまな明示的パッケージ参照をしなくて済みます。</span><span class="sxs-lookup"><span data-stu-id="a710d-114">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="a710d-115">`Microsoft.AspNetCore.All` メタパッケージをプロジェクトにインストールします。</span><span class="sxs-lookup"><span data-stu-id="a710d-115">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="a710d-116">メタパッケージを使うと、メタパッケージ内で参照されているパッケージはアプリでは展開されません。</span><span class="sxs-lookup"><span data-stu-id="a710d-116">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="a710d-117">.NET Core ランタイム ストアにはこれらのアセットが含まれており、パフォーマンス向上のためにプリコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="a710d-117">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="a710d-118">詳しくは、「[Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage)」(ASP.NET Core 2.x 用 Microsoft.AspNetCore.All メタパッケージ) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-118">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="a710d-119">プロジェクトの構造の違い</span><span class="sxs-lookup"><span data-stu-id="a710d-119">Project structure differences</span></span>
<span data-ttu-id="a710d-120">*.csproj* ファイルの形式は、ASP.NET Core では簡素化されています。</span><span class="sxs-lookup"><span data-stu-id="a710d-120">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="a710d-121">いくつかの重要な変更は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a710d-121">Some notable changes include:</span></span>
- <span data-ttu-id="a710d-122">ファイルがプロジェクトの一部と見なされるためにファイルを明示的に含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a710d-122">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="a710d-123">これにより、大規模なチームで作業する場合に XML のマージが競合するリスクが軽減されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-123">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="a710d-124">他のプロジェクトを GUID で参照することはなくなり、ファイルの読みやすさが向上します。</span><span class="sxs-lookup"><span data-stu-id="a710d-124">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="a710d-125">Visual Studio でアンロードせずにファイルを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="a710d-125">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Visual Studio 2017 の CSPROJ の編集コンテキスト メニュー オプション](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="a710d-127">Global.asax ファイルの置換</span><span class="sxs-lookup"><span data-stu-id="a710d-127">Global.asax file replacement</span></span>
<span data-ttu-id="a710d-128">ASP.NET Core では、アプリをブートストラップする新しいメカニズムが導入されました。</span><span class="sxs-lookup"><span data-stu-id="a710d-128">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="a710d-129">ASP.NET アプリケーションのエントリ ポイントは、*Global.asax* ファイルです。</span><span class="sxs-lookup"><span data-stu-id="a710d-129">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="a710d-130">ルート構成、フィルター、領域の登録などのタスクは、*Global.asax* ファイルで処理されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-130">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

<span data-ttu-id="a710d-131">[!code-csharp[Main](samples/globalasax-sample.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-131">[!code-csharp[Main](samples/globalasax-sample.cs)]</span></span>

<span data-ttu-id="a710d-132">このアプローチでは、アプリケーションとその展開先のサーバーが、実装を妨げるような方法で結合されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-132">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="a710d-133">結合を切り離すため、複数のフレームワークを一緒に使うさらにクリーンな方法を提供する [OWIN](http://owin.org/) が導入されました。</span><span class="sxs-lookup"><span data-stu-id="a710d-133">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="a710d-134">OWIN は、必要なモジュールのみを追加するためのパイプラインを提供します。</span><span class="sxs-lookup"><span data-stu-id="a710d-134">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="a710d-135">ホスティング環境は、[Startup](xref:fundamentals/startup) 関数を取得して、サービスとアプリの要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="a710d-135">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="a710d-136">`Startup` は、ミドルウェアのセットをアプリケーションに登録します。</span><span class="sxs-lookup"><span data-stu-id="a710d-136">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="a710d-137">アプリケーションは、要求ごとに、既存のハンドラーのセットに対するリンク リストのヘッド ポインターを指定して、各ミドルウェア コンポーネントを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a710d-137">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="a710d-138">各ミドルウェア コンポーネントは、要求処理パイプラインに 1 つ以上のハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a710d-138">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="a710d-139">これは、新しいリストのヘッドであるハンドラーへの参照を返すことによって行われます。</span><span class="sxs-lookup"><span data-stu-id="a710d-139">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="a710d-140">各ハンドラーは、リスト内の次のハンドラーを記憶して呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a710d-140">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="a710d-141">ASP.NET Core では、アプリケーションへのエントリ ポイントは `Startup` であり、*Global.asax* に依存する必要はなくなりました。</span><span class="sxs-lookup"><span data-stu-id="a710d-141">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="a710d-142">.NET Framework で OWIN を使うときは、パイプラインとして次のようなものを使います。</span><span class="sxs-lookup"><span data-stu-id="a710d-142">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

<span data-ttu-id="a710d-143">[!code-csharp[Main](samples/webapi-owin.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-143">[!code-csharp[Main](samples/webapi-owin.cs)]</span></span>

<span data-ttu-id="a710d-144">これにより既定のルートが構成され、既定では Json 経由の XmlSerialization です。</span><span class="sxs-lookup"><span data-stu-id="a710d-144">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="a710d-145">必要に応じて、このパイプラインに他のミドルウェアを追加します (サービスの読み込み、構成設定、静的ファイルなど)。</span><span class="sxs-lookup"><span data-stu-id="a710d-145">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="a710d-146">ASP.NET Core は同様のアプローチを使いますが、エントリを処理するために OWIN には依存しません。</span><span class="sxs-lookup"><span data-stu-id="a710d-146">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="a710d-147">代わりに、(コンソール アプリケーションと同じように) *Program.cs* の `Main` メソッドを通して行われ、そこから `Startup` が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a710d-147">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

<span data-ttu-id="a710d-148">[!code-csharp[Main](samples/program.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-148">[!code-csharp[Main](samples/program.cs)]</span></span>

<span data-ttu-id="a710d-149">`Startup` は、`Configure` メソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="a710d-149">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="a710d-150">`Configure` では、必要なミドルウェアをパイプラインに追加します。</span><span class="sxs-lookup"><span data-stu-id="a710d-150">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="a710d-151">(既定の Web サイト テンプレートからの) 次の例では、複数の拡張メソッドを使って、以下をサポートするパイプラインが構成されています。</span><span class="sxs-lookup"><span data-stu-id="a710d-151">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="a710d-152">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="a710d-152">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="a710d-153">エラー ページ</span><span class="sxs-lookup"><span data-stu-id="a710d-153">Error pages</span></span>
* <span data-ttu-id="a710d-154">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="a710d-154">Static files</span></span>
* <span data-ttu-id="a710d-155">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a710d-155">ASP.NET Core MVC</span></span>
* <span data-ttu-id="a710d-156">同一。</span><span class="sxs-lookup"><span data-stu-id="a710d-156">Identity</span></span>

<span data-ttu-id="a710d-157">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span><span class="sxs-lookup"><span data-stu-id="a710d-157">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span></span>

<span data-ttu-id="a710d-158">ホストとアプリケーションは切り離されており、将来別のプラットフォームに柔軟に移動できます。</span><span class="sxs-lookup"><span data-stu-id="a710d-158">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="a710d-159">**注:** ASP.NET Core のスタートアップとミドルウェアについて詳しくは、「[Application Startup in ASP.NET Core](xref:fundamentals/startup)」(ASP.NET Core でのアプリケーションのスタートアップ) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-159">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="a710d-160">保存の構成</span><span class="sxs-lookup"><span data-stu-id="a710d-160">Storing Configurations</span></span>
<span data-ttu-id="a710d-161">ASP.NET では保存の設定がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a710d-161">ASP.NET supports storing settings.</span></span> <span data-ttu-id="a710d-162">これらの設定は、たとえば、アプリケーションが展開された環境のサポートに使われます。</span><span class="sxs-lookup"><span data-stu-id="a710d-162">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="a710d-163">一般的な方法は、すべてのカスタム キー/値ペアを、*Web.config* ファイルの `<appSettings>` セクションに保存するというものでした。</span><span class="sxs-lookup"><span data-stu-id="a710d-163">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

<span data-ttu-id="a710d-164">[!code-xml[Main](samples/webconfig-sample.xml)]</span><span class="sxs-lookup"><span data-stu-id="a710d-164">[!code-xml[Main](samples/webconfig-sample.xml)]</span></span>

<span data-ttu-id="a710d-165">アプリケーションでは、`System.Configuration` 名前空間内の `ConfigurationManager.AppSettings` コレクションを使ってこれらの設定を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="a710d-165">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

<span data-ttu-id="a710d-166">[!code-csharp[Main](samples/read-webconfig.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-166">[!code-csharp[Main](samples/read-webconfig.cs)]</span></span>

<span data-ttu-id="a710d-167">ASP.NET Core では、アプリケーションの構成データを任意のファイルに保存し、ミドルウェアのブートストラップの一部として読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="a710d-167">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="a710d-168">プロジェクト テンプレートで使われる既定のファイルは、*appsettings.json* です。</span><span class="sxs-lookup"><span data-stu-id="a710d-168">The default file used in the project templates is *appsettings.json*:</span></span>

<span data-ttu-id="a710d-169">[!code-json[Main](samples/appsettings-sample.json)]</span><span class="sxs-lookup"><span data-stu-id="a710d-169">[!code-json[Main](samples/appsettings-sample.json)]</span></span>

<span data-ttu-id="a710d-170">アプリケーション内部の `IConfiguration` のインスタンスにこのファイルを読み込むには、*Startup.cs* が使われます。</span><span class="sxs-lookup"><span data-stu-id="a710d-170">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

<span data-ttu-id="a710d-171">[!code-csharp[Main](samples/startup-builder.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-171">[!code-csharp[Main](samples/startup-builder.cs)]</span></span>

<span data-ttu-id="a710d-172">アプリでは、`Configuration` から読み取って設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="a710d-172">The app reads from `Configuration` to get the settings:</span></span>

<span data-ttu-id="a710d-173">[!code-csharp[Main](samples/read-appsettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-173">[!code-csharp[Main](samples/read-appsettings.cs)]</span></span>

<span data-ttu-id="a710d-174">このアプローチにはプロセスをより堅牢にする拡張機能があります。たとえば、[依存性の注入](xref:fundamentals/dependency-injection) (DI) を使ってサービスとこれらの値を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="a710d-174">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="a710d-175">DI アプローチは、厳密に型指定された構成オブジェクトのセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="a710d-175">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="a710d-176">**注:** ASP.NET Core の構成について詳しくは、「[Configuration in ASP.NET Core](xref:fundamentals/configuration)」(ASP.NET Core の構成) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-176">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="a710d-177">ネイティブな依存性の注入</span><span class="sxs-lookup"><span data-stu-id="a710d-177">Native Dependency Injection</span></span>
<span data-ttu-id="a710d-178">大規模で拡張性の高いアプリケーションを構築するときの重要な目標は、コンポーネントとサービスの疎な結合です。</span><span class="sxs-lookup"><span data-stu-id="a710d-178">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="a710d-179">[依存性の注入](xref:fundamentals/dependency-injection)はこれを実現するための一般的な手法であり、ASP.NET Core のネイティブなコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="a710d-179">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="a710d-180">ASP.NET アプリケーションでは、開発者はサードパーティのライブラリに依存して依存性の注入を実装します。</span><span class="sxs-lookup"><span data-stu-id="a710d-180">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="a710d-181">[Unity](https://github.com/unitycontainer/unity) はそのようなライブラリの 1 つであり、Microsoft Patterns & Practices によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-181">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="a710d-182">Unity で依存性の注入を設定する例は、`UnityContainer` をラップする `IDependencyResolver` の実装です。</span><span class="sxs-lookup"><span data-stu-id="a710d-182">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

<span data-ttu-id="a710d-183">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-183">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span></span>

<span data-ttu-id="a710d-184">`UnityContainer` のインスタンスを作成し、サービスを登録して、`HttpConfiguration` の依存関係リゾルバーをコンテナー用の `UnityResolver` の新しいインスタンスに設定します。</span><span class="sxs-lookup"><span data-stu-id="a710d-184">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

<span data-ttu-id="a710d-185">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-185">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span></span>

<span data-ttu-id="a710d-186">必要な場所に `IProductRepository` を挿入します。</span><span class="sxs-lookup"><span data-stu-id="a710d-186">Inject `IProductRepository` where needed:</span></span>

<span data-ttu-id="a710d-187">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-187">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span></span>

<span data-ttu-id="a710d-188">依存性の注入は ASP.NET Core の一部であるため、*Startup.cs* の `ConfigureServices` メソッドに独自のサービスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a710d-188">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="a710d-189">[!code-csharp[Main](samples/configure-services.cs)]</span><span class="sxs-lookup"><span data-stu-id="a710d-189">[!code-csharp[Main](samples/configure-services.cs)]</span></span>

<span data-ttu-id="a710d-190">Unity でそうであったように、リポジトリは任意の場所に挿入できます。</span><span class="sxs-lookup"><span data-stu-id="a710d-190">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="a710d-191">**注:** ASP.NET Core での依存性の注入について詳しくは、「[Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)」(ASP.NET Core での依存性の注入) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-191">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="a710d-192">静的ファイルの提供</span><span class="sxs-lookup"><span data-stu-id="a710d-192">Serving Static Files</span></span>
<span data-ttu-id="a710d-193">Web 開発の重要な部分は、静的なクライアント側アセットを提供する機能です。</span><span class="sxs-lookup"><span data-stu-id="a710d-193">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="a710d-194">静的なファイルの最も一般的な例は、HTML、CSS、Javascript、およびイメージです。</span><span class="sxs-lookup"><span data-stu-id="a710d-194">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="a710d-195">これらのファイルは、アプリ (または CDN) の公開された場所に保存され、要求によって読み込めるように参照される必要があります。</span><span class="sxs-lookup"><span data-stu-id="a710d-195">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="a710d-196">このプロセスは、ASP.NET Core で変更されました。</span><span class="sxs-lookup"><span data-stu-id="a710d-196">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="a710d-197">ASP.NET では、静的ファイルはさまざまなディレクトリに保存され、ビューで参照されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-197">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="a710d-198">ASP.NET Core では、構成が変更されていない限り、静的ファイルは "Web ルート" (*&lt;コンテンツ ルート&gt;/wwwroot*) に保存されます。</span><span class="sxs-lookup"><span data-stu-id="a710d-198">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="a710d-199">ファイルは、`Startup.Configure` から `UseStaticFiles` 拡張メソッドを呼び出すことによって、要求パイプラインに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a710d-199">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="a710d-200">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="a710d-200">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="a710d-201">**注:** .NET Framework を対象にする場合は、NuGet パッケージ `Microsoft.AspNetCore.StaticFiles` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a710d-201">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="a710d-202">たとえば、*wwwroot/images* フォルダー内のイメージ アセットには、ブラウザーから `http://<app>/images/<imageFileName>` などの場所でアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="a710d-202">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="a710d-203">**注:** ASP.NET Core での静的ファイルの提供について詳しくは、「[Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files)」(ASP.NET Core での静的ファイルの使用の概要) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a710d-203">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a710d-204">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a710d-204">Additional Resources</span></span>
* [<span data-ttu-id="a710d-205">.NET Core にライブラリを移植する</span><span class="sxs-lookup"><span data-stu-id="a710d-205">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
