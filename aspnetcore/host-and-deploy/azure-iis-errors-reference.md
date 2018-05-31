---
title: ASP.NET Core を使用した Azure App Service および IIS の一般的なエラーのリファレンス
author: guardrex
description: Azure Apps Service と IIS で ASP.NET Core アプリをホストする場合の一般的なエラーを区別します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 995890a5e6b0cc1d9cebc21486917a7a39587076
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233340"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>ASP.NET Core を使用した Azure App Service および IIS の一般的なエラーのリファレンス

作成者: [Luke Latham](https://github.com/guardrex)

次の一覧に含まれていないエラーもあります。 ここにリストされていないエラーが発生した場合は、エラーを再現するための詳しい手順と共に[新しい問題を開いてください](https://github.com/aspnet/Docs/issues/new)。

次の情報を収集します。

* ブラウザーの動作
* アプリケーション イベント ログのエントリ
* ASP.NET Core モジュールの stdout ログのエントリ

情報を、次の一般的なエラーと比較します。 一致が見つかった場合は、トラブルシューティングのアドバイスに従います。

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>インストーラーが VC++ 再頒布可能パッケージを取得できない

* **インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー

* **インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)

  &#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。

トラブルシューティング:

* ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)からインストーラーを入手します。 インストーラーが失敗する場合、フレームワークに依存する展開 (FDD) をホストするために必要な .NET Core ランタイムをサーバーが受け取っていない可能性があります。 FDD をホストしている場合は、[プログラムと機能] でランタイムがインストールされていることを確認してください。 ランタイム インストーラーが必要な場合は、[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から入手します。 ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された

* **アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。 このデータはエラーです。

トラブルシューティング:

* **C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。 OS アップグレードより前に ASP.NET Core モジュールをインストールしていた場合、OS アップグレード後に 32 ビット モードで AppPool を実行しようとすると、この問題が発生します。 OS アップグレード後に ASP.NET Core モジュールを修復してください。 「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。 インストーラーを実行するときに **[修復]** を選択します。

## <a name="platform-conflicts-with-rid"></a>プラットフォームが RID と競合している

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"C:\{PATH}{assembly}.{exe|dll}" ' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : ff)

* **ASP.NET Core モジュールのログ:** ハンドルされない例外: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'. (ファイルまたはアセンブリ '{assembly}.dll' が読み込めませんでした) 正しくない形式のプログラムを読み込もうとしました。

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。

* *.csproj* 内の `<PlatformTarget>` が RID と競合していないことを確認します。 たとえば、`x86` の `<PlatformTarget>` を指定した場合は、*dotnet publish -c Release -r win10-x64* を使用するか、*.csproj* で `<RuntimeIdentifiers>` を `win10-x64` に設定するかを問わず、`win10-x64` の RID を使用して発行しないでください。 プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。

* Azure Apps 展開で、アプリケーションをアップグレードして新しいアセンブリを展開しようとしたときにこの例外が発生した場合は、以前の展開からすべてのファイルを手動で削除してください。 アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI のエンドポイントが間違っているか、Web サイトが停止している

* **ブラウザー:** ERR_CONNECTION_REFUSED

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* アプリに対して正しい URI エンドポイントが使用されていることを確認します。 バインドを確認します。

* IIS Web サイトが *[停止]* 状態でないことを確認します。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine または W3SVC サーバー機能が無効

* **OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)

トラブルシューティング:

* 適切な役割と機能が有効になっていることを確認します。 「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。

## <a name="incorrect-website-physical-path-or-app-missing"></a>Web サイト物理パスが間違っているか、アプリが見つからない

* **ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* IIS Web サイトの**基本設定**と物理アプリのフォルダーを確認します。 アプリが IIS Web サイトの**物理パス**にあるフォルダー内に配置されていることを確認します。

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可

* **ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* 適切な役割が有効になっていることを確認します。 「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。

* **[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。 インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。 「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。

* **[アプリケーション プール]** > **[プロセス モデル]** > **[ID]** が **ApplicationPoolIdentity** に設定されていることを確認します。または、アプリの展開フォルダーにアクセスするための正しいアクセス許可がカスタム ID に設定されていることを確認します。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '".\{assembly}.exe" ' でプロセスを開始できませんでした。ErrorCode = '0x80070002 : 0)

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。

* *web.config* で `<aspNetCore>` 要素の *processPath* 属性を調べ、フレームワークに依存する展開 (FDD) を示す *dotnet*、または自己完結型の展開 (SCD) を示す *.\{assembly}.exe* になっていることを確認します。

* FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。 *C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。

* FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。 AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。 *C:\Program Files\dotnet* とアプリのディレクトリに、AppPool ユーザー ID に対する拒否ルールが構成されていないことを確認します。

* FDD を配置し、IIS を再起動せずに .NET Core をインストールした可能性があります。 サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* ホスト システムで、 .NET Core ランタイムをインストールせずに、FDD を配置した可能性があります。 .NET Core ランタイムがインストールされていない場合は、システムで **.NET Core ホスティング バンドルのインストーラー**を実行します。 「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。 インターネットに接続せずに、.NET Core ランタイムをシステムにインストールする場合は、[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)のページからランタイムを入手し、ホスティング バンドル インストーラーを実行して ASP.NET Core モジュールをインストールします。 インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* FDD を配置し、*Microsoft Visual C++ 2015 再頒布可能パッケージ (x64)* がシステムにインストールされていない可能性があります。 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)からインストーラーを入手します。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>\<aspNetCore\> 要素の引数の誤り

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"dotnet" .\{assembly}.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)

* **ASP.NET Core モジュールのログ:** The application to execute does not exist: 'PATH\{assembly}.dll' (実行するアプリケーションが存在しません: 'PATH\{assembly}.dll')

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。

* *web.config* で `<aspNetCore>` 要素の *arguments* 属性を調べ、次のいずれかになっていることを確認します。(a) フレームワークに依存する展開 (FDD) の場合は *.\{assembly}.dll*、または (b) 自己完結型の展開 (SCD) の場合は、未指定の空の文字列 (*arguments=""*) か、アプリの引数のリスト (*arguments="arg1, arg2, ..."*)。

## <a name="missing-net-framework-version"></a>.NET Framework バージョンの欠落

* **ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)

* **アプリケーション ログ:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081. (ErrorCode = 物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"dotnet" .\{assembly}.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)

* **ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。 例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。

トラブルシューティング:

* システムにない .NET Framework のバージョンをインストールします。

* フレームワークに依存する展開 (FDD) では、正しいランタイムがシステムにインストールされていることを確認します。 プロジェクトを 1.1 から 2.0 にアップグレードして、ホスト システムに展開したときに、この例外が発生した場合は、ホスト システムに 2.0 Framework が存在することを確認します。

## <a name="stopped-application-pool"></a>アプリケーション プールの停止

* **ブラウザー:** 503 サービスを利用できません

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング

* アプリケーション プールが *[停止]* 状態でないことを確認します。

## <a name="iis-integration-middleware-not-implemented"></a>IIS 統合ミドルウェアが未実装

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンド ライン '"C:\{PATH}\{assembly}.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。

トラブルシューティング

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。

* 次のいずれかを確認します。
  * アプリの `WebHostBuilder` で `UseIISIntegration` メソッドを呼び出すことによって、IIS 統合ミドルウェアが参照される (ASP.NET Core 1.x)。
  * アプリが `CreateDefaultBuilder` メソッドを使用する (ASP.NET Core 2.x)。
  
  詳細については、「[ASP.NET Core でのホスティング](xref:fundamentals/host/index)」をご覧ください。

## <a name="sub-application-includes-a-handlers-section"></a>サブアプリケーションに \<handlers\> セクションが含まれている

* **ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ルート アプリに対してログ ファイルが作成され、正常動作を示します。 サブアプリに対してはログ ファイルは作成されません。

トラブルシューティング

* サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。

## <a name="stdout-log-path-incorrect"></a>stdout ログのパスが正しくありません

* **ブラウザー:** アプリは通常どおりに応答します。

* **アプリケーション ログ:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893. (警告: stdoutLogFile \?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log を作成できませんでした。ErrorCode = -2147024893.)

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング

* *web.config* の `<aspNetCore>` 要素で指定された `stdoutLogFile` パスが存在しません。 詳細については、ASP.NET Core モジュール構成の参照トピックの「[ログの作成とリダイレクト](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」セクションをご覧ください。

## <a name="application-configuration-general-issue"></a>アプリケーション構成の一般的な問題

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンド ライン '"C:\{PATH}\{assembly}.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング

* この一般例外はプロセスを開始できなかったことを示し、ほとんどの場合はアプリケーション構成の問題が原因です。 [ディレクトリ構造](xref:host-and-deploy/directory-structure)に関する説明を参照して、アプリの展開済みファイルとフォルダーが適切であることと、アプリの構成ファイルが存在し、そのファイルにアプリと環境に対する正しい設定が含まれていることを確認します。 詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。
