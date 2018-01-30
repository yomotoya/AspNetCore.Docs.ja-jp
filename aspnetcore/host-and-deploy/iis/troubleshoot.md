---
title: "IIS で ASP.NET Core をトラブルシューティングします。"
author: guardrex
description: "ASP.NET Core アプリケーションの IIS の展開に関する問題を診断する方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: c68070a9cba5667504d1ad4927b02b73f83e6573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS で ASP.NET Core をトラブルシューティングします。

作成者: [Luke Latham](https://github.com/guardrex)

問題を診断する IIS を展開します。

* ブラウザーの出力を調べます。
* **イベント ビューアー**でシステムの**アプリケーション** ログを調べます。
* `stdout` ログを有効にします。 **ASP.NET Core モジュール**のログは、*web.config*で `<aspNetCore>`要素の *stdoutLogFile* 属性に指定したパスにあります。この属性の値で指定されたパスにあるすべてのフォルダーが、展開環境に存在する必要があります。 設定*stdoutLogEnabled*に`true`です。 使用するアプリ、 `Microsoft.NET.Sdk.Web` SDK を作成する、 *web.config*既定のファイル、 *stdoutLogEnabled*設定`false`、手動で提供、 *web.config*ファイルまたは有効にするために、ファイルを変更する`stdout`ログ記録します。

これら 3 つのソースから情報を使用して、[一般的なエラー リファレンス トピック](xref:host-and-deploy/azure-iis-errors-reference)問題を特定します。 問題を解決するのには提供されるトラブルシューティング アドバイスに従います。

一般的なエラーのいくつかは、*startupTimeLimit* (既定値: 120 秒) と *startupRetryCount* (既定値: 2) が経過するまで、ブラウザー、アプリケーション ログ、ASP.NET Core モジュールのログに表示されません。 したがって、モジュールがアプリのプロセスを開始できなかったと判断する前に、完全に 6 分が経過するまで待ってください。

アプリが正常に動作しているかどうかを決定する 1 つの簡単な方法は、アプリを Kestrel で直接実行することです。 として、アプリが発行された場合、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)、実行`dotnet <assembly_name>.dll`配置フォルダーでは、アプリへの IIS の物理パス。 として、アプリが発行された場合、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd)、実行、アプリのコマンド プロンプトから直接実行可能ファイル`<assembly_name>.exe`、配置フォルダーでします。 アプリを入手する必要があります Kestrel が 5000 の既定のポートでリッスンしている場合`http://localhost:5000/`です。 応答した場合、アプリ通常 Kestrel エンドポイント アドレスで、問題がより多く関連のリバース プロキシの構成、およびアプリ内可能性が低い。

スタイル シート、スクリプト、またはイメージ内のアプリの静的ファイルからの静的ファイルの簡単な要求を実行するかどうかは、リバース プロキシが正常に動作を決定する方法の 1 つは、 *wwwroot*を使用して[静的ファイル ミドルウェア](xref:fundamentals/static-files). アプリは、静的なファイルを使用できる MVC ビューおよびその他のエンドポイントが失敗している場合は、問題には、リバース プロキシの構成に関連する可能性が低い (例、MVC ルーティングまたは 500 Internal Server Error) 用のアプリ内で可能性が高くなります。

環境変数を一時的に追加 Kestrel が正常に起動 IIS の背後にあるが、正常にローカルに実行した後、システムで、アプリは実行できません、 *web.config*を設定する、`ASPNETCORE_ENVIRONMENT`に`Development`です。 環境がアプリの起動時にオーバーライドされると、されていない限り、環境変数を設定できます、[開発者例外ページ](xref:fundamentals/error-handling)に表示されるときに、アプリを実行します。 環境変数を設定`ASPNETCORE_ENVIRONMENT`この方法でのみに対して推奨は公開されませんステージング/テスト サーバーをインターネットにします。 環境変数を削除してください、 *web.config*ファイルが完了します。 使用して環境変数の設定の詳細について*web.config*を参照してください[aspNetCore の子要素を environmentVariables](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)です。

ほとんどの場合、アプリケーションのログ記録を有効にすると、アプリまたはリバース プロキシに関する問題のトラブルシューティングに役立ちます。 詳細については、[ログ記録](xref:fundamentals/logging/index)に関する説明を参照してください。

最後のトラブルシューティングのヒントでは、.NET Core SDK が、開発コンピューターまたはパッケージのバージョンで、アプリ内のいずれかのアップグレード後の実行に失敗するアプリに関連します。 場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。 これらの問題のほとんどは、によりを修正できます。

* 削除すると、`bin`と`obj`のプロジェクトのフォルダーです。
* キャッシュの消去パッケージ`%UserProfile%\.nuget\packages\`と`%LocalAppData%\Nuget\v3-cache`です。
* 復元して、プロジェクトを再構築します。
* アプリを再展開する前に、サーバー上の以前の展開が完全に削除されたことを確認します。

> [!TIP]
> パッケージのキャッシュを消去する便利な方法は、実行する`dotnet nuget locals all --clear`コマンド プロンプトからです。
> 
> パッケージのキャッシュをクリアする行うこともできますを使用して、 [nuget.exe](https://www.nuget.org/downloads)ツールとコマンドを実行する`nuget locals all -clear`です。 *nuget.exe* Windows 10 とバンドルがインストールされていないし、NuGet の web サイトから個別に取得する必要があります。
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>その他の技術情報

* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
