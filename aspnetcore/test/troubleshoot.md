---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899290"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core プロジェクトをトラブルシューティングします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

次のリンクは、トラブルシューティングの指針を提供します。

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC カンファレンス (ロンドン、2018):ASP.NET Core アプリケーションで問題を診断します。](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET ブログ:ASP.NET Core のパフォーマンスの問題のトラブルシューティング](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK の警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> 32 と 64 ビット版の両方、.NET Core SDK がインストールされます。 インストールされている 64 ビット バージョンからのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

この警告が表示されるときに 32 ビット (x86) と 64 ビット (x64) 版の両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。 両方のバージョンがインストールされている可能性がありますが、一般的な理由は次のとおりです。

* 最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーをダウンロードが間でコピーし、64 ビット コンピューターにインストールされていること。
* 別のアプリケーションで 32 ビットの .NET Core SDK がインストールされました。
* 間違ったバージョンがダウンロードされ、インストールされています。

この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>複数の場所に、.NET Core SDK がインストールされています。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> .NET Core SDK は、複数の場所にインストールされます。 インストールされた SDK からのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合、このメッセージが表示 *c:\\Program Files\\dotnet\\sdk\\* します。 通常は、.NET Core SDK が、MSI インストーラーではなくコピー/貼り付けを使用して、マシンにデプロイされている場合に発生します。

この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。 アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。 警告が発生した理由とその影響を理解している場合は、警告を無視できます。

### <a name="no-net-core-sdks-were-detected"></a>.NET Core Sdk は検出されませんでした。

**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。

> .NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

場合にこの警告が表示されます、環境変数`PATH`コンピューターの任意の .NET Core 用 Sdk を指していません (たとえば、`C:\Program Files\dotnet\`と`C:\Program Files (x86)\dotnet\`)。 この問題を解決するには。

* インストールまたは .NET Core SDK がインストールされていることを確認します。 最新のインストーラーを入手[.NET ダウンロード](https://dotnet.microsoft.com/download)します。 
* いることを確認、`PATH`環境変数が、SDK がインストールされている場所を指します。 インストーラーが通常の設定、`PATH`します。

## <a name="obtain-data-from-an-app"></a>アプリからデータを取得する

アプリが要求に対応できる場合は、ミドルウェアを使用してアプリから、次のデータを取得できます。

* 要求&ndash;メソッド、スキーム、ホスト、pathbase、パス、クエリ文字列、ヘッダー
* 接続&ndash;リモート IP アドレス、リモート ポート、ローカル IP アドレス、ローカル ポート、クライアント証明書
* Identity&ndash;名、表示名
* 構成設定
* 環境変数

次の配置[ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)コードの先頭に、`Startup.Configure`メソッドの要求処理パイプライン。 ミドルウェアを実行して、コードが開発環境でのみ実行されることを確認する前に、環境がチェックされます。

環境を取得するには、次の方法のいずれかを使用します。

* 挿入、`IHostingEnvironment`に、`Startup.Configure`メソッドと、ローカル変数と環境をチェックします。 次のサンプル コードでは、この方法を示します。

* 環境を内のプロパティに割り当てる、`Startup`クラス。 プロパティを使用して環境を確認してください (たとえば、 `if (Environment.IsDevelopment())`)。

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
