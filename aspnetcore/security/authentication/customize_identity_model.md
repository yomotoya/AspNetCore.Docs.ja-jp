---
title: Id モデルのカスタマイズ
author: ajcvickers
description: この記事では、ASP.NET Core Id の基礎となる Entity Framework のコア データ モデルをカスタマイズする方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: avickers
ms.date: 04/12/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 41ea125414c5997ee36f4e312beba4ff318a4a8d
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077680"
---
# <a name="identity-model-customization"></a><span data-ttu-id="2907a-103">Id モデルのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="2907a-103">Identity model customization</span></span>

<span data-ttu-id="2907a-104">によって[Arthur ヴィッカース](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="2907a-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="2907a-105">ASP.NET Core の Id は、管理および ASP.NET Core アプリケーションでユーザー アカウントを保存するためのフレームワークを提供します。</span><span class="sxs-lookup"><span data-stu-id="2907a-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="2907a-106">Id は、「個々 のユーザー アカウント」を認証メカニズムとして選択すると、プロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2907a-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="2907a-107">既定では、ユーザーを使用する Entity Framework (EF) の主要なデータ モデル。</span><span class="sxs-lookup"><span data-stu-id="2907a-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="2907a-108">この記事では、Id モデルをカスタマイズする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2907a-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="2907a-109">Id および EF コアの移行</span><span class="sxs-lookup"><span data-stu-id="2907a-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="2907a-110">Identity のしくみを理解すると便利です、モデルを調べる前に[EF コア移行](/ef/core/managing-schemas/migrations/)を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="2907a-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="2907a-111">最上位のレベルでは、手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2907a-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="2907a-112">定義または更新、[コードでデータ モデル](/ef/core/modeling/)です。</span><span class="sxs-lookup"><span data-stu-id="2907a-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="2907a-113">このモデルに変更をデータベースに適用できる変換への移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="2907a-114">移行が正しく、その目的を表すことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2907a-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="2907a-115">モデルと同期するデータベースの更新への移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="2907a-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="2907a-116">さらに、モデルの調整、データベースの同期を保つ手順 1. ~ 4. を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="2907a-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="2907a-117">移行では、追加されを使用して適用します。</span><span class="sxs-lookup"><span data-stu-id="2907a-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="2907a-118">[Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) Visual Studio を使用している場合。</span><span class="sxs-lookup"><span data-stu-id="2907a-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="2907a-119">[Dotnet コマンド](/ef/core/miscellaneous/cli/dotnet)コマンド ラインでします。</span><span class="sxs-lookup"><span data-stu-id="2907a-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="2907a-120">アプリケーションの実行時に、エラー ページの移行のリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="2907a-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="2907a-121">ASP.NET Core では、アプリケーションを実行すると、移行を適用するために使用する開発時のエラー ページ ハンドラーがあります。</span><span class="sxs-lookup"><span data-stu-id="2907a-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="2907a-122">実稼働アプリケーションの方が、移行から SQL スクリプトを生成し、制御されたアプリケーションとデータベースの展開の一部としてデータベースにこれらを展開に適切な。</span><span class="sxs-lookup"><span data-stu-id="2907a-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="2907a-123">Id を使用して、新しいアプリケーションが作成されると、手順 1. と 2. を以上が既に完了します。</span><span class="sxs-lookup"><span data-stu-id="2907a-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="2907a-124">つまり、初期のデータ モデルが既に存在して、最初の移行がプロジェクトに追加されました。</span><span class="sxs-lookup"><span data-stu-id="2907a-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="2907a-125">最初の移行は、データベースに適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="2907a-126">または、アプリケーションの実行時にエラー ページを使用して、データベースの更新 (PMC)、dotnet ef database 更新 (.NET Core CLI) コマンドを実行して、最初の移行を実行できます。</span><span class="sxs-lookup"><span data-stu-id="2907a-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="2907a-127">上記の手順は、モデルに変更が加えられるを繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="2907a-128">Id モデル</span><span class="sxs-lookup"><span data-stu-id="2907a-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="2907a-129">エンティティ型</span><span class="sxs-lookup"><span data-stu-id="2907a-129">Entity types</span></span>

<span data-ttu-id="2907a-130">Id モデルは、7 つのエンティティ型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="2907a-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="2907a-131">`User` -ユーザーを表します</span><span class="sxs-lookup"><span data-stu-id="2907a-131">`User` - represents the user</span></span>
* <span data-ttu-id="2907a-132">`Role` ロールを表します</span><span class="sxs-lookup"><span data-stu-id="2907a-132">`Role` - represents a role</span></span>
* <span data-ttu-id="2907a-133">`UserClaim` -ユーザーがクレームを表します</span><span class="sxs-lookup"><span data-stu-id="2907a-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="2907a-134">`UserToken` -ユーザーの認証トークンを表します</span><span class="sxs-lookup"><span data-stu-id="2907a-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="2907a-135">`UserLogin` -ユーザーをログインに関連付けます</span><span class="sxs-lookup"><span data-stu-id="2907a-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="2907a-136">`RoleClaim` は、ロール内のすべてのユーザーに許可するクレームを表します</span><span class="sxs-lookup"><span data-stu-id="2907a-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="2907a-137">`UserRole` -ユーザーおよびロールに関連付けているエンティティを参加させる</span><span class="sxs-lookup"><span data-stu-id="2907a-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="2907a-138">エンティティ型の関係</span><span class="sxs-lookup"><span data-stu-id="2907a-138">Entity type relationships</span></span>

<span data-ttu-id="2907a-139">これらのエンティティ型は、次のように互いに関連します。</span><span class="sxs-lookup"><span data-stu-id="2907a-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="2907a-140">各`User`多くを持つことができます `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="2907a-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="2907a-141">各`User`多くを持つことができます `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="2907a-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="2907a-142">各`User`多くを持つことができます `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="2907a-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="2907a-143">各`Role`関連付けられた多くを持つことができます `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="2907a-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="2907a-144">各`User`関連付けられている多数格納できます`Roles`、および各`Role`多くのユーザーを関連付けることができます</span><span class="sxs-lookup"><span data-stu-id="2907a-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="2907a-145">これは、データベース内の結合テーブルを必要とする多対多リレーションシップです。</span><span class="sxs-lookup"><span data-stu-id="2907a-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="2907a-146">結合テーブルで表現され、`UserRole`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="2907a-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="2907a-147">既定のモデルの構成</span><span class="sxs-lookup"><span data-stu-id="2907a-147">Default model configuration</span></span>

<span data-ttu-id="2907a-148">Identity 定義から継承される「コンテキスト クラス」のさまざまな[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)を構成して、モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="2907a-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="2907a-149">この構成が行われますを使用して、 [EF コア コード最初 Fluent API](/ef/core/modeling/)で、 [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_)コンテキスト クラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="2907a-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="2907a-150">既定の構成は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2907a-150">The default configuration is:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a><span data-ttu-id="2907a-151">モデルのジェネリック型</span><span class="sxs-lookup"><span data-stu-id="2907a-151">Model generic types</span></span>

<span data-ttu-id="2907a-152">Id は、上記のエンティティ型の各の既定の CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="2907a-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="2907a-153">これらの型は、すべて先頭に付きます"Identity": `IdentityUser`、 `IdentityRole`、 `IdentityUserClaim`、 `IdentityUserToken`、 `IdentityUserLogin`、 `IdentityRoleClaim`、および`IdentityUserRole`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="2907a-154">これらの型を直接使用するのではなく アプリケーションの種類の型の基本クラスとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="2907a-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="2907a-155">`DbContext` Id を使用して定義されているクラスでは一般的なことで、モデルのエンティティ型の 1 つ以上の別の CLR 型を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2907a-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="2907a-156">これらのジェネリック型を変更する、ユーザーのプライマリ キーの型のこともできます。</span><span class="sxs-lookup"><span data-stu-id="2907a-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="2907a-157">ロールのサポートの Id を使用すると、 [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext)クラスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

```CSharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>

```

<span data-ttu-id="2907a-158">その場合、ロール (要求のみ) Id を使用することも、`IdentityUserContext`クラスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a><span data-ttu-id="2907a-159">モデルのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="2907a-159">Customizing the model</span></span>

<span data-ttu-id="2907a-160">モデルをカスタマイズするための出発点は適切なコンテキスト型から派生するには前のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2907a-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="2907a-161">このコンテキスト型が呼び出される慣例的`ApplicationDbContext`ASP.NET Core テンプレートによって作成されたとします。</span><span class="sxs-lookup"><span data-stu-id="2907a-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="2907a-162">コンテキストを使用して、2 つの方法で、モデルを構成します。</span><span class="sxs-lookup"><span data-stu-id="2907a-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="2907a-163">ジェネリック型パラメーターのエンティティとキーの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="2907a-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="2907a-164">オーバーライドすることで`OnModelCreating`これらの型のマッピングを変更します。</span><span class="sxs-lookup"><span data-stu-id="2907a-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="2907a-165">オーバーライドする場合`OnModelCreating`、`base.OnModelCreating`呼び出す必要があります最初に、オーバーライドして、構成を次に呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="2907a-166">EF コアは、通常の構成用のポリシーを最後の 1 つの wins を持っています。</span><span class="sxs-lookup"><span data-stu-id="2907a-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="2907a-167">たとえば場合、`ToTable`エンティティの型は、1 つのテーブルの名前の最初と呼ばれる別のテーブルの名前でもう一度後で、2 番目の呼び出し内のテーブル名は 1 つです。</span><span class="sxs-lookup"><span data-stu-id="2907a-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="2907a-168">カスタム ユーザーの種類を使用します。</span><span class="sxs-lookup"><span data-stu-id="2907a-168">Using a custom User type</span></span>

<span data-ttu-id="2907a-169">カスタム ユーザーの種類を使用する型を作成してそれを継承`IdentityUser`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="2907a-170">この型名前を指定するが通例`ApplicationUser`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="2907a-171">この型は、通常持ち追加のプロパティ、基本データ型ではなく、それ以外の場合はありますありません値作成中にします。</span><span class="sxs-lookup"><span data-stu-id="2907a-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="2907a-172">例えば:</span><span class="sxs-lookup"><span data-stu-id="2907a-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="2907a-173">次のコンテキストの汎用引数としてこの型を使用します。</span><span class="sxs-lookup"><span data-stu-id="2907a-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="2907a-174">更新`ConfigureServices`、新しい`ApplicationUser`クラス。</span><span class="sxs-lookup"><span data-stu-id="2907a-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="2907a-175">オーバーライドする必要はありません`OnModelCreating`EF コアにマップするためにここでは、`CustomTag`慣例プロパティです。</span><span class="sxs-lookup"><span data-stu-id="2907a-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="2907a-176">ただし、データベースが新しいに更新する必要がある`CustomTag`列です。</span><span class="sxs-lookup"><span data-stu-id="2907a-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="2907a-177">これを行うには、移行を追加し、」の説明に従って、データベースを更新[Id と EF コア移行](#identity-migrations)です。</span><span class="sxs-lookup"><span data-stu-id="2907a-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="2907a-178">キーの種類を変更します。</span><span class="sxs-lookup"><span data-stu-id="2907a-178">Changing the key type</span></span>

<span data-ttu-id="2907a-179">データベースが作成された後に主キー (PK) 列の型を変更するは、多くのデータベース システムで問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="2907a-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="2907a-180">通常、主キーを変更するには、削除と、テーブルを再作成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2907a-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="2907a-181">そのため、データベースの作成時に対象キーの種類が作成されるように、重要な型が最初の移行で指定できることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2907a-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="2907a-182">データベースが作成されている場合、し`Drop-Database`(PMC) または`dotnet ef database drop`削除するには (.NET Core CLI) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2907a-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="2907a-183">これには、データベースが存在しないことが確認されて、削除に最初の移行`Remove-Migration`(PMC) または`dotnet ef migrations remove`(.NET Core CLI)。</span><span class="sxs-lookup"><span data-stu-id="2907a-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="2907a-184">更新プログラム、`ApplicationDbContext`の新しいキーの種類を指定する、異なる基底クラスを使用する`TKey`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="2907a-185">たとえば、使用するため、`Guid`キー。</span><span class="sxs-lookup"><span data-stu-id="2907a-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="2907a-186">注意して、ジェネリック クラス`IdentityUser<TKey>`と`IdentityRole<TKey>`新しいキーの型を使用するも指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="2907a-187">`ConfigureServices` ジェネリック ユーザーを使用する更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="2907a-188">場合、カスタム`ApplicationUser`はから継承するように更新が使用されている`IdentityUser<TKey>`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="2907a-189">例えば:</span><span class="sxs-lookup"><span data-stu-id="2907a-189">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a><span data-ttu-id="2907a-190">ナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-190">Adding navigation properties</span></span>

<span data-ttu-id="2907a-191">リレーションシップのモデルの構成に対する変更よりも難しいその他の変更を加えることができます。</span><span class="sxs-lookup"><span data-stu-id="2907a-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="2907a-192">新しい追加のリレーションシップを作成するのではなく、既存のリレーションシップを置き換えるに注意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="2907a-193">具体的には、変更されたリレーションシップでは、既存のリレーションシップとして同じ外部キーのプロパティを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2907a-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="2907a-194">間のリレーションシップなど、`Users`と`UserClaims`として指定された既定では。</span><span class="sxs-lookup"><span data-stu-id="2907a-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="2907a-195">このリレーションシップの外部キーとして指定された、`UserClaim.UserId`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="2907a-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="2907a-196">`HasMany` および`WithOne`ナビゲーション プロパティのないリレーションシップを作成する引数を使用しないと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="2907a-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="2907a-197">ナビゲーション プロパティを追加`ApplicationUser`これにより、関連付けられている`UserClaims`ユーザーから参照されていること。</span><span class="sxs-lookup"><span data-stu-id="2907a-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="2907a-198">なお、`TKey`の`IdentityUserClaim<TKey>`ユーザー--この場合の主キーに指定された型は、`string`既定の設定を使用しているためです。</span><span class="sxs-lookup"><span data-stu-id="2907a-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="2907a-199">**いない**のプライマリ キーの種類、`UserClaim`エンティティ型。</span><span class="sxs-lookup"><span data-stu-id="2907a-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="2907a-200">ナビゲーション プロパティが存在するようになりましたことで構成する必要があります`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="2907a-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

<span data-ttu-id="2907a-201">呼び出しで指定されたナビゲーション プロパティでのみ前に、のとまったく同じ関係が構成されていることに注意してください。`HasMany`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="2907a-202">ナビゲーション プロパティは、データベースではない、EF モデルにのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="2907a-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="2907a-203">リレーションシップの外部キーが変更されていないために、このモデルの変更の種類は、データベースを更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2907a-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="2907a-204">これを変更を行った後に移行を追加することで確認する:`Up`と`Down`メソッドは空です。</span><span class="sxs-lookup"><span data-stu-id="2907a-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="2907a-205">すべてのユーザーのナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-205">Adding all User navigation properties</span></span>

<span data-ttu-id="2907a-206">上のセクションを使用して、ガイダンスとしてと、ユーザーのすべての関係の一方向のナビゲーション プロパティを構成する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2907a-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="2907a-207">ユーザーおよびロールのナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="2907a-208">上のセクションを使用して、ガイダンスとしてと、ユーザーおよびロールのすべての関係のナビゲーション プロパティを構成する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2907a-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

<span data-ttu-id="2907a-209">メモ:</span><span class="sxs-lookup"><span data-stu-id="2907a-209">Notes:</span></span>

* <span data-ttu-id="2907a-210">この例も含まれています、`UserRole`ロールにユーザーからの多対多リレーションシップを移動するために必要なエンティティを結合します。</span><span class="sxs-lookup"><span data-stu-id="2907a-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="2907a-211">内容を反映するナビゲーション プロパティの型を変更して`ApplicationXxx`型は、の代わりに使用されているようになりました`IdentityXxx`型です。</span><span class="sxs-lookup"><span data-stu-id="2907a-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="2907a-212">使用してください、`ApplicationXxx`ジェネリック`ApplicationContext`定義します。</span><span class="sxs-lookup"><span data-stu-id="2907a-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="2907a-213">すべてのナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-213">Adding all navigation properties</span></span>

<span data-ttu-id="2907a-214">上のセクションを使用して、ガイダンスとしてと、すべてのエンティティ型のすべての関係のナビゲーション プロパティを構成する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2907a-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });

    }
}
```

### <a name="using-composite-keys"></a><span data-ttu-id="2907a-215">複合キーを使用</span><span class="sxs-lookup"><span data-stu-id="2907a-215">Using composite keys</span></span>

<span data-ttu-id="2907a-216">前のセクションには、Id モデルで使用されるキーの型の変更が示されています。</span><span class="sxs-lookup"><span data-stu-id="2907a-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="2907a-217">複合キーを使用するには、Id キー モデルを変更またはされていないサポートされていることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2907a-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="2907a-218">Id を持つ複合キーを使用するには、Identity manager のコードで、モデルでは、これはサポートされていないカスタマイズし、このドキュメントの範囲を超えてがどのようにやり取りする方法の変更が行われます。</span><span class="sxs-lookup"><span data-stu-id="2907a-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="2907a-219">テーブルまたは列名とファセットの変更</span><span class="sxs-lookup"><span data-stu-id="2907a-219">Changing table/column names and facets</span></span>

<span data-ttu-id="2907a-220">テーブルと列の名前を変更するには、呼び出す`base.OnModelCreating`、既定値をオーバーライドする構成を追加します。</span><span class="sxs-lookup"><span data-stu-id="2907a-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="2907a-221">たとえば、すべてのユーザー テーブルの名前を変更するには、します。</span><span class="sxs-lookup"><span data-stu-id="2907a-221">For example, to change the name of all the identity tables:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

<span data-ttu-id="2907a-222">これらの例が既定の Id 型を使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2907a-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="2907a-223">アプリケーションの種類 がなどを使用するかどうかは`ApplicationUser`既定値の型ではなく、その種類を構成します。</span><span class="sxs-lookup"><span data-stu-id="2907a-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="2907a-224">一部の列名を変更する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2907a-224">Here is an example that changes some column names:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

<span data-ttu-id="2907a-225">特定のデータベース列のいくつかの種類を構成することができます_ファセット_許可されている文字列の最大長などです。</span><span class="sxs-lookup"><span data-stu-id="2907a-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="2907a-226">モデルのいくつかの文字列プロパティの列の最大長を設定する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2907a-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="2907a-227">別のスキーマへのマッピング</span><span class="sxs-lookup"><span data-stu-id="2907a-227">Mapping to a different schema</span></span>

<span data-ttu-id="2907a-228">別のデータベース プロバイダーでスキーマが異なる動作できますが、SQL Server,、既定値は"dbo"スキーマ内のすべてのテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2907a-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="2907a-229">これは、代わりにテーブルを作成する、別のスキーマに変更できます。</span><span class="sxs-lookup"><span data-stu-id="2907a-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="2907a-230">例えば:</span><span class="sxs-lookup"><span data-stu-id="2907a-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="2907a-231">遅延読み込み</span><span class="sxs-lookup"><span data-stu-id="2907a-231">Lazy loading</span></span>

<span data-ttu-id="2907a-232">このセクションでは、Id モデルでの遅延読み込みプロキシのサポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="2907a-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="2907a-233">これにより、それらが読み込まれる最初ことを確認せずに使用するナビゲーション プロパティを遅延読み込みは便利です。</span><span class="sxs-lookup"><span data-stu-id="2907a-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="2907a-234">エンティティの種類にできるに適した、いくつかの方法で遅延読み込み」の説明に従って、 [EF の主要なマニュアル](/ef/core/querying/related-data#lazy-loading)です。</span><span class="sxs-lookup"><span data-stu-id="2907a-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="2907a-235">簡略化のため、遅延読み込みのプロキシを使用し、これが必要ですがお。</span><span class="sxs-lookup"><span data-stu-id="2907a-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="2907a-236">インストール、 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="2907a-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="2907a-237">呼び出し`.UseLazyLoadingProxies()`内`AddDbContext`です。</span><span class="sxs-lookup"><span data-stu-id="2907a-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="2907a-238">パブリック仮想ナビゲーション プロパティを持つパブリック エンティティの種類。</span><span class="sxs-lookup"><span data-stu-id="2907a-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="2907a-239">呼び出す例を次に示します`.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="2907a-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="2907a-240">エンティティ型にナビゲーション プロパティを追加するため、上記の例を参照します。</span><span class="sxs-lookup"><span data-stu-id="2907a-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
