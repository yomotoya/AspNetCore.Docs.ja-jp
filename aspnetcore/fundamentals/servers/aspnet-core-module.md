---
title: ASP.NET Core モジュール
author: rick-anderson
description: Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910890"
---
# <a name="aspnet-core-module"></a>ASP.NET Core モジュール

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core モジュールでは、ASP.NET Core アプリを IIS ワーカー プロセス (*インプロセス*) で実行することも、ASP.NET Core アプリを IIS の背後でリバース プロキシの構成 (*アウトプロセス*) で実行することもできます。 IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core モジュールでは、リバース プロキシの構成で ASP.NET Core アプリを IIS の背後で実行することを許可します。 IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。

::: moniker-end

サポートされている Windows バージョン:

* Windows 7 以降
* Windows Server 2008 R2 以降

::: moniker range=">= aspnetcore-2.2"

インプロセスでホストする場合、モジュールには独自のサーバー実装 `IISHttpServer` があります。

アウト プロセスでホストする場合、モジュールは Kestrel に対してのみ機能します。 このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

モジュールは、Kestrel に対してのみ機能します。 このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。

::: moniker-end

## <a name="aspnet-core-module-description"></a>ASP.NET Core モジュールの説明

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core モジュールはネイティブな IIS モジュールであり、次のいずれかを目的として、IIS パイプラインにプラグインされます。

* IIS ワーカー プロセス (`w3wp.exe`) の内部で ASP.NET Core アプリをホストします。これは、[インプロセス ホスティング モデル](#in-process-hosting-model)と呼ばれています。

* [Kestrel サーバー](xref:fundamentals/servers/kestrel)を実行しているバックエンドの ASP.NET Core アプリに Web 要求を転送します。これは、[アウト プロセス ホスティング モデル](#out-of-process-hosting-model)と呼ばれています。

### <a name="in-process-hosting-model"></a>インプロセス ホスティング モデル

インプロセス ホスティング モデルを使用する場合、ASP.NET Core アプリはその IIS ワーカー プロセスと同じプロセスで実行されます。 これにより、アウト プロセス ホスティング モデルを使用する場合に、ループバック アダプター経由で要求をプロキシすることによるパフォーマンスの低下が解消されます。 IIS では [Windows プロセス アクティブ化サービス (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) を使用してプロセス管理が処理されます。

ASP.NET Core モジュール:

* アプリの初期化を実行します。
  * [CoreCLR](/dotnet/standard/glossary#coreclr) を読み込みます。
  * `Program.Main`.
* IIS ネイティブ要求の有効期間を処理します。

次の図は、IIS (ASP.NET Core モジュール) とインプロセスでホストされるアプリとの間のリレーションシップを示しています。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-inprocess.png)

Web からカーネル モードの HTTP.sys ドライバーに要求が届きます。 このドライバーによって、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) 上で IIS へのネイティブ要求がルーティングされます。 モジュールによってネイティブ要求が受信されると、モジュールから `IISHttpServer` に制御が渡されます。IISHttpServer では要求がネイティブからマネージドに変換されます。

`IISHttpServer` によって要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。 ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。 アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。

### <a name="out-of-process-hosting-model"></a>アウト プロセス ホスティング モデル

ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、プロセス管理はモジュールによって処理されます。 モジュールでは、最初の要求が届いたときに ASP.NET Core アプリのプロセスが開始され、プロセスがシャットダウンまたはクラッシュした場合はアプリが再起動されます。 この動作は、インプロセスで実行されるアプリであり、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理されるアプリと基本的に同じです。

次の図は、IIS (ASP.NET Core モジュール) とアウトプロセスでホストされるアプリとの間のリレーションシップを示しています。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-outofprocess.png)

要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。 ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。 モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。

モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。 追加のチェックが実行され、モジュールから発生していない要求は拒否されます。 モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。

Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。 ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。 IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。 アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core モジュールは、ネイティブな IIS モジュールであり、バックエンドの ASP.NET Core アプリに Web 要求を転送するために IIS パイプラインにプラグインされます。

ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、モジュールもプロセス管理を処理します。 モジュールは、最初の要求を受信したときに ASP.NET Core アプリのプロセスを開始し、プロセスがクラッシュした場合はアプリを再起動します。 この動作は、IIS のインプロセスで実行され、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理される ASP.NET 4.x アプリと基本的に同じです。

次の図は、IIS (ASP.NET Core モジュール) とアプリの間のリレーションシップを示しています。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm-outofprocess.png)

要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。 ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。 モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。

モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。 追加のチェックが実行され、モジュールから発生していない要求は拒否されます。 モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。

Kestrel によってモジュールから要求が取り込まれた後、その要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。 ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。 IIS 統合によって追加されたミドルウェアでは、カーネルへの要求の転送を考慮して、スキーム、リモート IP、およびパスベースが更新されます。 アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。

::: moniker-end

Windows 認証などの多くのネイティブなモジュールは、アクティブなままです。 このモジュールと共にアクティブな IIS モジュールの詳細については、「<xref:host-and-deploy/iis/modules>」を参照してください。

ASP.NET Core モジュールには、他にも機能があります。 モジュールでは次の操作を行うことができます。

* ワーカー プロセスの環境変数を設定する
* 起動に関する問題をトラブルシューティングするために、Stdout 出力をファイル ストレージに記録する
* Windows 認証トークンを転送する

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core モジュールをインストールして使用する方法

ASP.NET Core モジュールをインストールして使用する方法の詳細な手順については、「<xref:host-and-deploy/iis/index>」を参照してください。 モジュールの構成の詳細については、「<xref:host-and-deploy/aspnet-core-module>」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [ASP.NET Core モジュールの GitHub リポジトリ (ソース コード)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
