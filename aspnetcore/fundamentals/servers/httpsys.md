---
title: "ASP.NET Core の HTTP.sys web サーバーの実装"
author: rick-anderson
description: "HTTP.sys は、Windows 上の ASP.NET core web サーバーが導入されています。 Http.Sys のカーネル モード ドライバーでビルドする HTTP.sys は、代わりに IIS なしでインターネットに直接接続に使用することができます Kestrel です。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core の HTTP.sys web サーバーの実装

によって[Tom Dykstra](https://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> このトピックには、のみを ASP.NET Core 2.0 以降が適用されます。 以前のバージョンの ASP.NET Core、HTTP.sys が名前付き[WebListener](xref:fundamentals/servers/weblistener)です。

HTTP.sys は、 [ASP.NET core web server](index.md) Windows だけで実行されています。 に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。 HTTP.sys は、代わりに[Kestrel](kestrel.md) Kestel しない一部の機能を提供します。 **HTTP.sys と互換性がありません、IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**

HTTP.sys は、次の機能をサポートします。

- [Windows 認証](xref:security/authentication/windowsauth)
- ポート共有
- SNI を HTTPS
- Http/2 over TLS (Windows 10)
- ファイルを直接転送
- 応答のキャッシュ
- Websocket (Windows 8)

サポートされている Windows のバージョン:

- Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-httpsys"></a>HTTP.sys を使用する場合

HTTP.sys は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

Http.Sys で用意されているので HTTP.sys は、攻撃に対する保護のため、リバース プロキシ サーバーを必要としません。 Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。 Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。 

HTTP.sys をお勧めの内部の展開では使用できない Kestrel、Windows 認証などの機能を必要とするときにします。

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>HTTP.sys を使用する方法

次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。

### <a name="configure-windows-server"></a>Windows Server を構成します。

* など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://www.microsoft.com/net/download/core)または[.NET Framework](https://www.microsoft.com/net/download/framework)です。

* HTTP.sys は、バインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。

   Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。 唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。

   詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。

* HTTP.sys に到達するトラフィックを許可するファイアウォールのポートを開きます。

   使用することができます*netsh.exe*または[PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。

[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>HTTP.sys を使用する ASP.NET Core アプリケーションを構成します。

* 使用する場合は、パッケージのインストールは必要ありません、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage です。 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) metapackage でパッケージが含まれています。

* 呼び出す、`UseHttpSys`拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[HTTP.sys オプション](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)する必要がある、次の例で示すように。

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>HTTP.sys のオプションを構成します。

HTTP.sys の設定および構成できる制限のいくつか示します。

**最大クライアント接続**

次のコードを含むアプリケーション全体の同時実行の開いている TCP 接続の最大数を設定することができます*Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

接続の最大数は、既定で無制限 (null) です。

**最大要求本文のサイズ**

既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。

ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

内の特定の要求に設定を上書きできます*Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。 `IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。

その他の HTTP.sys オプションについては、次を参照してください。 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。 

### <a name="configure-urls-and-ports-to-listen-on"></a>Url とポートでリッスンするように構成します。 

既定では ASP.NET Core にバインド`http://localhost:5000`です。 URL プレフィックスとポートを構成するのに使用することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数では、ASPNETCORE_URLS 環境変数、または`UrlPrefixes`プロパティ[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。 次のコード例では`UrlPrefixes`します。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

利点`UrlPrefixes`が正しくフォーマットされているプレフィックスを追加しようとする場合にすぐに、エラー メッセージを取得することができます。 利点`UseUrls`(とは共有`urls`と ASPNETCORE_URLS) は Kestrel と HTTP.sys の間でより簡単に切り替えることができます。

両方を使用する場合`UseUrls`(または`urls`または ASPNETCORE_URLS) と`UrlPrefixes`、設定`UrlPrefixes`内のオーバーライド`UseUrls`です。 詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。

HTTP.sys を使用して、[文字列形式の HTTP サーバー API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。

> [!NOTE]
> 内の同じプレフィックス文字列を指定することを確認してください`UseUrls`または`UrlPrefixes`サーバーに事前登録します。 

### <a name="dont-use-iis"></a>IIS を使用しません。

IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。

Visual Studio では、既定の起動プロファイルは、IIS express はします。 コンソール アプリケーションとしてプロジェクトを実行するには、手動で変更、選択したプロファイルの次のスクリーン ショットに示すようにします。

![コンソール アプリのプロファイルを選択します。](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>URL プレフィックスを事前登録し、SSL を構成します。

IIS と HTTP.sys のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。 IIS では、management UI では、すべての構成に比較的簡単な方法です。 ただし、Http.Sys を構成する必要があります。 つまりを行うための組み込みツール*netsh.exe*です。 

*Netsh.exe* URL プレフィックスを予約し、SSL 証明書を割り当てることができます。 このツールには、管理者特権が必要です。

次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小値を示しています。

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

次の例では、SSL 証明書を割り当てる方法を示します。

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

ここでは、リファレンス ドキュメントを*netsh.exe*:

* [ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 文字列](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

次のリソースは、いくつかのシナリオの詳細な手順を提供します。 HttpListener を参照している記事は、Http.Sys に基づいている両方に、HTTP.sys に同じように適用します。

* [方法 : SSL 証明書を使用してポートを構成する](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。
* [方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)有用な情報で以前のブログをすぎますがこれです。

ここより簡単に使用できる一部のサード パーティ製ツール、 *netsh.exe*コマンドライン。 によって提供されるか、Microsoft によって承認されているこれらがありません。 ツールは、実行管理者として既定では、ので*netsh.exe*自体には、管理者特権が必要です。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。 UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。 新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys のソース コード](https://github.com/aspnet/HttpSysServer/)
* [ホスティング](xref:fundamentals/hosting)
