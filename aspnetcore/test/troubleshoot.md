---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895329"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="066e1-103">ASP.NET Core プロジェクトをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="066e1-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="066e1-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="066e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="066e1-105">次のリンクは、トラブルシューティングの指針を提供します。</span><span class="sxs-lookup"><span data-stu-id="066e1-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="066e1-106">NDC カンファレンス (ロンドン、2018):ASP.NET Core アプリケーションで問題を診断します。</span><span class="sxs-lookup"><span data-stu-id="066e1-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="066e1-107">ASP.NET ブログ:ASP.NET Core のパフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="066e1-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="066e1-108">.NET core SDK の警告</span><span class="sxs-lookup"><span data-stu-id="066e1-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="066e1-109">32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="066e1-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="066e1-110">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="066e1-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="066e1-111">32 と 64 ビット版の両方、.NET Core SDK がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="066e1-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="066e1-112">インストールされている 64 ビット バージョンからのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="066e1-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="066e1-113">この警告が表示されるときに 32 ビット (x86) と 64 ビット (x64) 版の両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="066e1-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="066e1-114">両方のバージョンがインストールされている可能性がありますが、一般的な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="066e1-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="066e1-115">最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーをダウンロードが間でコピーし、64 ビット コンピューターにインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="066e1-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="066e1-116">別のアプリケーションで 32 ビットの .NET Core SDK がインストールされました。</span><span class="sxs-lookup"><span data-stu-id="066e1-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="066e1-117">間違ったバージョンがダウンロードされ、インストールされています。</span><span class="sxs-lookup"><span data-stu-id="066e1-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="066e1-118">この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="066e1-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="066e1-119">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="066e1-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="066e1-120">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="066e1-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="066e1-121">複数の場所に、.NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="066e1-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="066e1-122">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="066e1-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="066e1-123">.NET Core SDK は、複数の場所にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="066e1-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="066e1-124">インストールされた SDK からのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="066e1-124">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="066e1-125">.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合、このメッセージが表示 *c:\\Program Files\\dotnet\\sdk\\* します。</span><span class="sxs-lookup"><span data-stu-id="066e1-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="066e1-126">通常は、.NET Core SDK が、MSI インストーラーではなくコピー/貼り付けを使用して、マシンにデプロイされている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="066e1-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="066e1-127">すべて 32 ビット .NET Core Sdk とランタイムをこの警告を防ぐためにアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="066e1-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="066e1-128">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="066e1-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="066e1-129">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="066e1-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="066e1-130">.NET Core Sdk は検出されませんでした。</span><span class="sxs-lookup"><span data-stu-id="066e1-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="066e1-131">Visual Studio で**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="066e1-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="066e1-132">.NET Core Sdk は検出されませんでした、環境変数に含まれていることを確認`PATH`します。</span><span class="sxs-lookup"><span data-stu-id="066e1-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="066e1-133">実行するときに、`dotnet`コマンドとして、警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="066e1-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="066e1-134">インストールされている、dotnet Sdk を検索できませんでした。</span><span class="sxs-lookup"><span data-stu-id="066e1-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="066e1-135">これらの警告が表示されるときに、環境変数`PATH`コンピューターの任意の .NET Core 用 Sdk を指していません。</span><span class="sxs-lookup"><span data-stu-id="066e1-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="066e1-136">この問題を解決するには。</span><span class="sxs-lookup"><span data-stu-id="066e1-136">To resolve this problem:</span></span>

* <span data-ttu-id="066e1-137">.NET Core SDK をインストールします。</span><span class="sxs-lookup"><span data-stu-id="066e1-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="066e1-138">最新のインストーラーを入手[.NET ダウンロード](https://dotnet.microsoft.com/download)します。</span><span class="sxs-lookup"><span data-stu-id="066e1-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="066e1-139">いることを確認、 `PATH` SDK がインストールされている場所を指す環境変数 (`C:\Program Files\dotnet\`の 64 ビット x64 または`C:\Program Files (x86)\dotnet\`の 32 ビット x86)。</span><span class="sxs-lookup"><span data-stu-id="066e1-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="066e1-140">通常、SDK インストーラーを設定、`PATH`します。</span><span class="sxs-lookup"><span data-stu-id="066e1-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="066e1-141">常に同じコンピューターに同じビット Sdk とランタイムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="066e1-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="066e1-142">.NET Core ホスティング バンドルをインストールした後、不足している SDK</span><span class="sxs-lookup"><span data-stu-id="066e1-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="066e1-143">インストール、 [.NET Core ホスティング バンドル](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)変更、 `PATH` .NET Core の 32 ビット (x86) バージョンを指すように、.NET Core ランタイムをインストールするときに (`C:\Program Files (x86)\dotnet\`)。</span><span class="sxs-lookup"><span data-stu-id="066e1-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="066e1-144">これにより、Sdk が不足しているときに、32 ビット (x86) .NET Core`dotnet`コマンドを使用して ([.NET Core Sdk が検出された](#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="066e1-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="066e1-145">この問題を解決するには、次のように移動します。`C:\Program Files\dotnet\`前の位置に`C:\Program Files (x86)\dotnet\`上、`PATH`します。</span><span class="sxs-lookup"><span data-stu-id="066e1-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="066e1-146">アプリからデータを取得する</span><span class="sxs-lookup"><span data-stu-id="066e1-146">Obtain data from an app</span></span>

<span data-ttu-id="066e1-147">アプリが要求に対応できる場合は、ミドルウェアを使用してアプリから、次のデータを取得できます。</span><span class="sxs-lookup"><span data-stu-id="066e1-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="066e1-148">要求&ndash;メソッド、スキーム、ホスト、pathbase、パス、クエリ文字列、ヘッダー</span><span class="sxs-lookup"><span data-stu-id="066e1-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="066e1-149">接続&ndash;リモート IP アドレス、リモート ポート、ローカル IP アドレス、ローカル ポート、クライアント証明書</span><span class="sxs-lookup"><span data-stu-id="066e1-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="066e1-150">Identity&ndash;名、表示名</span><span class="sxs-lookup"><span data-stu-id="066e1-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="066e1-151">構成設定</span><span class="sxs-lookup"><span data-stu-id="066e1-151">Configuration settings</span></span>
* <span data-ttu-id="066e1-152">環境変数</span><span class="sxs-lookup"><span data-stu-id="066e1-152">Environment variables</span></span>

<span data-ttu-id="066e1-153">次の配置[ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)コードの先頭に、`Startup.Configure`メソッドの要求処理パイプライン。</span><span class="sxs-lookup"><span data-stu-id="066e1-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="066e1-154">ミドルウェアを実行して、コードが開発環境でのみ実行されることを確認する前に、環境がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="066e1-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="066e1-155">環境を取得するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="066e1-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="066e1-156">挿入、`IHostingEnvironment`に、`Startup.Configure`メソッドと、ローカル変数と環境をチェックします。</span><span class="sxs-lookup"><span data-stu-id="066e1-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="066e1-157">次のサンプル コードでは、この方法を示します。</span><span class="sxs-lookup"><span data-stu-id="066e1-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="066e1-158">環境を内のプロパティに割り当てる、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="066e1-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="066e1-159">プロパティを使用して環境を確認してください (たとえば、 `if (Environment.IsDevelopment())`)。</span><span class="sxs-lookup"><span data-stu-id="066e1-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
