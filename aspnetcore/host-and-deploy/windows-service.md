---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758194"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスでの ASP.NET Core のホスト

著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。 Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="convert-a-project-into-a-windows-service"></a>プロジェクトを Windows サービスに変換する

サービスでとして実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。

1. プロジェクト ファイルで次を実行します。

   * Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、それをターゲット フレームワークを含む `<PropertyGroup>` に追加します。

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      複数の RID を発行するには、次の処理を実行します。

      * セミコロンで区切られたリストの形式で RID を指定します。
      * プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。

      詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。

   * [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。

1. `Program.Main` で次の変更を行います。

   * `host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。

   * [UseContentRoot](xref:fundamentals/host/web-host#content-root) を呼び出し、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. [dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。 Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。

   コマンド ライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行します。 プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。 次の例では、アプリが `win7-x64` ランタイムのリリース構成で、*c:\\svc* に作成されるフォルダーに発行されます。

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. `net user` コマンドを使用して、サービス用のユーザー アカウントを作成します。

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   サンプル アプリでは、名前 `ServiceUser` とパスワードを持つユーザー アカウントを作成します。 次のコマンド内の `{PASSWORD}` を、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)に置き換えます。

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   ユーザーをグループに追加する必要がある場合は、`net localgroup` コマンドを使用します。`{GROUP}` にはグループの名前を指定します。

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   詳細については、「[Service User Accounts](/windows/desktop/services/service-user-accounts)」(サービス ユーザー アカウント) をご覧ください。

1. [icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; アプリのフォルダーへのパス。
   * `{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。
   * `(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。
   * `(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。
   * `{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。
     * 書き込み (`W`)
     * 読み取り (`R`)
     * 実行 (`X`)
     * フル アクセス (`F`)
     * 変更 (`M`)
   * `/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。

   *c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、次のコマンドを使用します。

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。

1. [sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。 `binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。 **等号 (=) と、各パラメーターおよび値の引用符文字の間には、スペースが必要です。**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; [サービス コントロール マネージャー](/windows/desktop/services/service-control-manager)でサービスに割り当てる名前。
   * `{PATH}` &ndash; サービス実行可能ファイルへのパス。
   * `{DOMAIN}` (コンピューターがドメインに参加していない場合は、ローカル コンピューター名) および `{USER ACCOUNT}` &ndash; サービスを実行させるドメイン (またはローカル コンピューター名) とユーザー アカウント。 `obj` パラメーターを省略**しない**でください。 `obj` の既定値は、[LocalSystem アカウント](/windows/desktop/services/localsystem-account) アカウントです。 `LocalSystem` アカウントでサービスを実行すると、重大なセキュリティ リクスが生じます。 常に、サーバーに対する制限付きの特権を持つユーザー アカウントでサービスを実行します。
   * `{PASSWORD}` &ndash; ユーザー アカウントのパスワード。

   次に例を示します。

   * サービスは **MyService** という名前です。
   * 発行されたサービスは、*c:\\svc* フォルダーに配置されます。 *AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。 `binPath` 値は二重引用符 (") で囲まれます。
   * サービスは `ServiceUser` アカウントで実行されます。 `{DOMAIN}` を、ユーザー アカウントのドメインまたはローカル コンピューター名に置き換えます。 `obj` 値を二重引用符 (") 囲みます。 例: ホスト システムが `MairaPC` という名前のローカル コンピューターである場合は、`obj` を `"MairaPC\ServiceUser"` に設定にします。
   * `{PASSWORD}` をユーザー アカウントのパスワードに置き換えます。 `password` 値は二重引用符 (") で囲まれます。

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > パラメーターの等号とパラメーターの値の間にスペースがあることを確認します。

1. サービスを `sc start {SERVICE NAME}` コマンドで開始します。

   サンプル アプリ サービスを開始するには、次のコマンドを使用します。

   ```console
   sc start MyService
   ```

   このコマンドでサービスを開始するには数秒かかります。

1. サービスの状態を確認するには、`sc query {SERVICE NAME}` コマンドを使用します。 この状態は、次のいずれかの値として報告されます。

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。

   ```console
   sc query MyService
   ```

1. サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。

   サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。

1. `sc stop {SERVICE NAME}` コマンドを使用して、サービスを停止します。

   サンプル アプリ サービスは、次のコマンドで停止できます。

   ```console
   sc stop MyService
   ```

1. サービスの停止の少し後に、`sc delete {SERVICE NAME}` コマンドを使用して、サービスをアンインストールします。

   サンプル アプリ サービスの状態を確認します。

   ```console
   sc query MyService
   ```

   サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>サービスの外部でアプリを実行する

サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。 たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。

> [!NOTE]
> [統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントの処理

[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。

1. [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > [統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。

カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。 詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。

## <a name="configure-https"></a>HTTPS の構成

セキュリティで保護されたエンドポイントを使用してサービスを構成するには:

1. プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。
1. [Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。

サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。

## <a name="current-directory-and-content-root"></a>現在のディレクトリとコンテンツのルート

Windows サービスに対して `Directory.GetCurrentDirectory()` を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。 *system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使用する場合、次のいずれかの方法を使用して、サービスの資産と設定ファイルを [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) に格納し、アクセスします。

* コンテンツのルート パスを使用します。 `IHostingEnvironment.ContentRootPath` は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。 `Directory.GetCurrentDirectory()` を使用して設定ファイルのパスを作成する代わりに、コンテンツのルートパスを使用して、アプリのコンテンツ ルートにファイルを格納します。
* ディスク上の適切な場所にファイルを格納します。 ファイルを含むフォルダーを示す絶対パスを `SetBasePath` で指定します。

## <a name="additional-resources"></a>その他の技術情報

* [Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)
* <xref:fundamentals/host/web-host>
