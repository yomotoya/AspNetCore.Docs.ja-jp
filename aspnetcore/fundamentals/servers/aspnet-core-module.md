---
title: "ASP.NET Core モジュール"
author: tdykstra
description: "ASP.NET Core モジュール (ANCM)、IIS または IIS Express を使用して、リバース プロキシ サーバーとして Kestrel web サーバーができるように IIS モジュールが導入されています。"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 153c40f0e825ff5826e916c7ea877a25d81954f1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET Core モジュールの概要

によって[Tom Dykstra](https://github.com/tdykstra)、 [Rick Strahl](https://github.com/RickStrahl)、および[Chris Ross](https://github.com/Tratcher) 

ASP.NET Core モジュール (ANCM) では、アプリケーション、IIS の背後にある ASP.NET Core を実行できますこれは、新機能 (セキュリティ、管理容易性、および多数詳細) でも適切に IIS を使用すると、を使用して[Kestrel](kestrel.md)には、新機能 (されている非常に高速)、でも適切に取得しています、。一度に 2 つのテクノロジからの利点があります。 **ANCM は Kestrel; でのみ機能します。WebListener と互換性がない (ASP.NET Core で 1.x)、または HTTP.sys (2.x) にします。** 

サポートされている Windows のバージョン:

* Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-aspnet-core-module-does"></a>ASP.NET Core モジュールの実行内容

ANCM は、IIS のパイプラインにフックし、トラフィックをバックエンド ASP.NET Core アプリケーションにリダイレクトするネイティブの IIS モジュールです。 Windows 認証など、他のほとんどのモジュールは、まだ実行する機会を取得します。 ANCM はコントロールをだけは、ハンドラーが、要求を選択し、ハンドラー マッピングがアプリケーションで定義されている場合に*web.config*ファイル。

IIS ワーカー プロセスからの個別のプロセスで実行する ASP.NET Core アプリケーション、ため ANCM もが、処理の管理。 最初の要求はクラッシュして再起動し、ANCM は ASP.NET Core アプリケーションのプロセスを開始します。 これは、従来の ASP.NET アプリケーションと基本的に同じ動作を実行するインプロセス iis および WAS (Windows ライセンス認証サービスなど) によって管理されています。

ここでは、IIS、ANCM、および ASP.NET Core アプリケーションの間のリレーションシップを示す図。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm.png)

要求は、Web から受け取るし、プライマリ ポート (80) または SSL ポート (443) 上の IIS にルーティングする、カーネル モード Http.Sys ドライバーをヒットします。 ANCM は、ポート 80 および 443 ではないアプリケーション用に構成された HTTP ポート上の ASP.NET Core アプリケーションに要求を転送します。

Kestrel は、ANCM からのトラフィックをリッスンします。  ANCM が起動時に、環境変数を使用してポートを指定し、 [UseIISIntegration](#call-useiisintegration)メソッドでリッスンするサーバーの構成`http://localhost:{port}`です。 これは、追加のチェック ANCM からではなく要求を拒否します。 (ANCM はサポートされません HTTPS 転送、HTTPS 経由で IIS によって受信された場合でも要求が HTTP 経由で転送されるようにします。)

Kestrel が ANCM から要求を取得し、それらにし、それらを処理し、それらとして ASP.NET Core のミドルウェア パイプラインにプッシュ`HttpContext`アプリケーション ロジックのインスタンス。 アプリケーションの応答は、IIS、それらクライアントに返信、HTTP 要求を開始したどのプッシュに渡されます。

ANCM が他のいくつかの機能もあります。

* 環境変数を設定します。
* ログ`stdout`ファイル記憶域に出力します。
* Windows 認証トークンを転送します。

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>ASP.NET Core アプリケーションで ANCM を使用する方法

このセクションでは、IIS サーバーおよび ASP.NET Core アプリケーションを設定するため、プロセスの概要を示します。 詳細については、次を参照してください。 [IIS と Windows 上のホスト](xref:host-and-deploy/iis/index)です。

### <a name="install-ancm"></a>ANCM をインストールします。


ASP.NET Core モジュールは、インストールする IIS で、サーバーと IIS Express で、開発用コンピューターで持っています。 サーバーでは、ANCM に含まれる、 [.NET コア Windows Server をホストしているバンドル](https://aka.ms/dotnetcore-2-windowshosting)です。 開発用コンピューターの Visual Studio 自動的にインストール ANCM IIS および IIS Express で、コンピューターに既にインストールされている場合。

### <a name="install-the-iisintegration-nuget-package"></a>IISIntegration NuGet パッケージをインストールします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) ASP.NET Core metapackages にパッケージが含まれます ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/)と[Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). を使用しない、metapackages のいずれかの場合は、インストール`Microsoft.AspNetCore.Server.IISIntegration`とは別にします。 `IISIntegration`パッケージは、アプリを設定する ANCM によってブロードキャストされた環境変数を読み取るの相互運用性パックします。 環境変数では、リッスンするポートなどの構成情報を提供します。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションでインストール[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)です。 `IISIntegration`パッケージは、アプリを設定する ANCM によってブロードキャストされた環境変数を読み取るの相互運用性パックします。 環境変数では、リッスンするポートなどの構成情報を提供します。 

---

### <a name="call-useiisintegration"></a>呼び出し UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`UseIISIntegration`拡張メソッドを[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) IIS を実行したときに自動的に呼び出されます。

ASP.NET Core metapackages のいずれかを使用していないしがインストールされていないかどうか、`Microsoft.AspNetCore.Server.IISIntegration`パッケージ、実行時エラーを取得します。 呼び出す場合`UseIISIntegration`パッケージがインストールされていない場合、コンパイル時エラーに明示的に、取得します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションの`Main`メソッドを呼び出し、`UseIISIntegration`拡張メソッドを[ `WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)です。 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration`メソッドの ANCM 設定すると、環境変数とそのキャッシュなしでこれらがない場合。 この動作には、開発および macOS または Linux でのテストと IIS を実行しているサーバーへの展開のようなシナリオが容易になります。 で macOS または Linux を実行しているときに、Kestrel が web サーバーとして機能します。ただし、アプリが IIS 環境に展開されると、自動的に ANCM と IIS 使用されます。

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>ANCM ポートのバインドが他のポートのバインドをオーバーライドします

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM には、バックエンド プロセスに代入する動的なポートが生成されます。 `UseIISIntegration`メソッドは、この動的なポートを取得し、Kestrel でリッスンするように構成`http://locahost:{dynamicPort}/`です。 呼び出しなど、その他の URL の構成が上書きされます。`UseUrls`または[Kestrel のリッスン API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)です。 呼び出す必要はありませんので、`UseUrls`または Kestrel の`Listen`API ANCM を使用するとします。 呼び出す場合`UseUrls`または`Listen`Kestrel は IIS ことがなくアプリを実行するときに指定したポートでリッスンします。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM には、バックエンド プロセスに代入する動的なポートが生成されます。 `UseIISIntegration`メソッドは、この動的なポートを取得し、Kestrel でリッスンするように構成`http://locahost:{dynamicPort}/`です。 呼び出しなど、その他の URL の構成が上書きされます。`UseUrls`です。 呼び出す必要はありませんので、 `UseUrls` ANCM を使用するとします。 呼び出す場合`UseUrls`Kestrel は IIS ことがなくアプリを実行するときに指定したポートでリッスンします。

呼び出す場合に、ASP.NET Core 1.0 で`UseUrls`、それを呼び出す**する前に**呼び出す`UseIISIntegration`ANCM に構成されたポートの上書きを取得しないようにします。 ANCM 設定より優先されるため、この呼び出しの順序が ASP.NET Core 1.1 のため必要はありません`UseUrls`です。

---

### <a name="configure-ancm-options-in-webconfig"></a>Web.config で ANCM オプションを構成します。

ASP.NET Core モジュールの構成に保存、 *web.config*アプリケーションのルート フォルダーに配置されているファイル。 このファイルの設定は、スタートアップ コマンドおよび ASP.NET Core アプリを起動する引数をポイントします。 サンプルの*web.config*コードと構成オプションに関する説明を参照してください[ASP.NET コア モジュールの構成の参照](xref:host-and-deploy/aspnet-core-module)です。

### <a name="run-with-iis-express-in-development"></a>IIS Express を使って開発での実行します。

IIS Express は、Visual Studio の ASP.NET Core テンプレートで定義されている既定のプロファイルを使用して起動できます。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP プロトコルとペアリング トークンを使用するプロキシの構成

ANCM と Kestrel の間に作成されたプロキシは、HTTP プロトコルを使用します。 HTTP を使用して、パフォーマンスを最適化する ANCM と Kestrel 間のトラフィックが行われる、ループバック アドレスでネットワーク インターフェイスからです。 傍受 ANCM と、サーバーからの場所から Kestrel 間のトラフィックのリスクはありません。

ペアリング トークンは、Kestrel によって受信された要求が IIS によってプロキシと他のソースから取得していないことを保証するために使用されます。 ペアリング トークンが作成され、環境変数に設定 (`ASPNETCORE_TOKEN`)、ANCM でします。 ペアリング トークンは、ヘッダーにも設定 (`MSAspNetCoreToken`) プロキシ要求ごとにします。 IIS のミドルウェアのチェックは、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認する受信を要求します。 トークンの値が一致しない場合は、要求が記録され、拒否されました。 ペアリングのトークンの環境変数と ANCM と Kestrel 間のトラフィックは、サーバーからの場所からアクセスできません。 トークン ペアの値を知らなくても、攻撃者は、IIS ミドルウェア内でチェックのバイパスの要求を送信できません。

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core モジュールのソース コード](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
