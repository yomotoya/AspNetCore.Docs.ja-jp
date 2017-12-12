---
title: "Razor ページの EF コアな移行 - 4 8"
author: rick-anderson
description: "このチュートリアルでは、ASP.NET Core MVC アプリでデータ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。"
keywords: "ASP.NET Core,Entity Framework Core,移行"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="74a0e-104">移行 - EF コア Razor ページのチュートリアル (8 4)</span><span class="sxs-lookup"><span data-stu-id="74a0e-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="74a0e-105">によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74a0e-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="74a0e-106">このチュートリアルでは、データ モデルの変更を管理するための EF 中核となる移行機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="74a0e-107">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="74a0e-108">新しいアプリの開発時に、データ モデルの変更頻度。</span><span class="sxs-lookup"><span data-stu-id="74a0e-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="74a0e-109">毎回モデルの変更、モデルを取得、データベースと同期します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="74a0e-110">このチュートリアルでは、Entity Framework が存在しない場合、データベースの作成を構成することによって開始します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="74a0e-111">たびに、データ モデルの変更。</span><span class="sxs-lookup"><span data-stu-id="74a0e-111">Each time the data model changes:</span></span>

* <span data-ttu-id="74a0e-112">データベースは削除されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-112">The DB is dropped.</span></span>
* <span data-ttu-id="74a0e-113">EF は、モデルと一致するか、新しいを作成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="74a0e-114">アプリでは、テスト データを使用して DB シード処理されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="74a0e-115">DB のデータ モデルとの同期を維持するためにこのアプローチは、実稼働環境にアプリを展開するまでにも動作します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="74a0e-116">アプリは、実稼働環境で実行中は、データを維持する必要がある通常格納されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="74a0e-117">アプリは、DB のたびに変更 (新しい列を追加する) など、テストを始めることはできません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="74a0e-118">EF コア移行機能は、EF コアを新しいデータベースを作成する代わりにデータベースのスキーマの更新を有効にすると、この問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="74a0e-119">削除して、データ モデルの変更時にデータベースを再作成するではなくは、移行は、スキーマを更新し、既存のデータを保持します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="74a0e-120">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="74a0e-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="74a0e-121">移行を使用するを使用して、 **Package Manager Console** (PMC) またはコマンド ライン インターフェイス (CLI)。</span><span class="sxs-lookup"><span data-stu-id="74a0e-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="74a0e-122">これらのチュートリアルでは、CLI コマンドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="74a0e-123">PMC に関するについては、「[このチュートリアルの最後の](#pmc)します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="74a0e-124">コマンド ライン インターフェイス (CLI) の EF の主要なツールがで提供される[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="74a0e-125">このパッケージをインストールする追加して、`DotNetCliToolReference`内のコレクション、 *.csproj*ファイルを示すようにします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="74a0e-126">**注:**を編集して、このパッケージをインストールする必要があります、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="74a0e-127">`install-package`このパッケージをインストールするコマンドまたはパッケージ マネージャー GUI を使用できません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="74a0e-128">編集、 *.csproj*でプロジェクト名を右クリックしてファイル**ソリューション エクスプ ローラー**を選択して**編集 ContosoUniversity.csproj**です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="74a0e-129">次のマークアップを示しています、更新された*.csproj*強調表示されて、EF コア CLI ツール ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="74a0e-130">上記の例では、バージョン番号は、チュートリアルに書き込まれたときに現在でした。</span><span class="sxs-lookup"><span data-stu-id="74a0e-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="74a0e-131">他のパッケージで見つかった EF コア CLI ツールの同じバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="74a0e-132">接続文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-132">Change the connection string</span></span>

<span data-ttu-id="74a0e-133">*される appsettings.json*ファイル、ContosoUniversity2 を接続文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="74a0e-134">接続文字列でデータベース名を変更すると、新しいデータベースを作成する最初の移行がさせます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="74a0e-135">その名前を持つ 1 つが存在しないために、新しいデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="74a0e-136">接続文字列を変更するには必要ありません移行の概要です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="74a0e-137">DB 名を変更する代わりには、データベースを削除しています。</span><span class="sxs-lookup"><span data-stu-id="74a0e-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="74a0e-138">使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="74a0e-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="74a0e-139">次のセクションでは、CLI コマンドを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="74a0e-140">最初の移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-140">Create an initial migration</span></span>

<span data-ttu-id="74a0e-141">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-141">Build the project.</span></span>

<span data-ttu-id="74a0e-142">コマンド ウィンドウを開き、プロジェクトのフォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="74a0e-143">プロジェクト フォルダーに含まれる、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="74a0e-144">[コマンド] ウィンドウで、次を入力します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="74a0e-145">コマンド ウィンドウには、次のような情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="74a0e-146">メッセージで、移行が失敗した場合は"*... ファイルにアクセスできませんContosoUniversity.dll 別のプロセスによって使用されているためです。*"</span><span class="sxs-lookup"><span data-stu-id="74a0e-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="74a0e-147">表示されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-147">is displayed:</span></span>

* <span data-ttu-id="74a0e-148">IIS Express を停止します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="74a0e-149">終了し、Visual Studio を再起動または</span><span class="sxs-lookup"><span data-stu-id="74a0e-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="74a0e-150">IIS Express アイコンは、Windows のシステム トレイにあります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="74a0e-151">IIS Express のアイコンを右クリックし、をクリックして**ContosoUniversity > サイトの停止**です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="74a0e-152">エラー メッセージ"ビルドが失敗した場合です。"</span><span class="sxs-lookup"><span data-stu-id="74a0e-152">If the error message "Build failed."</span></span> <span data-ttu-id="74a0e-153">表示される場合、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-153">is displayed, run the command again.</span></span> <span data-ttu-id="74a0e-154">このエラーが発生する場合は、このチュートリアルの下部にあるメモにままにします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="74a0e-155">上矢印を確認し、下矢印メソッド</span><span class="sxs-lookup"><span data-stu-id="74a0e-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="74a0e-156">EF コア コマンド`migrations add`からデータベースを作成するコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="74a0e-157">この移行コードは、*移行\<タイムスタンプ > _InitialCreate.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="74a0e-158">`Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="74a0e-159">`Down`メソッドでは、それらが削除される次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="74a0e-160">移行の呼び出し、`Up`移行のためのデータ モデルの変更を実装するメソッド。</span><span class="sxs-lookup"><span data-stu-id="74a0e-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="74a0e-161">更新プログラム、移行の呼び出しをロールバックするためのコマンドを入力すると、`Down`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="74a0e-162">上記のコードでは、初期の移行のためです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="74a0e-163">そのコードが作成されたときに、`migrations add InitialCreate`コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="74a0e-164">移行の name パラメーター (この例では"InitialCreate") は、ファイル名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="74a0e-165">名前の移行は、任意の有効なファイル名を指定できます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="74a0e-166">単語または語句、移行で行われている新機能をまとめたものを選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="74a0e-167">たとえば、department テーブルを追加する移行という"AddDepartmentTable"</span><span class="sxs-lookup"><span data-stu-id="74a0e-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="74a0e-168">最初の移行が作成され、DB が終了した場合。</span><span class="sxs-lookup"><span data-stu-id="74a0e-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="74a0e-169">DB 作成コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="74a0e-170">DB 作成コードは、DB に既にデータ モデルと一致するために実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="74a0e-171">場合は、DB 作成コードを実行すると、DB に既にデータ モデルと一致するので、変更を加えますしません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="74a0e-172">新しい環境にアプリが展開されると、データベースを作成する DB 作成コードを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="74a0e-173">以前、DB の新しい名前を使用する接続文字列が変更されました。</span><span class="sxs-lookup"><span data-stu-id="74a0e-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="74a0e-174">指定されたデータベースが存在しないため、移行は、データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="74a0e-175">データ モデルのスナップショットを確認します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-175">Examine the data model snapshot</span></span>

<span data-ttu-id="74a0e-176">移行を作成、*スナップショット*で現在のデータベースのスキーマの*Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="74a0e-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="74a0e-177">現在のデータベースのスキーマは、コードで表される、ための移行を作成するデータベースと対話する EF コアがありません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="74a0e-178">移行を追加すると、EF コアは、スナップショット ファイルにデータ モデルを比較することによって変更内容を判断します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="74a0e-179">EF コアは、データベースを更新する必要がある場合にのみ、DB とやり取りします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="74a0e-180">スナップショット ファイルは、その作成元の移行での同期である必要があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="74a0e-181">という名前のファイルを削除することによって、移行を削除することはできません*\<タイムスタンプ > _\<migrationname > .cs*です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="74a0e-182">そのファイルが削除された場合、残りのマイグレーションは、データベース スナップショット ファイルと同期です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="74a0e-183">削除する追加の前回の移行を使用して、 [dotnet ef 移行削除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)コマンド。</span><span class="sxs-lookup"><span data-stu-id="74a0e-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="74a0e-184">EnsureCreated を削除します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-184">Remove EnsureCreated</span></span>

<span data-ttu-id="74a0e-185">初期の開発のため、`EnsureCreated`コマンドが使用されました。</span><span class="sxs-lookup"><span data-stu-id="74a0e-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="74a0e-186">このチュートリアルでは、migrations を使用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="74a0e-187">`EnsureCreated`次の limatitions があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="74a0e-188">移行をバイパスし、データベースとスキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="74a0e-189">移行テーブルは作成されません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="74a0e-190">できます*いない*の移行で使用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="74a0e-191">テストや迅速なプロトタイプを作成、データベースは削除して再作成頻繁にします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="74a0e-192">次の行を削除して`DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="74a0e-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="74a0e-193">開発中、DB に、移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="74a0e-194">コマンド ウィンドウで、データベースとテーブルを作成するには、次を入力します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="74a0e-195">メモ: 場合、`update`コマンドは、「失敗したビルドします。」というエラーを返します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="74a0e-196">コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-196">Run the command again.</span></span>
* <span data-ttu-id="74a0e-197">再度失敗した場合は、Visual Studio を終了し、実行、`update`コマンド。</span><span class="sxs-lookup"><span data-stu-id="74a0e-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="74a0e-198">ページの下部にあるメッセージのままにします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="74a0e-199">コマンドの出力がに似ていますが、`migrations add`コマンドを出力します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="74a0e-200">上記のコマンドは、DB をセットアップする SQL コマンドのログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="74a0e-201">ログのほとんどは、次のサンプル出力で除外されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-201">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="74a0e-202">ログ メッセージの詳細のレベルを下げることができます、ログでレベルを変更、 *appsettings です。Development.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="74a0e-203">詳細については、次を参照してください。[ログ記録の概要](xref:fundamentals/logging/index)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="74a0e-204">使用して**SQL Server オブジェクト エクスプ ローラー** DB の検査をします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="74a0e-205">追加に注意してください、`__EFMigrationsHistory`テーブル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="74a0e-206">`__EFMigrationsHistory`テーブルの追跡データベースに適用された移行の種類。</span><span class="sxs-lookup"><span data-stu-id="74a0e-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="74a0e-207">データを表示、 `__EFMigrationsHistory` 1 行の最初の移行データを示します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="74a0e-208">CLI の例の出力の最後のログは、この行が作成される INSERT ステートメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="74a0e-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="74a0e-209">アプリを実行して、すべてが適切に機能していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="74a0e-210">実稼働環境での移行の適用</span><span class="sxs-lookup"><span data-stu-id="74a0e-210">Appling migrations in production</span></span>

<span data-ttu-id="74a0e-211">運用アプリにする必要がありますをお勧め**いない**呼び出す[Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="74a0e-212">`Migrate`サーバー ファームでのアプリから呼び出すできません必要があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="74a0e-213">たとえば、次のように、アプリが (アプリの複数のインスタンスを実行する) スケール アウトによる展開クラウドをされている場合です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="74a0e-214">データベースの移行は、制御された方法で、展開の一部として行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="74a0e-215">実稼働データベースの移行方法は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="74a0e-216">移行を使用して SQL スクリプトを作成して、環境で SQL スクリプトを使用します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="74a0e-217">実行している`dotnet ef database update`制御された環境からです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="74a0e-218">EF コアを使用して、`__MigrationsHistory`テーブルを参照してを実行するかどうか、移行が必要があります。</span><span class="sxs-lookup"><span data-stu-id="74a0e-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="74a0e-219">データベースが最新では、移行は実行されません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="74a0e-220">コマンド ライン インターフェイス (CLI) とします。Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="74a0e-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="74a0e-221">移行を管理するためのツールを EF コアはから利用できます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="74a0e-222">.NET core CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="74a0e-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="74a0e-223">Visual Studio での PowerShell コマンドレット**Package Manager Console** (PMC) ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="74a0e-224">PMC を使用して一部の開発者が必要に応じて、このチュートリアルは CLI を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="74a0e-225">PMC の EF コア コマンドが、 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="74a0e-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="74a0e-226">このパッケージに含まれる、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="74a0e-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="74a0e-227">**重要:**を編集して、CLI をインストールするものと同じパッケージではない、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="74a0e-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="74a0e-228">この種類の名前で終わる`Tools`、終わる CLI パッケージ名とは異なり`Tools.DotNet`です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="74a0e-229">CLI コマンドの詳細については、次を参照してください。 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="74a0e-230">PMC コマンドの詳細については、次を参照してください。 [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="74a0e-231">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="74a0e-231">Troubleshooting</span></span>

<span data-ttu-id="74a0e-232">ダウンロード、[この段階でのアプリを完成](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)です。</span><span class="sxs-lookup"><span data-stu-id="74a0e-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="74a0e-233">アプリには、次の例外が生成されます。</span><span class="sxs-lookup"><span data-stu-id="74a0e-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="74a0e-234">ソリューション: を実行`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="74a0e-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="74a0e-235">場合、`update`コマンドは、「失敗したビルドします。」というエラーを返します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="74a0e-236">コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="74a0e-236">Run the command again.</span></span>
* <span data-ttu-id="74a0e-237">ページの下部にあるメッセージのままにします。</span><span class="sxs-lookup"><span data-stu-id="74a0e-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="74a0e-238">[前へ](xref:data/ef-rp/sort-filter-page)
[次へ](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="74a0e-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
