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
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>ASP.NET メンバーシップ認証から ASP.NET Core 2.0 Id への移行します。

著者: [Isaac Levin](https://isaaclevin.com)

この記事では、ASP.NET Core 2.0 Id にメンバーシップの認証を使用して ASP.NET アプリ用のデータベース スキーマの移行について説明します。

> [!NOTE]
> このドキュメントでは、ASP.NET メンバーシップ ベースのアプリのデータベース スキーマを ASP.NET Core Id に使用されるデータベース スキーマに移行するために必要な手順を説明します。 ASP.NET メンバーシップに基づく認証から ASP.NET の Id への移行の詳細については、次を参照してください。 [SQL メンバーシップから ASP.NET の Id に、既存のアプリを移行](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)です。 ASP.NET Core Id に関する詳細については、次を参照してください。 [ASP.NET Core での Id の概要](xref:security/authentication/identity)です。

## <a name="review-of-membership-schema"></a>メンバーシップのスキーマの確認

ASP.NET 2.0 では、前に開発者が任務を負って、認証と承認のプロセスの全がアプリを作成します。 ASP.NET 2.0 では、メンバーシップ導入された、ASP.NET アプリケーション内のセキュリティの処理に定型ソリューションを提供します。 開発者によって使用する SQL Server データベースにスキーマをブートス トラップ今すぐできた、 [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)コマンド。 このコマンドを実行すると、次の表は、データベースで作成されました。

  ![メンバーシップ テーブル](identity/_static/membership-tables.png)

既存のアプリを移行すると、ASP.NET Core 2.0 Id に、これらのテーブル内のデータを新しい Id スキーマで使用されるテーブルに移行する必要があります。

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 スキーマ

ASP.NET Core 2.0 に依存して、 [Identity](/aspnet/identity/index) ASP.NET 4.5 で導入されたプリンシパル。 原則を共有すると、フレームワークの実装では ASP.NET Core のバージョン間でさえも、異なる (を参照してください[ASP.NET Core 2.0 への認証と Id の移行](xref:migration/1x-to-2x/index))。

ASP.NET Core 2.0 Identity のスキーマを表示する最も簡単な方法では、新しい ASP.NET Core 2.0 アプリを作成します。 Visual Studio 2017 でこれらの手順に従います。

* **[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。
* 新しい**ASP.NET Core Web アプリケーション**、プロジェクトの名前と*CoreIdentitySample*です。
* ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。 このテンプレートを生成、 [Razor ページ](xref:mvc/razor-pages/index)アプリ。 クリックする前に**OK**をクリックして**認証の変更**です。
* 選択**個々 のユーザー アカウント**Identity テンプレート。 最後に、をクリックして**OK**、し**OK**です。 Visual Studio では、ASP.NET Core Id テンプレートを使用して、プロジェクトを作成します。

ASP.NET Core 2.0 の Id を使用して[Entity Framework Core](/ef/core)認証データを格納するデータベースと対話します。 新しく作成されたアプリ動作するためは、このデータを格納するデータベースを指定する必要があります。 新しいアプリを作成した後は、データベース環境のスキーマを検査する最も簡単な方法は、Entity Framework migrations を使用してデータベースを作成します。 このプロセスを作成、データベース、他の場所で、またはいずれかのローカルでそのスキーマと同じ動作です。 詳細については、前のドキュメントを確認します。

Asp.net Core スキーマを持つデータベースを作成するには、実行、 `Update-Database` Visual studio のコマンド**Package Manager Console** (PMC) ウィンドウ&mdash;に配置されている**ツール** > **NuGet Package Manager** > **Package Manager Console**です。 PMC には、Entity Framework コマンドの実行がサポートされています。

Entity Framework のコマンドで指定したデータベースの接続文字列を使用して*される appsettings.json*です。 次の接続文字列では、データベースを対象に*localhost*という*asp net コア id*です。 この設定は、Entity Framework が使用するよう構成、`DefaultConnection`接続文字列。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

このコマンドは、スキーマで指定されたデータベースとアプリの初期化に必要なすべてのデータを作成します。 次の図は、上記の手順で作成されるテーブル構造を示しています。

   ![Identity テーブル](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>スキーマを移行します

テーブルの構造とメンバーシップと ASP.NET Core Id の両方のフィールドにわずかな違いがあります。 パターンは、ASP.NET および ASP.NET Core アプリによる認証/承認の大幅に変更されました。 Id で引き続き使用されるキー オブジェクトが*ユーザー*と*ロール*です。 ここでは、マッピング テーブルを*ユーザー*、*ロール*、および*UserRoles*です。

### <a name="users"></a>ユーザー

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **フィールド名** | **Type**  |   **フィールド名** | **Type**  |
|`Id` | string | `aspnet_Users.UserId` | string
|`UserName` | string | `aspnet_Users.UserName` | string
|`Email` | string | `aspnet_Membership.Email` | string
|`NormalizedUserName` | string | `aspnet_Users.LoweredUserName` | string
|`NormalizedEmail` | string | `aspnet_Membership.LoweredEmail` | string
|`PhoneNumber` | string | `aspnet_Users.MobileAlias` | string
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> すべてのフィールド マッピングのメンバーシップから ASP.NET Core Id に一対一のリレーションシップのようになります。 上記の表は既定のメンバーシップ ユーザーのスキーマを使用し、ASP.NET Core Id スキーマにマップします。 メンバーシップの使用されたその他のカスタム フィールドは、手動でマップする必要があります。 このマッピングのマップがない、パスワードのようにパスワードの基準とパスワードの salt の両方を 2 つの間で移行できません。 **パスワードが null のままにして、パスワードをリセットするかどうかを勧めがします。** ASP.NET Core Id で`LockoutEnd`ユーザーがロックアウトされた場合、将来のいくつかの日付に設定する必要があります。これは、移行スクリプトに表示されます。

### <a name="roles"></a>役割

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **フィールド名** | **Type**  |   **フィールド名** | **Type**  |
|`Id` | string | `RoleId` | string
|`Name` | string | `RoleName` | string
|`NormalizedName` | string | `LoweredRoleName` | string

### <a name="user-roles"></a>ユーザー ロール

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **フィールド名** | **Type**  |   **フィールド名** | **Type**  |
|`RoleId` | string | `RoleId` | string
|`UserId` | string | `UserId` | string

移行スクリプトを作成するときに、上記のマッピング テーブルを参照*ユーザー*と*ロール*です。 次の例では、データベース サーバーに 2 つのデータベースがあると仮定します。 1 つのデータベースには、既存の ASP.NET メンバーシップ スキーマとデータが含まれています。 その他のデータベースは、既に説明した手順を使用して作成されました。 コメントは、詳細についてはインラインで含まれています。

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

このスクリプトの完了したら、先ほど作成した Asp.net Core アプリケーションにはメンバーシップ ユーザーが格納されます。 ユーザーでのログ記録する前にパスワードを変更する必要があります。

> [!NOTE]
> メンバーシップ システムは、ユーザーの電子メール アドレスに一致していないユーザー名を持つユーザーがある、変更はこれに合わせて以前に作成したアプリに必要があります。 既定のテンプレートが必要ですが`UserName`と`Email`同じにします。 いる別の場合、ログイン プロセスを使用するように変更する必要がある`UserName`の代わりに`Email`です。

`PageModel`にある、ログイン ページの*Pages\Account\Login.cshtml.cs*、削除、`[EmailAddress]`属性から、*電子メール*プロパティです。 名前を変更して*UserName*です。 変更が必要です限り`EmailAddress`が記述されて、*ビュー*と*PageModel*です。 結果は、次のようになります。

 ![固定のログイン](identity/_static/fixed-login.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、ユーザーが SQL メンバーシップから ASP.NET Core 2.0 の Id を移植する方法を学習しました。 ASP.NET Core Id に関する詳細については、次を参照してください。 [Id の概要](xref:security/authentication/identity)です。
