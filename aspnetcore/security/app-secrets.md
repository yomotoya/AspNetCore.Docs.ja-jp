---
title: アプリ シークレットは、ASP.NET Core での開発での安全な格納場所
author: rick-anderson
description: 保存し、アプリ シークレットは、ASP.NET Core アプリケーションの開発時に重要な情報を取得する方法を説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="b1941-103">アプリ シークレットは、ASP.NET Core での開発での安全な格納場所</span><span class="sxs-lookup"><span data-stu-id="b1941-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="b1941-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、および[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b1941-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b1941-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b1941-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b1941-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="b1941-107">このドキュメントでは、格納および ASP.NET Core アプリケーションの開発中に機密データを取得するためのテクニックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b1941-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="b1941-108">ソース コードで、パスワードや他の機密データを格納する必要がありますしない、モードをテストまたは開発で実稼働の機密情報を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b1941-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="b1941-109">格納しと Azure のテストおよび運用シークレットを保護することができます、 [Azure Key Vault の構成プロバイダー](xref:security/key-vault-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="b1941-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="b1941-110">環境変数</span><span class="sxs-lookup"><span data-stu-id="b1941-110">Environment variables</span></span>

<span data-ttu-id="b1941-111">環境変数は、アプリ シークレットは、コードまたはでローカルの構成ファイルの格納を避けるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="b1941-112">環境変数は、以前に指定した構成のすべてのソースの構成値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="b1941-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-113">環境変数の値の読み取りを呼び出すことによって構成[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="b1941-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="b1941-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b1941-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="b1941-115">ASP.NET Core web アプリを検討してください**個々 のユーザー アカウント**セキュリティが有効にします。</span><span class="sxs-lookup"><span data-stu-id="b1941-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="b1941-116">プロジェクトの既定のデータベース接続文字列が含まれている*される appsettings.json*キーを持つファイル`DefaultConnection`です。</span><span class="sxs-lookup"><span data-stu-id="b1941-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="b1941-117">既定の接続文字列では、LocalDB では、ユーザー モードで実行され、パスワードを必要としないのです。</span><span class="sxs-lookup"><span data-stu-id="b1941-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="b1941-118">アプリの展開中に、`DefaultConnection`キーの値を環境変数の値で上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="b1941-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="b1941-119">環境変数には、機密性の高い資格情報を持つ完全な接続文字列を格納できます。</span><span class="sxs-lookup"><span data-stu-id="b1941-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="b1941-120">環境変数は、暗号化されていないプレーン テキストに一般的に格納されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="b1941-121">コンピューターまたはプロセスが侵害された場合、環境変数は、信頼されていないパーティによってアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b1941-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="b1941-122">追加の手段をユーザーの機密情報の漏えいを防ぐためには、必要な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b1941-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="b1941-123">シークレット マネージャー</span><span class="sxs-lookup"><span data-stu-id="b1941-123">Secret Manager</span></span>

<span data-ttu-id="b1941-124">シークレット マネージャー ツールは、ASP.NET Core プロジェクトの開発中に機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="b1941-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="b1941-125">このコンテキストでは、機密データの一部は、アプリのシークレットがします。</span><span class="sxs-lookup"><span data-stu-id="b1941-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="b1941-126">アプリ シークレットは、プロジェクト ツリーから別の場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="b1941-127">アプリ シークレットは、特定のプロジェクトに関連付けられているか、いくつかのプロジェクト間で共有します。</span><span class="sxs-lookup"><span data-stu-id="b1941-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="b1941-128">アプリ シークレットは、ソース管理にチェックインされません。</span><span class="sxs-lookup"><span data-stu-id="b1941-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="b1941-129">シークレット マネージャー ツールは、保存されたシークレットを暗号化しないし、信頼できるストアとして扱われるべきではありません。</span><span class="sxs-lookup"><span data-stu-id="b1941-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="b1941-130">これは、開発目的でのみです。</span><span class="sxs-lookup"><span data-stu-id="b1941-130">It's for development purposes only.</span></span> <span data-ttu-id="b1941-131">キーと値は、ユーザーのプロファイル ディレクトリに JSON 構成ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="b1941-132">シークレット マネージャー ツールの動作</span><span class="sxs-lookup"><span data-stu-id="b1941-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="b1941-133">値が格納されている場所と方法など、実装の詳細を抽象シークレット Manager ツール。</span><span class="sxs-lookup"><span data-stu-id="b1941-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="b1941-134">これらの実装の詳細を知ることがなくツールを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b1941-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="b1941-135">値が格納されている、 [JSON](https://json.org/)システムで保護されたユーザー プロファイル フォルダーにローカル コンピューター上の構成ファイル。</span><span class="sxs-lookup"><span data-stu-id="b1941-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b1941-136">Windows</span><span class="sxs-lookup"><span data-stu-id="b1941-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="b1941-137">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="b1941-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="b1941-138">macOS</span><span class="sxs-lookup"><span data-stu-id="b1941-138">macOS</span></span>](#tab/macos)

<span data-ttu-id="b1941-139">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="b1941-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="b1941-140">Linux</span><span class="sxs-lookup"><span data-stu-id="b1941-140">Linux</span></span>](#tab/linux)

<span data-ttu-id="b1941-141">ファイル システム パス。</span><span class="sxs-lookup"><span data-stu-id="b1941-141">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="b1941-142">上記の「ファイル パス、それぞれ置き換えます`<user_secrets_id>`で、`UserSecretsId`で指定された値、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b1941-142">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="b1941-143">場所またはシークレット マネージャー ツールを使用して保存されたデータの形式に依存するコードを記述しません。</span><span class="sxs-lookup"><span data-stu-id="b1941-143">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="b1941-144">これらの実装の詳細を変更することがあります。</span><span class="sxs-lookup"><span data-stu-id="b1941-144">These implementation details may change.</span></span> <span data-ttu-id="b1941-145">たとえば、秘密の値は暗号化されませんが、後である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b1941-145">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="b1941-146">シークレット マネージャー ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b1941-146">Install the Secret Manager tool</span></span>

<span data-ttu-id="b1941-147">シークレット マネージャー ツールには、.NET Core SDK 2.1 では、.NET Core CLI が付属しています。</span><span class="sxs-lookup"><span data-stu-id="b1941-147">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="b1941-148">.NET Core SDK 2.0 と前に、ツールのインストールは必要があります。</span><span class="sxs-lookup"><span data-stu-id="b1941-148">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="b1941-149">インストール、 [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core プロジェクトでの NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="b1941-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="b1941-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="b1941-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="b1941-151">ツールのインストールを検証するコマンド シェルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b1941-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="b1941-152">シークレット マネージャー ツールには、サンプルの使用方法、オプション、およびコマンドのヘルプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="b1941-153">同じディレクトリにする必要があります、 *.csproj*で定義されているツールを実行するファイル、 *.csproj*ファイルの`DotNetCliToolReference`要素。</span><span class="sxs-lookup"><span data-stu-id="b1941-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="b1941-154">シークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="b1941-154">Set a secret</span></span>

<span data-ttu-id="b1941-155">シークレット マネージャー ツールは、ユーザー プロファイルに格納されているプロジェクトに固有の構成設定で動作します。</span><span class="sxs-lookup"><span data-stu-id="b1941-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="b1941-156">ユーザーの機密情報を使用して、定義、`UserSecretsId`内の要素、`PropertyGroup`の *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b1941-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="b1941-157">値`UserSecretsId`任意で、プロジェクトに固有です。</span><span class="sxs-lookup"><span data-stu-id="b1941-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="b1941-158">開発者が通常の GUID を生成、`UserSecretsId`です。</span><span class="sxs-lookup"><span data-stu-id="b1941-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b1941-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b1941-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b1941-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="b1941-161">Visual Studio で、ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**管理ユーザーの機密情報**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="b1941-161">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b1941-162">このジェスチャを追加、`UserSecretsId`要素、GUID に設定、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b1941-162">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="b1941-163">Visual Studio を開き、 *secrets.json*ファイル テキスト エディターでします。</span><span class="sxs-lookup"><span data-stu-id="b1941-163">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="b1941-164">内容を置き換える*secrets.json*を格納するキー/値ペアにします。</span><span class="sxs-lookup"><span data-stu-id="b1941-164">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="b1941-165">例: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="b1941-165">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="b1941-166">キーとその値で構成されるアプリのシークレットを定義します。</span><span class="sxs-lookup"><span data-stu-id="b1941-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="b1941-167">シークレットは、プロジェクトの関連付けられた`UserSecretsId`値。</span><span class="sxs-lookup"><span data-stu-id="b1941-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="b1941-168">たとえば、次のコマンドを実行するディレクトリから、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="b1941-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="b1941-169">この例では、コロンのあることを意味`Movies`オブジェクトでリテラルでは、`ServiceApiKey`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b1941-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="b1941-170">シークレット マネージャー ツールは、その他のディレクトリからも使用できます。</span><span class="sxs-lookup"><span data-stu-id="b1941-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="b1941-171">使用して、`--project`をファイル システム パスを指定するオプション、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="b1941-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="b1941-172">例えば:</span><span class="sxs-lookup"><span data-stu-id="b1941-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="b1941-173">複数の機密情報を設定します。</span><span class="sxs-lookup"><span data-stu-id="b1941-173">Set multiple secrets</span></span>

<span data-ttu-id="b1941-174">JSON をパイプして機密データのバッチを設定することができます、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="b1941-174">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="b1941-175">次の例で、 *input.json*ファイルの内容は、パイプを使用して、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="b1941-175">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b1941-176">Windows</span><span class="sxs-lookup"><span data-stu-id="b1941-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="b1941-177">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b1941-177">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="b1941-178">macOS</span><span class="sxs-lookup"><span data-stu-id="b1941-178">macOS</span></span>](#tab/macos)

<span data-ttu-id="b1941-179">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b1941-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b1941-180">Linux</span><span class="sxs-lookup"><span data-stu-id="b1941-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="b1941-181">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b1941-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="b1941-182">シークレットをアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b1941-182">Access a secret</span></span>

<span data-ttu-id="b1941-183">[ASP.NET Core の構成 API](xref:fundamentals/configuration/index)シークレット Manager の機密情報へのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="b1941-183">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="b1941-184">かどうか .NET Core をターゲットとする 1.x または .NET Framework をインストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="b1941-184">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-185">ユーザーの機密情報の構成ソースを追加、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="b1941-185">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="b1941-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="b1941-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="b1941-187">ユーザーの機密情報を取得できます、 `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="b1941-187">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="b1941-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b1941-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="b1941-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="b1941-190">シークレットの文字列の置換</span><span class="sxs-lookup"><span data-stu-id="b1941-190">String replacement with secrets</span></span>

<span data-ttu-id="b1941-191">パスワードがプレーン テキストで格納することは危険です。</span><span class="sxs-lookup"><span data-stu-id="b1941-191">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="b1941-192">たとえば、データベース接続文字列の格納*される appsettings.json*指定したユーザーのパスワードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b1941-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="b1941-193">安全な方法では、シークレットとしてパスワードを格納します。</span><span class="sxs-lookup"><span data-stu-id="b1941-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="b1941-194">例えば:</span><span class="sxs-lookup"><span data-stu-id="b1941-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="b1941-195">パスワードを置き換える*される appsettings.json*プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="b1941-195">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="b1941-196">次の例では、`{0}`フォームにプレース ホルダーとして使用される、[複合書式指定文字列](/dotnet/standard/base-types/composite-formatting#composite-format-string)です。</span><span class="sxs-lookup"><span data-stu-id="b1941-196">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="b1941-197">シークレットの値は、接続文字列を完了するプレース ホルダーに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="b1941-197">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b1941-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="b1941-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b1941-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="b1941-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="b1941-200">シークレットを一覧します。</span><span class="sxs-lookup"><span data-stu-id="b1941-200">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b1941-201">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="b1941-201">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="b1941-202">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-202">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="b1941-203">前の例では、キー名にコロンは内でオブジェクト階層を表します。 *secrets.json*です。</span><span class="sxs-lookup"><span data-stu-id="b1941-203">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="b1941-204">1 つのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="b1941-204">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b1941-205">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="b1941-205">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="b1941-206">アプリの*secrets.json*に関連付けられているキー/値ペアを削除するファイルが変更された、`MoviesConnectionString`キー。</span><span class="sxs-lookup"><span data-stu-id="b1941-206">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="b1941-207">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-207">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="b1941-208">すべてのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="b1941-208">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b1941-209">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="b1941-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="b1941-210">アプリのすべてのユーザーの機密情報はから削除されている、 *secrets.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b1941-210">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="b1941-211">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b1941-211">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="b1941-212">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b1941-212">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>