---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: ASP.NET Id のカスタムの記憶域プロバイダーの概要 |Microsoft ドキュメント
author: tfitzmac
description: ASP.NET Identity は、独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することにより、拡張可能なシステムがしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 06e3ad3b74bf94806f56da9f579255bf2917bc48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>ASP.NET Id のカスタムの記憶域プロバイダーの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することができます。 このトピックでは、ASP.NET Identity のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。 独自の記憶域プロバイダーを作成するための重要な概念に対応が、カスタムの記憶域プロバイダーの実装の詳細なチュートリアルではありません。
> 
> カスタムの記憶域プロバイダーの実装の例は、次を参照してください。[カスタム MySQL ASP.NET Identity 記憶域プロバイダーを実装する](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)です。
> 
> このトピックは、ASP.NET Identity 2.0 用に更新されました。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Visual Studio 2013 Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>はじめに

既定では、ASP.NET Identity システムは、SQL Server データベースにユーザー情報を格納し、Entity Framework Code First を使用して、データベースを作成します。 多くのアプリケーションでは、この方法でもです。 ただし、別の種類の Azure テーブル ストレージなどの永続化メカニズムを使用することができます。 または既定の実装よりも大幅に異なる構造を持つデータベース テーブルが既にことがあります。 どちらの場合、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、アプリケーションにそのプロバイダーを接続できます。

既定では、Visual Studio 2013 のテンプレートの多くでは、ASP.NET の Id が含まれます。 更新プログラムを取得するにはを介して ASP.NET Identity を[Microsoft AspNet Identity EntityFramework NuGet パッケージ](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)です。

このトピックには、次のセクションがあります。

- [アーキテクチャを理解します。](#architecture)
- [格納されているデータを理解します。](#data)
- [データ アクセス層を作成します。](#dal)
- [ユーザー クラスをカスタマイズします。](#user)
- [ユーザーのストアをカスタマイズします。](#userstore)
- [Role クラスをカスタマイズします。](#role)
- [ロール ストアをカスタマイズします。](#rolestore)
- [新しい記憶域プロバイダーを使用するアプリケーションを再構成します。](#reconfigure)
- [他のカスタムの記憶域プロバイダーの実装](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>アーキテクチャを理解します。

ASP.NET Identity は、マネージャーおよびストアと呼ばれるクラスで構成されます。 管理者は、ASP.NET Identity システムに、ユーザーを作成するなどの操作を実行するアプリケーション開発者が使用する高レベルのクラスです。 ストアは、ユーザーやロールなどのエンティティを永続化する方法を指定する下位のクラスです。 ストアが永続化メカニズムと組み合わせると密接がマネージャーは切り離されてストアからアプリケーション全体を中断することがなく、永続化メカニズムを置き換えることができますを意味します。

次の図は、web アプリケーションが、マネージャーとやり取りする方法と、ストアと、データ アクセス レイヤーの対話します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

ASP.NET Identity のカスタマイズされた記憶域プロバイダーを作成するには、このデータ アクセス層を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成する必要があります。 ユーザーが別の記憶域システムにデータが保存されるので、データ操作を実行する同じマネージャー Api を使用して続行できます。

ユーザー クラスの型を指定する UserManager または RoleManager の新しいインスタンスを作成するときにあるために、マネージャー クラスをカスタマイズし、ストアのクラスのインスタンスを引数として渡す必要はありません。 この方法では、既存の構造に、カスタマイズしたクラスを接続することができます。 UserManager と RoleManager セクションで、カスタマイズされたストア クラスのインスタンスを作成する方法がわかります[新しい記憶域プロバイダーを使用するアプリケーションを再構成](#reconfigure)です。

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>格納されているデータを理解します。

カスタムの記憶域プロバイダーを実装するのには ASP.NET の Id で使用されるデータの種類を理解して、アプリケーションに関連する機能を決定します。

| データ | 説明 |
| --- | --- |
| ユーザー | Web サイトの登録ユーザー。 ユーザー Id とユーザー名が含まれます。 ユーザーが、サイトに固有の資格情報でログオンする場合、ハッシュされたパスワードを含めることがなく Facebook などの外部のサイトからの資格情報を使用して)、およびユーザーの資格情報で変更された内容かどうかを示すためにセキュリティ スタンプします。 電話番号、現在の数が失敗したログインで 2 要素認証が有効になっているかどうか、電子メール アドレスが含まれますも、アカウントがロックされているかどうか。 |
| ユーザーの信頼性情報 | ユーザーの id を表すユーザーに関するステートメント (または要求) のセット。 ロールで実現できるよりも、ユーザーの id の大きい式を有効にできます。 |
| ユーザーのログイン | (Facebook) などの外部認証プロバイダーについては、ユーザーのログイン時に使用します。 |
| 役割 | サイトのグループを承認します。 ロールの Id とロール名 ("Admin"、"Employee"など) が含まれます。 |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>データ アクセス層を作成します。

このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れていると仮定します。 このトピックでは、リポジトリまたはデータ アクセス クラスを作成する方法の詳細については紹介しません代わりに、ASP.NET の Id を使用するときにする必要があるの設計に関する決定事項についていくつかの方法を示します。

カスタマイズ用のリポジトリを設計する際の自由ストア プロバイダーの余裕があります。 アプリケーションで使用する予定の機能のリポジトリを作成する必要があるだけです。 たとえば、アプリケーションでロールを使用しない場合は、ロールまたはユーザー ロール用のストレージを作成する必要はありません。 テクノロジと既存のインフラストラクチャには、構造体は ASP.NET Identity の既定の実装と大きく異なることがある場合があります。 データ アクセス層、リポジトリの構造を操作するためのロジックを提供します。

ASP.NET Identity 2.0 のデータのリポジトリの MySQL 実装を参照してください。 [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)です。

データ アクセス層では、ASP.NET Identity から、データ ソースにデータを保存するためのロジックを提供します。 カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。

| クラス | 説明 | 例 |
| --- | --- | --- |
| コンテキスト | 永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。 このクラスは、データ アクセス レイヤーの中心です。 その他のデータ クラスには、その操作を実行するには、このクラスのインスタンスが必要です。 このクラスのインスタンスと、ストア クラスが初期化されます。 | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| ユーザーのストレージ | 格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。 | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| ロールのストレージ | 格納し、(ロール名など) のロール情報を取得します。 | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims ストレージ | 格納し、要求の種類と値の場合) などのユーザー要求の情報を取得します。 | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| UserLogins ストレージ | 格納し、(外部認証プロバイダーの場合) などのユーザーのログイン情報を取得します。 | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole 記憶域 | 格納し、ユーザーに割り当てられているロールを取得します。 | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

再び、のみ、アプリケーションで使用するクラスを実装する必要があります。

データ アクセス クラスでは、特定の永続化メカニズムのデータ操作を実行するコードを提供します。 たとえば、MySQL 実装内で UserTable クラスには、ユーザー データベースのテーブルに新しいレコードを挿入するメソッドが含まれます。 という名前の変数`_database`MySQLDatabase クラスのインスタンスです。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

データ アクセス クラスを作成した後に、データ アクセス レイヤーで特定のメソッドを呼び出すストア クラスを作成する必要があります。

<a id="user"></a>
## <a name="customize-the-user-class"></a>ユーザー クラスをカスタマイズします。

等価であるユーザー クラスを作成する必要があります、独自の記憶域プロバイダーを実装する場合、 [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx)クラス内で、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。

次の図は、このクラスで実装するインターフェイスおよび作成する必要がありますを IdentityUser クラスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)インターフェイスは UserManager が操作を要求を実行するときに呼び出すしようとするプロパティを定義します。 インターフェイスには、次の 2 つのプロパティ - Id とユーザー名が含まれています。 [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)インターフェイスでは、ジェネリックを通じてユーザーのキーの種類を指定することができます**TKey**パラメーター。 Id プロパティの型では、TKey パラメーターの値と一致します。

Identity framework も用意されています、 [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx)インターフェイス (ジェネリック パラメーター) がない場合、キーの文字列値を使用する場合。

IdentityUser クラスは、IUser を実装し、任意の追加のプロパティまたは web サイトでユーザーのコンス トラクターが含まれています。 次の例では、キーの整数を使用する IdentityUser クラスを示します。 Id フィールドに設定されている**int**ジェネリック パラメーターの値と一致しません。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 完全な実装では、次を参照してください。 [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs)です。 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>ユーザーのストアをカスタマイズします。

ユーザーに対するすべてのデータ操作のメソッドを提供する UserStore クラスを作成します。 このクラスは、 [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx)クラス内で、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。 実装する、UserStore クラスで、 [IUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)と省略可能なインターフェイスのいずれか。 実装する省略可能なインターフェイスを選択すると、アプリケーションに提供する機能に基づいています。

次の図は、作成する必要があります、UserStore クラスと関連するインターフェイスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Visual Studio での既定のプロジェクト テンプレートには、ユーザー ストアに多くの省略可能なインターフェイス実装されている前提としているコードが含まれています。 カスタマイズされたユーザー ストアで、既定のテンプレートを使用している場合、ユーザー ストアに省略可能なインターフェイスを実装するかにメソッドを実装していないインターフェイスの呼び出しは不要になったテンプレート コードを変更します。

 次の例では、単純なユーザー ストアのクラスを示します。 **TUser**ジェネリック パラメーターには、通常は定義した IdentityUser クラスであるユーザー クラスの型。 **TKey**ジェネリック パラメーターは、ユーザー キーの種類を取得します。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 この例では、パラメーターを受け取るコンス トラクターが名前付き*データベース*ExampleDatabase は、データ アクセス クラスに渡す方法の具体的な型のです。 たとえば、このコンス トラクターは、実装では、MySQL、MySQLDatabase 型のパラメーターを受け取ります。 

UserStore クラス内では、操作を実行するために作成したデータ アクセス クラスを使用します。 たとえば、実装では、MySQL、UserStore クラスを UserTable のインスタンスを使用して、新しいレコードを挿入する CreateAsync メソッドにあります。 **挿入**メソッドを**userTable**オブジェクトは、前のセクションで示したのと同じ方法です。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>ユーザーのストアをカスタマイズするときに実装するインターフェイス

次のイメージは、各インターフェイスで定義されている機能の詳細を示します。 IUserStore からすべての省略可能なインターフェイスを継承します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  [IUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx)インターフェイスは、唯一のインターフェイス、ユーザーのストアで実装する必要があります。 作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx)インターフェイス メソッドを定義するユーザーの信頼性情報を有効にする、ユーザーのストアで実装する必要があります。 メソッドまたは追加、削除および取得するユーザーの信頼性情報が含まれています。
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx)メソッドを定義します外部認証プロバイダーを有効にする、ユーザーのストアで実装する必要があります。 追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。
- **IUserRoleStore**  
  [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)インターフェイス メソッドを定義するロールにユーザーをマップするユーザー ストアで実装する必要があります。 追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx)インターフェイス定義を保持する、ユーザー ストアに実装する必要がありますメソッドのパスワードをハッシュします。 ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッド取得および設定のためのメソッドが含まれています。
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx)インターフェイス メソッドを定義しますセキュリティ スタンプを使用して、ユーザーのアカウント情報が変更されたかどうかを示すために、ユーザー ストアに実装する必要があります. ユーザーをパスワードを変更または追加または削除のログイン時に、このスタンプが更新されます。 取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx)インターフェイス 2 要素認証が実装に実装する必要があるメソッドを定義します。 取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx)インターフェイスがユーザーの電話番号を格納するために実装する必要がありますメソッドを定義します。 電話番号および電話番号が確認されているかどうか取得および設定のためのメソッドが含まれています。
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx)インターフェイスがユーザーの電子メール アドレスを格納するために実装する必要がありますメソッドを定義します。 電子メール アドレスおよび電子メールが確認されているかどうか取得および設定のためのメソッドが含まれています。
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx)インターフェイスがアカウントのロックに関する情報を格納するために実装する必要がありますメソッドを定義します。 失敗したアクセス試行の現在の数を取得、取得し設定を取得する、アカウントをロックすることができ、試行に失敗しましたの番号のインクリメント ロックアウトの終了日を設定するかどうかおよび失敗した回数をリセットするためのメソッドが含まれています。
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx)インターフェイス メンバーを定義しますクエリ可能なユーザー ストアを提供するために実装する必要があります。 クエリ可能なユーザーを保持するプロパティが含まれています。

  アプリケーション内では、必要なインターフェイスを実装します。など、IUserClaimStore、IUserLoginStore、IUserRoleStore、IUserPasswordStore、および IUserSecurityStampStore は、次に示すようにインターフェイスです。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

(すべてのインターフェイスを含む)、完全な実装を参照してください。 [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs)です。

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、IdentityUserLogin、および IdentityUserRole

Microsoft.AspNet.Identity.EntityFramework 名前空間の実装を含む、 [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx)、 [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)、および[IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx)クラス。 これらの機能を使用している場合は、これらのクラスの独自バージョンを作成して、アプリケーションのプロパティを定義することがあります。 ただし、場合もありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。 代わりに、バックエンド ストア クラスは、データ ソースで直接これらの操作を実行できます。 たとえば、UserStore.GetClaimsAsync() メソッドは、userClaimTable.FindByUserId(user. を呼び出すことができます。Id) でクエリを実行するをメソッドを直接の表に信頼性情報の一覧を返します。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Role クラスをカスタマイズします。

等価であるロール クラスを作成する必要があります、独自の記憶域プロバイダーを実装する場合、 [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx)クラス内で、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。

次の図は、このクラスで実装するインターフェイスおよび作成する必要がありますを IdentityRole クラスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)インターフェイスは、RoleManager が操作を要求を実行するときに呼び出すしようとするプロパティを定義します。 インターフェイスには、次の 2 つのプロパティ - Id と名前が含まれています。 [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)インターフェイスでは、ジェネリックを経由して、ロールのキーの種類を指定することができます**TKey**パラメーター。 Id プロパティの型では、TKey パラメーターの値と一致します。

Identity framework も用意されています、 [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx)インターフェイス (ジェネリック パラメーター) がない場合、キーの文字列値を使用する場合。

次の例では、キーの整数を使用する IdentityRole クラスを示します。 Id フィールドは、ジェネリック パラメーターの値に一致する int に設定されます。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 完全な実装では、次を参照してください。 [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs)です。 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>ロール ストアをカスタマイズします。

ロールでのすべてのデータ操作のメソッドを提供する RoleStore クラスを作成します。 このクラスは、 [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework 名前空間のクラスです。 実装する、RoleStore クラスで、 [IRoleStore&lt;TRole、TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx)し、必要に応じて、 [IQueryableRoleStore&lt;TRole、TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx)インターフェイスです。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

次の例では、ロール ストアのクラスを示します。 TRole ジェネリック パラメーターでは、通常は定義した IdentityRole クラスであるロール クラスの型を取得します。 TKey ジェネリック パラメーターでは、ロール、キーの種類を取得します。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx)インターフェイスは、ロール ストアのクラスに実装するメソッドを定義します。 作成、更新、削除、およびロールを取得するためのメソッドが含まれています。
- **RoleStore&lt;TRole&gt;**  
  RoleStore をカスタマイズするには、IRoleStore インターフェイスを実装するクラスを作成します。 のみがある場合は、このクラスを実装する、システムの役割を使用します。 という名前のパラメーターを受け取るコンス トラクター*データベース*ExampleDatabase は、データ アクセス クラスに渡す方法の具体的な型のです。 たとえば、このコンス トラクターは、実装では、MySQL、MySQLDatabase 型のパラメーターを受け取ります。  
  
  完全な実装では、次を参照してください。 [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs)です。

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>新しい記憶域プロバイダーを使用するアプリケーションを再構成します。

新しい記憶域プロバイダーを実装しています。 ここで、この記憶域プロバイダーを使用するようアプリケーションを構成する必要があります。 既定の記憶域プロバイダーがプロジェクトに含まれていた場合、既定のプロバイダーを削除と、プロバイダーと置換を行う必要があります。

### <a name="replace-default-storage-provider-in-mvc-project"></a>MVC プロジェクトで既定の記憶域プロバイダーを置き換える

1. **NuGet パッケージの管理**ウィンドウで、アンインストール、 **Microsoft ASP.NET Identity EntityFramework**パッケージです。 このパッケージは、Identity.EntityFramework のインストール済みのパッケージ内で検索できます。  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Entity Framework をアンインストールするかどうかも求められますされます。 必要はありませんが、アプリケーションの他の部分で、アンインストールすることができます。
2. Models フォルダー IdentityModels.cs ファイルで削除するか、コメント アウト、 **ApplicationUser**と**ApplicationDbContext**クラスです。 MVC アプリケーションでは、IdentityModels.cs ファイル全体を削除できます。 Web フォーム アプリケーションで 2 つのクラスを削除しますが、IdentityModels.cs ファイルにも配置されているヘルパー クラスを保持するかどうかを確認します。
3. 別のプロジェクトで、記憶域プロバイダーが存在する場合は、web アプリケーションでへの参照を追加します。
4. すべての参照を置き換える`using Microsoft.AspNet.Identity.EntityFramework;`を使用して、、記憶域プロバイダーの名前空間のステートメント。
5. **Startup.Auth.cs**クラス、変更、 **ConfigureAuth**適切なコンテキストの 1 つのインスタンスを使用する方法です。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. アプリで\_開始フォルダーを開き、 **IdentityConfig.cs**です。 ApplicationUserManager クラスでは、変更、**作成**カスタマイズされたユーザー ストアを使用しているユーザーのマネージャーを返します。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. すべての参照を置き換える**ApplicationUser**で**IdentityUser**です。
8. 既定のプロジェクトには、一部のメンバーには、IUser インターフェイスで定義されていないユーザー クラスが含まれています電子メール、PasswordHash、GenerateUserIdentityAsync など。 User クラスには、これらのメンバーがない場合、それらを実装かこれらのメンバーを使用するコードを変更します。
9. RoleManager のすべてのインスタンスを作成した場合は、新しい RoleStore クラスを使用するには、そのコードを変更します。  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. 既定のプロジェクトは、キーの文字列値を持つユーザーのクラスに適しています。 User クラスに、キー (整数) などの別の種類がある場合、型を使用するプロジェクトを変更する必要があります。 参照してください[ASP.NET Identity のユーザーのプライマリ キーを変更する](change-primary-key-for-users-in-aspnet-identity.md)です。
11. 必要な場合は、Web.config ファイルに接続文字列を追加します。

<a id="other"></a>
## <a name="other-resources"></a>その他のリソース

- ブログ: [ASP.NET Identity の実装](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- チュートリアルおよび GIT のコード: [Simple.Data Asp.Net Id プロバイダー](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- チュートリアル:[基本の Id アカウントを設定して、それらを外部データベースを指して](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)です。 によって[ @xivSolutions](https://twitter.com/xivSolutions)です。
- チュートリアル[: カスタム MySQL ASP.NET Identity の記憶域プロバイダーの実装](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent エンティティ](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)によって[SoftFluent](http://www.softfluent.com/)
- [Azure テーブル ストレージ](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/)James 部長、Randall でします。
- Azure テーブル ストレージ: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)によって[ @stuartleeks](https://twitter.com/stuartleeks)です。
- [CouchDB/Daniel Wertheim によって Cloudant です。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- 弾力性検索[h: Elastic Identity](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. によって
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely でします。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos でします。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)によって[ @tourismgeek](https://twitter.com/tourismgeek)です。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)によって[ILMServices](http://www.ilmservice.com/)です。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- 「データベースの最初」のユーザー ストアの EF を生成する、T4 テンプレートのコード: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
