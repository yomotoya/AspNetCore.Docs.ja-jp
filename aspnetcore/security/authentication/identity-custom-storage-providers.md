---
title: "ASP.NET Core Id のカスタムの記憶域プロバイダー"
author: ardalis
description: "ASP.NET Core Id のカスタムの記憶域プロバイダーを構成する方法を説明します。"
manager: wpickett
ms.author: riande
ms.date: 05/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 8cadb550eaa2dbc4541f945dc8d8d49fa757d4d3
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET Core Id のカスタムの記憶域プロバイダー

作成者: [Steve Smith](https://ardalis.com/)

ASP.NET Core の Id は、カスタムの記憶域プロバイダーを作成し、アプリに接続することにより拡張可能なシステムです。 このトピックでは、ASP.NET Core Id 用のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。 独自の記憶域プロバイダーを作成するための重要な概念について説明しますが、ステップ バイ ステップ チュートリアルではありません。

[GitHub のサンプルを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。

## <a name="introduction"></a>はじめに

既定では、ASP.NET Core Id システムは、Entity Framework のコアを使用して SQL Server データベースにユーザー情報を格納します。 多くのアプリは、このアプローチはします。 ただし、別の永続化メカニズムまたはデータ スキーマを使用することができます。 例:

* 使用する[Azure テーブル ストレージ](https://docs.microsoft.com/azure/storage/)または別のデータ ストアです。
* データベースのテーブルでは、別の構造があります。 
* など、さまざまなデータ アクセス方法を使用することができます[Dapper](https://github.com/StackExchange/Dapper)です。 

このような場合の各、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、そのプロバイダーをアプリに接続できます。

ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。

.NET Core CLI を使用して、追加`-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET Core Id アーキテクチャ

ASP.NET Core Id は、マネージャーおよびストアと呼ばれるクラスで構成されます。 *マネージャー*アイデンティティ ユーザーを作成するなどの操作を実行するアプリの開発者が使用する高レベルのクラスです。 *ストア*ユーザーやロールなどのエンティティを永続化する方法を指定した下位のクラスです。 次のストア、[リポジトリ パターン](http://deviq.com/repository-pattern/)し、永続化メカニズムと組み合わせると密接にします。 マネージャーは、つまり、(構成) を除くアプリケーション コードを変更せず、永続化メカニズムを置き換えることができます、ストアから切り離されます。

Web アプリと対話する方法、マネージャー ストアと、データ アクセス レイヤーの対話中の図は、次のとおりです。

![ASP.NET Core アプリケーション管理者 (たとえば、'UserManager'、'RoleManager') を使用します。 マネージャーは、ストア (たとえば、' UserStore') を伝える Entity Framework Core と同様に、ライブラリを使用してデータ ソースで動作します。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

カスタムの記憶域プロバイダーを作成するには、このデータ アクセス層 (緑、および灰色のボックス上のダイアグラムの) を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成します。 管理者またはそれら (青いボックス上記) と連携する、アプリ コードをカスタマイズする必要はありません。

新しいインスタンスを作成するときに`UserManager`または`RoleManager`するには、ユーザー クラスの種類を指定し、ストアのクラスのインスタンスを引数として渡します。 この方法では、ASP.NET Core にカスタマイズしたクラスを接続することができます。 

[新しい記憶域プロバイダーを使用するアプリを再構成](#reconfigure-app-to-use-new-storage-provider)のインスタンスを作成する方法を示します`UserManager`と`RoleManager`カスタマイズされたストアとします。

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core の Id データ型を格納します。

[ASP.NET Core Id](https://github.com/aspnet/identity)データ型は、次のセクションで詳しく説明します。

### <a name="users"></a>ユーザー

Web サイトの登録ユーザー。 [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)型を拡張または独自のカスタム型の例として使用される可能性があります。 独自のカスタム id の記憶域ソリューションを実装する特定の型から継承する必要はありません。

### <a name="user-claims"></a>ユーザーの信頼性情報

ステートメントのセット (または[クレーム](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) をユーザーの id を表すユーザーに関するします。 ロールで実現できるよりも、ユーザーの id の大きい式を有効にできます。

### <a name="user-logins"></a>ユーザーのログイン

外部認証プロバイダー (Facebook、Microsoft アカウントなど) に関する情報をユーザーにログインするときに使用します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>役割

サイトのグループを承認します。 ロールの Id とロール名 ("Admin"、"Employee"など) が含まれます。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>データ アクセス層

このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れていると仮定します。 このトピックは、リポジトリまたはデータ アクセス クラスを作成する方法の詳細を提供しませんASP.NET Core の Id を使用する場合は、設計に関する決定事項についていくつかの提案を提供します。

カスタマイズされたストア プロバイダーのデータ アクセス レイヤーをデザインするときは、多数の自由度があります。 アプリで使用する予定の機能の永続化メカニズムを作成する必要があるだけです。 たとえば、アプリでの役割を使用しない場合に、役割またはユーザー ロールの関連付けの記憶域を作成する必要はありません。 テクノロジと既存のインフラストラクチャには、構造体は ASP.NET Core Id の既定の実装と大きく異なることがある場合があります。 データ アクセス層は、ストレージの実装の構造を操作するためのロジックを提供します。

データ アクセス レイヤーでは、データ ソースへの ASP.NET Core Id からデータを保存するためのロジックを提供します。 カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。

### <a name="context-class"></a>Context クラス

永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。 いくつかのデータ クラスでは、このクラスは、依存関係の挿入によって提供される通常のインスタンスが必要です。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)です。

### <a name="user-storage"></a>ユーザーのストレージ

格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>ロールのストレージ

格納し、(ロール名など) のロール情報を取得します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims ストレージ

格納し、要求の種類と値の場合) などのユーザー要求の情報を取得します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins ストレージ

格納し、(外部認証プロバイダーの場合) などのユーザーのログイン情報を取得します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole 記憶域

格納し、どのユーザーに割り当てられたロールを取得します。 [例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**ヒント:**のみ、アプリで使用するクラスを実装します。

データ アクセス クラスでは、永続化メカニズムのデータ操作を実行するコードを提供します。 たとえば、カスタム プロバイダーにする必要がありますで新しいユーザーを作成するには、次のコード、*格納*クラス。

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

ユーザーを作成するための実装ロジックにあり、``_usersTable.CreateAsync``メソッドを次に示すです。

## <a name="customize-the-user-class"></a>ユーザー クラスをカスタマイズします。

記憶域プロバイダーを実装する場合と同じであるユーザー クラスを作成、 [ `IdentityUser`クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)です。

少なくとも、ユーザー クラスを含める必要があります、`Id`と`UserName`プロパティです。

`IdentityUser`クラス、プロパティを定義する、``UserManager``要求された操作の呼び出しを実行するとき。 既定の種類、`Id`プロパティは、文字列から継承することができますが、`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`し、別の種類を指定します。 フレームワークでは、データ型変換を処理するストレージの実装が必要です。

## <a name="customize-the-user-store"></a>ユーザーのストアをカスタマイズします。

作成、`UserStore`ユーザーに対するすべてのデータ操作のメソッドを提供するクラス。 このクラスは、 [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)クラスです。 `UserStore`クラスを実装`IUserStore<TUser>`と必要とする省略可能なインターフェイスです。 実装する省略可能なインターフェイスを選択すると、アプリで提供される機能に基づいています。

### <a name="optional-interfaces"></a>省略可能なインターフェイス

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore <!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

省略可能なインターフェイスを継承`IUserStore`です。 部分的に実装されているサンプルのユーザー ストアを確認できます[ここ](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)です。

内で、`UserStore`クラス、操作を実行するために作成したデータ アクセス クラスを使用します。 依存関係の挿入を使用して渡されます。 口ひげ実装では、使用して、SQL サーバーなどで、`UserStore`クラスには、`CreateAsync`メソッドのインスタンスを使用する`DapperUsersTable`新しいレコードを挿入します。

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>ユーザーのストアをカスタマイズするときに実装するインターフェイス

- **IUserStore**  
 [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1)インターフェイスは、のみのインターフェイスで、ユーザーのストアを実装する必要があります。 作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1)インターフェイスがユーザーの信頼性情報を有効にするために実装するメソッドを定義します。 追加、削除、およびユーザーの信頼性情報を取得するためのメソッドが含まれています。
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1)外部認証プロバイダーを有効にするために実装するメソッドを定義します。 追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1)インターフェイス ロールにユーザーをマップするために実装するメソッドを定義します。 追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)インターフェイス ハッシュされたパスワードを永続化するために実装するメソッドを定義します。 ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッド取得および設定のためのメソッドが含まれています。
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)インターフェイス セキュリティ スタンプを使用して、ユーザーのアカウント情報が変更されたかどうかを示すために実装するメソッドを定義します。 ユーザーをパスワードを変更または追加または削除のログイン時に、このスタンプが更新されます。 取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)インターフェイス 2 要素認証をサポートするために実装するメソッドを定義します。 取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)インターフェイスがユーザーの電話番号を格納するために実装するメソッドを定義します。 電話番号および電話番号が確認されているかどうか取得および設定のためのメソッドが含まれています。
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1)インターフェイスがユーザーの電子メール アドレスを格納するために実装するメソッドを定義します。 電子メール アドレスおよび電子メールが確認されているかどうか取得および設定のためのメソッドが含まれています。
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)インターフェイスがアカウントのロックに関する情報を格納するために実装するメソッドを定義します。 失敗したアクセス試行とロックアウトを追跡するためのメソッドが含まれています。
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)インターフェイスは、クエリ可能なユーザー ストアを提供するメンバーの実装を定義します。

インターフェイスを実装するだけのために必要なアプリでします。 例:

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

``Microsoft.AspNet.Identity.EntityFramework``名前空間の実装を含む、 [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、 [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)、および[IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)クラスです。 これらの機能を使用している場合は、これらのクラスの独自バージョンを作成し、アプリのプロパティを定義することがあります。 ただし、場合もありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。 代わりに、バックエンド ストア クラスは、データ ソースで直接これらの操作を実行できます。 たとえば、``UserStore.GetClaimsAsync``メソッドを呼び出すことができます、``userClaimTable.FindByUserId(user.Id)``メソッドでクエリを実行するテーブルの直接要求の一覧を返します。

## <a name="customize-the-role-class"></a>Role クラスをカスタマイズします。

役割の記憶域プロバイダーを実装する場合は、カスタム ロールの種類を作成できます。 特定のインターフェイスを実装していない必要がある必要があります、`Id`し、通常が、`Name`プロパティです。

ロールのクラスの例を次に示します。

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>ロール ストアをカスタマイズします。

作成することができます、``RoleStore``ロール上のすべてのデータ操作のメソッドを提供するクラス。 このクラスは、 [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)クラスです。 `RoleStore`クラスを実装する、``IRoleStore<TRole>``し、必要に応じて、``IQueryableRoleStore<TRole>``インターフェイスです。

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1)インターフェイスは、ロール ストアのクラスに実装するメソッドを定義します。 作成、更新、削除、およびロールを取得するためのメソッドが含まれています。
- **RoleStore&lt;TRole&gt;**  
 カスタマイズする`RoleStore`を実装するクラスを作成、`IRoleStore`インターフェイスです。 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>新しい記憶域プロバイダーを使用するアプリを再構成します。

記憶域プロバイダーを実装した後、それを使用するアプリを構成します。 アプリでは、既定のプロバイダーを使用する場合は、カスタム プロバイダーで置き換えます。

1. 削除、 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet パッケージです。
1. 記憶域プロバイダーが別のプロジェクトまたはパッケージが存在する場合への参照を追加します。
1. すべての参照を置き換える`Microsoft.AspNetCore.EntityFramework.Identity`を使用して、、記憶域プロバイダーの名前空間のステートメント。
1. ``ConfigureServices``メソッド、変更、`AddIdentity`カスタム型を使用する方法です。 この目的のための独自の拡張メソッドを作成することができます。 参照してください[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)例についてはします。
1. ロールを使用している場合は、更新、`RoleManager`を使用する、`RoleStore`クラスです。
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

- [ASP.NET Id のカスタムの記憶域プロバイダー](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Id](https://github.com/aspnet/identity) -このリポジトリには、ストア プロバイダーを管理するコミュニティへのリンクが含まれています。
