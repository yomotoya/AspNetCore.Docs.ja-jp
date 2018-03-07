---
title: "ASP.NET Core での HTTP.sys Web サーバーの実装"
author: tdykstra
description: "Windows 上の ASP.NET Core 用 Web サーバーである HTTP.sys について説明します。 HTTP.sys は、Http.sys カーネル モード ドライバーに基づいて構築された、IIS なしで直接インターネットに接続するために使用できる Kestrel の代替製品です。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 730ecf12f718f6bbbdefb7cdc561481b126c995b
ms.sourcegitcommit: c5ecda3c5b1674b62294cfddcb104e7f0b9ce465
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core での HTTP.sys Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Luke Latham](https://github.com/guardrex)

> [!NOTE]
> このトピックは、ASP.NET Core 2.0 以降に適用されます。 以前のバージョンの ASP.NET Core では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) という名前です。

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) は、Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](xref:fundamentals/servers/index)です。 HTTP.sys は [Kestrel](xref:fundamentals/servers/kestrel) の代替製品であり、Kestrel では提供されない機能がいくつか用意されています。

> [!IMPORTANT]
> HTTP.sys は [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)と互換性がなく、IIS または IIS Express と共に使用することはできません。

HTTP.sys は、次の機能をサポートします。

* [Windows 認証](xref:security/authentication/windowsauth)
* ポート共有
* SNI を使用する HTTPS
* HTTP/2 over TLS (Windows 10 以降)
* 直接ファイル伝送
* 応答キャッシュ
* WebSocket (Windows 8 以降)

サポートされている Windows バージョン:

* Windows 7 以降
* Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-httpsys"></a>HTTP.sys を使用するタイミング

HTTP.sys は、次のような展開に適しています。

* IIS を使用せず、インターネットに直接サーバーを公開する必要がある。

  ![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

* 内部の展開で、[Windows 認証](xref:security/authentication/windowsauth)などの、Kestrel では使用できない機能が要求されている。

  ![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

HTTP.sys は、さまざまな種類の攻撃を防ぎ、フル機能の Web サーバーとして堅牢性、セキュリティ、スケーラビリティを提供する、成熟したテクノロジです。 IIS 自体が、HTTP.sys 上で HTTP リスナーとして実行されています。 

## <a name="how-to-use-httpsys"></a>HTTP.sys の使用方法

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>HTTP.sys を使用するように ASP.NET Core アプリを構成する

1. [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)) (ASP.NET Core 2.0 以降) を使用する場合は、プロジェクト ファイルのパッケージ参照は必要ありません。 `Microsoft.AspNetCore.All` メタパッケージを使用しない場合は、[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) にパッケージ参照を追加します。

1. Web ホストを構築するときに [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 拡張メソッドを呼び出し、必要な [HTTP.sys オプション](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)を指定します。

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   HTTP.sys の追加の構成は、[レジストリ設定](https://support.microsoft.com/kb/820129)を通じて処理されます。

   **HTTP.sys オプション**

   | プロパティ | 説明 | 既定値 |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | `HttpContext.Request.Body` および `HttpContext.Response.Body` に対して、入力/出力の同期を許可するかどうかを制御します。 | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | 匿名要求を許可します。 | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | 許可される認証方式を指定します。 リスナーを破棄する前ならいつでも変更できます。 値は [AuthenticationSchemes 列挙型](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) (`Basic`、`Kerberos`、`Negotiate`、`None`、および `NTLM`) によって指定します。 | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | 対象となるヘッダーを持つ応答に対して、[カーネル モード](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)のキャッシュを試行します。 `Set-Cookie`、`Vary`、または `Pragma` ヘッダーを含む応答は対象外です。 応答は、`public` である `Cache-Control` ヘッダーと `shared-max-age` または `max-age` の値のいずれかを含むか、または `Expires` ヘッダーを含む必要があります。 | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | 同時受け入れの最大数です。 | 5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | 受け入れる同時接続の最大数です。 無限にするには、`-1` を使用します。 コンピューター全体のレジストリ設定を使用するには、`null` を使用します。 | `null`<br>(無制限) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | 「<a href="#maxrequestbodysize">MaxRequestBodySize</a>」セクションを参照してください。 | 30000000 バイト<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | キューに置くことができる要求の最大数。 | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | 応答本文の書き込みがクライアントの接続の切断によって失敗した場合、例外をスローするか、または正常に完了するかどうかを指定します。 | `false`<br>(正常に完了する) |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 構成を公開します。レジストリでも構成することができます。 各設定に関する既定値などの詳細については、API のリンクを参照してください。<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; HTTP サーバー API が Keep-Alive 接続でエンティティ本体をドレインするまでに許容される時間です。</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; 要求のエンティティ本体が到着するまでに許容される時間です。</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; HTTP サーバー API が要求ヘッダーを解析するまでに許容される時間です。</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; 接続で許容されるアイドル時間です。</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; 応答の最小の送信率です。</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; 要求が、アプリにピック アップされるまでに要求キューの中に留まっていられる時間です。</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | HTTP.sys に登録する [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) を指定します。 最も便利なのは [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add) です。これを使用して、コレクションにプレフィックスを追加できます。 これらは、リスナーを破棄する前ならいつでも変更できます。 |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   要求本文の最大許容サイズ (バイト単位) です。 `null` に設定する場合、要求本文の最大サイズは制限されません。 この制限は、アップグレード済みの接続 (常に無制限) には影響しません。

   1 つの `IActionResult` に対する ASP.NET Core MVC アプリの制限を上書きする方法としては、次のように、アクション メソッドに対して [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性を使用することをお勧めします。
   
   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   アプリが要求の読み取りを開始した後に、アプリが要求に対する制限を構成しようとすると、例外がスローされます。 `IsReadOnly` プロパティを使用して、`MaxRequestBodySize` プロパティが読み取り専用状態にあるかどうか、つまり制限を構成するには遅すぎるかどうかを示すことができます。

   アプリで要求ごとに [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) を上書きする必要がある場合は、次のように、[IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature) を使用します。

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

1. Visual Studio を使用する場合は、アプリが IIS または IIS Express を実行するように構成されていないことを確認します。

   Visual Studio では、既定の起動プロファイルは IIS Express 用です。 プロジェクトをコンソール アプリとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更します。

   ![コンソール アプリのプロファイルを選択する](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Windows Server を構成する

1. アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)である場合は、.NET Core、.NET Framework、またはその両方 (アプリが .NET Framework をターゲットとする .NET Core アプリである場合) をインストールします。

   * **.NET Core** &ndash; アプリが .NET Core を必要とする場合は、[.NET ダウンロード](https://www.microsoft.com/net/download/windows)から .NET Core インストーラーを取得して実行します。
   * **.NET framework** &ndash; アプリが .NET Framework を必要とする場合は、[.NET Framework のインストール ガイド](/dotnet/framework/install/)に関する記事を参照してインストール手順を確認します。 必要な .NET Framework をインストールします。 最新の .NET Framework のインストーラーは、[.NET ダウンロード](https://www.microsoft.com/net/download/windows)にあります。

1. アプリ用の URL とポートを構成します。

   既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。 URL プレフィックスとポートを構成するには、次のオプションが使用できます。

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` コマンド ライン引数
   * `ASPNETCORE_URLS` 環境変数
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) を使用する方法を、次のコード例に示します。

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   `UrlPrefixes` の利点は、プレフィックスの形式が正しくなかった場合、すぐにエラー メッセージが生成されることです。

   `UrlPrefixes` の設定は `UseUrls`/`urls`/`ASPNETCORE_URLS` の設定を上書きします。 したがって、`UseUrls`、`urls`、および `ASPNETCORE_URLS` 環境変数の利点は、Kestrel と HTTP.sys を簡単に切り替えられることです。 `UseUrls`、`urls`、および `ASPNETCORE_URLS` について詳しくは、[ホスティング](xref:fundamentals/hosting)に関する記事を参照してください。

   HTTP.sys では、[HTTP サーバー API の UrlPrefix 文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)が使用されます。

1. HTTP.sys にバインドする URL プレフィックスを事前登録し、x.509 証明書を設定します。

   URL プレフィックスが Windows に事前登録されていない場合は、管理者特権でアプリを実行します。 唯一の例外は、(HTTPS ではなく) HTTPを使用して、1024 より大きいポート番号で、localhost にバインドする場合です。 この場合、管理者特権は必要ありません。

   1. HTTP.sys を構成するための組み込みツールは、*netsh.exe* です。 *netsh.exe* を使用して、URL プレフィックスを予約し、X.509 証明書を割り当てることができます。 ツールを使用するには管理者特権が必要です。

      ポート 80 と 443 の URL プレフィックスを予約するためのコマンドを、次の例に示します。

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      次の例は、X.509 証明書を割り当てる方法を示しています。

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
      ```

      以下は、*netsh.exe* のリファレンス ドキュメントです。

      * [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)
      * [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)

   1. 必要な場合は、自己署名 X.509 証明書を作成します。

     [!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

1. トラフィックが HTTP.sys に到達できるようにファイアウォールのポートを開きます。 *netsh.exe* または [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906) を使用します。

## <a name="additional-resources"></a>その他の技術情報

* [HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [aspnet/HttpSysServer GitHub リポジトリ (ソース コード)](https://github.com/aspnet/HttpSysServer/)
* [ホスティング](xref:fundamentals/hosting)
