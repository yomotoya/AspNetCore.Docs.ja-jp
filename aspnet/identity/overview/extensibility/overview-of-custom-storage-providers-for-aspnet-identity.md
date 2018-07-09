---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: ASP.NET Identity のカスタム ストレージ プロバイダーの概要 |Microsoft Docs
author: tfitzmac
description: ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成して、アプリケーションを再動作せず、アプリケーションに組み込むことができます.
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 35c8986024279bf66f239dbd969fba5ca02f3d01
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831739"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>ASP.NET Identity のカスタム ストレージ プロバイダーの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成して、アプリケーションを再動作せず、アプリケーションに組み込むことができます。 このトピックでは、ASP.NET Identity のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。 独自の記憶域プロバイダーを作成するための重要な概念について説明しますが、カスタム ストレージ プロバイダーの実装の詳細なチュートリアルではありません。
> 
> カスタム ストレージ プロバイダーの実装の例は、次を参照してください。[カスタム MySQL ASP.NET Identity ストレージ プロバイダーを実装する](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)します。
> 
> このトピックでは、ASP.NET Identity 2.0 用に更新されました。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Visual Studio 2013 with Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>はじめに

既定では、ASP.NET Identity のシステムは、SQL Server データベースにユーザー情報を格納し、Entity Framework Code First を使用して、データベースを作成します。 多くのアプリケーションでは、この方法はも機能します。 ただし、別の種類の Azure Table Storage などの永続化メカニズムを使用することもできます。 または既定の実装よりも非常に異なる構造を持つデータベース テーブルを既に必要があります。 どちらの場合、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、アプリケーションにそのプロバイダーをプラグインできます。

既定では、Visual Studio 2013 のテンプレートの多くでは、ASP.NET Identity が含まれます。 ASP.NET identity で更新プログラムを入手できます[Microsoft AspNet Identity EntityFramework NuGet パッケージ](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)します。

このトピックには、次のセクションがあります。

- [アーキテクチャを理解します。](#architecture)
- [格納されているデータを理解します。](#data)
- [データ アクセス層を作成します。](#dal)
- [ユーザー クラスをカスタマイズします。](#user)
- [ユーザー ストアをカスタマイズします。](#userstore)
- [Role クラスをカスタマイズします。](#role)
- [ロール ストアをカスタマイズします。](#rolestore)
- [新しい記憶域プロバイダーを使用するアプリケーションを再構成します。](#reconfigure)
- [他のカスタム ストレージ プロバイダーの実装](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>アーキテクチャを理解します。

ASP.NET Identity は、マネージャーおよびストアと呼ばれるクラスで構成されます。 管理者は、アプリケーション開発者を使用して、ASP.NET Identity で、ユーザーの作成などの操作を実行する高度なクラスです。 ストアは、ユーザーとロールをなどのエンティティを永続化する方法を指定する低レベル クラスです。 ストアが、永続化メカニズムと密接に結び付けられるが、マネージャーから分離ストアはアプリケーション全体を中断せずに永続化メカニズムを置き換えることができます。

次の図は、web アプリケーションが、マネージャーと対話する方法と、ストアは、データ アクセス層と対話します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

ASP.NET Identity のカスタマイズされた記憶域プロバイダーを作成するには、このデータ アクセス層を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成する必要があります。 ユーザーが別のストレージ システムにデータが保存されるので、データの操作を実行する同じマネージャー Api を使用して続行できます。

ユーザー クラスの型を UserManager または RoleManager の新しいインスタンスを作成するときに指定するため、manager クラスをカスタマイズし、ストア クラスのインスタンスを引数として渡す必要はありません。 このアプローチでは、既存の構造に、カスタマイズしたクラスをプラグインすることができます。 セクションでは、カスタマイズされたストア クラスを UserManager や RoleManager をインスタンス化する方法について説明[新しい記憶域プロバイダーを使用するアプリケーションを再構成](#reconfigure)します。

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>格納されているデータを理解します。

カスタム ストレージ プロバイダーを実装するには ASP.NET の Id で使用されるデータの種類を理解して、アプリケーションに関連する機能を決定します。

| データ | 説明 |
| --- | --- |
| ユーザー | Web サイトの登録済みユーザー。 ユーザー Id とユーザー名が含まれています。 ユーザーは、サイトに固有の資格情報でログインしている場合にハッシュされたパスワードが含まれます (なく Facebook などの外部サイトからの資格情報を使用して)、およびセキュリティ スタンプをユーザーの資格情報で何かが変更されたかどうかを示します。 電話番号、失敗したログインを現在の数の 2 要素認証が有効になっているかどうか、また電子メール アドレスが含まれますかどうか、アカウントがロックされています。 |
| ユーザーの信頼性情報 | ユーザーの id を表すユーザーに関するステートメント (または要求) のセット。 ロールで実現できるよりも、ユーザーの id の大きい式を有効にすることができます。 |
| ユーザー ログイン | 外部認証プロバイダー (Facebook) などについては、ユーザーのログイン時に使用します。 |
| 役割 | サイトのグループを承認します。 ロールの Id とロール名 ("Admin"、"Employee"など) が含まれています。 |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>データ アクセス層を作成します。

このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れてを前提としています。 このトピックでは、リポジトリまたはデータ アクセス クラスを作成する方法の詳細については提供されません。代わりに、ASP.NET Identity で作業する場合にする必要があるの設計に関する決定事項についていくつかの候補を提供します。

ストア プロバイダーのカスタマイズされたリポジトリのデザイン時の自由度の多くがあります。 アプリケーションで使用する機能のリポジトリを作成する必要があるだけです。 たとえば、アプリケーションでロールを使用していない場合は、ロールまたはユーザー ロールのストレージを作成する必要はありません。 既存のインフラストラクチャ、テクノロジは ASP.NET Identity の既定の実装を非常に異なる構造を必要があります。 データ アクセス層では、リポジトリの構造を使用するロジックを提供します。

ASP.NET Identity 2.0 向けのデータ リポジトリの MySQL の実装を参照してください。 [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)します。

データ アクセス層では、ASP.NET Identity からのデータ ソースにデータを保存するためのロジックを提供します。 カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。

| クラス | 説明 | 例 |
| --- | --- | --- |
| コンテキスト | 永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。 このクラスは、データ アクセス層の中心となります。 その他のデータ クラスには、その操作を実行するには、このクラスのインスタンスが必要です。 このクラスのインスタンスで、ストア クラスが初期化されます。 | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| ユーザーのストレージ | 格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。 | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| ロールのストレージ | 格納し、(ロール名) などのロール情報を取得します。 | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims ストレージ | 格納し、(要求の種類と値の場合) などのユーザー要求の情報を取得します。 | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| UserLogins ストレージ | 格納し、(外部認証プロバイダーの場合) などのユーザー ログイン情報を取得します。 | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole 記憶域 | 格納し、どのロールに割り当てられているユーザーを取得します。 | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

ここでも、のみ、アプリケーションで使用するクラスを実装する必要があります。

データ アクセス クラスでは、特定の永続化メカニズムのデータの操作を実行するコードを提供します。 たとえば、MySQL の実装内で UserTable クラスにはユーザー データベースのテーブルに新しいレコードを挿入するメソッドが含まれます。 という名前の変数`_database`MySQLDatabase クラスのインスタンスです。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

データ アクセス クラスを作成した後に、データ アクセス層で特定のメソッドを呼び出すストア クラスを作成する必要があります。

<a id="user"></a>
## <a name="customize-the-user-class"></a>ユーザー クラスをカスタマイズします。

等価であるユーザー クラスを作成する必要があります、独自の記憶域プロバイダーを実装する場合、 [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx)クラス、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。

次の図は、このクラスで実装するインターフェイスおよび作成する必要がありますを IdentityUser クラスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)インターフェイスは、UserManager が操作を要求を実行するときに呼び出すしようとするプロパティを定義します。 インターフェイスには、2 つのプロパティ - Id とユーザー名が含まれています。 [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)インターフェイスでは、ジェネリックを通じてユーザーのキーの種類を指定できます。 **TKey**パラメーター。 Id プロパティの型では、TKey パラメーターの値と一致します。

Identity framework も用意されています。、 [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx)インターフェイス (ジェネリック パラメーター) がない場合、キーの文字列値を使用する場合。

IdentityUser クラスは IUser を実装し、任意の追加のプロパティまたは web サイトでユーザーのコンス トラクターが含まれています。 次の例では、キーの整数を使用する IdentityUser クラスを示します。 Id フィールドに設定されている**int**ジェネリック パラメーターの値と一致しません。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 完全な実装を参照してください。 [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs)します。 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>ユーザー ストアをカスタマイズします。

ユーザーに対するすべてのデータ操作のメソッドを提供する UserStore クラスを作成します。 このクラスは、 [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx)クラス、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。 実装する、UserStore クラスで、 [IUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)と省略可能なインターフェイスのいずれか。 実装する省略可能なインターフェイスを選択すると、アプリケーションで提供する機能に基づいています。

次の図は、作成する必要があります、UserStore クラスと関連するインターフェイスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Visual Studio での既定のプロジェクト テンプレートには、ユーザー ストアで実装される多くの省略可能なインターフェイスが想定するコードが含まれています。 カスタマイズされたユーザー ストアと、既定のテンプレートを使用する場合は、ユーザー ストアに省略可能なインターフェイスを実装することもいずれかまたは不要になった実装していないインターフェイスでメソッドを呼び出す、テンプレート コードを変更する必要があります。

 次の例では、シンプルなユーザー ストア クラスを示します。 **TUser**ジェネリック パラメーターには、通常、IdentityUser クラスを定義するには、ユーザー クラスの型。 **TKey**ジェネリック パラメーターは、ユーザー キーの型を受け取ります。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 この例では、パラメーターを受け取るコンス トラクターがという名前*データベース*ExampleDatabase 型ののみをデータ アクセス クラスを渡す方法の図に示します。 たとえば、このコンス トラクターは、MySQL の実装で MySQLDatabase 型のパラメーターを受け取ります。 

UserStore クラス内では、操作を実行するために作成したデータ アクセス クラスを使用します。 たとえば、MySQL の実装では、UserStore クラスと、UserTable のインスタンスを使用して新しいレコードを挿入する CreateAsync メソッドがあります。 **挿入**メソッドを**userTable**オブジェクトは、前のセクションで示したのと同じメソッドです。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>ユーザー ストアをカスタマイズする際に実装するインターフェイス

次のイメージには、各インターフェイスで定義されている機能についての詳細が表示されます。 すべての省略可能なインターフェイスは、IUserStore から継承します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  [IUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx)インターフェイスは、唯一のインターフェイスで、ユーザー ストアを実装する必要があります。 作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx)インターフェイス メソッドを定義するユーザーの信頼性情報を有効にするユーザー ストアで実装する必要があります。 メソッドまたは追加、削除、ユーザーの信頼性情報の取得が含まれています。
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx)メソッドを定義します外部認証プロバイダーを有効にするユーザー ストアで実装する必要があります。 追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。
- **IUserRoleStore**  
  [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)インターフェイス メソッドを定義するユーザーをロールにマップするユーザー ストアで実装する必要があります。 追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx)インターフェイスは、永続化する、ユーザー ストアに実装する必要がありますメソッドを定義します。 パスワードをハッシュします。 ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッドを取得および設定のためのメソッドが含まれています。
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx)インターフェイスは、ユーザーのアカウント情報が変更されたかどうかを示すためにセキュリティ スタンプを使用するユーザー ストアで実装する必要がありますメソッドを定義します. ユーザー、パスワードを変更または追加またはログインを削除します。 このタイムスタンプが更新されます。 取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx)インターフェイスは、2 要素認証が実装に実装する必要があるメソッドを定義します。 取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx)インターフェイスは、ユーザーの電話番号を格納するために実装する必要がありますメソッドを定義します。 取得および電話番号と電話番号が確認されているかどうかを設定するためのメソッドが含まれています。
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx)インターフェイスは、ユーザーのメール アドレスを格納するために実装する必要がありますメソッドを定義します。 取得、および電子メール アドレスと、電子メールが確認されているかどうかを設定するためのメソッドが含まれています。
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx)インターフェイスは、アカウントのロックに関する情報を格納するために実装する必要がありますメソッドを定義します。 失敗したアクセス試行の現在の数を取得、取得し設定を取得するアカウントをロックでき、数の増加に失敗した試行をロックアウトの終了日を設定するかどうかおよび試行失敗の回数をリセットするためのメソッドが含まれています。
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser、TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx)インターフェイスは、クエリ可能なユーザー ストアを提供するために実装する必要がありますメンバーを定義します。 クエリ可能なユーザーを保持するプロパティが含まれています。

  アプリケーション内で必要なインターフェイスを実装します。次のように、IUserClaimStore、IUserLoginStore、IUserRoleStore IUserPasswordStore、および IUserSecurityStampStore 次に示すインターフェイスです。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

(すべてのインターフェイスを含む) 完全な実装を参照してください。 [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs)します。

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、IdentityUserLogin、および IdentityUserRole

Microsoft.AspNet.Identity.EntityFramework 名前空間には実装が含まれています、 [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx)、 [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)、および[IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx)クラス。 これらの機能を使用している場合は、これらのクラスの独自のバージョンを作成し、アプリケーションのプロパティを定義することがあります。 ただし、場合がありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。 代わりに、バックエンド ストア クラスには、データ ソースで直接これらの操作を実行できます。 たとえば、UserStore.GetClaimsAsync() メソッドは、userClaimTable.FindByUserId(user. を呼び出すことができます。Id) でクエリを実行するをメソッドを直接テーブル要求の一覧を返します。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Role クラスをカスタマイズします。

等価であるロール クラスを作成する必要があります、独自の記憶域プロバイダーを実装する場合、 [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx)クラス、 [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)名前空間。

次の図は、このクラスで実装するインターフェイスおよび作成する必要がありますを IdentityRole クラスを示します。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)インターフェイスは、RoleManager が要求された操作を実行するときに呼び出すしようとするプロパティを定義します。 インターフェイスには、2 つのプロパティ - Id と名前が含まれています。 [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)インターフェイスでは、ジェネリックを経由して、ロールのキーの種類を指定できます。 **TKey**パラメーター。 Id プロパティの型では、TKey パラメーターの値と一致します。

Identity framework も用意されています。、 [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx)インターフェイス (ジェネリック パラメーター) がない場合、キーの文字列値を使用する場合。

次の例では、キーの整数を使用する IdentityRole クラスを示します。 Id フィールドは、ジェネリック パラメーターの値に一致するように int に設定されます。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 完全な実装を参照してください。 [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs)します。 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>ロール ストアをカスタマイズします。

ロールに対するすべてのデータ操作のメソッドを提供する RoleStore クラスを作成します。 このクラスは、 [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework 名前空間のクラス。 実装する、RoleStore クラスで、 [IRoleStore&lt;TRole、TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx)と、必要に応じて、 [IQueryableRoleStore&lt;TRole、TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx)インターフェイスです。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

次の例では、ロール ストア クラスを示します。 TRole のジェネリック パラメーターは、通常、定義した IdentityRole クラスであるロール クラスの型を取得します。 TKey のジェネリック パラメーターは、ロール、キーの種類を取得します。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx)インターフェイスは、ロール ストア クラスに実装するメソッドを定義します。 作成、更新、削除、およびロールを取得するためのメソッドが含まれています。
- **RoleStore&lt;TRole&gt;**  
  RoleStore をカスタマイズするには、IRoleStore インターフェイスを実装するクラスを作成します。 のみがある場合は、このクラスを実装するために、システムの役割を使用します。 という名前のパラメーターを受け取るコンス トラクター*データベース*ExampleDatabase 型ののみをデータ アクセス クラスを渡す方法の図に示します。 たとえば、このコンス トラクターは、MySQL の実装で MySQLDatabase 型のパラメーターを受け取ります。  
  
  完全な実装を参照してください。 [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs)します。

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>新しい記憶域プロバイダーを使用するアプリケーションを再構成します。

新しい記憶域プロバイダーを実装します。 ここで、この記憶域プロバイダーを使用するアプリケーションを構成する必要があります。 既定の記憶域プロバイダーがプロジェクトに含まれていた場合は、既定のプロバイダーを削除する必要があり、プロバイダーに置き換えます。

### <a name="replace-default-storage-provider-in-mvc-project"></a>MVC プロジェクトで既定の記憶域プロバイダーを置き換える

1. **NuGet パッケージの管理**ウィンドウで、アンインストール、 **Microsoft ASP.NET Identity EntityFramework**パッケージ。 インストール済みのパッケージで Identity.EntityFramework のでは、このパッケージを検索できます。  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Entity Framework をアンインストールするかどうかも要求されます。 必要はありませんが、アプリケーションの他の部分で、アンインストールすることができます。
2. Models フォルダーに IdentityModels.cs ファイルで削除またはコメント アウト、 **ApplicationUser**と**ApplicationDbContext**クラス。 MVC アプリケーションでは、IdentityModels.cs ファイル全体を削除できます。 Web フォーム アプリケーションで 2 つのクラスを削除しますが、IdentityModels.cs ファイル内にも配置するヘルパー クラスを保持するかどうかを確認します。
3. 別のプロジェクトで、記憶域プロバイダーが存在する場合、web アプリケーションへの参照を追加します。
4. すべての参照を置き換える`using Microsoft.AspNet.Identity.EntityFramework;`を使用すると、記憶域プロバイダーの名前空間のステートメント。
5. **Startup.Auth.cs**クラス、変更、 **ConfigureAuth**適切なコンテキストの 1 つのインスタンスを使用するメソッド。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. アプリで\_開始フォルダーを開く**IdentityConfig.cs**します。 ApplicationUserManager クラスでは、変更、**作成**カスタマイズされたユーザー ストアを使用するユーザーのマネージャーを返します。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. すべての参照を置き換える**ApplicationUser**で**IdentityUser**します。
8. 既定のプロジェクトには、一部のメンバーには、IUser インターフェイスで定義されていないユーザー クラスが含まれています電子メール、PasswordHash、GenerateUserIdentityAsync など。 ユーザー クラスがこれらのメンバーを持たない場合、それらを実装か、または、これらのメンバーを使用するコードを変更する必要があります。
9. RoleManager のすべてのインスタンスを作成した場合は、新しい RoleStore クラスを使用するためのコードを変更します。  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. 既定のプロジェクトは、キーの文字列値を持つユーザー クラスに適しています。 User クラスには、キー (整数) などのさまざまな型にある場合は、型を使用するプロジェクトを変更する必要があります。 参照してください[ASP.NET Identity でユーザーのプライマリ キーを変更する](change-primary-key-for-users-in-aspnet-identity.md)します。
11. 必要な場合は、Web.config ファイルに接続文字列を追加します。

<a id="other"></a>
## <a name="other-resources"></a>その他のリソース

- ブログ: [ASP.NET Identity の実装](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- チュートリアルと GIT のコード: [Simple.Data Asp.Net Id プロバイダー](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- チュートリアル:[基本の Id アカウントを設定し、外部の DB をポイントして](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)します。 によって[ @xivSolutions](https://twitter.com/xivSolutions)します。
- チュートリアル[: カスタム MySQL ASP.NET Identity ストレージ プロバイダーを実装します。](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent エンティティ](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)によって[SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) James Randall でします。
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)によって[ @stuartleeks](https://twitter.com/stuartleeks)します。
- [CouchDB/Daniel Wertheim によって Cloudant します。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- エラスティック検索[h: エラスティック Identity](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. によって
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely でします。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos でします。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)によって[ @tourismgeek](https://twitter.com/tourismgeek)します。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)によって[ILMServices](http://www.ilmservice.com/)します。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- EF を生成する T4 テンプレートの「最初にデータベース」のユーザー ストアのコード: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
