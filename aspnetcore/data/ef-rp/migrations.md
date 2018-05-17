---
title: ASP.NET Core の Razor ページと EF Core - 移行 - 4/8
author: rick-anderson
description: このチュートリアルでは、ASP.NET Core MVC アプリでデータ モデルの変更を管理するための EF Core の移行機能の使用を開始します。
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="756e0-103">ASP.NET Core の Razor ページと EF Core - 移行 - 4/8</span><span class="sxs-lookup"><span data-stu-id="756e0-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="756e0-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="756e0-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="756e0-105">このチュートリアルでは、データ モデルの変更を管理するための EF Core の移行機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="756e0-106">解決できない問題が発生した場合は、[このステージの完成したアプリ](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="756e0-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="756e0-107">新しいアプリを開発するときには、頻繁にデータ モデルを変更します。</span><span class="sxs-lookup"><span data-stu-id="756e0-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="756e0-108">モデルを変更するたびに、モデルはデータベースと同期されなくなります。</span><span class="sxs-lookup"><span data-stu-id="756e0-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="756e0-109">このチュートリアルでは、最初にデータベースが存在しない場合にデータベースを作成するように Entity Framework を構成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="756e0-110">データ モデルが変更されるたびに次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-110">Each time the data model changes:</span></span>

* <span data-ttu-id="756e0-111">データベースが削除されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-111">The DB is dropped.</span></span>
* <span data-ttu-id="756e0-112">EF がモデルと一致する新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="756e0-113">アプリがテスト データを DB に格納します。</span><span class="sxs-lookup"><span data-stu-id="756e0-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="756e0-114">このアプローチは、実稼働環境にアプリを展開するまで、データベースとデータ モデルの同期の維持がうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="756e0-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="756e0-115">アプリが実稼働環境で実行されているときには、通常は維持する必要があるデータをアプリが格納します。</span><span class="sxs-lookup"><span data-stu-id="756e0-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="756e0-116">変更 (新しい列の追加など) が加えられるたびにテスト データベースを使用してアプリを開始することはできません。</span><span class="sxs-lookup"><span data-stu-id="756e0-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="756e0-117">EF コア移行機能は、新しいデータベースを作成する代わりに EF Core でデータベース スキーマを更新できるようにすることでこの問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="756e0-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="756e0-118">データ モデルが変更されたときにデータベースを削除して再作成する代わりに、移行によってスキーマを更新し、既存のデータを維持します。</span><span class="sxs-lookup"><span data-stu-id="756e0-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="756e0-119">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="756e0-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="756e0-120">移行を使用するには、**パッケージ マネージャー コンソール** (PMC) またはコマンドライン インターフェイス (CLI) を使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="756e0-121">これらのチュートリアルでは、CLI コマンドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="756e0-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="756e0-122">PMC については、[このチュートリアルの最後](#pmc)に説明します。</span><span class="sxs-lookup"><span data-stu-id="756e0-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="756e0-123">コマンド ライン インターフェイス (CLI) の EF Core ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="756e0-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="756e0-124">このパッケージをインストールするには、下に示すように、*.csproj* ファイルの `DotNetCliToolReference` コレクションにパッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="756e0-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="756e0-125">**注:** このパッケージをインストールするには、*.csproj* ファイルを編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="756e0-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="756e0-126">`install-package` コマンドまたはパッケージ マネージャー GUI を使用してこのパッケージをインストールすることはできません。</span><span class="sxs-lookup"><span data-stu-id="756e0-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="756e0-127">*.csproj* ファイルを編集するには、**ソリューション エクスプローラー**でプロジェクト名を右クリックし、**[ContosoUniversity.csproj の編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="756e0-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="756e0-128">次のマークアップは、更新された *.csproj* ファイルと強調表示された EF Core CLI ツールを示しています。</span><span class="sxs-lookup"><span data-stu-id="756e0-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="756e0-129">上記の例では、バージョン番号は、チュートリアルが記述された時点で最新でした。</span><span class="sxs-lookup"><span data-stu-id="756e0-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="756e0-130">他のパッケージで見つかった EF Core CLI ツールの同じバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="756e0-131">接続文字列を変更する</span><span class="sxs-lookup"><span data-stu-id="756e0-131">Change the connection string</span></span>

<span data-ttu-id="756e0-132">*appsettings.json* ファイルで、接続文字列内のデータベースの名前を ContosoUniversity2 に変更します。</span><span class="sxs-lookup"><span data-stu-id="756e0-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="756e0-133">接続文字列でデータベース名を変更すると、最初の移行で新しいデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="756e0-134">新しいデータベースが作成されるのは、その名前のデータベースが存在していないためです。</span><span class="sxs-lookup"><span data-stu-id="756e0-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="756e0-135">移行で開始するには、接続文字列を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="756e0-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="756e0-136">データベース名を変更する代わりにデータベースを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="756e0-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="756e0-137">**SQL Server オブジェクト エクスプローラー** (SSOX) または `database drop` CLI コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="756e0-138">次のセクションでは、CLI コマンドを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="756e0-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="756e0-139">初期移行を作成する</span><span class="sxs-lookup"><span data-stu-id="756e0-139">Create an initial migration</span></span>

<span data-ttu-id="756e0-140">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="756e0-140">Build the project.</span></span>

<span data-ttu-id="756e0-141">コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="756e0-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="756e0-142">プロジェクト フォルダーには *Startup.cs* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="756e0-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="756e0-143">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="756e0-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="756e0-144">コマンド ウィンドウには、次のような情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="756e0-145">移行が失敗した場合は、"*ファイル ...ContosoUniversity.dll は、別のプロセスで使用されているため、アクセスできません。*"</span><span class="sxs-lookup"><span data-stu-id="756e0-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="756e0-146">というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-146">is displayed:</span></span>

* <span data-ttu-id="756e0-147">IIS Express を終了する</span><span class="sxs-lookup"><span data-stu-id="756e0-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="756e0-148">Visual Studio を終了して再起動します。または、</span><span class="sxs-lookup"><span data-stu-id="756e0-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="756e0-149">Windows のシステム トレイで IIS Express アイコンを見つけます。</span><span class="sxs-lookup"><span data-stu-id="756e0-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="756e0-150">IIS Express アイコンを右クリックし、**[ContosoUniversity] > [サイトの停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="756e0-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="756e0-151">"ビルドに失敗しました" というエラー メッセージが</span><span class="sxs-lookup"><span data-stu-id="756e0-151">If the error message "Build failed."</span></span> <span data-ttu-id="756e0-152">表示された場合、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-152">is displayed, run the command again.</span></span> <span data-ttu-id="756e0-153">このエラーが発生する場合は、このチュートリアルの最後にメモを残してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="756e0-154">Up および Down メソッドを確認する</span><span class="sxs-lookup"><span data-stu-id="756e0-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="756e0-155">EF Core コマンド `migrations add` は、データベースの作成元のコードを生成しました。</span><span class="sxs-lookup"><span data-stu-id="756e0-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="756e0-156">この移行コードは *Migrations\<timestamp>_InitialCreate.cs* ファイルにあります。</span><span class="sxs-lookup"><span data-stu-id="756e0-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="756e0-157">`InitialCreate` クラスの `Up` メソッドは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="756e0-158">次の例で示すように、`Down` メソッドは、それらを削除します。</span><span class="sxs-lookup"><span data-stu-id="756e0-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="756e0-159">移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。</span><span class="sxs-lookup"><span data-stu-id="756e0-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="756e0-160">更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="756e0-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="756e0-161">上記のコードは、初期の移行のためのコードです。</span><span class="sxs-lookup"><span data-stu-id="756e0-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="756e0-162">そのコードは、`migrations add InitialCreate` コマンドが実行されたときに作成されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="756e0-163">移行の name パラメーター (この例では "InitialCreate") は、ファイル名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="756e0-164">移行名には、任意の有効なファイル名を指定できます。</span><span class="sxs-lookup"><span data-stu-id="756e0-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="756e0-165">移行で行われている処理をまとめた単語または語句を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="756e0-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="756e0-166">たとえば、department テーブルを追加する移行に "AddDepartmentTable" という名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="756e0-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="756e0-167">最初の移行が作成され、データベースが存在している場合:</span><span class="sxs-lookup"><span data-stu-id="756e0-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="756e0-168">データベース作成コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="756e0-169">データベースは既にデータ モデルと一致しているため、データベース作成コードを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="756e0-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="756e0-170">データベース作成コードが実行された場合、データベースは既にデータ モデルと一致しているため、何も変更しません。</span><span class="sxs-lookup"><span data-stu-id="756e0-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="756e0-171">新しい環境にアプリが展開されるときには、データベースを作成するためのデータベース作成コードを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="756e0-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="756e0-172">接続文字列は前にデータベースの新しい名前を使用するように変更されました。</span><span class="sxs-lookup"><span data-stu-id="756e0-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="756e0-173">指定されたデータベースが存在しないため、移行は、データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="756e0-174">データ モデルのスナップショット</span><span class="sxs-lookup"><span data-stu-id="756e0-174">The data model snapshot</span></span>

<span data-ttu-id="756e0-175">移行は、現在のデータベース スキーマの*スナップショット*を *Migrations/SchoolContextModelSnapshot.cs* 内に作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="756e0-176">移行を追加するときに、EF は、スナップショット ファイルとデータ モデルを比較することによって変更内容を判断します。</span><span class="sxs-lookup"><span data-stu-id="756e0-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="756e0-177">移行を削除するには、[dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="756e0-178">`dotnet ef migrations remove` によって移行が削除され、スナップショットが正しくリセットされたことが確認されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="756e0-179">スナップショット ファイルの使用方法の詳細については、「[EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams)」 (チーム環境での EF Core 移行) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="756e0-180">Remove EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="756e0-180">Remove EnsureCreated</span></span>

<span data-ttu-id="756e0-181">初期の開発では、`EnsureCreated` コマンドが使用されました。</span><span class="sxs-lookup"><span data-stu-id="756e0-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="756e0-182">このチュートリアルでは、migrations を使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="756e0-183">`EnsureCreated` には次の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="756e0-184">移行をバイパスし、データベースとスキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="756e0-185">移行テーブルは作成しません。</span><span class="sxs-lookup"><span data-stu-id="756e0-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="756e0-186">移行と共に使用することは*できません*。</span><span class="sxs-lookup"><span data-stu-id="756e0-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="756e0-187">データベースが頻繁に削除および再作成されるようなテストや迅速なプロトタイプのために設計されています。</span><span class="sxs-lookup"><span data-stu-id="756e0-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="756e0-188">`DbInitializer` から次の行を削除します。</span><span class="sxs-lookup"><span data-stu-id="756e0-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="756e0-189">開発中のデータベースに移行を適用する</span><span class="sxs-lookup"><span data-stu-id="756e0-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="756e0-190">コマンド ウィンドウで、次のコマンドを入力してデータベースとテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="756e0-191">メモ: `update` コマンドが "ビルドに失敗しました" というエラーを返した場合は、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="756e0-192">コマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-192">Run the command again.</span></span>
* <span data-ttu-id="756e0-193">再度失敗した場合は、Visual Studio を終了し、`update` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="756e0-194">ページの下部にメッセージを残します。</span><span class="sxs-lookup"><span data-stu-id="756e0-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="756e0-195">コマンドの出力は、`migrations add` コマンドの出力に似ています。</span><span class="sxs-lookup"><span data-stu-id="756e0-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="756e0-196">上記のコマンドでは、データベースをセットアップする SQL コマンドのログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="756e0-197">次のサンプル出力ではログのほとんどは省略されています。</span><span class="sxs-lookup"><span data-stu-id="756e0-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="756e0-198">ログ メッセージの詳細レベルを下げるために、*appsettings.Development.json* ファイルでログ レベルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="756e0-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="756e0-199">詳細については、[ログ記録の概要](xref:fundamentals/logging/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="756e0-200">**SQL Server オブジェクト エクスプローラー**を使用してデータベースを検査します。</span><span class="sxs-lookup"><span data-stu-id="756e0-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="756e0-201">`__EFMigrationsHistory` テーブルが追加されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="756e0-202">`__EFMigrationsHistory` テーブルは、どの移行がデータベースに適用されたかを追跡します。</span><span class="sxs-lookup"><span data-stu-id="756e0-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="756e0-203">`__EFMigrationsHistory` テーブルのデータを表示すると、最初の移行の 1 つの行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="756e0-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="756e0-204">前の CLI の出力例の最後のログは、この行を作成する INSERT ステートメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="756e0-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="756e0-205">アプリを実行して、すべてが適切に機能していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="756e0-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="756e0-206">運用環境で移行を適用する</span><span class="sxs-lookup"><span data-stu-id="756e0-206">Applying migrations in production</span></span>

<span data-ttu-id="756e0-207">実稼働アプリケーションでは、アプリケーションの起動時に [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) を**呼び出さない**ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="756e0-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="756e0-208">`Migrate` をサーバー ファームのアプリから呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="756e0-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="756e0-209">たとえば、アプリがスケールアウト (アプリの複数のインスタンスを実行する) を使用してクラウドに展開されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="756e0-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="756e0-210">データベースの移行は、展開の一部として制御された方法で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="756e0-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="756e0-211">実稼働データベースの移行には次の方法があります。</span><span class="sxs-lookup"><span data-stu-id="756e0-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="756e0-212">移行を使用して SQL スクリプトを作成し、展開で SQL スクリプトを使用します。</span><span class="sxs-lookup"><span data-stu-id="756e0-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="756e0-213">制御された環境から `dotnet ef database update` を実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="756e0-214">EF Core は、`__MigrationsHistory` テーブルを使用して、移行を実行する必要があるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="756e0-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="756e0-215">データベースが最新の状態になっている場合、移行は実行されません。</span><span class="sxs-lookup"><span data-stu-id="756e0-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="756e0-216">コマンド ライン インターフェイス (CLI) とパッケージ マネージャー コンソール (PMI) の比較</span><span class="sxs-lookup"><span data-stu-id="756e0-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="756e0-217">移行を管理するための EF Core ツールを次の方法で使用できます。</span><span class="sxs-lookup"><span data-stu-id="756e0-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="756e0-218">.NET Core CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="756e0-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="756e0-219">Visual Studio **パッケージ マネージャー コンソール** (PMC) ウィンドウの PowerShell コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="756e0-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="756e0-220">このチュートリアルでは、CLI を使用する方法を示します。PMC を好む開発者もいます。</span><span class="sxs-lookup"><span data-stu-id="756e0-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="756e0-221">PMC の EF Core コマンドは、[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) パッケージ内にあります。</span><span class="sxs-lookup"><span data-stu-id="756e0-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="756e0-222">このパッケージは [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージに含まれています。インストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="756e0-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="756e0-223">**重要:** このパッケージは、*.csproj* ファイルを編集することで、CLI 用にインストールするパッケージと同じものではありません。</span><span class="sxs-lookup"><span data-stu-id="756e0-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="756e0-224">`Tools.DotNet` で終わる CLI パッケージ名とは異なり、このパッケージの名前は `Tools` で終わります。</span><span class="sxs-lookup"><span data-stu-id="756e0-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="756e0-225">CLI コマンドの詳細については、「[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="756e0-226">PMC コマンドの詳細については、「[パッケージ マネージャー コンソール (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="756e0-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="756e0-227">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="756e0-227">Troubleshooting</span></span>

<span data-ttu-id="756e0-228">[このステージの完成したアプリ](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="756e0-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="756e0-229">アプリは次の例外を生成します。</span><span class="sxs-lookup"><span data-stu-id="756e0-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="756e0-230">Solution: Run `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="756e0-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="756e0-231">`update` コマンドが "ビルドに失敗しました" というエラーを返した場合は、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="756e0-232">コマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="756e0-232">Run the command again.</span></span>
* <span data-ttu-id="756e0-233">ページの下部にメッセージを残します。</span><span class="sxs-lookup"><span data-stu-id="756e0-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="756e0-234">[前へ](xref:data/ef-rp/sort-filter-page)
> [次へ](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="756e0-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
