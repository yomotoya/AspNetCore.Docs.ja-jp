---
title: ASP.NET Core での開発中のアプリ シークレットの安全な格納
author: rick-anderson
description: 保存して、アプリ シークレットと ASP.NET Core アプリの開発中の機密情報を取得する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 195901e466262020fd1217bd9dfb6162910bb861
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893219"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="1fe92-103">ASP.NET Core での開発中のアプリ シークレットの安全な格納</span><span class="sxs-lookup"><span data-stu-id="1fe92-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="1fe92-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、および[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="1fe92-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="1fe92-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1fe92-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1fe92-106">このドキュメントでは、格納して、ASP.NET Core アプリの開発時に機密データを取得するための手法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="1fe92-107">ソース コードでパスワードや他の機密データを保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="1fe92-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="1fe92-108">運用シークレットを使用してはならない開発やテストします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="1fe92-109">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)により、Azure テストと運用のシークレットを格納し、保護することが可能です。</span><span class="sxs-lookup"><span data-stu-id="1fe92-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="1fe92-110">環境変数</span><span class="sxs-lookup"><span data-stu-id="1fe92-110">Environment variables</span></span>

<span data-ttu-id="1fe92-111">環境変数は、コードまたはローカルの構成ファイルでのアプリ シークレットのストレージを回避するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="1fe92-112">環境変数は、以前に指定した構成のすべてのソースの構成値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1fe92-113">環境変数の値の読み取りを呼び出すことによって構成<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="1fe92-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="1fe92-114">ASP.NET Core web アプリを検討してください**個々 のユーザー アカウント**セキュリティを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="1fe92-115">プロジェクトの既定のデータベース接続文字列が含まれている*appsettings.json*キーを持つファイル`DefaultConnection`します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="1fe92-116">既定の接続文字列は、LocalDB では、ユーザー モードで実行し、パスワードを必要としないからです。</span><span class="sxs-lookup"><span data-stu-id="1fe92-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="1fe92-117">アプリのデプロイ時に、`DefaultConnection`キーの値を環境変数の値で上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="1fe92-118">環境変数には、機密の資格情報の完全な接続文字列を格納できます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="1fe92-119">環境変数は、暗号化されていないプレーン テキストで一般的に格納されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="1fe92-120">コンピューターまたはプロセスが侵害された場合、環境変数は、信頼されていないパーティによってアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="1fe92-121">ユーザーの機密情報の漏えいを防ぐためにその他の対策は、必要な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1fe92-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="1fe92-122">Secret Manager</span><span class="sxs-lookup"><span data-stu-id="1fe92-122">Secret Manager</span></span>

<span data-ttu-id="1fe92-123">Secret Manager ツールは、ASP.NET Core プロジェクトの開発時に機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="1fe92-124">このコンテキストでは、機密データの一部は、アプリのシークレットは。</span><span class="sxs-lookup"><span data-stu-id="1fe92-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="1fe92-125">アプリ シークレットは、プロジェクト ツリーから別の場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="1fe92-126">アプリ シークレットは、特定のプロジェクトに関連付けられているか、いくつかのプロジェクト間で共有します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="1fe92-127">アプリ シークレットは、ソース管理にチェックインされません。</span><span class="sxs-lookup"><span data-stu-id="1fe92-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="1fe92-128">Secret Manager ツールでは、保存されたシークレットを暗号化しないし、信頼できるストアとして扱うべきではありません。</span><span class="sxs-lookup"><span data-stu-id="1fe92-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="1fe92-129">開発目的でのみです。</span><span class="sxs-lookup"><span data-stu-id="1fe92-129">It's for development purposes only.</span></span> <span data-ttu-id="1fe92-130">キーと値は、ユーザー プロファイル ディレクトリ内の JSON 構成ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="1fe92-131">Secret Manager ツールのしくみ</span><span class="sxs-lookup"><span data-stu-id="1fe92-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="1fe92-132">値が格納されている場所と方法など、実装の詳細を抽象 Secret Manager ツール。</span><span class="sxs-lookup"><span data-stu-id="1fe92-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="1fe92-133">これらの実装の詳細を知ることがなくツールを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="1fe92-134">値は、ローカル コンピューターのシステムで保護されているユーザーのプロファイル フォルダーに JSON 構成ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1fe92-135">Windows</span><span class="sxs-lookup"><span data-stu-id="1fe92-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="1fe92-136">ファイル システム パス:</span><span class="sxs-lookup"><span data-stu-id="1fe92-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="1fe92-137">Linux と macOS</span><span class="sxs-lookup"><span data-stu-id="1fe92-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="1fe92-138">ファイル システム パス:</span><span class="sxs-lookup"><span data-stu-id="1fe92-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="1fe92-139">上記の「ファイル パスは、置換`<user_secrets_id>`で、`UserSecretsId`で指定された値、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="1fe92-140">場所または Secret Manager ツールを使用して保存データの形式に依存するコードを記述しません。</span><span class="sxs-lookup"><span data-stu-id="1fe92-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="1fe92-141">これらの実装の詳細を変更することがあります。</span><span class="sxs-lookup"><span data-stu-id="1fe92-141">These implementation details may change.</span></span> <span data-ttu-id="1fe92-142">たとえば、シークレットの値は暗号化されませんが、今後可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1fe92-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="1fe92-143">Secret Manager ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="1fe92-144">Secret Manager ツールは、.NET Core SDK 2.1.300 で .NET Core CLI を使用してバンドル以降です。</span><span class="sxs-lookup"><span data-stu-id="1fe92-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="1fe92-145">2.1.300 より前に、のバージョンを .NET Core SDK、ツールのインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1fe92-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="1fe92-146">実行`dotnet --version`からコマンド シェルをインストール済みの .NET Core SDK バージョン番号を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1fe92-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="1fe92-147">使用されている .NET Core SDK には、ツールが含まれている場合は、警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="1fe92-148">インストール、 [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core プロジェクトで NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="1fe92-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="1fe92-149">例:</span><span class="sxs-lookup"><span data-stu-id="1fe92-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="1fe92-150">ツールのインストールを検証するコマンド シェルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="1fe92-151">Secret Manager ツールには、サンプルの使用法、オプション、およびコマンドのヘルプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="1fe92-152">同じディレクトリである必要があります、 *.csproj*ファイルで定義されているツールを実行する、 *.csproj*ファイルの`DotNetCliToolReference`要素。</span><span class="sxs-lookup"><span data-stu-id="1fe92-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="1fe92-153">機密情報のストレージを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-153">Enable secret storage</span></span>

<span data-ttu-id="1fe92-154">Secret Manager ツールは、ユーザー プロファイルに格納されているプロジェクト固有の構成設定では動作します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1fe92-155">Secret Manager ツールが含まれています、`init`で .NET Core SDK 3.0.100 コマンドまたはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="1fe92-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="1fe92-156">ユーザー シークレットを使用するには、プロジェクト ディレクトリに、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-156">To use user secrets, run the following command in the project directory:</span></span>

```console
dotnet user-secrets init
```

<span data-ttu-id="1fe92-157">上記のコマンドを追加、`UserSecretsId`内の要素を`PropertyGroup`の *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="1fe92-158">既定での内部テキ スト`UserSecretsId`は GUID です。</span><span class="sxs-lookup"><span data-stu-id="1fe92-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="1fe92-159">内部テキ ストは任意で、プロジェクトに一意です。</span><span class="sxs-lookup"><span data-stu-id="1fe92-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="1fe92-160">ユーザー シークレットを使用する定義を`UserSecretsId`内の要素を`PropertyGroup`の *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="1fe92-161">内部テキ スト`UserSecretsId`は任意ですが、プロジェクトに一意です。</span><span class="sxs-lookup"><span data-stu-id="1fe92-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="1fe92-162">開発者は通常の GUID を生成、`UserSecretsId`します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="1fe92-163">Visual Studio では、ソリューション エクスプ ローラーでプロジェクトを右クリックして**ユーザー シークレットの管理**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="1fe92-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="1fe92-164">このジェスチャを追加、`UserSecretsId`に GUID が設定されている要素、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="1fe92-165">シークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-165">Set a secret</span></span>

<span data-ttu-id="1fe92-166">キーとその値から成るアプリのシークレットを定義します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="1fe92-167">シークレットは、プロジェクトの関連付け`UserSecretsId`値。</span><span class="sxs-lookup"><span data-stu-id="1fe92-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="1fe92-168">たとえば、先のディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="1fe92-169">前の例では、コロンのあることを示します`Movies`はオブジェクトのリテラルを`ServiceApiKey`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="1fe92-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="1fe92-170">Secret Manager ツールは、その他のディレクトリからも使用できます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="1fe92-171">使用して、`--project`をファイル システム パスを指定するオプション、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="1fe92-172">例:</span><span class="sxs-lookup"><span data-stu-id="1fe92-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="1fe92-173">Visual Studio でのフラット化する JSON 構造体</span><span class="sxs-lookup"><span data-stu-id="1fe92-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="1fe92-174">Visual Studio の**ユーザー シークレットの管理**ジェスチャが開き、 *secrets.json*テキスト エディターでファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="1fe92-175">内容を置き換える*secrets.json*キーと値のペアを格納するとします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="1fe92-176">例:</span><span class="sxs-lookup"><span data-stu-id="1fe92-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="1fe92-177">使用して変更した後、JSON の構造がフラット化`dotnet user-secrets remove`または`dotnet user-secrets set`します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="1fe92-178">たとえば、実行している`dotnet user-secrets remove "Movies:ConnectionString"`折りたたみます、`Movies`オブジェクト リテラル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="1fe92-179">変更したファイルには、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1fe92-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="1fe92-180">複数のシークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-180">Set multiple secrets</span></span>

<span data-ttu-id="1fe92-181">JSON をパイプしてシークレットのバッチを設定することができます、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="1fe92-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="1fe92-182">次の例では、 *input.json*ファイルの内容はパイプを使用して、`set`コマンド。</span><span class="sxs-lookup"><span data-stu-id="1fe92-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1fe92-183">Windows</span><span class="sxs-lookup"><span data-stu-id="1fe92-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="1fe92-184">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-184">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="1fe92-185">Linux と macOS</span><span class="sxs-lookup"><span data-stu-id="1fe92-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="1fe92-186">コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-186">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="1fe92-187">シークレットにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-187">Access a secret</span></span>

<span data-ttu-id="1fe92-188">[ASP.NET Core 構成 API](xref:fundamentals/configuration/index) Secret Manager シークレットへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="1fe92-189">プロジェクトが .NET Framework を対象とする場合は、インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="1fe92-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1fe92-190">ASP.NET Core 2.0 以降では、ユーザー シークレット構成ソースが自動的に追加の開発モードで、プロジェクトを呼び出すと<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>事前構成済みの既定値を持つホストの新しいインスタンスを初期化します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="1fe92-191">`CreateDefaultBuilder` 呼び出し<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>ときに、<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>は<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="1fe92-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="1fe92-192">ときに`CreateDefaultBuilder`いない呼び出すことによって、ユーザー シークレット構成ソースを明示的に追加呼び出されると、<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="1fe92-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="1fe92-193">呼び出す`AddUserSecrets`のみ実行すると、アプリ開発環境で次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1fe92-194">インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="1fe92-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="1fe92-195">呼び出してユーザー シークレット構成ソースの追加<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>で、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="1fe92-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="1fe92-196">ユーザー シークレットを取得できます、 `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="1fe92-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="1fe92-197">POCO に機密情報をマップします。</span><span class="sxs-lookup"><span data-stu-id="1fe92-197">Map secrets to a POCO</span></span>

<span data-ttu-id="1fe92-198">POCO (単純な .NET クラスのプロパティを持つ) への全体のオブジェクト リテラルのマッピングは、関連するプロパティを集約するために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1fe92-199">上記のシークレットを POCO をマップするには使用、 `Configuration` API の[オブジェクト グラフのバインド](xref:fundamentals/configuration/index#bind-to-an-object-graph)機能します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="1fe92-200">次のコードにカスタム バインド`MovieSettings`POCO およびアクセス、`ServiceApiKey`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="1fe92-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="1fe92-201">`Movies:ConnectionString`と`Movies:ServiceApiKey`シークレットは、それぞれのプロパティにマップされます`MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="1fe92-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="1fe92-202">秘密情報の文字列の置換</span><span class="sxs-lookup"><span data-stu-id="1fe92-202">String replacement with secrets</span></span>

<span data-ttu-id="1fe92-203">プレーン テキストでパスワードを格納することは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="1fe92-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="1fe92-204">たとえば、データベース接続文字列に格納されている*appsettings.json*指定したユーザーのパスワードを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="1fe92-205">安全なアプローチでは、パスワードをシークレットとして格納します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="1fe92-206">例えば:</span><span class="sxs-lookup"><span data-stu-id="1fe92-206">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="1fe92-207">削除、`Password`キー/値ペア内の接続文字列から*appsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="1fe92-208">例:</span><span class="sxs-lookup"><span data-stu-id="1fe92-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="1fe92-209">シークレットの値を設定することができます、<xref:System.Data.SqlClient.SqlConnectionStringBuilder>オブジェクトの<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*>接続文字列を完了するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="1fe92-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="1fe92-210">シークレットの一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1fe92-211">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="1fe92-212">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="1fe92-213">前の例では、キー名にコロンが内のオブジェクト階層を示しています。 *secrets.json*します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="1fe92-214">1 つのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1fe92-215">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="1fe92-216">アプリの*secrets.json*に関連付けられているキー/値ペアを削除するファイルが変更された、`MoviesConnectionString`キー。</span><span class="sxs-lookup"><span data-stu-id="1fe92-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="1fe92-217">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="1fe92-218">すべてのシークレットを削除します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1fe92-219">ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="1fe92-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="1fe92-220">アプリのすべてのユーザーの機密情報が削除されて、 *secrets.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1fe92-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="1fe92-221">実行している`dotnet user-secrets list`次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1fe92-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="1fe92-222">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1fe92-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
