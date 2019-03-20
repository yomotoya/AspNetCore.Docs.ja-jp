---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: 1f53375a6aa504c989920a0bff83b400b7bd83ec
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208844"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="48b63-103">ASP.NET Core プロジェクトをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="48b63-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="48b63-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48b63-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48b63-105">次のリンクは、トラブルシューティングの指針を提供します。</span><span class="sxs-lookup"><span data-stu-id="48b63-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="48b63-106">NDC カンファレンス (ロンドン、2018):ASP.NET Core アプリケーションで問題を診断します。</span><span class="sxs-lookup"><span data-stu-id="48b63-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="48b63-107">ASP.NET ブログ:ASP.NET Core のパフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="48b63-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="48b63-108">.NET core SDK の警告</span><span class="sxs-lookup"><span data-stu-id="48b63-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="48b63-109">32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="48b63-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="48b63-110">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48b63-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="48b63-111">32 と 64 ビット版の両方、.NET Core SDK がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="48b63-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="48b63-112">インストールされている 64 ビット バージョンからのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48b63-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="48b63-114">この警告が表示されるときに 32 ビット (x86) と 64 ビット (x64) 版の両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="48b63-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="48b63-115">両方のバージョンがインストールされている可能性がありますが、一般的な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="48b63-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="48b63-116">最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーをダウンロードが間でコピーし、64 ビット コンピューターにインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="48b63-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="48b63-117">別のアプリケーションで 32 ビットの .NET Core SDK がインストールされました。</span><span class="sxs-lookup"><span data-stu-id="48b63-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="48b63-118">間違ったバージョンがダウンロードされ、インストールされています。</span><span class="sxs-lookup"><span data-stu-id="48b63-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="48b63-119">この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="48b63-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="48b63-120">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="48b63-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="48b63-121">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="48b63-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="48b63-122">複数の場所に、.NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="48b63-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="48b63-123">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48b63-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="48b63-124">.NET Core SDK は、複数の場所にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="48b63-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="48b63-125">インストールされた SDK からのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48b63-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="48b63-127">.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合、このメッセージが表示 *c:\\Program Files\\dotnet\\sdk\\* します。</span><span class="sxs-lookup"><span data-stu-id="48b63-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="48b63-128">通常は、.NET Core SDK が、MSI インストーラーではなくコピー/貼り付けを使用して、マシンにデプロイされている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="48b63-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="48b63-129">すべて 32 ビット .NET Core Sdk とランタイムをこの警告を防ぐためにアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="48b63-129">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="48b63-130">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="48b63-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="48b63-131">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="48b63-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="48b63-132">.NET Core Sdk は検出されませんでした。</span><span class="sxs-lookup"><span data-stu-id="48b63-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="48b63-133">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48b63-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="48b63-134">.NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="48b63-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="48b63-136">場合にこの警告が表示されます、環境変数`PATH`コンピューターの任意の .NET Core 用 Sdk を指していません (たとえば、`C:\Program Files\dotnet\`と`C:\Program Files (x86)\dotnet\`)。</span><span class="sxs-lookup"><span data-stu-id="48b63-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="48b63-137">この問題を解決するには。</span><span class="sxs-lookup"><span data-stu-id="48b63-137">To resolve this problem:</span></span>

* <span data-ttu-id="48b63-138">インストールまたは .NET Core SDK がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="48b63-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="48b63-139">最新のインストーラーを入手[.NET ダウンロード](https://dotnet.microsoft.com/download)します。</span><span class="sxs-lookup"><span data-stu-id="48b63-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="48b63-140">いることを確認、`PATH`環境変数が、SDK がインストールされている場所を指します。</span><span class="sxs-lookup"><span data-stu-id="48b63-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="48b63-141">インストーラーが通常の設定、`PATH`します。</span><span class="sxs-lookup"><span data-stu-id="48b63-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="48b63-142">アプリからデータを取得する</span><span class="sxs-lookup"><span data-stu-id="48b63-142">Obtain data from an app</span></span>

<span data-ttu-id="48b63-143">アプリが要求に対応できる場合は、ミドルウェアを使用してアプリから、次のデータを取得できます。</span><span class="sxs-lookup"><span data-stu-id="48b63-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="48b63-144">要求&ndash;メソッド、スキーム、ホスト、pathbase、パス、クエリ文字列、ヘッダー</span><span class="sxs-lookup"><span data-stu-id="48b63-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="48b63-145">接続&ndash;リモート IP アドレス、リモート ポート、ローカル IP アドレス、ローカル ポート、クライアント証明書</span><span class="sxs-lookup"><span data-stu-id="48b63-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="48b63-146">Identity&ndash;名、表示名</span><span class="sxs-lookup"><span data-stu-id="48b63-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="48b63-147">構成設定</span><span class="sxs-lookup"><span data-stu-id="48b63-147">Configuration settings</span></span>
* <span data-ttu-id="48b63-148">環境変数</span><span class="sxs-lookup"><span data-stu-id="48b63-148">Environment variables</span></span>

<span data-ttu-id="48b63-149">次の配置[ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)コードの先頭に、`Startup.Configure`メソッドの要求処理パイプライン。</span><span class="sxs-lookup"><span data-stu-id="48b63-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="48b63-150">ミドルウェアを実行して、コードが開発環境でのみ実行されることを確認する前に、環境がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="48b63-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="48b63-151">環境を取得するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="48b63-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="48b63-152">挿入、`IHostingEnvironment`に、`Startup.Configure`メソッドと、ローカル変数と環境をチェックします。</span><span class="sxs-lookup"><span data-stu-id="48b63-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="48b63-153">次のサンプル コードでは、この方法を示します。</span><span class="sxs-lookup"><span data-stu-id="48b63-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="48b63-154">環境を内のプロパティに割り当てる、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="48b63-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="48b63-155">プロパティを使用して環境を確認してください (たとえば、 `if (Environment.IsDevelopment())`)。</span><span class="sxs-lookup"><span data-stu-id="48b63-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
