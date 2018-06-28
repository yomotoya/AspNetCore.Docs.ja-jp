---
title: Id モデルのカスタマイズ
author: ajcvickers
description: この記事では、ASP.NET Core Id の基礎となる Entity Framework のコア データ モデルをカスタマイズする方法について説明します。
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036895"
---
# <a name="identity-model-customization"></a>Id モデルのカスタマイズ

によって[Arthur ヴィッカース](https://github.com/ajcvickers)

ASP.NET Core の Id は、管理および ASP.NET Core アプリケーションでユーザー アカウントを保存するためのフレームワークを提供します。 Id は、「個々 のユーザー アカウント」を認証メカニズムとして選択すると、プロジェクトに追加されます。 既定では、ユーザーを使用する Entity Framework (EF) の主要なデータ モデル。 この記事では、Id モデルをカスタマイズする方法について説明します。

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Id および EF コアの移行

Identity のしくみを理解すると便利です、モデルを調べる前に[EF コア移行](/ef/core/managing-schemas/migrations/)を作成し、データベースを更新します。 最上位のレベルでは、手順は次のとおりです。

1. 定義または更新、[コードでデータ モデル](/ef/core/modeling/)です。
1. このモデルに変更をデータベースに適用できる変換への移行を追加します。
1. 移行が正しく、その目的を表すことを確認してください。
1. モデルと同期するデータベースの更新への移行を適用します。
1. さらに、モデルの調整、データベースの同期を保つ手順 1. ~ 4. を繰り返します。

移行では、追加されを使用して適用します。

* [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) Visual Studio を使用している場合。
* [Dotnet コマンド](/ef/core/miscellaneous/cli/dotnet)コマンド ラインでします。
* アプリケーションの実行時に、エラー ページの移行のリンクを選択します。

ASP.NET Core では、アプリケーションを実行すると、移行を適用するために使用する開発時のエラー ページ ハンドラーがあります。 実稼働アプリケーションの方が、移行から SQL スクリプトを生成し、制御されたアプリケーションとデータベースの展開の一部としてデータベースにこれらを展開に適切な。

Id を使用して、新しいアプリケーションが作成されると、手順 1. と 2. を以上が既に完了します。 つまり、初期のデータ モデルが既に存在して、最初の移行がプロジェクトに追加されました。 最初の移行は、データベースに適用する必要があります。 または、アプリケーションの実行時にエラー ページを使用して、データベースの更新 (PMC)、dotnet ef database 更新 (.NET Core CLI) コマンドを実行して、最初の移行を実行できます。

上記の手順は、モデルに変更が加えられるを繰り返す必要があります。

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Id モデル

### <a name="entity-types"></a>エンティティ型

Id モデルは、7 つのエンティティ型で構成されます。

* `User` -ユーザーを表します
* `Role` ロールを表します
* `UserClaim` -ユーザーがクレームを表します
* `UserToken` -ユーザーの認証トークンを表します
* `UserLogin` -ユーザーをログインに関連付けます
* `RoleClaim` は、ロール内のすべてのユーザーに許可するクレームを表します
* `UserRole` -ユーザーおよびロールに関連付けているエンティティを参加させる

### <a name="entity-type-relationships"></a>エンティティ型の関係

これらのエンティティ型は、次のように互いに関連します。

* 各`User`多くを持つことができます `UserClaims`
* 各`User`多くを持つことができます `UserLogins`
* 各`User`多くを持つことができます `UserTokens`
* 各`Role`関連付けられた多くを持つことができます `RoleClaims`
* 各`User`関連付けられている多数格納できます`Roles`、および各`Role`多くのユーザーを関連付けることができます
  * これは、データベース内の結合テーブルを必要とする多対多リレーションシップです。 結合テーブルで表現され、`UserRole`エンティティです。

### <a name="default-model-configuration"></a>既定のモデルの構成

Identity 定義から継承される「コンテキスト クラス」のさまざまな[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)を構成して、モデルを使用します。 この構成が行われますを使用して、 [EF コア コード最初 Fluent API](/ef/core/modeling/)で、 [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_)コンテキスト クラスのメソッドです。 既定の構成は次のとおりです。

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

### <a name="model-generic-types"></a>モデルのジェネリック型

Id は、上記のエンティティ型の各の既定の CLR 型を定義します。 これらの型は、すべて先頭に付きます"Identity": `IdentityUser`、 `IdentityRole`、 `IdentityUserClaim`、 `IdentityUserToken`、 `IdentityUserLogin`、 `IdentityRoleClaim`、および`IdentityUserRole`です。

これらの型を直接使用するのではなく アプリケーションの種類の型の基本クラスとして使用できます。 `DbContext` Id を使用して定義されているクラスでは一般的なことで、モデルのエンティティ型の 1 つ以上の別の CLR 型を使用できます。 これらのジェネリック型を変更する、ユーザーのプライマリ キーの型のこともできます。

ロールのサポートの Id を使用すると、 [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext)クラスを使用する必要があります。

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

その場合、ロール (要求のみ) Id を使用することも、`IdentityUserContext`クラスを使用する必要があります。


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

## <a name="customizing-the-model"></a>モデルのカスタマイズ

モデルをカスタマイズするための出発点は適切なコンテキスト型から派生するには前のセクションを参照してください。 このコンテキスト型が呼び出される慣例的`ApplicationDbContext`ASP.NET Core テンプレートによって作成されたとします。

コンテキストを使用して、2 つの方法で、モデルを構成します。
* ジェネリック型パラメーターのエンティティとキーの型を指定します。
* オーバーライドすることで`OnModelCreating`これらの型のマッピングを変更します。

オーバーライドする場合`OnModelCreating`、`base.OnModelCreating`呼び出す必要があります最初に、オーバーライドして、構成を次に呼び出す必要があります。 EF コアは、通常の構成用のポリシーを最後の 1 つの wins を持っています。 たとえば場合、`ToTable`エンティティの型は、1 つのテーブルの名前の最初と呼ばれる別のテーブルの名前でもう一度後で、2 番目の呼び出し内のテーブル名は 1 つです。

### <a name="using-a-custom-user-type"></a>カスタム ユーザーの種類を使用します。

カスタム ユーザーの種類を使用する型を作成してそれを継承`IdentityUser`です。 この型名前を指定するが通例`ApplicationUser`です。 この型は、通常持ち追加のプロパティ、基本データ型ではなく、それ以外の場合はありますありません値作成中にします。 例えば:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

次のコンテキストの汎用引数としてこの型を使用します。

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

更新`ConfigureServices`、新しい`ApplicationUser`クラス。

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

オーバーライドする必要はありません`OnModelCreating`EF コアにマップするためにここでは、`CustomTag`慣例プロパティです。 ただし、データベースが新しいに更新する必要がある`CustomTag`列です。 これを行うには、移行を追加し、」の説明に従って、データベースを更新[Id と EF コア移行](#identity-migrations)です。

### <a name="changing-the-key-type"></a>キーの種類を変更します。

データベースが作成された後に主キー (PK) 列の型を変更するは、多くのデータベース システムで問題が発生します。 通常、主キーを変更するには、削除と、テーブルを再作成が含まれます。 そのため、データベースの作成時に対象キーの種類が作成されるように、重要な型が最初の移行で指定できることをお勧めします。

データベースが作成されている場合、し`Drop-Database`(PMC) または`dotnet ef database drop`削除するには (.NET Core CLI) を使用できます。

これには、データベースが存在しないことが確認されて、削除に最初の移行`Remove-Migration`(PMC) または`dotnet ef migrations remove`(.NET Core CLI)。

更新プログラム、`ApplicationDbContext`の新しいキーの種類を指定する、異なる基底クラスを使用する`TKey`です。 たとえば、使用するため、`Guid`キー。

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

注意して、ジェネリック クラス`IdentityUser<TKey>`と`IdentityRole<TKey>`新しいキーの型を使用するも指定する必要があります。 `ConfigureServices` ジェネリック ユーザーを使用する更新する必要があります。

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

場合、カスタム`ApplicationUser`はから継承するように更新が使用されている`IdentityUser<TKey>`です。 例えば:

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

### <a name="adding-navigation-properties"></a>ナビゲーション プロパティを追加します。

リレーションシップのモデルの構成に対する変更よりも難しいその他の変更を加えることができます。 新しい追加のリレーションシップを作成するのではなく、既存のリレーションシップを置き換えるに注意する必要があります。 具体的には、変更されたリレーションシップでは、既存のリレーションシップとして同じ外部キーのプロパティを指定する必要があります。 間のリレーションシップなど、`Users`と`UserClaims`として指定された既定では。

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

このリレーションシップの外部キーとして指定された、`UserClaim.UserId`プロパティです。 `HasMany` および`WithOne`ナビゲーション プロパティのないリレーションシップを作成する引数を使用しないと呼ばれます。

ナビゲーション プロパティを追加`ApplicationUser`これにより、関連付けられている`UserClaims`ユーザーから参照されていること。

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

なお、`TKey`の`IdentityUserClaim<TKey>`ユーザー--この場合の主キーに指定された型は、`string`既定の設定を使用しているためです。 **いない**のプライマリ キーの種類、`UserClaim`エンティティ型。

ナビゲーション プロパティが存在するようになりましたことで構成する必要があります`OnModelCreating`:

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

呼び出しで指定されたナビゲーション プロパティでのみ前に、のとまったく同じ関係が構成されていることに注意してください。`HasMany`です。

ナビゲーション プロパティは、データベースではない、EF モデルにのみ存在します。 リレーションシップの外部キーが変更されていないために、このモデルの変更の種類は、データベースを更新する必要はありません。 これを変更を行った後に移行を追加することで確認する:`Up`と`Down`メソッドは空です。

### <a name="adding-all-user-navigation-properties"></a>すべてのユーザーのナビゲーション プロパティを追加します。

上のセクションを使用して、ガイダンスとしてと、ユーザーのすべての関係の一方向のナビゲーション プロパティを構成する例を次に示します。

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

### <a name="adding-user-and-role-navigation-properties"></a>ユーザーおよびロールのナビゲーション プロパティを追加します。

上のセクションを使用して、ガイダンスとしてと、ユーザーおよびロールのすべての関係のナビゲーション プロパティを構成する例を次に示します。

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

メモ:

* この例も含まれています、`UserRole`ロールにユーザーからの多対多リレーションシップを移動するために必要なエンティティを結合します。
* 内容を反映するナビゲーション プロパティの型を変更して`ApplicationXxx`型は、の代わりに使用されているようになりました`IdentityXxx`型です。
* 使用してください、`ApplicationXxx`ジェネリック`ApplicationContext`定義します。

### <a name="adding-all-navigation-properties"></a>すべてのナビゲーション プロパティを追加します。

上のセクションを使用して、ガイダンスとしてと、すべてのエンティティ型のすべての関係のナビゲーション プロパティを構成する例を次に示します。

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

### <a name="using-composite-keys"></a>複合キーを使用

前のセクションには、Id モデルで使用されるキーの型の変更が示されています。 複合キーを使用するには、Id キー モデルを変更またはされていないサポートされていることをお勧めします。 Id を持つ複合キーを使用するには、Identity manager のコードで、モデルでは、これはサポートされていないカスタマイズし、このドキュメントの範囲を超えてがどのようにやり取りする方法の変更が行われます。

### <a name="changing-tablecolumn-names-and-facets"></a>テーブルまたは列名とファセットの変更

テーブルと列の名前を変更するには、呼び出す`base.OnModelCreating`、既定値をオーバーライドする構成を追加します。 たとえば、すべてのユーザー テーブルの名前を変更するには、します。

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

これらの例が既定の Id 型を使用することに注意してください。 アプリケーションの種類 がなどを使用するかどうかは`ApplicationUser`既定値の型ではなく、その種類を構成します。

一部の列名を変更する例を次に示します。

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

特定のデータベース列のいくつかの種類を構成することができます_ファセット_許可されている文字列の最大長などです。 モデルのいくつかの文字列プロパティの列の最大長を設定する例を次に示します。

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

### <a name="mapping-to-a-different-schema"></a>別のスキーマへのマッピング

別のデータベース プロバイダーでスキーマが異なる動作できますが、SQL Server,、既定値は"dbo"スキーマ内のすべてのテーブルを作成します。 これは、代わりにテーブルを作成する、別のスキーマに変更できます。 例えば:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>遅延読み込み

このセクションでは、Id モデルでの遅延読み込みプロキシのサポートが追加されます。 これにより、それらが読み込まれる最初ことを確認せずに使用するナビゲーション プロパティを遅延読み込みは便利です。

エンティティの種類にできるに適した、いくつかの方法で遅延読み込み」の説明に従って、 [EF の主要なマニュアル](/ef/core/querying/related-data#lazy-loading)です。 簡略化のため、遅延読み込みのプロキシを使用し、これが必要ですがお。

* インストール、 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)パッケージです。
* 呼び出し`.UseLazyLoadingProxies()`内`AddDbContext`です。
* パブリック仮想ナビゲーション プロパティを持つパブリック エンティティの種類。

呼び出す例を次に示します`.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

エンティティ型にナビゲーション プロパティを追加するため、上記の例を参照します。
