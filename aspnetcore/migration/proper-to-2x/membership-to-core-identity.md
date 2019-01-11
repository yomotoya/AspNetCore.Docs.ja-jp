---
title: 認証の ASP.NET メンバーシップから ASP.NET Core 2.0 Identity に移行します。
author: isaac2004
description: ASP.NET Core 2.0 identity メンバーシップ認証を使用して既存の ASP.NET アプリを移行する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 0b7001a311eeaaa78e3d52e2ec66d33ad057c381
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207409"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="76684-103">認証の ASP.NET メンバーシップから ASP.NET Core 2.0 Identity に移行します。</span><span class="sxs-lookup"><span data-stu-id="76684-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="76684-104">著者: [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="76684-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="76684-105">この記事では、ASP.NET Core 2.0 identity メンバーシップ認証を使用して ASP.NET アプリ用のデータベース スキーマの移行を示します。</span><span class="sxs-lookup"><span data-stu-id="76684-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="76684-106">このドキュメントでは、ASP.NET Core Id に使用されるデータベース スキーマに ASP.NET メンバーシップ ベースのアプリのデータベース スキーマを移行するために必要な手順が説明します。</span><span class="sxs-lookup"><span data-stu-id="76684-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="76684-107">ASP.NET メンバーシップ ベースの認証から ASP.NET Identity に移行する詳細については、次を参照してください。 [SQL メンバーシップから ASP.NET Identity に既存のアプリを移行する](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)します。</span><span class="sxs-lookup"><span data-stu-id="76684-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="76684-108">詳細については、ASP.NET Core Identity は、次を参照してください。[の ASP.NET core Identity の概要](xref:security/authentication/identity)します。</span><span class="sxs-lookup"><span data-stu-id="76684-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="76684-109">メンバーシップ スキーマの確認</span><span class="sxs-lookup"><span data-stu-id="76684-109">Review of Membership schema</span></span>

<span data-ttu-id="76684-110">ASP.NET 2.0 では、前に開発者はアプリ全体の認証および承認プロセスの作成に取り組みました。</span><span class="sxs-lookup"><span data-stu-id="76684-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="76684-111">ASP.NET 2.0 でメンバーシップ導入された、ASP.NET アプリ内のセキュリティを処理する定型コード ソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="76684-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="76684-112">開発者は、SQL Server データベースにスキーマをブートス トラップすること、 [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)コマンド。</span><span class="sxs-lookup"><span data-stu-id="76684-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="76684-113">このコマンドを実行した後、次の表は、データベースに作成されました。</span><span class="sxs-lookup"><span data-stu-id="76684-113">After running this command, the following tables were created in the database.</span></span>

  ![メンバーシップ テーブル](identity/_static/membership-tables.png)

<span data-ttu-id="76684-115">既存のアプリを ASP.NET Core 2.0 Identity に移行するには、これらのテーブル内のデータは、新しい Identity スキーマで使用されるテーブルに移行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="76684-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="76684-116">ASP.NET Core Identity 2.0 スキーマ</span><span class="sxs-lookup"><span data-stu-id="76684-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="76684-117">ASP.NET core 2.0、 [Identity](/aspnet/identity/index)原則 ASP.NET 4.5 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="76684-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="76684-118">フレームワークの実装は ASP.NET Core のバージョン間であっても、別の原則を共有すると、(を参照してください[認証と Id を ASP.NET Core 2.0 に移行](xref:migration/1x-to-2x/index))。</span><span class="sxs-lookup"><span data-stu-id="76684-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="76684-119">ASP.NET Core 2.0 の Identity のスキーマを表示する最も簡単な方法では、新しい ASP.NET Core 2.0 アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="76684-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="76684-120">Visual Studio 2017 でのこれらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="76684-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="76684-121">**[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。</span><span class="sxs-lookup"><span data-stu-id="76684-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="76684-122">新規作成**ASP.NET Core Web アプリケーション**という名前のプロジェクト*CoreIdentitySample*します。</span><span class="sxs-lookup"><span data-stu-id="76684-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="76684-123">選択**ASP.NET Core 2.0**選択し、ドロップダウンで**Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="76684-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="76684-124">このテンプレートによって生成されます、 [Razor ページ](xref:razor-pages/index)アプリ。</span><span class="sxs-lookup"><span data-stu-id="76684-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="76684-125">クリックする前に**OK**、 をクリックして**認証の変更**します。</span><span class="sxs-lookup"><span data-stu-id="76684-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="76684-126">選択**個々 のユーザー アカウント**Identity テンプレート。</span><span class="sxs-lookup"><span data-stu-id="76684-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="76684-127">最後に、をクリックして**OK**、し**OK**します。</span><span class="sxs-lookup"><span data-stu-id="76684-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="76684-128">Visual Studio では、ASP.NET Core Identity テンプレートを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="76684-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="76684-129">選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**を開く、**パッケージ マネージャー コンソール**(PMC) ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="76684-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="76684-130">PMC で、プロジェクトのルートに移動し、実行、 [Entity Framework (EF) Core](/ef/core) `Update-Database`コマンド。</span><span class="sxs-lookup"><span data-stu-id="76684-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="76684-131">ASP.NET Core 2.0 Identity では、EF Core を使用して、認証データを格納するデータベースと対話します。</span><span class="sxs-lookup"><span data-stu-id="76684-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="76684-132">新しく作成されたアプリを操作の順番は、このデータを格納するデータベースをする必要があります。</span><span class="sxs-lookup"><span data-stu-id="76684-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="76684-133">新しいアプリを作成した後、データベース環境でスキーマを検査する最も簡単な方法を使用してデータベースを作成する[EF コア移行](/ef/core/managing-schemas/migrations/)します。</span><span class="sxs-lookup"><span data-stu-id="76684-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="76684-134">このプロセスを作成、データベース、その他の場所またはローカルそのスキーマと同じ動作をします。</span><span class="sxs-lookup"><span data-stu-id="76684-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="76684-135">詳細については、前のドキュメントを確認します。</span><span class="sxs-lookup"><span data-stu-id="76684-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="76684-136">EF Core のコマンドで指定したデータベースの接続文字列を使用して*appsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="76684-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="76684-137">次の接続文字列でデータベースを対象と*localhost*という*asp net core identity*します。</span><span class="sxs-lookup"><span data-stu-id="76684-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="76684-138">この設定で、EF Core が使用するよう構成、`DefaultConnection`接続文字列。</span><span class="sxs-lookup"><span data-stu-id="76684-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```
1. <span data-ttu-id="76684-139">選択**ビュー** > **SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="76684-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="76684-140">指定されたデータベース名に対応するノードを展開し、`ConnectionStrings:DefaultConnection`プロパティの*appsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="76684-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="76684-141">`Update-Database`コマンドは、スキーマで指定されたデータベースとアプリの初期化に必要なすべてのデータを作成します。</span><span class="sxs-lookup"><span data-stu-id="76684-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="76684-142">次の図は、上記の手順で作成されるテーブル構造を示しています。</span><span class="sxs-lookup"><span data-stu-id="76684-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Identity テーブル](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="76684-144">スキーマを移行します。</span><span class="sxs-lookup"><span data-stu-id="76684-144">Migrate the schema</span></span>

<span data-ttu-id="76684-145">テーブルの構造とメンバーシップと ASP.NET Core Identity の両方のフィールドにわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="76684-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="76684-146">パターンは、ASP.NET と ASP.NET Core アプリでの認証/承認の大幅に変更されました。</span><span class="sxs-lookup"><span data-stu-id="76684-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="76684-147">Id で引き続き使用されるキー オブジェクトが*ユーザー*と*ロール*します。</span><span class="sxs-lookup"><span data-stu-id="76684-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="76684-148">マッピング テーブルを次のとおり*ユーザー*、*ロール*、および*UserRoles*します。</span><span class="sxs-lookup"><span data-stu-id="76684-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="76684-149">ユーザー</span><span class="sxs-lookup"><span data-stu-id="76684-149">Users</span></span>

|<span data-ttu-id="76684-150">*Identity<br>(dbo します。AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="76684-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="76684-151">*メンバーシップ<br>(dbo.aspnet_Users/dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="76684-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="76684-152">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-152">**Field Name**</span></span>                 |<span data-ttu-id="76684-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-153">**Type**</span></span>|<span data-ttu-id="76684-154">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-154">**Field Name**</span></span>                                    |<span data-ttu-id="76684-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="76684-156">string</span><span class="sxs-lookup"><span data-stu-id="76684-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="76684-157">string</span><span class="sxs-lookup"><span data-stu-id="76684-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="76684-158">string</span><span class="sxs-lookup"><span data-stu-id="76684-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="76684-159">string</span><span class="sxs-lookup"><span data-stu-id="76684-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="76684-160">string</span><span class="sxs-lookup"><span data-stu-id="76684-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="76684-161">string</span><span class="sxs-lookup"><span data-stu-id="76684-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="76684-162">string</span><span class="sxs-lookup"><span data-stu-id="76684-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="76684-163">string</span><span class="sxs-lookup"><span data-stu-id="76684-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="76684-164">string</span><span class="sxs-lookup"><span data-stu-id="76684-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="76684-165">string</span><span class="sxs-lookup"><span data-stu-id="76684-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="76684-166">string</span><span class="sxs-lookup"><span data-stu-id="76684-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="76684-167">string</span><span class="sxs-lookup"><span data-stu-id="76684-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="76684-168">bit</span><span class="sxs-lookup"><span data-stu-id="76684-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="76684-169">bit</span><span class="sxs-lookup"><span data-stu-id="76684-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="76684-170">すべてのフィールド マッピングのメンバーシップから ASP.NET Core Id への一対一のリレーションシップのようになります。</span><span class="sxs-lookup"><span data-stu-id="76684-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="76684-171">上記の表は、メンバーシップ ユーザーの既定のスキーマを受け取り、ASP.NET Core Identity スキーマにマップします。</span><span class="sxs-lookup"><span data-stu-id="76684-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="76684-172">メンバーシップの使用された他のカスタム フィールドは、手動でマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="76684-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="76684-173">このマッピングはありませんマップのパスワードのように、2 つの間でパスワードの基準とパスワードの salt の両方の移行はありません。</span><span class="sxs-lookup"><span data-stu-id="76684-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="76684-174">**パスワードが null のままにして、パスワードをリセットするユーザーに確認することがお勧めします。**</span><span class="sxs-lookup"><span data-stu-id="76684-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="76684-175">ASP.NET Core Identity で`LockoutEnd`ユーザーがロックアウトされた場合、将来の日付に設定する必要があります。これは、移行スクリプトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="76684-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="76684-176">役割</span><span class="sxs-lookup"><span data-stu-id="76684-176">Roles</span></span>

|<span data-ttu-id="76684-177">*Identity<br>(dbo します。AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="76684-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="76684-178">*メンバーシップ<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="76684-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="76684-179">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-179">**Field Name**</span></span>                 |<span data-ttu-id="76684-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-180">**Type**</span></span>|<span data-ttu-id="76684-181">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-181">**Field Name**</span></span>   |<span data-ttu-id="76684-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="76684-183">string</span><span class="sxs-lookup"><span data-stu-id="76684-183">string</span></span>  |`RoleId`         | <span data-ttu-id="76684-184">string</span><span class="sxs-lookup"><span data-stu-id="76684-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="76684-185">string</span><span class="sxs-lookup"><span data-stu-id="76684-185">string</span></span>  |`RoleName`       | <span data-ttu-id="76684-186">string</span><span class="sxs-lookup"><span data-stu-id="76684-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="76684-187">string</span><span class="sxs-lookup"><span data-stu-id="76684-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="76684-188">string</span><span class="sxs-lookup"><span data-stu-id="76684-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="76684-189">ユーザー ロール</span><span class="sxs-lookup"><span data-stu-id="76684-189">User Roles</span></span>

|<span data-ttu-id="76684-190">*Identity<br>(dbo します。AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="76684-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="76684-191">*メンバーシップ<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="76684-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="76684-192">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-192">**Field Name**</span></span>           |<span data-ttu-id="76684-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-193">**Type**</span></span>  |<span data-ttu-id="76684-194">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="76684-194">**Field Name**</span></span>|<span data-ttu-id="76684-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="76684-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="76684-196">string</span><span class="sxs-lookup"><span data-stu-id="76684-196">string</span></span>    |`RoleId`      |<span data-ttu-id="76684-197">string</span><span class="sxs-lookup"><span data-stu-id="76684-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="76684-198">string</span><span class="sxs-lookup"><span data-stu-id="76684-198">string</span></span>    |`UserId`      |<span data-ttu-id="76684-199">string</span><span class="sxs-lookup"><span data-stu-id="76684-199">string</span></span>                     |

<span data-ttu-id="76684-200">移行スクリプトを作成する場合は、上記のマッピング テーブルを参照*ユーザー*と*ロール*します。</span><span class="sxs-lookup"><span data-stu-id="76684-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="76684-201">次の例では、データベース サーバーに 2 つのデータベースがある前提としています。</span><span class="sxs-lookup"><span data-stu-id="76684-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="76684-202">1 つのデータベースには、既存の ASP.NET メンバーシップ スキーマとデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="76684-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="76684-203">他の*CoreIdentitySample*データベースは、前述の手順を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="76684-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="76684-204">コメントは、詳細についてはインラインで含まれています。</span><span class="sxs-lookup"><span data-stu-id="76684-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and 
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="76684-205">上記のスクリプトの完了後、先ほど作成した ASP.NET Core Identity アプリにはメンバーシップ ユーザーが設定されます。</span><span class="sxs-lookup"><span data-stu-id="76684-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="76684-206">ユーザーは、ログイン前にパスワードを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="76684-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="76684-207">メンバーシップ システムは、電子メール アドレスに一致していないユーザー名を持つユーザーがある、変更はこれを対応するために以前に作成したアプリに必要があります。</span><span class="sxs-lookup"><span data-stu-id="76684-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="76684-208">既定のテンプレートが必要ですが`UserName`と`Email`同じにします。</span><span class="sxs-lookup"><span data-stu-id="76684-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="76684-209">いる別の場合、ログイン プロセスを使用するように変更する必要がある`UserName`の代わりに`Email`します。</span><span class="sxs-lookup"><span data-stu-id="76684-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="76684-210">`PageModel`でのログイン ページが*Pages\Account\Login.cshtml.cs*、削除、`[EmailAddress]`属性を*電子メール*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="76684-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="76684-211">名前を変更する*UserName*します。</span><span class="sxs-lookup"><span data-stu-id="76684-211">Rename it to *UserName*.</span></span> <span data-ttu-id="76684-212">変更が必要です限り`EmailAddress`で、記載されて、*ビュー*と*PageModel*します。</span><span class="sxs-lookup"><span data-stu-id="76684-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="76684-213">結果は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="76684-213">The result looks like the following:</span></span>

 ![固定のログイン](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="76684-215">次の手順</span><span class="sxs-lookup"><span data-stu-id="76684-215">Next steps</span></span>

<span data-ttu-id="76684-216">このチュートリアルでは、ユーザーが SQL メンバーシップから ASP.NET Core 2.0 の Id を移植する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="76684-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="76684-217">ASP.NET Core Identity に関する詳細については、次を参照してください。 [Id の概要](xref:security/authentication/identity)します。</span><span class="sxs-lookup"><span data-stu-id="76684-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
