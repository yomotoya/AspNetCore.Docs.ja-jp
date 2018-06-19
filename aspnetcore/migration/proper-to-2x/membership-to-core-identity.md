---
title: ASP.NET メンバーシップ認証から ASP.NET Core 2.0 Id への移行します。
author: isaac2004
description: ASP.NET Core 2.0 Id にメンバーシップの認証を使用して既存の ASP.NET アプリを移行する方法を説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851544"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="4b08c-103">ASP.NET メンバーシップ認証から ASP.NET Core 2.0 Id への移行します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="4b08c-104">著者: [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="4b08c-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="4b08c-105">この記事では、ASP.NET Core 2.0 Id にメンバーシップの認証を使用して ASP.NET アプリ用のデータベース スキーマの移行について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="4b08c-106">このドキュメントでは、ASP.NET メンバーシップ ベースのアプリのデータベース スキーマを ASP.NET Core Id に使用されるデータベース スキーマに移行するために必要な手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="4b08c-107">ASP.NET メンバーシップに基づく認証から ASP.NET の Id への移行の詳細については、次を参照してください。 [SQL メンバーシップから ASP.NET の Id に、既存のアプリを移行](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="4b08c-108">ASP.NET Core Id に関する詳細については、次を参照してください。 [ASP.NET Core での Id の概要](xref:security/authentication/identity)です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="4b08c-109">メンバーシップのスキーマの確認</span><span class="sxs-lookup"><span data-stu-id="4b08c-109">Review of Membership schema</span></span>

<span data-ttu-id="4b08c-110">ASP.NET 2.0 では、前に開発者が任務を負って、認証と承認のプロセスの全がアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="4b08c-111">ASP.NET 2.0 では、メンバーシップ導入された、ASP.NET アプリケーション内のセキュリティの処理に定型ソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="4b08c-112">開発者によって使用する SQL Server データベースにスキーマをブートス トラップ今すぐできた、 [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)コマンド。</span><span class="sxs-lookup"><span data-stu-id="4b08c-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="4b08c-113">このコマンドを実行すると、次の表は、データベースで作成されました。</span><span class="sxs-lookup"><span data-stu-id="4b08c-113">After running this command, the following tables were created in the database.</span></span>

  ![メンバーシップ テーブル](identity/_static/membership-tables.png)

<span data-ttu-id="4b08c-115">既存のアプリを移行すると、ASP.NET Core 2.0 Id に、これらのテーブル内のデータを新しい Id スキーマで使用されるテーブルに移行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="4b08c-116">ASP.NET Core Identity 2.0 スキーマ</span><span class="sxs-lookup"><span data-stu-id="4b08c-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="4b08c-117">ASP.NET Core 2.0 に依存して、 [Identity](/aspnet/identity/index) ASP.NET 4.5 で導入されたプリンシパル。</span><span class="sxs-lookup"><span data-stu-id="4b08c-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="4b08c-118">原則を共有すると、フレームワークの実装では ASP.NET Core のバージョン間でさえも、異なる (を参照してください[ASP.NET Core 2.0 への認証と Id の移行](xref:migration/1x-to-2x/index))。</span><span class="sxs-lookup"><span data-stu-id="4b08c-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="4b08c-119">ASP.NET Core 2.0 Identity のスキーマを表示する最も簡単な方法では、新しい ASP.NET Core 2.0 アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="4b08c-120">Visual Studio 2017 でこれらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="4b08c-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="4b08c-121">**[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="4b08c-122">新しい**ASP.NET Core Web アプリケーション**、プロジェクトの名前と*CoreIdentitySample*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="4b08c-123">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="4b08c-124">このテンプレートを生成、 [Razor ページ](xref:mvc/razor-pages/index)アプリ。</span><span class="sxs-lookup"><span data-stu-id="4b08c-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="4b08c-125">クリックする前に**OK**をクリックして**認証の変更**です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="4b08c-126">選択**個々 のユーザー アカウント**Identity テンプレート。</span><span class="sxs-lookup"><span data-stu-id="4b08c-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="4b08c-127">最後に、をクリックして**OK**、し**OK**です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="4b08c-128">Visual Studio では、ASP.NET Core Id テンプレートを使用して、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="4b08c-129">ASP.NET Core 2.0 の Id を使用して[Entity Framework Core](/ef/core)認証データを格納するデータベースと対話します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="4b08c-130">新しく作成されたアプリ動作するためは、このデータを格納するデータベースを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="4b08c-131">新しいアプリを作成した後は、データベース環境のスキーマを検査する最も簡単な方法は、Entity Framework migrations を使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="4b08c-132">このプロセスを作成、データベース、他の場所で、またはいずれかのローカルでそのスキーマと同じ動作です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="4b08c-133">詳細については、前のドキュメントを確認します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="4b08c-134">Asp.net Core スキーマを持つデータベースを作成するには、実行、 `Update-Database` Visual studio のコマンド**Package Manager Console** (PMC) ウィンドウ&mdash;に配置されている**ツール** > **NuGet Package Manager** > **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="4b08c-135">PMC には、Entity Framework コマンドの実行がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4b08c-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="4b08c-136">Entity Framework のコマンドで指定したデータベースの接続文字列を使用して*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="4b08c-137">次の接続文字列では、データベースを対象に*localhost*という*asp net コア id*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="4b08c-138">この設定は、Entity Framework が使用するよう構成、`DefaultConnection`接続文字列。</span><span class="sxs-lookup"><span data-stu-id="4b08c-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="4b08c-139">このコマンドは、スキーマで指定されたデータベースとアプリの初期化に必要なすべてのデータを作成します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="4b08c-140">次の図は、上記の手順で作成されるテーブル構造を示しています。</span><span class="sxs-lookup"><span data-stu-id="4b08c-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Identity テーブル](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="4b08c-142">スキーマを移行します</span><span class="sxs-lookup"><span data-stu-id="4b08c-142">Migrate the schema</span></span>

<span data-ttu-id="4b08c-143">テーブルの構造とメンバーシップと ASP.NET Core Id の両方のフィールドにわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="4b08c-144">パターンは、ASP.NET および ASP.NET Core アプリによる認証/承認の大幅に変更されました。</span><span class="sxs-lookup"><span data-stu-id="4b08c-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="4b08c-145">Id で引き続き使用されるキー オブジェクトが*ユーザー*と*ロール*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="4b08c-146">ここでは、マッピング テーブルを*ユーザー*、*ロール*、および*UserRoles*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="4b08c-147">ユーザー</span><span class="sxs-lookup"><span data-stu-id="4b08c-147">Users</span></span>

| <span data-ttu-id="4b08c-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="4b08c-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="4b08c-150">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-150">**Field Name**</span></span> | <span data-ttu-id="4b08c-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-151">**Type**</span></span>  |   <span data-ttu-id="4b08c-152">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-152">**Field Name**</span></span> | <span data-ttu-id="4b08c-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="4b08c-154">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="4b08c-155">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-155">string</span></span>
|`UserName` | <span data-ttu-id="4b08c-156">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="4b08c-157">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-157">string</span></span>
|`Email` | <span data-ttu-id="4b08c-158">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="4b08c-159">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="4b08c-160">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="4b08c-161">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="4b08c-162">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="4b08c-163">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="4b08c-164">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="4b08c-165">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="4b08c-166">bit</span><span class="sxs-lookup"><span data-stu-id="4b08c-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="4b08c-167">bit</span><span class="sxs-lookup"><span data-stu-id="4b08c-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="4b08c-168">すべてのフィールド マッピングのメンバーシップから ASP.NET Core Id に一対一のリレーションシップのようになります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="4b08c-169">上記の表は既定のメンバーシップ ユーザーのスキーマを使用し、ASP.NET Core Id スキーマにマップします。</span><span class="sxs-lookup"><span data-stu-id="4b08c-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="4b08c-170">メンバーシップの使用されたその他のカスタム フィールドは、手動でマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="4b08c-171">このマッピングのマップがない、パスワードのようにパスワードの基準とパスワードの salt の両方を 2 つの間で移行できません。</span><span class="sxs-lookup"><span data-stu-id="4b08c-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="4b08c-172">**パスワードが null のままにして、パスワードをリセットするかどうかを勧めがします。**</span><span class="sxs-lookup"><span data-stu-id="4b08c-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="4b08c-173">ASP.NET Core Id で`LockoutEnd`ユーザーがロックアウトされた場合、将来のいくつかの日付に設定する必要があります。これは、移行スクリプトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="4b08c-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="4b08c-174">役割</span><span class="sxs-lookup"><span data-stu-id="4b08c-174">Roles</span></span>

| <span data-ttu-id="4b08c-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="4b08c-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="4b08c-177">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-177">**Field Name**</span></span> | <span data-ttu-id="4b08c-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-178">**Type**</span></span>  |   <span data-ttu-id="4b08c-179">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-179">**Field Name**</span></span> | <span data-ttu-id="4b08c-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="4b08c-181">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-181">string</span></span> | `RoleId` | <span data-ttu-id="4b08c-182">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-182">string</span></span>
|`Name` | <span data-ttu-id="4b08c-183">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-183">string</span></span> | `RoleName` | <span data-ttu-id="4b08c-184">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="4b08c-185">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="4b08c-186">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="4b08c-187">ユーザー ロール</span><span class="sxs-lookup"><span data-stu-id="4b08c-187">User Roles</span></span>

| <span data-ttu-id="4b08c-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="4b08c-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="4b08c-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="4b08c-190">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-190">**Field Name**</span></span> | <span data-ttu-id="4b08c-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-191">**Type**</span></span>  |   <span data-ttu-id="4b08c-192">**フィールド名**</span><span class="sxs-lookup"><span data-stu-id="4b08c-192">**Field Name**</span></span> | <span data-ttu-id="4b08c-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="4b08c-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="4b08c-194">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-194">string</span></span> | `RoleId` | <span data-ttu-id="4b08c-195">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-195">string</span></span>
|`UserId` | <span data-ttu-id="4b08c-196">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-196">string</span></span> | `UserId` | <span data-ttu-id="4b08c-197">string</span><span class="sxs-lookup"><span data-stu-id="4b08c-197">string</span></span>

<span data-ttu-id="4b08c-198">移行スクリプトを作成するときに、上記のマッピング テーブルを参照*ユーザー*と*ロール*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="4b08c-199">次の例では、データベース サーバーに 2 つのデータベースがあると仮定します。</span><span class="sxs-lookup"><span data-stu-id="4b08c-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="4b08c-200">1 つのデータベースには、既存の ASP.NET メンバーシップ スキーマとデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4b08c-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="4b08c-201">その他のデータベースは、既に説明した手順を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="4b08c-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="4b08c-202">コメントは、詳細についてはインラインで含まれています。</span><span class="sxs-lookup"><span data-stu-id="4b08c-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="4b08c-203">このスクリプトの完了したら、先ほど作成した Asp.net Core アプリケーションにはメンバーシップ ユーザーが格納されます。</span><span class="sxs-lookup"><span data-stu-id="4b08c-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="4b08c-204">ユーザーでのログ記録する前にパスワードを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="4b08c-205">メンバーシップ システムは、ユーザーの電子メール アドレスに一致していないユーザー名を持つユーザーがある、変更はこれに合わせて以前に作成したアプリに必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="4b08c-206">既定のテンプレートが必要ですが`UserName`と`Email`同じにします。</span><span class="sxs-lookup"><span data-stu-id="4b08c-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="4b08c-207">いる別の場合、ログイン プロセスを使用するように変更する必要がある`UserName`の代わりに`Email`です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="4b08c-208">`PageModel`にある、ログイン ページの*Pages\Account\Login.cshtml.cs*、削除、`[EmailAddress]`属性から、*電子メール*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="4b08c-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="4b08c-209">名前を変更して*UserName*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-209">Rename it to *UserName*.</span></span> <span data-ttu-id="4b08c-210">変更が必要です限り`EmailAddress`が記述されて、*ビュー*と*PageModel*です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="4b08c-211">結果は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4b08c-211">The result looks like the following:</span></span>

 ![固定のログイン](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="4b08c-213">次の手順</span><span class="sxs-lookup"><span data-stu-id="4b08c-213">Next steps</span></span>

<span data-ttu-id="4b08c-214">このチュートリアルでは、ユーザーが SQL メンバーシップから ASP.NET Core 2.0 の Id を移植する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="4b08c-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="4b08c-215">ASP.NET Core Id に関する詳細については、次を参照してください。 [Id の概要](xref:security/authentication/identity)です。</span><span class="sxs-lookup"><span data-stu-id="4b08c-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
