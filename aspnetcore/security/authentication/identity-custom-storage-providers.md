---
title: ASP.NET Core Identity 用のカスタム ストレージ プロバイダー
author: ardalis
description: ASP.NET Core Identity 用のカスタム ストレージ プロバイダーを構成する方法について説明します。
ms.author: riande
ms.date: 05/24/2017
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 4b210a52ae9761bb838dd5611e86ce8f71345499
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835673"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="fce44-103">ASP.NET Core Identity 用のカスタム ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fce44-103">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="fce44-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fce44-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fce44-105">ASP.NET Core Identity は、拡張可能なシステム カスタム ストレージ プロバイダーを作成し、アプリに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="fce44-105">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="fce44-106">このトピックでは、ASP.NET Core Identity 用のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fce44-106">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="fce44-107">独自の記憶域プロバイダーを作成するための重要な概念について説明しますが、詳細なチュートリアルはありません。</span><span class="sxs-lookup"><span data-stu-id="fce44-107">It covers the important concepts for creating your own storage provider, but isn't a step-by-step walkthrough.</span></span>

<span data-ttu-id="fce44-108">[GitHub のサンプルを表示またはダウンロードしてください](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。</span><span class="sxs-lookup"><span data-stu-id="fce44-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="fce44-109">はじめに</span><span class="sxs-lookup"><span data-stu-id="fce44-109">Introduction</span></span>

<span data-ttu-id="fce44-110">既定では、ASP.NET Core Identity システムは、Entity Framework Core を使用して SQL Server データベースにユーザー情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="fce44-110">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="fce44-111">多くのアプリでは、この方法はも機能します。</span><span class="sxs-lookup"><span data-stu-id="fce44-111">For many apps, this approach works well.</span></span> <span data-ttu-id="fce44-112">ただし、別の永続化メカニズムまたはデータ スキーマを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="fce44-112">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="fce44-113">例えば:</span><span class="sxs-lookup"><span data-stu-id="fce44-113">For example:</span></span>

* <span data-ttu-id="fce44-114">使用する[Azure Table Storage](https://docs.microsoft.com/azure/storage/)または別のデータ ストア。</span><span class="sxs-lookup"><span data-stu-id="fce44-114">You use [Azure Table Storage](https://docs.microsoft.com/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="fce44-115">データベースのテーブルでは、さまざまな構造があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-115">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="fce44-116">など、さまざまなデータ アクセス手法を使用したい場合があります[Dapper](https://github.com/StackExchange/Dapper)します。</span><span class="sxs-lookup"><span data-stu-id="fce44-116">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="fce44-117">、このような場合の各に、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、そのプロバイダーをアプリにプラグインできます。</span><span class="sxs-lookup"><span data-stu-id="fce44-117">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="fce44-118">ASP.NET Core Identity は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="fce44-118">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="fce44-119">.NET Core CLI を使用する場合は、追加`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="fce44-119">When using the .NET Core CLI, add `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="fce44-120">ASP.NET Core Identity アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="fce44-120">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="fce44-121">ASP.NET Core Identity は、マネージャーおよびストアと呼ばれるクラスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="fce44-121">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="fce44-122">*マネージャー*は、アプリの開発者を使用して Identity ユーザーの作成などの操作を実行する高度なクラスです。</span><span class="sxs-lookup"><span data-stu-id="fce44-122">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="fce44-123">*ストア*は低レベル クラスなど、ユーザーとロールをエンティティの保存方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="fce44-123">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="fce44-124">次のストア、[リポジトリ パターン](xref:fundamentals/repository-pattern)永続化メカニズムと密接に結び付けられているとします。</span><span class="sxs-lookup"><span data-stu-id="fce44-124">Stores follow the [repository pattern](xref:fundamentals/repository-pattern) and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="fce44-125">管理者は、つまり、(構成) を除く、アプリケーション コードを変更することがなく、永続化メカニズムを置き換えることができます、ストアから切り離されます。</span><span class="sxs-lookup"><span data-stu-id="fce44-125">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="fce44-126">Web アプリと対話する方法、マネージャー ストアとデータ アクセス層の対話中に次の図を示しています。</span><span class="sxs-lookup"><span data-stu-id="fce44-126">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core アプリは、管理者 (たとえば、'UserManager'、'RoleManager') で機能します。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="fce44-129">カスタム ストレージ プロバイダーを作成するには、このデータ アクセス層 (緑色と灰色のボックスで、上記の図) を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="fce44-129">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="fce44-130">マネージャーは、またはそれら (上記青のボックス) と連携するアプリ コードをカスタマイズする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fce44-130">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="fce44-131">新しいインスタンスを作成するときに`UserManager`または`RoleManager`ユーザー クラスの型を指定し、ストア クラスのインスタンスを引数として渡します。</span><span class="sxs-lookup"><span data-stu-id="fce44-131">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="fce44-132">このアプローチでは、ASP.NET Core に、カスタマイズしたクラスをプラグインすることができます。</span><span class="sxs-lookup"><span data-stu-id="fce44-132">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="fce44-133">[新しい記憶域プロバイダーを使用するアプリを再構成](#reconfigure-app-to-use-a-new-storage-provider)をインスタンス化する方法を示します`UserManager`と`RoleManager`カスタマイズされたストアとします。</span><span class="sxs-lookup"><span data-stu-id="fce44-133">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-a-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="fce44-134">ASP.NET Core Identity データ型を格納します。</span><span class="sxs-lookup"><span data-stu-id="fce44-134">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="fce44-135">[ASP.NET Core Identity](https://github.com/aspnet/identity)データ型は、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="fce44-135">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="fce44-136">ユーザー</span><span class="sxs-lookup"><span data-stu-id="fce44-136">Users</span></span>

<span data-ttu-id="fce44-137">Web サイトの登録済みユーザー。</span><span class="sxs-lookup"><span data-stu-id="fce44-137">Registered users of your web site.</span></span> <span data-ttu-id="fce44-138">[IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)型を拡張または独自のカスタム型の例として使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-138">The [IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="fce44-139">カスタム id ストレージ ソリューションを実装する特定の型から継承する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fce44-139">You don't need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="fce44-140">ユーザーの信頼性情報</span><span class="sxs-lookup"><span data-stu-id="fce44-140">User Claims</span></span>

<span data-ttu-id="fce44-141">ステートメントのセット (または[クレーム](/dotnet/api/system.security.claims.claim)) について、ユーザーの id を表すユーザー。</span><span class="sxs-lookup"><span data-stu-id="fce44-141">A set of statements (or [Claims](/dotnet/api/system.security.claims.claim)) about the user that represent the user's identity.</span></span> <span data-ttu-id="fce44-142">ロールで実現できるよりも、ユーザーの id の大きい式を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="fce44-142">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="fce44-143">ユーザー ログイン</span><span class="sxs-lookup"><span data-stu-id="fce44-143">User Logins</span></span>

<span data-ttu-id="fce44-144">外部認証プロバイダー (Facebook、Microsoft アカウントなど) については、ユーザーのログイン時に使用します。</span><span class="sxs-lookup"><span data-stu-id="fce44-144">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="fce44-145">例</span><span class="sxs-lookup"><span data-stu-id="fce44-145">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="fce44-146">役割</span><span class="sxs-lookup"><span data-stu-id="fce44-146">Roles</span></span>

<span data-ttu-id="fce44-147">サイトのグループを承認します。</span><span class="sxs-lookup"><span data-stu-id="fce44-147">Authorization groups for your site.</span></span> <span data-ttu-id="fce44-148">ロールの Id とロール名 ("Admin"、"Employee"など) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-148">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="fce44-149">例</span><span class="sxs-lookup"><span data-stu-id="fce44-149">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="fce44-150">データ アクセス層</span><span class="sxs-lookup"><span data-stu-id="fce44-150">The data access layer</span></span>

<span data-ttu-id="fce44-151">このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れてを前提としています。</span><span class="sxs-lookup"><span data-stu-id="fce44-151">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="fce44-152">このトピックでは、リポジトリまたはデータ アクセス クラスを作成する方法についての詳細を提供しませんASP.NET Core Identity を使用する場合は、設計に関する決定事項についていくつかの候補を提供します。</span><span class="sxs-lookup"><span data-stu-id="fce44-152">This topic doesn't provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="fce44-153">カスタマイズされたストア プロバイダーのデータ アクセス層を設計するときにあまり自由度があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-153">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="fce44-154">のみ、アプリで使用する予定の機能を永続化メカニズムを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-154">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="fce44-155">たとえば、アプリでのロールを使用していない場合は、役割またはユーザー ロールの関連付けのストレージを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fce44-155">For example, if you are not using roles in your app, you don't need to create storage for roles or user role associations.</span></span> <span data-ttu-id="fce44-156">既存のインフラストラクチャ、テクノロジは ASP.NET Core Identity の既定の実装を非常に異なる構造を必要があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-156">Your technology and existing infrastructure may require a structure that's very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="fce44-157">データ アクセス層は、ストレージの実装の構造を使用するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="fce44-157">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="fce44-158">データ アクセス層は、ASP.NET Core Identity からデータ ソースにデータを保存するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="fce44-158">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="fce44-159">カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fce44-159">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="fce44-160">Context クラス</span><span class="sxs-lookup"><span data-stu-id="fce44-160">Context class</span></span>

<span data-ttu-id="fce44-161">永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="fce44-161">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="fce44-162">いくつかのデータ クラスには、このクラスは、依存関係の挿入によって提供される通常のインスタンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="fce44-162">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="fce44-163">[例](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)します。</span><span class="sxs-lookup"><span data-stu-id="fce44-163">[Example](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="fce44-164">ユーザーのストレージ</span><span class="sxs-lookup"><span data-stu-id="fce44-164">User Storage</span></span>

<span data-ttu-id="fce44-165">格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fce44-165">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="fce44-166">例</span><span class="sxs-lookup"><span data-stu-id="fce44-166">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="fce44-167">ロールのストレージ</span><span class="sxs-lookup"><span data-stu-id="fce44-167">Role Storage</span></span>

<span data-ttu-id="fce44-168">格納し、(ロール名) などのロール情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fce44-168">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="fce44-169">例</span><span class="sxs-lookup"><span data-stu-id="fce44-169">Example</span></span>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="fce44-170">UserClaims ストレージ</span><span class="sxs-lookup"><span data-stu-id="fce44-170">UserClaims Storage</span></span>

<span data-ttu-id="fce44-171">格納し、(要求の種類と値の場合) などのユーザー要求の情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fce44-171">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="fce44-172">例</span><span class="sxs-lookup"><span data-stu-id="fce44-172">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="fce44-173">UserLogins ストレージ</span><span class="sxs-lookup"><span data-stu-id="fce44-173">UserLogins Storage</span></span>

<span data-ttu-id="fce44-174">格納し、(外部認証プロバイダーの場合) などのユーザー ログイン情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fce44-174">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="fce44-175">例</span><span class="sxs-lookup"><span data-stu-id="fce44-175">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="fce44-176">UserRole 記憶域</span><span class="sxs-lookup"><span data-stu-id="fce44-176">UserRole Storage</span></span>

<span data-ttu-id="fce44-177">格納し、どのユーザーに割り当てられたロールを取得します。</span><span class="sxs-lookup"><span data-stu-id="fce44-177">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="fce44-178">例</span><span class="sxs-lookup"><span data-stu-id="fce44-178">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="fce44-179">**ヒント:** のみ、アプリで使用するクラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="fce44-179">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="fce44-180">データ アクセス クラスで、永続化メカニズムのデータの操作を実行するコードを提供します。</span><span class="sxs-lookup"><span data-stu-id="fce44-180">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="fce44-181">たとえば、カスタム プロバイダーでは、内にある必要がありますで新しいユーザーを作成する次のコード、*格納*クラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-181">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="fce44-182">ユーザーを作成するための実装ロジックは、`_usersTable.CreateAsync`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="fce44-182">The implementation logic for creating the user is in the `_usersTable.CreateAsync` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="fce44-183">ユーザー クラスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="fce44-183">Customize the user class</span></span>

<span data-ttu-id="fce44-184">記憶域プロバイダーを実装する場合と同じであるユーザー クラスを作成、 [ `IdentityUser`クラス](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)します。</span><span class="sxs-lookup"><span data-stu-id="fce44-184">When implementing a storage provider, create a user class which is equivalent to the [`IdentityUser` class](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="fce44-185">少なくとも、ユーザー クラスを含める必要があります、`Id`と`UserName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fce44-185">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="fce44-186">`IdentityUser`クラス、プロパティを定義する、`UserManager`要求された操作の呼び出しを実行する場合。</span><span class="sxs-lookup"><span data-stu-id="fce44-186">The `IdentityUser` class defines the properties that the `UserManager` calls when performing requested operations.</span></span> <span data-ttu-id="fce44-187">既定の型、`Id`プロパティは、文字列が継承できる`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`し、別の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="fce44-187">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="fce44-188">フレームワークには、データ型変換を処理するために、ストレージの実装が必要です。</span><span class="sxs-lookup"><span data-stu-id="fce44-188">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="fce44-189">ユーザー ストアをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="fce44-189">Customize the user store</span></span>

<span data-ttu-id="fce44-190">作成、`UserStore`ユーザーに対するすべてのデータ操作のメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-190">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="fce44-191">このクラスは、 [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)クラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-191">This class is equivalent to the [UserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="fce44-192">`UserStore`クラスを実装`IUserStore<TUser>`と必要とする省略可能なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="fce44-192">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="fce44-193">実装する省略可能なインターフェイスを選択すると、アプリで提供される機能に基づいています。</span><span class="sxs-lookup"><span data-stu-id="fce44-193">You select which optional interfaces to implement based on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="fce44-194">省略可能なインターフェイス</span><span class="sxs-lookup"><span data-stu-id="fce44-194">Optional interfaces</span></span>

* [<span data-ttu-id="fce44-195">IUserRoleStore</span><span class="sxs-lookup"><span data-stu-id="fce44-195">IUserRoleStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [<span data-ttu-id="fce44-196">IUserClaimStore</span><span class="sxs-lookup"><span data-stu-id="fce44-196">IUserClaimStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [<span data-ttu-id="fce44-197">IUserPasswordStore</span><span class="sxs-lookup"><span data-stu-id="fce44-197">IUserPasswordStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [<span data-ttu-id="fce44-198">IUserSecurityStampStore</span><span class="sxs-lookup"><span data-stu-id="fce44-198">IUserSecurityStampStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [<span data-ttu-id="fce44-199">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="fce44-199">IUserEmailStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [<span data-ttu-id="fce44-200">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="fce44-200">IPhoneNumberStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [<span data-ttu-id="fce44-201">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="fce44-201">IQueryableUserStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [<span data-ttu-id="fce44-202">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="fce44-202">IUserLoginStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [<span data-ttu-id="fce44-203">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="fce44-203">IUserTwoFactorStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [<span data-ttu-id="fce44-204">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="fce44-204">IUserLockoutStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

<span data-ttu-id="fce44-205">省略可能なインターフェイスを継承`IUserStore<TUser>`します。</span><span class="sxs-lookup"><span data-stu-id="fce44-205">The optional interfaces inherit from `IUserStore<TUser>`.</span></span> <span data-ttu-id="fce44-206">格納部分的に実装されているサンプル ユーザーを確認できます、[サンプル アプリ](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)します。</span><span class="sxs-lookup"><span data-stu-id="fce44-206">You can see a partially implemented sample user store in the [sample app](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="fce44-207">内で、`UserStore`クラス、操作を実行するために作成したデータ アクセス クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="fce44-207">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="fce44-208">依存関係の挿入を使用して渡されます。</span><span class="sxs-lookup"><span data-stu-id="fce44-208">These are passed in using dependency injection.</span></span> <span data-ttu-id="fce44-209">たとえば、Dapper の実装を含む SQL Server で、`UserStore`クラスには、`CreateAsync`メソッドのインスタンスを使用している`DapperUsersTable`新しいレコードを挿入します。</span><span class="sxs-lookup"><span data-stu-id="fce44-209">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="fce44-210">ユーザー ストアをカスタマイズする際に実装するインターフェイス</span><span class="sxs-lookup"><span data-stu-id="fce44-210">Interfaces to implement when customizing user store</span></span>

- <span data-ttu-id="fce44-211">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-211">**IUserStore**</span></span>  
 <span data-ttu-id="fce44-212">[IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1)インターフェイスは、唯一のインターフェイスで、ユーザー ストアを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fce44-212">The [IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="fce44-213">作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-213">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
- <span data-ttu-id="fce44-214">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-214">**IUserClaimStore**</span></span>  
 <span data-ttu-id="fce44-215">[IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)インターフェイスは、ユーザーの信頼性情報を有効にするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-215">The [IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="fce44-216">追加、削除、およびユーザーの信頼性情報を取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-216">It contains methods for adding, removing and retrieving user claims.</span></span>
- <span data-ttu-id="fce44-217">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-217">**IUserLoginStore**</span></span>  
 <span data-ttu-id="fce44-218">[IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)外部認証プロバイダーを有効にするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-218">The [IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="fce44-219">追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-219">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
- <span data-ttu-id="fce44-220">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-220">**IUserRoleStore**</span></span>  
 <span data-ttu-id="fce44-221">[IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)インターフェイスを実装するロールにユーザーをマップするメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-221">The [IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="fce44-222">追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-222">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
- <span data-ttu-id="fce44-223">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-223">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="fce44-224">[IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)インターフェイスは、ハッシュされたパスワードを保持するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-224">The [IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="fce44-225">ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッドを取得および設定のためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-225">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
- <span data-ttu-id="fce44-226">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-226">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="fce44-227">[IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)インターフェイスは、ユーザーのアカウント情報が変更されたかどうかを示すためにセキュリティ スタンプを使用するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-227">The [IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="fce44-228">ユーザー、パスワードを変更または追加またはログインを削除します。 このタイムスタンプが更新されます。</span><span class="sxs-lookup"><span data-stu-id="fce44-228">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="fce44-229">取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-229">It contains methods for getting and setting the security stamp.</span></span>
- <span data-ttu-id="fce44-230">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-230">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="fce44-231">[IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)インターフェイスは、2 要素認証をサポートするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-231">The [IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="fce44-232">取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-232">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
- <span data-ttu-id="fce44-233">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-233">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="fce44-234">[IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)インターフェイスは、ユーザーの電話番号を格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-234">The [IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="fce44-235">取得および電話番号と電話番号が確認されているかどうかを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-235">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
- <span data-ttu-id="fce44-236">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-236">**IUserEmailStore**</span></span>  
 <span data-ttu-id="fce44-237">[IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)インターフェイスは、ユーザーのメール アドレスを格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-237">The [IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="fce44-238">取得、および電子メール アドレスと、電子メールが確認されているかどうかを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-238">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
- <span data-ttu-id="fce44-239">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-239">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="fce44-240">[IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)インターフェイスは、アカウントのロックに関する情報を格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-240">The [IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="fce44-241">失敗したアクセス試行とロックアウトを追跡するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-241">It contains methods for tracking failed access attempts and lockouts.</span></span>
- <span data-ttu-id="fce44-242">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="fce44-242">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="fce44-243">[IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)インターフェイスは、クエリ可能なユーザー ストアを提供するために実装するメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-243">The [IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members you implement to provide a queryable user store.</span></span>

<span data-ttu-id="fce44-244">アプリに必要なインターフェイスだけを実装します。</span><span class="sxs-lookup"><span data-stu-id="fce44-244">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="fce44-245">例えば:</span><span class="sxs-lookup"><span data-stu-id="fce44-245">For example:</span></span>

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="fce44-246">IdentityUserClaim、IdentityUserLogin、および IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="fce44-246">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="fce44-247">`Microsoft.AspNet.Identity.EntityFramework`名前空間には実装が含まれています、 [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、 [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)、および[IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)クラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-247">The `Microsoft.AspNet.Identity.EntityFramework` namespace contains implementations of the [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="fce44-248">これらの機能を使用している場合は、これらのクラスの独自のバージョンを作成し、アプリのプロパティを定義することがあります。</span><span class="sxs-lookup"><span data-stu-id="fce44-248">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="fce44-249">ただし、場合がありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。</span><span class="sxs-lookup"><span data-stu-id="fce44-249">However, sometimes it's more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="fce44-250">代わりに、バックエンド ストア クラスには、データ ソースで直接これらの操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="fce44-250">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="fce44-251">たとえば、`UserStore.GetClaimsAsync`メソッドを呼び出すことができます、`userClaimTable.FindByUserId(user.Id)`でクエリを実行するメソッドは直接テーブルをクレームの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="fce44-251">For example, the `UserStore.GetClaimsAsync` method can call the `userClaimTable.FindByUserId(user.Id)` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="fce44-252">Role クラスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="fce44-252">Customize the role class</span></span>

<span data-ttu-id="fce44-253">ロールの記憶域プロバイダーを実装する場合は、カスタム ロールの種類を作成できます。</span><span class="sxs-lookup"><span data-stu-id="fce44-253">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="fce44-254">特定のインターフェイスを実装していない必要がある必要があります、`Id`通常には、`Name`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fce44-254">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="fce44-255">ロールのクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fce44-255">The following is an example role class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="fce44-256">ロール ストアをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="fce44-256">Customize the role store</span></span>

<span data-ttu-id="fce44-257">作成することができます、`RoleStore`ロールに対するすべてのデータ操作のメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-257">You can create a `RoleStore` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="fce44-258">このクラスは、 [RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)クラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-258">This class is equivalent to the [RoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="fce44-259">`RoleStore`クラスを実装する、`IRoleStore<TRole>`と、必要に応じて、`IQueryableRoleStore<TRole>`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="fce44-259">In the `RoleStore` class, you implement the `IRoleStore<TRole>` and optionally the `IQueryableRoleStore<TRole>` interface.</span></span>

- <span data-ttu-id="fce44-260">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="fce44-260">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="fce44-261">[IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1)インターフェイスは、ロール ストア クラスに実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fce44-261">The [IRoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="fce44-262">作成、更新、削除、およびロールを取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-262">It contains methods for creating, updating, deleting, and retrieving roles.</span></span>
- <span data-ttu-id="fce44-263">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="fce44-263">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="fce44-264">カスタマイズする`RoleStore`、実装するクラスを作成、`IRoleStore<TRole>`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="fce44-264">To customize `RoleStore`, create a class that implements the `IRoleStore<TRole>` interface.</span></span> 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a><span data-ttu-id="fce44-265">新しい記憶域プロバイダーを使用するアプリを再構成します。</span><span class="sxs-lookup"><span data-stu-id="fce44-265">Reconfigure app to use a new storage provider</span></span>

<span data-ttu-id="fce44-266">記憶域プロバイダーが実装されると、それを使用するアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="fce44-266">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="fce44-267">アプリでは、既定のプロバイダーを使用する場合は、カスタム プロバイダーで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fce44-267">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="fce44-268">削除、 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="fce44-268">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="fce44-269">記憶域プロバイダーを別のプロジェクトまたはパッケージが存在する場合への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="fce44-269">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="fce44-270">すべての参照を置き換える`Microsoft.AspNetCore.EntityFramework.Identity`を使用すると、記憶域プロバイダーの名前空間のステートメント。</span><span class="sxs-lookup"><span data-stu-id="fce44-270">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="fce44-271">`ConfigureServices`メソッド、変更、`AddIdentity`カスタム型を使用する方法。</span><span class="sxs-lookup"><span data-stu-id="fce44-271">In the `ConfigureServices` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="fce44-272">この目的のための独自の拡張メソッドを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="fce44-272">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="fce44-273">参照してください[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)例についてはします。</span><span class="sxs-lookup"><span data-stu-id="fce44-273">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="fce44-274">ロールを使用している場合は、更新、`RoleManager`を使用する、`RoleStore`クラス。</span><span class="sxs-lookup"><span data-stu-id="fce44-274">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="fce44-275">アプリの構成に、接続文字列と資格情報を更新します。</span><span class="sxs-lookup"><span data-stu-id="fce44-275">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="fce44-276">例:</span><span class="sxs-lookup"><span data-stu-id="fce44-276">Example:</span></span>

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

## <a name="references"></a><span data-ttu-id="fce44-277">参照</span><span class="sxs-lookup"><span data-stu-id="fce44-277">References</span></span>

- [<span data-ttu-id="fce44-278">ASP.NET 4.x Identity 用のカスタム ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fce44-278">Custom Storage Providers for ASP.NET 4.x Identity</span></span>](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- <span data-ttu-id="fce44-279">[ASP.NET Core Identity](https://github.com/aspnet/identity) -このリポジトリには、ストア プロバイダーを管理するコミュニティへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fce44-279">[ASP.NET Core Identity](https://github.com/aspnet/identity) - This repository includes links to community maintained store providers.</span></span>
