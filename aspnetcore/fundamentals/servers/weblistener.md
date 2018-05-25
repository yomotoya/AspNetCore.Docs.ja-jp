---
title: ASP.NET Core への WebListener Web サーバーの実装
author: rick-anderson
description: IIS なしでインターネットに直接接続するために使用できる Windows 上の ASP.NET Core の Web サーバーである WebListener について説明します。
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core への WebListener Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> このトピックは、ASP.NET Core 1.x にのみ適用されます。 ASP.NET Core 2.0 では、WebListener は [HTTP.sys](httpsys.md) と呼ばれます。

WebListener は Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](index.md)です。 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づいて構築されています。 WebListener は、IIS に依存せずにリバース プロキシ サーバーとしてインターネットに直接接続するために使用できる [Kestrel](kestrel.md) の代替製品です。 実際、**WebListener は [ASP.NET Core モジュール](aspnet-core-module.md)と互換性がないため、IIS または IIS Express で使用することはできません。**

WebListener は ASP.NET Core 用に開発されましたが、[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージを介して任意の .NET Core または.NET Framework アプリケーションで直接使用できます。

WebListener は、次の機能をサポートします。

- [Windows 認証](xref:security/authentication/windowsauth)
- ポート共有
- SNI を使用する HTTPS
- HTTP/2 over TLS (Windows 10)
- 直接ファイル伝送
- 応答キャッシュ
- WebSocket (Windows 8)

サポートされている Windows バージョン:

- Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-weblistener"></a>WebListener を使用する場合

WebListener は、IIS を使用せずにインターネットにサーバーを直接公開する必要がある展開に役立ちます。

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

WebListener は Http.Sys に基づいて構築されているため、攻撃から保護するためのリバース プロキシ サーバーは必要ありません。 Http.Sys とは、さまざまな種類の攻撃から保護し、完全な機能を備えた Web サーバーの堅牢性、セキュリティ、およびスケーラビリティを提供する成熟したテクノロジです。 IIS 自体が Http.Sys 上で HTTP リスナーとして実行されています。 

WebListener は、Kestrel を使用して取得できない機能のいずれかが必要な場合の内部展開にも適しています。

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>WebListener を使用する方法

ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要について説明します。

### <a name="configure-windows-server"></a>Windows Server を構成する

* [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)、.NET Framework 4.5.1 など、アプリケーションに必要な .NET のバージョンをインストールします。

* WebListener にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します

   Windows で URL プレフィックスを事前登録していない場合は、管理者特権でアプリケーションを実行する必要があります。 唯一の例外は、1024 より大きいポート番号で (HTTPS ではなく) HTTP を使用して localhost にバインドする場合です。この場合、管理者権限は必要ありません。

   詳細については、この記事で後述する「[URL プレフィックスの事前登録と SSL の構成](#preregister-url-prefixes-and-configure-ssl)」を参照してください。

* トラフィックが WebListener に到達できるようにファイアウォールのポートを開きます。

   netsh.exe または [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)を使用できます。

[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。

### <a name="configure-your-aspnet-core-application"></a>ASP.NET Core アプリケーションを構成する

* NuGet パッケージ [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/) をインストールします。 これで、依存関係として [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) もインストールされます。

* 次の例で示すように、必要な WebListener [オプション](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)と[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)を指定して、`Main` メソッド内で [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に対して `UseWebListener` 拡張メソッドを呼び出します。

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* リッスンする URL とポートを構成する 

  既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。 URL プレフィックスとポートを構成するには、`UseURLs` 拡張メソッド、`urls` コマンド ライン引数、または ASP.NET Core 構成システムを使用できます。 詳しくは、「[ASP.NET Core でのホスティング]\(xref:fundamentals/host/index)」をご覧ください。

  Web Listener は、[Http.Sys プレフィックス文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)を使用します。 WebListener に固有のプレフィックス文字列形式の要件はありません。

  > [!WARNING]
  > 最上位のワイルドカードのバインド (`http://*:80/` と `http://+:80`) は使用しては**いけません**。 最上位のワイルドカードのバインドは、セキュリティの脆弱性に対してアプリを切り開くことができます。 これは、強力と脆弱の両方のワイルドカードに適用されます。 ワイルドカードではなく、明示的なホスト名を使用します。 全体の親ドメインを制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.mysub.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。 詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。

  > [!NOTE]
  > サーバーで事前登録する `UseUrls` に同じプレフィックス文字列を指定してください。 

* アプリケーションが IIS または IIS Express を実行するように構成されていないことを確認します。

  Visual Studio では、既定の起動プロファイルは IIS Express 用です。  プロジェクトをコンソール アプリケーションとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更する必要があります。

  ![コンソール アプリのプロファイルを選択する](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>ASP.NET Core の外部で WebListener を使用する方法

* [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージをインストールします。

* ASP.NET Core で使用する場合と同様に、[WebListener にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します](#preregister-url-prefixes-and-configure-ssl)。

[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。


次のコード サンプルは、ASP.NET Core の外部で WebListener を使用する方法を示しています。

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL プレフィックスの事前登録と SSL の構成

IIS と WebListener はいずれも、要求をリッスンして初期処理を行うために、基になる Http.Sys カーネル モード ドライバーに依存しています。 IIS では、管理 UI を使用すると、すべての構成を比較的簡単に実行できます。 ただし、WebListener を使用している場合は、Http.Sys を自分で構成する必要があります。 この処理に使用する組み込みツールは netsh.exe です。 

netsh.exe を使用するために必要な最も一般的なタスクは、URL プレフィックスの予約と SSL 証明書の割り当てです。

NetSh.exe は、初心者向けの簡単なツールではありません。 次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小限の値を示しています。

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

次の例は、SSL 証明書を割り当てる方法を示しています。

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

公式のリファレンス ドキュメントは次のとおりです。

* [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)
* [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)

次のリソースでは、いくつかのシナリオの詳細な手順を説明しています。 `HttpListener` を参照する記事は、両方とも Http.Sys に基づいているため、`WebListener` に同様に適用されます。

* [方法 : SSL 証明書を使用してポートを構成する](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通信 - HttpListener ベースのホスティングとクライアントの認証): サードパーティのブログであり、かなり古い投稿ですが、有用な情報が含まれています。
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (方法: SSL Simple Server として HttpListener または Http Server アンマネージ コード (C++) を使用するチュートリアル): これも古いブログですが、有用な情報が含まれています。
* [How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (SSL を使用して.NET Core WebListener を設定する方法)

netsh.exe コマンド ラインよりも使いやすいサードパーティ製ツールをいくつか紹介します。 これらのツールは Microsoft 製ではなく、Microsoft が推奨するものでもありません。 netsh.exe 自体に管理者特権が必要なので、これらのツールは既定で管理者として実行されます。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) には、SSL 証明書とオプション、プレフィックス予約、および証明書信頼リストを一覧表示および設定するための UI があります。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) では、SSL 証明書と URL プレフィックスを一覧表示したり、設定することができます。 UI は http.sys Manager より洗練されており、公開されている構成オプションも少し多くなっていますが、その他の機能は同様です。 新しい証明書信頼リスト (CTL) を作成することはできませんが、既存の証明書信頼リストを割り当てることができます。

自己署名 SSL 証明書を生成する場合は、Microsoft が提供するコマンド ラインツールである [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) と PowerShell コマンドレットの [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate) を利用できます。 また、自己署名 SSL 証明書を簡単に生成できるサードパーティ製 UI ツールもあります。

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener ソース コード](https://github.com/aspnet/HttpSysServer/)
* [ホスティング](xref:fundamentals/host/index)
