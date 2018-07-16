---
title: ASP.NET Core MVC と EF Core - 移行 - 4/10
author: rick-anderson
description: このチュートリアルでは、ASP.NET Core MVC アプリケーションでデータ モデルの変更を管理するための EF Core の移行機能の使用を開始します。
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: f710b33ac1a6017b0e3d7e8c3e528675a41424bb
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194171"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="06346-103">ASP.NET Core MVC と EF Core - 移行 - 4/10</span><span class="sxs-lookup"><span data-stu-id="06346-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="06346-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06346-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="06346-105">Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06346-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="06346-106">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="06346-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="06346-107">このチュートリアルでは、データ モデルの変更を管理するための EF Core の移行機能の使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="06346-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="06346-108">チュートリアルの後半では、データ モデルを変更するときに移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="06346-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="06346-109">移行の概要</span><span class="sxs-lookup"><span data-stu-id="06346-109">Introduction to migrations</span></span>

<span data-ttu-id="06346-110">新しいアプリケーションを開発して、データ モデルが頻繁に変更される場合、モデルが変更されるたびに、モデルはデータベースと同期されなくなります。</span><span class="sxs-lookup"><span data-stu-id="06346-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="06346-111">データベースが存在しない場合にデータベースを作成するように Entity Framework を構成して、これらのチュートリアルを開始しました。</span><span class="sxs-lookup"><span data-stu-id="06346-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="06346-112">そのため、データ モデルを変更する (エンティティ クラスを追加、削除、または変更するか、DbContext クラスを変更する) たびに、データベースを削除することができ、EF でモデルに一致する新しいデータベースを作成し、テスト データを使ってシードを設定します。</span><span class="sxs-lookup"><span data-stu-id="06346-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="06346-113">このメソッドは、実稼働環境にアプリケーションを展開するまで、データベースとデータ モデルの同期の維持がうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="06346-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="06346-114">実稼働環境でアプリケーションを実行している場合、通常、保持する必要があるデータが保存され、新しい列の追加などの変更を加えるたびにすべてが失われないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="06346-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="06346-115">EF Core の移行機能は、新しいデータベースを作成する代わりに、EF でデータベース スキーマを更新できるようにすることで、この問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="06346-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="06346-116">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="06346-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="06346-117">移行を使用するには、**パッケージ マネージャー コンソール** (PMC) またはコマンド ライン インターフェイス (CLI) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="06346-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="06346-118">このチュートリアルでは、CLI コマンドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06346-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="06346-119">PMC については、[このチュートリアルの最後](#pmc)に説明します。</span><span class="sxs-lookup"><span data-stu-id="06346-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="06346-120">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="06346-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="06346-121">このパッケージをインストールするには、以下のように、*.csproj* ファイルの `DotNetCliToolReference` コレクションにパッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="06346-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="06346-122">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="06346-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="06346-123">*.csproj* ファイルを編集するには、**[ソリューション エクスプローラー]** でプロジェクト名を右クリックし、**[ContosoUniversity.csproj の編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="06346-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="06346-124">(この例のバージョン番号は、チュートリアルが記述された時点で最新でした)。</span><span class="sxs-lookup"><span data-stu-id="06346-124">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="06346-125">接続文字列を変更する</span><span class="sxs-lookup"><span data-stu-id="06346-125">Change the connection string</span></span>

<span data-ttu-id="06346-126">*appsettings.json* ファイルで、接続文字列のデータベース名を ContosoUniversity2 に変更するか、使用しているコンピューターではまだ使用していない別の名前に変更します。</span><span class="sxs-lookup"><span data-stu-id="06346-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="06346-127">この変更では、最初の移行で新しいデータベースが作成されるように、プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="06346-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="06346-128">これは移行の作業を開始するために必要ではありませんが、後でこの設定をお勧めする理由がわかります。</span><span class="sxs-lookup"><span data-stu-id="06346-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="06346-129">データベース名を変更する代わりに、データベースを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="06346-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="06346-130">**SQL Server オブジェクト エクスプローラー** (SSOX) または `database drop` CLI コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="06346-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="06346-131">次のセクションでは、CLI コマンドを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="06346-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="06346-132">初期移行を作成する</span><span class="sxs-lookup"><span data-stu-id="06346-132">Create an initial migration</span></span>

<span data-ttu-id="06346-133">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="06346-133">Save your changes and build the project.</span></span> <span data-ttu-id="06346-134">次に、コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="06346-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="06346-135">この操作を行う簡単な方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="06346-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="06346-136">**[ソリューション エクスプローラー]** で、プロジェクトを右クリックし、コンテキスト メニューの **[エクスプローラーで開く]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="06346-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![[エクスプローラーで開く] メニュー項目](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="06346-138">アドレス バーに「cmd」と入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="06346-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![コマンド ウィンドウを開く](migrations/_static/open-command-window.png)

<span data-ttu-id="06346-140">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="06346-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="06346-141">コマンド ウィンドウに次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="06346-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="06346-142">"*"dotnet-ef" コマンドに一致する実行可能ファイルが見つかりませんでした*" というエラー メッセージが表示された場合、トラブルシューティングのヘルプについては、[このブログ記事](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="06346-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="06346-143">"*別のプロセスによって使用されているため、ContosoUniversity.dll ファイルにアクセスできません。*" というエラー メッセージが表示された場合は、Windows のシステム トレイの IIS Express アイコンを見つけて右クリックし、**[ContosoUniversity]、[サイトの停止]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="06346-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="06346-144">Up および Down メソッドを確認する</span><span class="sxs-lookup"><span data-stu-id="06346-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="06346-145">`migrations add` コマンドを実行したときに、EF では最初からデータベースを作成するコードが生成されています。</span><span class="sxs-lookup"><span data-stu-id="06346-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="06346-146">このコードは、*\<タイムスタンプ>_InitialCreate.cs* という名前のファイルにある *[Migrations]* フォルダー内にあります。</span><span class="sxs-lookup"><span data-stu-id="06346-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="06346-147">`InitialCreate` クラスの `Up` メソッドでは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down` メソッドでは、次の例に示すようにそれらを削除します。</span><span class="sxs-lookup"><span data-stu-id="06346-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="06346-148">移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。</span><span class="sxs-lookup"><span data-stu-id="06346-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="06346-149">更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="06346-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="06346-150">このコードは、`migrations add InitialCreate` コマンドを入力したときに作成された初期移行向けのものです。</span><span class="sxs-lookup"><span data-stu-id="06346-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="06346-151">移行の name パラメーター (この例では "InitialCreate") は、ファイル名に使用され、どのような名前でも付けることができます。</span><span class="sxs-lookup"><span data-stu-id="06346-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="06346-152">移行で行われている処理をまとめた単語または語句を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="06346-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="06346-153">たとえば、後で移行に "AddDepartmentTable" という名前を付ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="06346-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="06346-154">データベースが既に存在するときに、初期移行を作成した場合、データベースの作成コードが生成されますが、データベースは既にデータと一致しているため、作成コードを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="06346-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="06346-155">データベースがまだ存在しない別の環境にアプリを展開する場合、データベースを作成するために、このコードが実行されるため、最初にテストを行うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="06346-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="06346-156">これが、移行で新しいデータベースを最初から作成できるように、前の接続文字列でデータベースの名前を変更した理由です。</span><span class="sxs-lookup"><span data-stu-id="06346-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="06346-157">データ モデルのスナップショット</span><span class="sxs-lookup"><span data-stu-id="06346-157">The data model snapshot</span></span>

<span data-ttu-id="06346-158">移行は、現在のデータベース スキーマの*スナップショット*を *Migrations/SchoolContextModelSnapshot.cs* 内に作成します。</span><span class="sxs-lookup"><span data-stu-id="06346-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="06346-159">移行を追加するときに、EF は、スナップショット ファイルとデータ モデルを比較することによって変更内容を判断します。</span><span class="sxs-lookup"><span data-stu-id="06346-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="06346-160">移行を削除するには、[dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="06346-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="06346-161">`dotnet ef migrations remove` によって移行が削除され、スナップショットが正しくリセットされたことが確認されます。</span><span class="sxs-lookup"><span data-stu-id="06346-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="06346-162">スナップショット ファイルの使用方法の詳細については、[チーム環境での EF Core 移行](/ef/core/managing-schemas/migrations/teams)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="06346-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="06346-163">移行をデータベースに適用する</span><span class="sxs-lookup"><span data-stu-id="06346-163">Apply the migration to the database</span></span>

<span data-ttu-id="06346-164">コマンド ウィンドウで、次のコマンドを入力してデータベースとデータベース内のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="06346-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="06346-165">コマンドからの出力は、データベースを設定する SQL コマンドのログを表示する以外は、`migrations add` コマンドと同様です。</span><span class="sxs-lookup"><span data-stu-id="06346-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="06346-166">次のサンプル出力では、ログのほとんどは省略されています。</span><span class="sxs-lookup"><span data-stu-id="06346-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="06346-167">ログ メッセージの詳細レベルを表示しない場合は、*appsettings.Development.json* ファイルでログ レベルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="06346-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="06346-168">詳細については、[ログ記録の概要](xref:fundamentals/logging/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="06346-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="06346-169">**SQL Server オブジェクト エクスプローラー**を使用して、最初のチュートリアルで行ったように、データベースを調べます。</span><span class="sxs-lookup"><span data-stu-id="06346-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="06346-170">データベースに適用されている移行を記録する __EFMigrationsHistory テーブルに追加があることがわかります。</span><span class="sxs-lookup"><span data-stu-id="06346-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="06346-171">テーブルのデータを表示すると、最初の移行の 1 行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="06346-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="06346-172">(前の CLI の出力例の最後のログは、この行を作成する INSERT ステートメントを示しています)。</span><span class="sxs-lookup"><span data-stu-id="06346-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="06346-173">アプリケーションを実行して、すべてが以前と同じように動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="06346-173">Run the application to verify that everything still works the same as before.</span></span>

![Students インデックス ページ](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="06346-175">コマンド ライン インターフェイス (CLI) とパッケージ マネージャー コンソール (PMC) の比較</span><span class="sxs-lookup"><span data-stu-id="06346-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="06346-176">移行を管理するための EF ツールは、.NET Core CLI コマンドから、または Visual Studio **パッケージ マネージャー コンソール** (PMC) ウィンドウの PowerShell コマンドレットから利用できます。</span><span class="sxs-lookup"><span data-stu-id="06346-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="06346-177">このチュートリアルでは、CLI の使用方法を示しますが、好みに応じて PMC を使用できます。</span><span class="sxs-lookup"><span data-stu-id="06346-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="06346-178">PMC コマンドの EF コマンドは、[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) パッケージ内にあります。</span><span class="sxs-lookup"><span data-stu-id="06346-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="06346-179">このパッケージは既に [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージに含まれているため、インストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="06346-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="06346-180">**重要:** このパッケージは、*.csproj* ファイルを編集することで、CLI 用にインストールするパッケージと同じものではありません。</span><span class="sxs-lookup"><span data-stu-id="06346-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="06346-181">`Tools.DotNet` で終わる CLI パッケージ名とは異なり、このパッケージの名前は `Tools` で終わります。</span><span class="sxs-lookup"><span data-stu-id="06346-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="06346-182">CLI コマンドの詳細については、「[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="06346-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="06346-183">PMC コマンドの詳細については、「[パッケージ マネージャー コンソール (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="06346-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="06346-184">まとめ</span><span class="sxs-lookup"><span data-stu-id="06346-184">Summary</span></span>

<span data-ttu-id="06346-185">このチュートリアルでは、最初の移行を作成して適用する方法が示されました。</span><span class="sxs-lookup"><span data-stu-id="06346-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="06346-186">次のチュートリアルでは、データ モデルを展開して、詳細なトピックについて確認します。</span><span class="sxs-lookup"><span data-stu-id="06346-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="06346-187">その途中で、追加の移行を作成して適用することになります。</span><span class="sxs-lookup"><span data-stu-id="06346-187">Along the way you'll create and apply additional migrations.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="06346-188">[前へ](sort-filter-page.md)
> [次へ](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="06346-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>
