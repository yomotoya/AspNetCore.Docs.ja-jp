---
title: 認証の ASP.NET メンバーシップから ASP.NET Core 2.0 Identity に移行します。
author: isaac2004
description: ASP.NET Core 2.0 identity メンバーシップ認証を使用して既存の ASP.NET アプリを移行する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264732"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>認証の ASP.NET メンバーシップから ASP.NET Core 2.0 Identity に移行します。

著者: [Isaac Levin](https://isaaclevin.com)

この記事では、ASP.NET Core 2.0 identity メンバーシップ認証を使用して ASP.NET アプリ用のデータベース スキーマの移行を示します。

> [!NOTE]
> このドキュメントでは、ASP.NET Core Id に使用されるデータベース スキーマに ASP.NET メンバーシップ ベースのアプリのデータベース スキーマを移行するために必要な手順が説明します。 ASP.NET メンバーシップ ベースの認証から ASP.NET Identity に移行する詳細については、次を参照してください。 [SQL メンバーシップから ASP.NET Identity に既存のアプリを移行する](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)します。 詳細については、ASP.NET Core Identity は、次を参照してください。[の ASP.NET core Identity の概要](xref:security/authentication/identity)します。

## <a name="review-of-membership-schema"></a>メンバーシップ スキーマの確認

ASP.NET 2.0 では、前に開発者はアプリ全体の認証および承認プロセスの作成に取り組みました。 ASP.NET 2.0 でメンバーシップ導入された、ASP.NET アプリ内のセキュリティを処理する定型コード ソリューションを提供します。 開発者は、SQL Server データベースにスキーマをブートス トラップすること、 [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)コマンド。 このコマンドを実行した後、次の表は、データベースに作成されました。

  ![メンバーシップ テーブル](identity/_static/membership-tables.png)

既存のアプリを ASP.NET Core 2.0 Identity に移行するには、これらのテーブル内のデータは、新しい Identity スキーマで使用されるテーブルに移行する必要があります。

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 スキーマ

ASP.NET core 2.0、 [Identity](/aspnet/identity/index)原則 ASP.NET 4.5 で導入されました。 フレームワークの実装は ASP.NET Core のバージョン間であっても、別の原則を共有すると、(を参照してください[認証と Id を ASP.NET Core 2.0 に移行](xref:migration/1x-to-2x/index))。

ASP.NET Core 2.0 の Identity のスキーマを表示する最も簡単な方法では、新しい ASP.NET Core 2.0 アプリを作成します。 Visual Studio 2017 でのこれらの手順に従います。

1. **[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。
1. 新規作成**ASP.NET Core Web アプリケーション**という名前のプロジェクト*CoreIdentitySample*します。
1. 選択**ASP.NET Core 2.0**選択し、ドロップダウンで**Web アプリケーション**します。 このテンプレートによって生成されます、 [Razor ページ](xref:razor-pages/index)アプリ。 クリックする前に**OK**、 をクリックして**認証の変更**します。
1. 選択**個々 のユーザー アカウント**Identity テンプレート。 最後に、をクリックして**OK**、し**OK**します。 Visual Studio では、ASP.NET Core Identity テンプレートを使用してプロジェクトを作成します。
1. 選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**を開く、**パッケージ マネージャー コンソール**(PMC) ウィンドウです。
1. PMC で、プロジェクトのルートに移動し、実行、 [Entity Framework (EF) Core](/ef/core) `Update-Database`コマンド。

    ASP.NET Core 2.0 Identity では、EF Core を使用して、認証データを格納するデータベースと対話します。 新しく作成されたアプリを操作の順番は、このデータを格納するデータベースをする必要があります。 新しいアプリを作成した後、データベース環境でスキーマを検査する最も簡単な方法を使用してデータベースを作成する[EF コア移行](/ef/core/managing-schemas/migrations/)します。 このプロセスを作成、データベース、その他の場所またはローカルそのスキーマと同じ動作をします。 詳細については、前のドキュメントを確認します。

    EF Core のコマンドで指定したデータベースの接続文字列を使用して*appsettings.json*します。 次の接続文字列でデータベースを対象と*localhost*という*asp net core identity*します。 この設定で、EF Core が使用するよう構成、`DefaultConnection`接続文字列。

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. 選択**ビュー** > **SQL Server オブジェクト エクスプ ローラー**します。 指定されたデータベース名に対応するノードを展開し、`ConnectionStrings:DefaultConnection`プロパティの*appsettings.json*します。

    `Update-Database`コマンドは、スキーマで指定されたデータベースとアプリの初期化に必要なすべてのデータを作成します。 次の図は、上記の手順で作成されるテーブル構造を示しています。

    ![Identity テーブル](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>スキーマを移行します。

テーブルの構造とメンバーシップと ASP.NET Core Identity の両方のフィールドにわずかな違いがあります。 パターンは、ASP.NET と ASP.NET Core アプリでの認証/承認の大幅に変更されました。 Id で引き続き使用されるキー オブジェクトが*ユーザー*と*ロール*します。 マッピング テーブルを次のとおり*ユーザー*、*ロール*、および*UserRoles*します。

### <a name="users"></a>Users

|*Identity<br>(dbo します。AspNetUsers)*        ||*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**フィールド名**                 |**Type**|**フィールド名**                                    |**Type**|
|`Id`                           |string  |`aspnet_Users.UserId`                             |string  |
|`UserName`                     |string  |`aspnet_Users.UserName`                           |string  |
|`Email`                        |string  |`aspnet_Membership.Email`                         |string  |
|`NormalizedUserName`           |string  |`aspnet_Users.LoweredUserName`                    |string  |
|`NormalizedEmail`              |string  |`aspnet_Membership.LoweredEmail`                  |string  |
|`PhoneNumber`                  |string  |`aspnet_Users.MobileAlias`                        |string  |
|`LockoutEnabled`               |bit     |`aspnet_Membership.IsLockedOut`                   |bit     |

> [!NOTE]
> すべてのフィールド マッピングのメンバーシップから ASP.NET Core Id への一対一のリレーションシップのようになります。 上記の表は、メンバーシップ ユーザーの既定のスキーマを受け取り、ASP.NET Core Identity スキーマにマップします。 メンバーシップの使用された他のカスタム フィールドは、手動でマップする必要があります。 このマッピングはありませんマップのパスワードのように、2 つの間でパスワードの基準とパスワードの salt の両方の移行はありません。 **パスワードが null のままにして、パスワードをリセットするユーザーに確認することがお勧めします。** ASP.NET Core Identity で`LockoutEnd`ユーザーがロックアウトされた場合、将来の日付に設定する必要があります。これは、移行スクリプトに表示されます。

### <a name="roles"></a>役割

|*Identity<br>(dbo.AspNetRoles)*        ||*Membership<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**フィールド名**                 |**Type**|**フィールド名**   |**Type**         |
|`Id`                           |string  |`RoleId`         | string          |
|`Name`                         |string  |`RoleName`       | string          |
|`NormalizedName`               |string  |`LoweredRoleName`| string          |

### <a name="user-roles"></a>ユーザー ロール

|*Identity<br>(dbo.AspNetUserRoles)*||*Membership<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**フィールド名**           |**Type**  |**フィールド名**|**Type**                   |
|`RoleId`                 |string    |`RoleId`      |string                     |
|`UserId`                 |string    |`UserId`      |string                     |

移行スクリプトを作成する場合は、上記のマッピング テーブルを参照*ユーザー*と*ロール*します。 次の例では、データベース サーバーに 2 つのデータベースがある前提としています。 1 つのデータベースには、既存の ASP.NET メンバーシップ スキーマとデータが含まれています。 他の*CoreIdentitySample*データベースは、前述の手順を使用して作成されました。 コメントは、詳細についてはインラインで含まれています。

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

上記のスクリプトの完了後、先ほど作成した ASP.NET Core Identity アプリにはメンバーシップ ユーザーが設定されます。 ユーザーは、ログイン前にパスワードを変更する必要があります。

> [!NOTE]
> メンバーシップ システムは、電子メール アドレスに一致していないユーザー名を持つユーザーがある、変更はこれを対応するために以前に作成したアプリに必要があります。 既定のテンプレートが必要ですが`UserName`と`Email`同じにします。 いる別の場合、ログイン プロセスを使用するように変更する必要がある`UserName`の代わりに`Email`します。

`PageModel`でのログイン ページが*Pages\Account\Login.cshtml.cs*、削除、`[EmailAddress]`属性を*電子メール*プロパティ。 名前を変更する*UserName*します。 変更が必要です限り`EmailAddress`で、記載されて、*ビュー*と*PageModel*します。 結果は、次のようになります。

 ![固定のログイン](identity/_static/fixed-login.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、ユーザーが SQL メンバーシップから ASP.NET Core 2.0 の Id を移植する方法について説明しました。 ASP.NET Core Identity に関する詳細については、次を参照してください。 [Id の概要](xref:security/authentication/identity)します。
