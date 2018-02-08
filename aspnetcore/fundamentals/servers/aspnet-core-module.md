---
title: "ASP.NET Core モジュール"
author: tdykstra
description: "Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための IIS モジュールである ASP.NET Core モジュール (ANCM) について紹介します。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET Core モジュールの概要

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher) 

ASP.NET Core モジュール (ANCM) では、IIS の背後で ASP.NET Core アプリケーションを実行することができます。この場合は、セキュリティ、管理容易性、その他さまざまな点で優れている IIS、および高速化に適している [Kestrel](kestrel.md) が使用され、両方の技術の利点が一度に活用されます。 **ANCM は Kestrel でのみ機能します。WebListener (ASP.NET Core 1.x) または HTTP.sys (2.x) と互換性はありません。** 

サポートされている Windows バージョン:

* Windows 7 および Windows Server 2008 R2 以降

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-aspnet-core-module-does"></a>ASP.NET Core モジュールの機能

ANCM とは、IIS パイプラインにフックしてトラフィックをバックエンドの ASP.NET Core アプリケーションにリダイレクトするネイティブの IIS モジュールです。 Windows 認証など、他のほとんどのモジュールは引き続き実行することができます。 ANCM が制御を取得するのは、要求に対してハンドラーが選択され、ハンドラー マッピングがアプリケーション *web.config* ファイルに定義される場合のみです。

ASP.NET Core アプリケーションは IIS ワーカー プロセスとは独立したプロセスで実行されるため、ANCM もプロセス管理を行います。 ANCM は、最初の要求を受信したときに ASP.NET Core アプリケーションのプロセスを開始し、プロセスがクラッシュしたときは再起動します。 この動作は、IIS のインプロセスで実行され、WAS (Windows プロセス アクティブ化サービス ) によって管理される従来の ASP.NET アプリケーションと基本的に同じです。

次の図は、IIS アプリケーション、ANCM アプリケーション、および ASP.NET Core アプリケーションの間のリレーションシップを示しています。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm.png)

要求は Web から送信され、カーネル モード Http.Sys ドライバーに到達します。このドライバーはプライマリ ポート (80) または SSL ポート (443) で IIS に要求をルーティングします。 ANCM は、アプリケーション用に構成された、ポート 80/443 以外の HTTP ポートで ASP.NET Core アプリケーションに要求を転送します。

Kestrel は ANCM からのトラフィックをリッスンします。  ANCM が起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、[UseIISIntegration](#call-useiisintegration) メソッドによって構成されます。 ANCM 以外からの要求を拒否するためのに追加のチェックが行われます  (ANCM では HTTPS 転送がサポートされていないのため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます)。

Kestrel は ANCM から要求を取得し、それを ASP.NET Core ミドルウェア パイプラインにプッシュします。そこで要求は処理され、`HttpContext` インスタンスとしてアプリケーション ロジックに渡されます。 アプリケーションの応答が IIS に渡され、IIS はその応答を、要求を開始した HTTP クライアントに返します。

ANCM には他にもいくつかの機能があります。

* 環境変数を設定します。
* `stdout` 出力をファイル記憶域にログ記録します。
* Windows 認証トークンを転送します。

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>ASP.NET Core アプリで ANCM を使用する方法

このセクションでは、IIS サーバーおよび ASP.NET Core アプリケーションを設定するためのプロセスの概要を説明します。 詳細については、「[IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index)」を参照してください。

### <a name="install-ancm"></a>ANCM をインストールする


ASP.NET Core モジュールは、サーバー上では IIS にインストールし、開発用コンピューター上では IIS Express にインストールする必要があります。 サーバーの場合、ANCM は [.NET Core Windows Server Hosting バンドル](https://aka.ms/dotnetcore-2-windowshosting)に含まれています。 開発用コンピューターの場合、ANCM は Visual Studio によって自動的に IIS Express にインストールされ、コンピューターに既にインストールされている場合は IIS にインストールされます。

### <a name="install-the-iisintegration-nuget-package"></a>IISIntegration NuGet パッケージをインストールする

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージは、ASP.NET Core メタパッケージ ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) および [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)) に含まれます。 メタパッケージの 1 つを使用しない場合は、`Microsoft.AspNetCore.Server.IISIntegration` を個別にインストールします。 `IISIntegration` パッケージは、アプリを設定するために ANCM によってブロードキャストされた環境変数を読み取る相互運用性パックです。 環境変数は、リッスンするポートなどの構成情報を提供します。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションで、[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) をインストールします。 `IISIntegration` パッケージは、アプリを設定するために ANCM によってブロードキャストされた環境変数を読み取る相互運用性パックです。 環境変数は、リッスンするポートなどの構成情報を提供します。 

---

### <a name="call-useiisintegration"></a>UseIISIntegration の呼び出し

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上の `UseIISIntegration` 拡張メソッドは、IIS で実行するときに自動的に呼び出されます。

ASP.NET Core メタパッケージのいずれか 1 つを使用しておらず、さらに `Microsoft.AspNetCore.Server.IISIntegration` パッケージがインストールされていない場合は、実行時エラーが返されます。 `UseIISIntegration` を明示的に呼び出した場合、パッケージがインストールされていないと、コンパイル時エラーが発生します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションの `Main` メソッドでは、[`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) で `UseIISIntegration` 拡張メソッドを呼び出します。 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` メソッドは ANCM によって設定された環境変数を探し、見つからない場合は動作しません。 このようなしくみは、macOS または Linux で開発およびテストを行い、IIS を実行しているサーバーに展開するようなシナリオが容易になります。 macOS または Linux で実行している間、Kestrel は Web サーバーとして機能しますが、アプリが IIS 環境に展開されると、自動的に ANCM および IIS を使用するようになります。

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>ANCM ポート バインディングで他のポート バインディングを上書きする

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM は、バックエンド プロセスに割り当てる動的なポートを生成します。 `UseIISIntegration` メソッドは、この動的なポートを取得すると共に、`http://locahost:{dynamicPort}/` をリッスンするように Kestrel を構成します。 これにより、`UseUrls` または [Kestrel の Listen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) の呼び出しなど、その他の URL 構成が上書きされます。 したがって、ANCM を使用するときに、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。 `UseUrls` または `Listen` を呼び出す場合、IIS なしでアプリを実行するときに指定するポートが Kestrel によってリッスンされます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM は、バックエンド プロセスに割り当てる動的なポートを生成します。 `UseIISIntegration` メソッドは、この動的なポートを取得すると共に、`http://locahost:{dynamicPort}/` をリッスンするように Kestrel を構成します。 これにより、`UseUrls` の呼び出しなど、その他の URL 構成が上書きされます。 したがって、ANCM を使用するときに、`UseUrls` を呼び出す必要はありません。 `UseUrls` を呼び出す場合、IIS なしでアプリを実行するときに指定するポートが Kestrel によってリッスンされます。

ASP.NET Core 1.0 では、`UseUrls` を呼び出す場合、これを呼び出して**から**、`UseIISIntegration` を呼び出します。そうすれば、ANCM で構成されたポートは上書きされません。 この呼び出しの順序は ASP.NET Core 1.1 では必要ありません。ANCM 設定は `UseUrls` より優先されるからです。

---

### <a name="configure-ancm-options-in-webconfig"></a>Web.config で ANCM オプションを構成する

ASP.NET Core モジュールの構成は、アプリケーションのルート フォルダーに配置された *web.config* ファイルに格納されます。 このファイル内の設定は、ASP.NET Core アプリを起動するスタートアップ コマンドと引数をポイントします。 *web.config* コードのサンプルと構成オプションのガイダンスについては、「[ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module)」 (ASP.NET Core モジュール構成の参照) を参照してください。

### <a name="run-with-iis-express-in-development"></a>開発における IIS Express での実行

IIS Express を Visual Studio で起動するには、ASP.NET Core テンプレートによって定義された既定のプロファイルを使用します。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>プロキシの構成で HTTP プロトコルとペアリング トークンを使用する

ANCM と Kestrel の間で作成されたプロキシで、HTTP プロトコルを使用します。 HTTP を使用することは、パフォーマンス最適化の手段です。この場合は、ANCM と Kestrel の間のトラフィックがネットワーク インターフェイスから離れたループバック アドレスで発生します。 ANCM と Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険性があります。

ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。 ペアリング トークンは、ANCM によって作成され、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。 ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。 IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。 トークンの値が一致しない場合、要求はログに記録され、拒否されます。 ペアリング トークン環境変数および、ANCM と Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。 ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [この記事のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core モジュールのソース コード](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
