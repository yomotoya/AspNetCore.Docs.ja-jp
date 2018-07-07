---
title: ASP.NET Core Identity 用のカスタム ストレージ プロバイダー
author: ardalis
description: ASP.NET Core Identity 用のカスタム ストレージ プロバイダーを構成する方法について説明します。
ms.author: riande
ms.date: 05/24/2017
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: bdde9b93449c2f3f8d43cc4ff86472ed8a60ed1c
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889169"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET Core Identity 用のカスタム ストレージ プロバイダー

作成者: [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity は、拡張可能なシステム カスタム ストレージ プロバイダーを作成し、アプリに接続することができます。 このトピックでは、ASP.NET Core Identity 用のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。 独自の記憶域プロバイダーを作成するための重要な概念について説明しますが、詳細なチュートリアルはありません。

[GitHub のサンプルを表示またはダウンロードしてください](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。

## <a name="introduction"></a>はじめに

既定では、ASP.NET Core Identity システムは、Entity Framework Core を使用して SQL Server データベースにユーザー情報を格納します。 多くのアプリでは、この方法はも機能します。 ただし、別の永続化メカニズムまたはデータ スキーマを使用することもできます。 例えば:

* 使用する[Azure Table Storage](https://docs.microsoft.com/azure/storage/)または別のデータ ストア。
* データベースのテーブルでは、さまざまな構造があります。 
* など、さまざまなデータ アクセス手法を使用したい場合があります[Dapper](https://github.com/StackExchange/Dapper)します。 

、このような場合の各に、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、そのプロバイダーをアプリにプラグインできます。

ASP.NET Core Identity は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。

.NET Core CLI を使用する場合は、追加`-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET Core Identity アーキテクチャ

ASP.NET Core Identity は、マネージャーおよびストアと呼ばれるクラスで構成されます。 *マネージャー*は、アプリの開発者を使用して Identity ユーザーの作成などの操作を実行する高度なクラスです。 *ストア*は低レベル クラスなど、ユーザーとロールをエンティティの保存方法を指定します。 次のストア、[リポジトリ パターン](http://deviq.com/repository-pattern/)永続化メカニズムと密接に結び付けられているとします。 管理者は、つまり、(構成) を除く、アプリケーション コードを変更することがなく、永続化メカニズムを置き換えることができます、ストアから切り離されます。

Web アプリと対話する方法、マネージャー ストアとデータ アクセス層の対話中に次の図を示しています。

![ASP.NET Core アプリは、管理者 (たとえば、'UserManager'、'RoleManager') で機能します。 マネージャーは、ストアで (たとえば、' UserStore') と通信する Entity Framework Core などのライブラリを使用してデータ ソースで動作します。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

カスタム ストレージ プロバイダーを作成するには、このデータ アクセス層 (緑色と灰色のボックスで、上記の図) を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成します。 マネージャーは、またはそれら (上記青のボックス) と連携するアプリ コードをカスタマイズする必要はありません。

新しいインスタンスを作成するときに`UserManager`または`RoleManager`ユーザー クラスの型を指定し、ストア クラスのインスタンスを引数として渡します。 このアプローチでは、ASP.NET Core に、カスタマイズしたクラスをプラグインすることができます。 

[新しい記憶域プロバイダーを使用するアプリを再構成](#reconfigure-app-to-use-a-new-storage-provider)をインスタンス化する方法を示します`UserManager`と`RoleManager`カスタマイズされたストアとします。

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identity データ型を格納します。

[ASP.NET Core Identity](https://github.com/aspnet/identity)データ型は、次のセクションで詳しく説明します。

### <a name="users"></a>ユーザー

Web サイトの登録済みユーザー。 [IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)型を拡張または独自のカスタム型の例として使用する場合があります。 カスタム id ストレージ ソリューションを実装する特定の型から継承する必要はありません。

### <a name="user-claims"></a>ユーザーの信頼性情報

ステートメントのセット (または[クレーム](/dotnet/api/system.security.claims.claim)) について、ユーザーの id を表すユーザー。 ロールで実現できるよりも、ユーザーの id の大きい式を有効にすることができます。

### <a name="user-logins"></a>ユーザー ログイン

外部認証プロバイダー (Facebook、Microsoft アカウントなど) については、ユーザーのログイン時に使用します。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>役割

サイトのグループを承認します。 ロールの Id とロール名 ("Admin"、"Employee"など) が含まれています。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>データ アクセス層

このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れてを前提としています。 このトピックでは、リポジトリまたはデータ アクセス クラスを作成する方法についての詳細を提供しませんASP.NET Core Identity を使用する場合は、設計に関する決定事項についていくつかの候補を提供します。

カスタマイズされたストア プロバイダーのデータ アクセス層を設計するときにあまり自由度があります。 のみ、アプリで使用する予定の機能を永続化メカニズムを作成する必要があります。 たとえば、アプリでのロールを使用していない場合は、役割またはユーザー ロールの関連付けのストレージを作成する必要はありません。 既存のインフラストラクチャ、テクノロジは ASP.NET Core Identity の既定の実装を非常に異なる構造を必要があります。 データ アクセス層は、ストレージの実装の構造を使用するロジックを提供します。

データ アクセス層は、ASP.NET Core Identity からデータ ソースにデータを保存するロジックを提供します。 カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。

### <a name="context-class"></a>Context クラス

永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。 いくつかのデータ クラスには、このクラスは、依存関係の挿入によって提供される通常のインスタンスが必要です。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)します。

### <a name="user-storage"></a>ユーザーのストレージ

格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>ロールのストレージ

格納し、(ロール名) などのロール情報を取得します。 [例](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims ストレージ

格納し、(要求の種類と値の場合) などのユーザー要求の情報を取得します。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins ストレージ

格納し、(外部認証プロバイダーの場合) などのユーザー ログイン情報を取得します。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole 記憶域

格納し、どのユーザーに割り当てられたロールを取得します。 [例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**ヒント:** のみ、アプリで使用するクラスを実装します。

データ アクセス クラスで、永続化メカニズムのデータの操作を実行するコードを提供します。 たとえば、カスタム プロバイダーでは、内にある必要がありますで新しいユーザーを作成する次のコード、*格納*クラス。

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

ユーザーを作成するための実装ロジックは、`_usersTable.CreateAsync`メソッドを次に示します。

## <a name="customize-the-user-class"></a>ユーザー クラスをカスタマイズします。

記憶域プロバイダーを実装する場合と同じであるユーザー クラスを作成、 [ `IdentityUser`クラス](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)します。

少なくとも、ユーザー クラスを含める必要があります、`Id`と`UserName`プロパティ。

`IdentityUser`クラス、プロパティを定義する、`UserManager`要求された操作の呼び出しを実行する場合。 既定の型、`Id`プロパティは、文字列が継承できる`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`し、別の種類を指定します。 フレームワークには、データ型変換を処理するために、ストレージの実装が必要です。

## <a name="customize-the-user-store"></a>ユーザー ストアをカスタマイズします。

作成、`UserStore`ユーザーに対するすべてのデータ操作のメソッドを提供するクラス。 このクラスは、 [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)クラス。 `UserStore`クラスを実装`IUserStore<TUser>`と必要とする省略可能なインターフェイスです。 実装する省略可能なインターフェイスを選択すると、アプリで提供される機能に基づいています。

### <a name="optional-interfaces"></a>省略可能なインターフェイス

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

省略可能なインターフェイスを継承`IUserStore<TUser>`します。 格納部分的に実装されているサンプル ユーザーを確認できます、[サンプル アプリ](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)します。

内で、`UserStore`クラス、操作を実行するために作成したデータ アクセス クラスを使用します。 依存関係の挿入を使用して渡されます。 たとえば、Dapper の実装を含む SQL Server で、`UserStore`クラスには、`CreateAsync`メソッドのインスタンスを使用している`DapperUsersTable`新しいレコードを挿入します。

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>ユーザー ストアをカスタマイズする際に実装するインターフェイス

- **IUserStore**  
 [IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1)インターフェイスは、唯一のインターフェイスで、ユーザー ストアを実装する必要があります。 作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)インターフェイスは、ユーザーの信頼性情報を有効にするために実装するメソッドを定義します。 追加、削除、およびユーザーの信頼性情報を取得するためのメソッドが含まれています。
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)外部認証プロバイダーを有効にするために実装するメソッドを定義します。 追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)インターフェイスを実装するロールにユーザーをマップするメソッドを定義します。 追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)インターフェイスは、ハッシュされたパスワードを保持するために実装するメソッドを定義します。 ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッドを取得および設定のためのメソッドが含まれています。
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)インターフェイスは、ユーザーのアカウント情報が変更されたかどうかを示すためにセキュリティ スタンプを使用するために実装するメソッドを定義します。 ユーザー、パスワードを変更または追加またはログインを削除します。 このタイムスタンプが更新されます。 取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)インターフェイスは、2 要素認証をサポートするために実装するメソッドを定義します。 取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)インターフェイスは、ユーザーの電話番号を格納するために実装するメソッドを定義します。 取得および電話番号と電話番号が確認されているかどうかを設定するためのメソッドが含まれています。
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)インターフェイスは、ユーザーのメール アドレスを格納するために実装するメソッドを定義します。 取得、および電子メール アドレスと、電子メールが確認されているかどうかを設定するためのメソッドが含まれています。
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)インターフェイスは、アカウントのロックに関する情報を格納するために実装するメソッドを定義します。 失敗したアクセス試行とロックアウトを追跡するためのメソッドが含まれています。
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)インターフェイスは、クエリ可能なユーザー ストアを提供するために実装するメンバーを定義します。

アプリに必要なインターフェイスだけを実装します。 例えば:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、IdentityUserLogin、および IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework`名前空間には実装が含まれています、 [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、 [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)、および[IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)クラス。 これらの機能を使用している場合は、これらのクラスの独自のバージョンを作成し、アプリのプロパティを定義することがあります。 ただし、場合がありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。 代わりに、バックエンド ストア クラスには、データ ソースで直接これらの操作を実行できます。 たとえば、`UserStore.GetClaimsAsync`メソッドを呼び出すことができます、`userClaimTable.FindByUserId(user.Id)`でクエリを実行するメソッドは直接テーブルをクレームの一覧を返します。

## <a name="customize-the-role-class"></a>Role クラスをカスタマイズします。

ロールの記憶域プロバイダーを実装する場合は、カスタム ロールの種類を作成できます。 特定のインターフェイスを実装していない必要がある必要があります、`Id`通常には、`Name`プロパティ。

ロールのクラスの例を次に示します。

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>ロール ストアをカスタマイズします。

作成することができます、`RoleStore`ロールに対するすべてのデータ操作のメソッドを提供するクラス。 このクラスは、 [RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)クラス。 `RoleStore`クラスを実装する、`IRoleStore<TRole>`と、必要に応じて、`IQueryableRoleStore<TRole>`インターフェイス。

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1)インターフェイスは、ロール ストア クラスに実装するメソッドを定義します。 作成、更新、削除、およびロールを取得するためのメソッドが含まれています。
- **RoleStore&lt;TRole&gt;**  
 カスタマイズする`RoleStore`、実装するクラスを作成、`IRoleStore<TRole>`インターフェイス。 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>新しい記憶域プロバイダーを使用するアプリを再構成します。

記憶域プロバイダーが実装されると、それを使用するアプリを構成します。 アプリでは、既定のプロバイダーを使用する場合は、カスタム プロバイダーで置き換えます。

1. 削除、 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet パッケージ。
1. 記憶域プロバイダーを別のプロジェクトまたはパッケージが存在する場合への参照を追加します。
1. すべての参照を置き換える`Microsoft.AspNetCore.EntityFramework.Identity`を使用すると、記憶域プロバイダーの名前空間のステートメント。
1. `ConfigureServices`メソッド、変更、`AddIdentity`カスタム型を使用する方法。 この目的のための独自の拡張メソッドを作成することができます。 参照してください[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)例についてはします。
1. ロールを使用している場合は、更新、`RoleManager`を使用する、`RoleStore`クラス。
1. アプリの構成に、接続文字列と資格情報を更新します。

例:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>参照

- [ASP.NET Identity のカスタム ストレージ プロバイダー](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -このリポジトリには、ストア プロバイダーを管理するコミュニティへのリンクが含まれています。
