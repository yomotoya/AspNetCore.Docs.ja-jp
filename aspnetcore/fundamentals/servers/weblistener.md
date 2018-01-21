---
title: "ASP.NET Core の WebListener web サーバーの実装"
author: rick-anderson
description: "WebListener、Windows 上の ASP.NET core web サーバーが導入されています。 Http.Sys のカーネル モード ドライバーで WebListener は、IIS なしでインターネットに直接接続に使用することができます Kestrel する代わりにします。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1bdbc723e4602c2e53723aff91ec5d254f4bd93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core の WebListener web サーバーの実装

によって[Tom Dykstra](https://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> このトピックは ASP.NET Core にのみ 1.x です。 ASP.NET Core 2.0 では、WebListener の名前[HTTP.sys](httpsys.md)です。

WebListener は、 [ASP.NET core web server](index.md) Windows だけで実行されています。 に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。 WebListener が代わりに[Kestrel](kestrel.md)リバース プロキシ サーバーとして IIS に依存せず、インターネットに直接接続に使用できます。 実際には、 **WebListener と互換性がないと IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**

経由で任意の .NET Core または .NET Framework アプリケーションで直接使用できますが、WebListener は、ASP.NET Core 用に開発されて、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。

WebListener には、次の機能がサポートされています。

- [Windows 認証](xref:security/authentication/windowsauth)
- ポート共有
- SNI を HTTPS
- Http/2 over TLS (Windows 10)
- ファイルを直接転送
- 応答のキャッシュ
- Websocket (Windows 8)

サポートされている Windows のバージョン:

- Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-weblistener"></a>WebListener を使用する場合

WebListener は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

Http.Sys で用意されているので WebListener は攻撃から保護するため、リバース プロキシ サーバーを必要としません。 Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。 Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。 

WebListener も内部環境に適して Kestrel を使用して取得できない場合、提供される機能のいずれかの操作を必要なとき。

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>WebListener を使用する方法

次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。

### <a name="configure-windows-server"></a>Windows Server を構成します。

* など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)または .NET Framework 4.5.1。

* WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。

   Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。 唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。

   詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。

* WebListener に到達するトラフィックを許可するファイアウォールのポートを開きます。

   Netsh.exe を使用するか、 [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。

[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。

### <a name="configure-your-aspnet-core-application"></a>ASP.NET Core アプリケーションを構成します。

* NuGet パッケージのインストール[Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)です。 これもインストール[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/)依存関係として。

* 呼び出す、`UseWebListener`拡張メソッドを[WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)で、`Main`任意 WebListener を指定して、メソッド[オプション](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)と[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)必要があります。、次の例で示すようにします。

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Url とポートでリッスンするように構成します。 

  既定では ASP.NET Core にバインド`http://localhost:5000`です。 URL プレフィックスとポートを構成するのに使用することができます、`UseURLs`の拡張メソッドで、`urls`コマンドライン引数または ASP.NET Core の構成システムです。 詳細については、[ホスティング](../../fundamentals/hosting.md)に関するページを参照してください。

  リスナーは web、 [Http.Sys プレフィックス文字列書式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。 WebListener に固有のプレフィックス文字列形式の要件はありません。

  > [!NOTE]
  > 内の同じプレフィックス文字列を指定することを確認してください`UseUrls`サーバーに事前登録します。 

* IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。

  Visual Studio では、既定の起動プロファイルは、IIS express はします。  コンソール アプリケーションとして、プロジェクトを実行するには、必要がある、選択したプロファイルを手動で変更する次のスクリーン ショットに示すようにします。

  ![コンソール アプリのプロファイルを選択します。](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>ASP.NET Core の外部で WebListener を使用する方法

* インストール、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。

* [WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録](#preregister-url-prefixes-and-configure-ssl)ASP.NET Core で使用するのと同様です。

[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。


ASP.NET Core の外部で WebListener の使用方法を示すコード サンプルを次に示します。

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL プレフィックスを事前登録し、SSL を構成します。

IIS と WebListener のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。 IIS では、management UI では、すべての構成に比較的簡単な方法です。 ただし、WebListener を使用している場合は、Http.Sys を構成する必要があります。 これを行うための組み込みツールは、netsh.exe です。 

Netsh.exe を使用する必要があります。 最も一般的なタスクは URL プレフィックスを予約して、SSL 証明書を割り当てます。

NetSh.exe は、初心者向けの使いやすいツールではありません。 次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最低限のものを示しています。

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

次の例では、SSL 証明書を割り当てる方法を示します。

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

公式のリファレンス ドキュメントを次に示します。

* [ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 文字列](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

次のリソースは、いくつかのシナリオの詳細な手順を提供します。 参照している記事`HttpListener`に均等に適用`WebListener`Http.Sys に基づいて、両方は、します。

* [方法 : SSL 証明書を使用してポートを構成する](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。
* [方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)有用な情報で以前のブログをすぎますがこれです。
* [SSL を使用して .NET Core WebListener を設定する方法は?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Netsh.exe コマンド ラインよりも簡単に使用できる一部のサード パーティ製ツールを次に示します。 によって提供されるか、Microsoft によって承認されているこれらがありません。 ツールは既定では、管理者として実行ため netsh.exe 自体には、管理者特権が必要です。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。 UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。 新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。

自己署名 SSL 証明書を生成するのには、マイクロソフトは、コマンド ライン ツールを提供しています: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968)し、PowerShell コマンドレット[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)です。 自己署名 SSL 証明書を生成するための簡略化するサード パーティの UI ツールもあります。

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert の UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener ソース コード](https://github.com/aspnet/HttpSysServer/)
* [ホスティング](../hosting.md)
