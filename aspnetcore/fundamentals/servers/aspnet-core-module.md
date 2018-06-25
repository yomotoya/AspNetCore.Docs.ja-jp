---
title: ASP.NET Core モジュール
author: rick-anderson
description: Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 319ab80f20f3917dae921f43566e368b51c29f0b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272336"
---
# <a name="aspnet-core-module"></a>ASP.NET Core モジュール

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher) 

ASP.NET Core モジュールでは、リバース プロキシの構成で ASP.NET Core アプリを IIS の背後で実行することを許可します。 IIS では、高度な Web アプリ セキュリティ機能と管理機能が提供されます。

サポートされている Windows バージョン:

* Windows 7 以降
* Windows Server 2008 R2 以降

ASP.NET Core モジュールは、Kestrel でのみ機能します。 このモジュールは、[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) と互換性がありません。

## <a name="aspnet-core-module-description"></a>ASP.NET Core モジュールの説明

ASP.NET Core モジュールは、ネイティブな IIS モジュールであり、バックエンドの ASP.NET Core アプリに Web 要求をリダイレクトするために IIS パイプラインにプラグされます。 Windows 認証などの多くのネイティブなモジュールは、アクティブなままです。 このモジュールと共にアクティブな IIS モジュールの詳細については、[IIS モジュール](xref:host-and-deploy/iis/modules)に関するページを参照してください。

ASP.NET Core アプリは IIS ワーカー プロセスとは独立したプロセスで実行されるため、モジュールもプロセス管理を処理します。 モジュールは、最初の要求を受信したときに ASP.NET Core アプリのプロセスを開始し、プロセスがクラッシュした場合はアプリを再起動します。 この動作は、IIS のインプロセスで実行され、[WAS (Windows プロセス アクティブ化サービス)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) によって管理される ASP.NET 4.x アプリと基本的に同じです。

次の図は、IIS (ASP.NET Core モジュール) と ASP.NET Core アプリの間のリレーションシップを示しています。

![ASP.NET Core モジュール](aspnet-core-module/_static/ancm.png)

要求は、Web からカーネル モードの HTTP.sys ドライバーに到着します。 ドライバーは、Web サイトの構成ポート (通常は 80 (HTTP) または 443 (HTTPS)) で IIS への要求をルーティングします。 モジュールでは、アプリのランダムなポートで Kestrel に要求を転送します。このポートは 80/443 ではありません。

モジュールが起動時に環境変数を介してポートを指定すると、サーバーは `http://localhost:{port}` をリッスンするように、IIS 統合ミドルウェアによって構成されます。 追加のチェックが実行され、モジュールから発生していない要求は拒否されます。 モジュールでは HTTPS 転送がサポートされていないため、要求は HTTPS を介して IIS によって受信された場合でも、HTTP を介して転送されます。

Kestrel でモジュールから要求を取り込んだ後、要求は ASP.NET Core ミドルウェア パイプラインにプッシュされます。 ミドルウェア パイプラインは要求を処理し、`HttpContext` インスタンスとしてアプリのロジックに渡します。 アプリの応答が IIS に戻され、IIS はその応答を、要求を開始した HTTP クライアントに返します。

ASP.NET Core モジュールには、他にも機能があります。 モジュールでは次の操作を行うことができます。

* ワーカー プロセスの環境変数を設定する
* 起動に関する問題をトラブルシューティングするために、Stdout 出力をファイル ストレージに記録する
* Windows 認証トークンを転送する

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core モジュールをインストールして使用する方法

ASP.NET Core モジュールをインストールして使用する方法の詳細な手順については、「[IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)」を参照してください。 モジュールを構成する方法については、「[ASP.NET Core モジュール構成の参照](xref:host-and-deploy/aspnet-core-module)」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core モジュールの GitHub リポジトリ (ソース コード)](https://github.com/aspnet/AspNetCoreModule)
