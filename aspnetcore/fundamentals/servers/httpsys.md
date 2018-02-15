---
title: "ASP.NET Core での HTTP.sys Web サーバーの実装"
author: rick-anderson
description: "Windows 上の ASP.NET Core 用 Web サーバーである HTTP.sys について紹介します。 HTTP.sys は、Http.Sys カーネル モード ドライバーに基づいて構築され、IIS なしでインターネットに直接接続するために使用できる Kestrel の代替製品です。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core での HTTP.sys Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> このトピックは、ASP.NET Core 2.0 以降にのみ適用されます。 以前のバージョンの ASP.NET Core では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) という名前です。

HTTP.sys は Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](index.md)です。 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づいて構築されています。 HTTP.sys は、Kestel では提供されないいくつかの機能を提供する [Kestrel](kestrel.md) の代替製品です。 **HTTP.sys は [ASP.NET Core モジュール](aspnet-core-module.md)と互換性がないため、IIS や IIS Express で使用することはできません。**

HTTP.sys は、次の機能をサポートします。

- [Windows 認証](xref:security/authentication/windowsauth)
- ポート共有
- SNI を使用する HTTPS
- HTTP/2 over TLS (Windows 10)
- 直接ファイル伝送
- 応答キャッシュ
- WebSocket (Windows 8)

サポートされている Windows バージョン:

- Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-httpsys"></a>HTTP.sys を使用するタイミング

HTTP.sys は、IIS を使用せずにインターネットにサーバーを直接公開する必要がある展開に役立ちます。

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

HTTP.sys は Http.Sys に基づいて構築されているため、攻撃から保護するためのリバース プロキシ サーバーは必要ありません。 Http.Sys とは、さまざまな種類の攻撃から保護し、完全な機能を備えた Web サーバーの堅牢性、セキュリティ、およびスケーラビリティを提供する成熟したテクノロジです。 IIS 自体が Http.Sys 上で HTTP リスナーとして実行されています。 

HTTP.sys は、Windows 認証など、Kestrel で使用できない機能が必要な場合の内部展開に適しています。

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>HTTP.sys の使用方法

ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要について説明します。

### <a name="configure-windows-server"></a>Windows Server を構成する

* [.NET Core](https://www.microsoft.com/net/download/core) や [.NET Framework](https://www.microsoft.com/net/download/framework) など、アプリケーションに必要な .NET のバージョンをインストールします。

* HTTP.sys にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します

   Windows で URL プレフィックスを事前登録していない場合は、管理者特権でアプリケーションを実行する必要があります。 唯一の例外は、1024 より大きいポート番号で (HTTPS ではなく) HTTP を使用して localhost にバインドする場合です。この場合、管理者権限は必要ありません。

   詳細については、この記事で後述する「[URL プレフィックスの事前登録と SSL の構成](#preregister-url-prefixes-and-configure-ssl)」を参照してください。

* トラフィックが HTTP.sys に到達できるようにファイアウォールのポートを開きます。

   *netsh.exe* または [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)を使用できます。

[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>HTTP.sys を使用するように ASP.NET Core アプリケーションを構成する

* [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージを使用する場合は、パッケージのインストールは必要はありません。 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) パッケージはメタパッケージに含まれています。

* 次の例のように、`Main` メソッド内の `WebHostBuilder` で `UseHttpSys` 拡張メソッドを呼び出して、必要な [HTTP.sys オプション](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)を指定します。

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>HTTP.sys オプションを構成する

構成できる HTTP.sys の設定と制限をいくつか以下に示します。

**クライアントの最大接続数**

*Program.cs* で次のコードを使用することで、アプリケーション全体に対して同時に開かれる TCP 接続の最大数を設定できます。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

接続の最大数は既定では無制限 (null) です。

**要求本文の最大サイズ**

既定の要求本文の最大サイズは、30,000,000 バイトです。これは約 28.6 MB になります。

ASP.NET Core MVC アプリでの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 属性を使用することをお勧めします。

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

次の例では、アプリケーション全体を対象にしてすべての要求に対する制約を構成する方法を示しています。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

*Startup.cs* の特定の要求に関する設定をオーバーライドすることができます。

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
アプリケーションで要求の読み取りが開始された後、要求に対する制限を構成しようとすると、例外がスローされます。 `MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを知らせる `IsReadOnly` プロパティがあります。

その他の HTTP.sys オプションについては、「[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)」を参照してください。 

### <a name="configure-urls-and-ports-to-listen-on"></a>リッスンする URL とポートを構成する 

既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。 URL プレフィックスとポートを構成する場合、`UseUrls` 拡張メソッド、`urls` コマンドライン引数、ASPNETCORE_URLS 環境引数、または「[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)」の `UrlPrefixes` プロパティを使用できます。 `UrlPrefixes` を使用するコード例を以下に示します。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

`UrlPrefixes` の利点は、正しくフォーマットされていないプレフィックスを追加しようとした場合にすぐにエラー メッセージが表示されることです。 `UseUrls` (`urls` と ASPNETCORE_URLS で共有) の利点は、Kestrel と HTTP.sys をより簡単に切り替えられることです。

`UseUrls` (または `urls` あるいは ASPNETCORE_URLS) と `UrlPrefixes` を両方使用する場合、`UrlPrefixes` の設定で `UseUrls` の設定がオーバーライドされます。 詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。

HTTP.sys では、[HTTP サーバー API の UrlPrefix 文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)が使用されます。

> [!NOTE]
> サーバーで事前登録する `UseUrls` または `UrlPrefixes` に同じプレフィックス文字列を指定してください。 

### <a name="dont-use-iis"></a>IIS を使用しない

アプリケーションが IIS または IIS Express を実行するように構成されていないことを確認します。

Visual Studio では、既定の起動プロファイルは IIS Express 用です。 プロジェクトをコンソール アプリケーションとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更します。

![コンソール アプリのプロファイルを選択する](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL プレフィックスの事前登録と SSL の構成

IIS と HTTP.sys はいずれも、要求をリッスンして初期処理を行うために、基になる Http.Sys カーネル モード ドライバーに依存しています。 IIS では、管理 UI を使用すると、すべての構成を比較的簡単に実行できます。 ただし、Http.Sys を自分で構成する必要があります。 この処理に使用する組み込みツールは *netsh.exe* です。 

*netsh.exe* を使用して、URL プレフィックスを予約し、SSL 証明書を割り当てることができます。 このツールには管理特権が必要です。

次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小限の値を示しています。

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

次の例は、SSL 証明書を割り当てる方法を示しています。

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

以下は、*netsh.exe* のリファレンス ドキュメントです。

* [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)
* [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)

次のリソースでは、いくつかのシナリオの詳細な手順を説明しています。 HttpListener を参照する記事は、両方とも Http.Sys に基づいているため、HTTP.sys に同様に適用されます。

* [方法 : SSL 証明書を使用してポートを構成する](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通信 - HttpListener ベースのホスティングとクライアントの認証): サードパーティのブログであり、かなり古い投稿ですが、有用な情報が含まれています。
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (方法: SSL Simple Server として HttpListener または Http Server アンマネージ コード (C++) を使用するチュートリアル): これも古いブログですが、有用な情報が含まれています。

*netsh.exe* コマンド ラインよりも使いやすいサードパーティ製ツールをいくつか紹介します。 これらのツールは Microsoft 製ではなく、Microsoft が推奨するものでもありません。 *netsh.exe* 自体に管理者特権が必要なので、これらのツールは既定で管理者として実行されます。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) には、SSL 証明書とオプション、プレフィックス予約、および証明書信頼リストを一覧表示および構成するための UI があります。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) では、SSL 証明書と URL プレフィックスを一覧表示したり、設定することができます。 UI は http.sys Manager より洗練されており、公開されている構成オプションも少し多くなっていますが、その他の機能は同様です。 新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のものを割り当てることができます。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys のソース コード](https://github.com/aspnet/HttpSysServer/)
* [ホスティング](xref:fundamentals/hosting)
