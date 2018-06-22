---
title: アプリ シークレットは、ASP.NET Core での開発での安全な格納場所
author: rick-anderson
description: 保存し、アプリ シークレットは、ASP.NET Core アプリケーションの開発時に重要な情報を取得する方法を説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
uid: security/app-secrets
ms.openlocfilehash: 8db1d1069958d3b08bbb3b9727ddbc47ae5747df
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275426"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="61ea6-103">アプリ シークレットは、ASP.NET Core での開発での安全な格納場所</span><span class="sxs-lookup"><span data-stu-id="61ea6-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="61ea6-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、および[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="61ea6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="61ea6-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="61ea6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="61ea6-106">このドキュメントでは、格納および ASP.NET Core アプリケーションの開発中に機密データを取得するためのテクニックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="61ea6-107">ソース コードで、パスワードや他の機密データを格納する必要がありますしない、モードをテストまたは開発で実稼働の機密情報を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="61ea6-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="61ea6-108">格納しと Azure のテストおよび運用シークレットを保護することができます、 [Azure Key Vault の構成プロバイダー](xref:security/key-vault-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="61ea6-109">環境変数</span><span class="sxs-lookup"><span data-stu-id="61ea6-109">Environment variables</span></span>

<span data-ttu-id="61ea6-110">環境変数は、アプリ シークレットは、コードまたはでローカルの構成ファイルの格納を避けるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="61ea6-111">環境変数は、以前に指定した構成のすべてのソースの構成値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="61ea6-112">環境変数の値の読み取りを呼び出すことによって構成[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="61ea6-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="61ea6-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="61ea6-114">ASP.NET Core web アプリを検討してください**個々 のユーザー アカウント**セキュリティが有効にします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="61ea6-115">プロジェクトの既定のデータベース接続文字列が含まれている*される appsettings.json*キーを持つファイル`DefaultConnection`です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="61ea6-116">既定の接続文字列では、LocalDB では、ユーザー モードで実行され、パスワードを必要としないのです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="61ea6-117">アプリの展開中に、`DefaultConnection`キーの値を環境変数の値で上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="61ea6-118">環境変数には、機密性の高い資格情報を持つ完全な接続文字列を格納できます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="61ea6-119">環境変数は、暗号化されていないプレーン テキストに一般的に格納されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="61ea6-120">コンピューターまたはプロセスが侵害された場合、環境変数は、信頼されていないパーティによってアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="61ea6-121">追加の手段をユーザーの機密情報の漏えいを防ぐためには、必要な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="61ea6-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="61ea6-122">シークレット マネージャー</span><span class="sxs-lookup"><span data-stu-id="61ea6-122">Secret Manager</span></span>

<span data-ttu-id="61ea6-123">シークレット マネージャー ツールは、ASP.NET Core プロジェクトの開発中に機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="61ea6-124">このコンテキストでは、機密データの一部は、アプリのシークレットがします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="61ea6-125">アプリ シークレットは、プロジェクト ツリーから別の場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="61ea6-126">アプリ シークレットは、特定のプロジェクトに関連付けられているか、いくつかのプロジェクト間で共有します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="61ea6-127">アプリ シークレットは、ソース管理にチェックインされません。</span><span class="sxs-lookup"><span data-stu-id="61ea6-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="61ea6-128">シークレット マネージャー ツールは、保存されたシークレットを暗号化しないし、信頼できるストアとして扱われるべきではありません。</span><span class="sxs-lookup"><span data-stu-id="61ea6-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="61ea6-129">これは、開発目的でのみです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-129">It's for development purposes only.</span></span> <span data-ttu-id="61ea6-130">キーと値は、ユーザーのプロファイル ディレクトリに JSON 構成ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="61ea6-131">シークレット マネージャー ツールの動作</span><span class="sxs-lookup"><span data-stu-id="61ea6-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="61ea6-132">値が格納されている場所と方法など、実装の詳細を抽象シークレット Manager ツール。</span><span class="sxs-lookup"><span data-stu-id="61ea6-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="61ea6-133">これらの実装の詳細を知ることがなくツールを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="61ea6-134">値は、ローカル コンピューター上のシステムで保護されたユーザー プロファイル フォルダーに JSON 構成ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="61ea6-135">Windows</span><span class="sxs-lookup"><span data-stu-id="61ea6-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="61ea6-136">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="61ea6-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="61ea6-137">macOS</span><span class="sxs-lookup"><span data-stu-id="61ea6-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="61ea6-138">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="61ea6-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="61ea6-139">Linux</span><span class="sxs-lookup"><span data-stu-id="61ea6-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="61ea6-140">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="61ea6-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="61ea6-141">上記の「ファイル パス、それぞれ置き換えます`<user_secrets_id>`で、`UserSecretsId`で指定された値、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="61ea6-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="61ea6-142">場所またはシークレット マネージャー ツールを使用して保存されたデータの形式に依存するコードを記述しません。</span><span class="sxs-lookup"><span data-stu-id="61ea6-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="61ea6-143">これらの実装の詳細を変更することがあります。</span><span class="sxs-lookup"><span data-stu-id="61ea6-143">These implementation details may change.</span></span> <span data-ttu-id="61ea6-144">たとえば、秘密の値は暗号化されませんが、後である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="61ea6-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="61ea6-145">シークレット マネージャー ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="61ea6-146">シークレット マネージャー ツールには、.NET Core SDK 2.1.300 時点で、.NET Core CLI が付属しています。</span><span class="sxs-lookup"><span data-stu-id="61ea6-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="61ea6-147">2.1.300 より前に、のバージョンを .NET Core SDK、ツールのインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="61ea6-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="61ea6-148">実行`dotnet --version`インストールされている .NET Core SDK バージョン番号を表示するコマンド シェルからです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="61ea6-149">使用されている、.NET Core SDK には、ツールが含まれている場合は、警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="61ea6-150">インストール、 [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core プロジェクトでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="61ea6-151">例えば:</span><span class="sxs-lookup"><span data-stu-id="61ea6-151">For example:</span></span>

<span data-ttu-id="61ea6-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="61ea6-153">ツールのインストールを検証するコマンド シェルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="61ea6-154">シークレット マネージャー ツールには、サンプルの使用方法、オプション、およびコマンドのヘルプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="61ea6-155">同じディレクトリにする必要があります、 *.csproj*で定義されているツールを実行するファイル、 *.csproj*ファイルの`DotNetCliToolReference`要素。</span><span class="sxs-lookup"><span data-stu-id="61ea6-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="61ea6-156">シークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-156">Set a secret</span></span>

<span data-ttu-id="61ea6-157">シークレット マネージャー ツールは、ユーザー プロファイルに格納されているプロジェクトに固有の構成設定で動作します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="61ea6-158">ユーザーの機密情報を使用して、定義、`UserSecretsId`内の要素、`PropertyGroup`の *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="61ea6-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="61ea6-159">値`UserSecretsId`任意で、プロジェクトに固有です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="61ea6-160">開発者が通常の GUID を生成、`UserSecretsId`です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="61ea6-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="61ea6-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="61ea6-163">Visual Studio で、ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**管理ユーザーの機密情報**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="61ea6-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="61ea6-164">このジェスチャを追加、`UserSecretsId`要素、GUID に設定、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="61ea6-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="61ea6-165">Visual Studio を開き、 *secrets.json*ファイル テキスト エディターでします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="61ea6-166">内容を置き換える*secrets.json*を格納するキー/値ペアにします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="61ea6-167">例: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="61ea6-168">キーとその値で構成されるアプリのシークレットを定義します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="61ea6-169">シークレットは、プロジェクトの関連付けられた`UserSecretsId`値。</span><span class="sxs-lookup"><span data-stu-id="61ea6-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="61ea6-170">たとえば、次のコマンドを実行するディレクトリから、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="61ea6-171">この例では、コロンのあることを意味`Movies`オブジェクトでリテラルでは、`ServiceApiKey`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="61ea6-172">シークレット マネージャー ツールは、その他のディレクトリからも使用できます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="61ea6-173">使用して、`--project`をファイル システム パスを指定するオプション、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="61ea6-174">例えば:</span><span class="sxs-lookup"><span data-stu-id="61ea6-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="61ea6-175">複数の機密情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-175">Set multiple secrets</span></span>

<span data-ttu-id="61ea6-176">JSON をパイプして機密データのバッチを設定することができます、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="61ea6-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="61ea6-177">次の例で、 *input.json*ファイルの内容は、パイプを使用して、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="61ea6-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="61ea6-178">Windows</span><span class="sxs-lookup"><span data-stu-id="61ea6-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="61ea6-179">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="61ea6-180">macOS</span><span class="sxs-lookup"><span data-stu-id="61ea6-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="61ea6-181">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="61ea6-182">Linux</span><span class="sxs-lookup"><span data-stu-id="61ea6-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="61ea6-183">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="61ea6-184">シークレットにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="61ea6-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="61ea6-185">[ASP.NET Core の構成 API](xref:fundamentals/configuration/index)シークレット Manager の機密情報へのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="61ea6-186">インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="61ea6-187">呼び出しには、ユーザーの機密情報構成ソースを追加[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="61ea6-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="61ea6-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="61ea6-189">[ASP.NET Core の構成 API](xref:fundamentals/configuration/index)シークレット Manager の機密情報へのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="61ea6-190">プロジェクトは .NET Framework をターゲットの場合は、インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="61ea6-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="61ea6-191">ASP.NET Core 2.0 以降では、ユーザーの機密情報の構成ソースが自動的に追加の開発モードで、プロジェクトを呼び出すと[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)を事前に構成された既定値を持つホストの新しいインスタンスを初期化します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="61ea6-192">`CreateDefaultBuilder` 呼び出し[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)ときに、 [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)は[開発](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="61ea6-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="61ea6-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="61ea6-194">ときに`CreateDefaultBuilder`いないへの呼び出しには、ユーザーの機密情報構成ソースを追加するホストの構築時に呼び出されると、 [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="61ea6-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="61ea6-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="61ea6-196">ユーザーの機密情報を取得できます、 `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="61ea6-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="61ea6-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="61ea6-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="61ea6-199">シークレットの文字列の置換</span><span class="sxs-lookup"><span data-stu-id="61ea6-199">String replacement with secrets</span></span>

<span data-ttu-id="61ea6-200">パスワードがプレーン テキストで格納することは、安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="61ea6-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="61ea6-201">たとえば、データベース接続文字列の格納*される appsettings.json*指定したユーザーのパスワードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="61ea6-202">安全な方法では、シークレットとしてパスワードを格納します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="61ea6-203">例えば:</span><span class="sxs-lookup"><span data-stu-id="61ea6-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="61ea6-204">削除、`Password`キー/値ペア内の接続文字列から*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="61ea6-205">例えば:</span><span class="sxs-lookup"><span data-stu-id="61ea6-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="61ea6-206">シークレットの値を設定することができます、 [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder)オブジェクトの[パスワード](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password)するには、接続文字列のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="61ea6-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="61ea6-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="61ea6-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="61ea6-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="61ea6-209">シークレットを一覧します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="61ea6-210">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="61ea6-211">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="61ea6-212">前の例では、キー名にコロンは内でオブジェクト階層を表します。 *secrets.json*です。</span><span class="sxs-lookup"><span data-stu-id="61ea6-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="61ea6-213">1 つのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="61ea6-214">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="61ea6-215">アプリの*secrets.json*に関連付けられているキー/値ペアを削除するファイルが変更された、`MoviesConnectionString`キー。</span><span class="sxs-lookup"><span data-stu-id="61ea6-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="61ea6-216">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="61ea6-217">すべてのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="61ea6-218">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="61ea6-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="61ea6-219">アプリのすべてのユーザーの機密情報はから削除されている、 *secrets.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="61ea6-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="61ea6-220">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ea6-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="61ea6-221">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="61ea6-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
