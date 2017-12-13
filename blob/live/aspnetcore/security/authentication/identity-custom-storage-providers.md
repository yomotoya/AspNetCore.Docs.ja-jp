---
title: "ASP.NET Core Id のカスタムの記憶域プロバイダー |Microsoft ドキュメント"
author: ardalis
description: "ASP.NET Core Id のカスタムの記憶域プロバイダーを構成する方法。"
keywords: "ASP.NET Core、Identity、カスタムの記憶域プロバイダー"
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 687ca96be5121502e816bdc856e17dcd5923fe05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="ef1d9-104">ASP.NET Core Id のカスタムの記憶域プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ef1d9-104">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="ef1d9-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ef1d9-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ef1d9-106">ASP.NET Core の Id は、カスタムの記憶域プロバイダーを作成し、アプリに接続することにより拡張可能なシステムです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-106">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="ef1d9-107">このトピックでは、ASP.NET Core Id 用のカスタマイズされた記憶域プロバイダーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-107">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="ef1d9-108">独自の記憶域プロバイダーを作成するための重要な概念について説明しますが、ステップ バイ ステップ チュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-108">It covers the important concepts for creating your own storage provider, but is not a step-by-step walkthrough.</span></span>

<span data-ttu-id="ef1d9-109">[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-109">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="ef1d9-110">はじめに</span><span class="sxs-lookup"><span data-stu-id="ef1d9-110">Introduction</span></span>

<span data-ttu-id="ef1d9-111">既定では、ASP.NET Core Id システムは、Entity Framework のコアを使用して SQL Server データベースにユーザー情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-111">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="ef1d9-112">多くのアプリは、このアプローチはします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-112">For many apps, this approach works well.</span></span> <span data-ttu-id="ef1d9-113">ただし、別の永続化メカニズムまたはデータ スキーマを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-113">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="ef1d9-114">例:</span><span class="sxs-lookup"><span data-stu-id="ef1d9-114">For example:</span></span>

* <span data-ttu-id="ef1d9-115">使用する[Azure テーブル ストレージ](https://docs.microsoft.com/azure/storage/)または別のデータ ストアです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-115">You use [Azure Table Storage](https://docs.microsoft.com/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="ef1d9-116">データベースのテーブルでは、別の構造があります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-116">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="ef1d9-117">など、さまざまなデータ アクセス方法を使用することができます[Dapper](https://github.com/StackExchange/Dapper)です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-117">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="ef1d9-118">このような場合の各、記憶域メカニズムのカスタマイズされたプロバイダーを作成し、そのプロバイダーをアプリに接続できます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-118">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="ef1d9-119">ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-119">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="ef1d9-120">.NET Core CLI を使用して、追加`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="ef1d9-120">When using the .NET Core CLI, add `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="ef1d9-121">ASP.NET Core Id アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="ef1d9-121">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="ef1d9-122">ASP.NET Core Id は、マネージャーおよびストアと呼ばれるクラスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-122">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="ef1d9-123">*マネージャー*アイデンティティ ユーザーを作成するなどの操作を実行するアプリの開発者が使用する高レベルのクラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-123">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="ef1d9-124">*ストア*ユーザーやロールなどのエンティティを永続化する方法を指定した下位のクラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-124">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="ef1d9-125">次のストア、[リポジトリ パターン](http://deviq.com/repository-pattern/)し、永続化メカニズムと組み合わせると密接にします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-125">Stores follow the [repository pattern](http://deviq.com/repository-pattern/) and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="ef1d9-126">マネージャーは、つまり、(構成) を除くアプリケーション コードを変更せず、永続化メカニズムを置き換えることができます、ストアから切り離されます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-126">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="ef1d9-127">Web アプリと対話する方法、マネージャー ストアと、データ アクセス レイヤーの対話中の図は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-127">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core アプリケーション管理者 (たとえば、'UserManager'、'RoleManager') を使用します。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="ef1d9-130">カスタムの記憶域プロバイダーを作成するには、このデータ アクセス層 (緑、および灰色のボックス上のダイアグラムの) を持つデータ ソース、データ アクセス層、および対話するストア クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-130">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="ef1d9-131">管理者またはそれら (青いボックス上記) と連携する、アプリ コードをカスタマイズする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-131">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="ef1d9-132">新しいインスタンスを作成するときに`UserManager`または`RoleManager`するには、ユーザー クラスの種類を指定し、ストアのクラスのインスタンスを引数として渡します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-132">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="ef1d9-133">この方法では、ASP.NET Core にカスタマイズしたクラスを接続することができます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-133">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="ef1d9-134">[新しい記憶域プロバイダーを使用するアプリを再構成](#reconfigure-app-to-use-new-storage-provider)のインスタンスを作成する方法を示します`UserManager`と`RoleManager`カスタマイズされたストアとします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-134">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="ef1d9-135">ASP.NET Core の Id データ型を格納します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-135">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="ef1d9-136">[ASP.NET Core Id](https://github.com/aspnet/identity)データ型は、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-136">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="ef1d9-137">Users</span><span class="sxs-lookup"><span data-stu-id="ef1d9-137">Users</span></span>

<span data-ttu-id="ef1d9-138">Web サイトの登録ユーザー。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-138">Registered users of your web site.</span></span> <span data-ttu-id="ef1d9-139">[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)型を拡張または独自のカスタム型の例として使用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-139">The [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="ef1d9-140">独自のカスタム id の記憶域ソリューションを実装する特定の型から継承する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-140">You do not need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="ef1d9-141">ユーザーの信頼性情報</span><span class="sxs-lookup"><span data-stu-id="ef1d9-141">User Claims</span></span>

<span data-ttu-id="ef1d9-142">ステートメントのセット (または[クレーム](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) をユーザーの id を表すユーザーに関するします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-142">A set of statements (or [Claims](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) about the user that represent the user's identity.</span></span> <span data-ttu-id="ef1d9-143">ロールで実現できるよりも、ユーザーの id の大きい式を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-143">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="ef1d9-144">ユーザーのログイン</span><span class="sxs-lookup"><span data-stu-id="ef1d9-144">User Logins</span></span>

<span data-ttu-id="ef1d9-145">外部認証プロバイダー (Facebook、Microsoft アカウントなど) に関する情報をユーザーにログインするときに使用します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-145">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="ef1d9-146">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-146">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="ef1d9-147">役割</span><span class="sxs-lookup"><span data-stu-id="ef1d9-147">Roles</span></span>

<span data-ttu-id="ef1d9-148">サイトのグループを承認します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-148">Authorization groups for your site.</span></span> <span data-ttu-id="ef1d9-149">ロールの Id とロール名 ("Admin"、"Employee"など) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-149">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="ef1d9-150">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-150">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="ef1d9-151">データ アクセス層</span><span class="sxs-lookup"><span data-stu-id="ef1d9-151">The data access layer</span></span>

<span data-ttu-id="ef1d9-152">このトピックでは、永続化メカニズムを使用しているし、その機構のエンティティを作成する方法に慣れていると仮定します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-152">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="ef1d9-153">このトピックでは、リポジトリまたはデータ アクセス クラスを作成する方法の詳細については紹介しませんASP.NET Core の Id を使用する場合は、設計に関する決定事項についていくつかの提案を提供します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-153">This topic does not provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="ef1d9-154">カスタマイズされたストア プロバイダーのデータ アクセス レイヤーをデザインするときは、多数の自由度があります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-154">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="ef1d9-155">アプリで使用する予定の機能の永続化メカニズムを作成する必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-155">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="ef1d9-156">たとえば、アプリでの役割を使用しない場合の役割またはユーザー ロールの関連付けの記憶域を作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-156">For example, if you are not using roles in your app, you do not need to create storage for roles or user role associations.</span></span> <span data-ttu-id="ef1d9-157">テクノロジと既存のインフラストラクチャには、構造体は ASP.NET Core Id の既定の実装と大きく異なることがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-157">Your technology and existing infrastructure may require a structure that is very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="ef1d9-158">データ アクセス層は、ストレージの実装の構造を操作するためのロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-158">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="ef1d9-159">データ アクセス レイヤーでは、データ ソースへの ASP.NET Core Id からデータを保存するためのロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-159">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="ef1d9-160">カスタマイズされた記憶域プロバイダーのデータ アクセス層には、ユーザーおよびロールの情報を格納する、次のクラスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-160">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="ef1d9-161">Context クラス</span><span class="sxs-lookup"><span data-stu-id="ef1d9-161">Context class</span></span>

<span data-ttu-id="ef1d9-162">永続化メカニズムに接続し、クエリを実行する情報をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-162">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="ef1d9-163">いくつかのデータ クラスでは、このクラスは、依存関係の挿入によって提供される通常のインスタンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-163">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="ef1d9-164">[例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-164">[Example](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="ef1d9-165">ユーザーのストレージ</span><span class="sxs-lookup"><span data-stu-id="ef1d9-165">User Storage</span></span>

<span data-ttu-id="ef1d9-166">格納し、(ユーザー名とパスワードのハッシュ) などのユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-166">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="ef1d9-167">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-167">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="ef1d9-168">ロールのストレージ</span><span class="sxs-lookup"><span data-stu-id="ef1d9-168">Role Storage</span></span>

<span data-ttu-id="ef1d9-169">格納し、(ロール名など) のロール情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-169">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="ef1d9-170">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-170">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="ef1d9-171">UserClaims ストレージ</span><span class="sxs-lookup"><span data-stu-id="ef1d9-171">UserClaims Storage</span></span>

<span data-ttu-id="ef1d9-172">格納し、要求の種類と値の場合) などのユーザー要求の情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-172">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="ef1d9-173">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-173">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="ef1d9-174">UserLogins ストレージ</span><span class="sxs-lookup"><span data-stu-id="ef1d9-174">UserLogins Storage</span></span>

<span data-ttu-id="ef1d9-175">格納し、(外部認証プロバイダーの場合) などのユーザーのログイン情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-175">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="ef1d9-176">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-176">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="ef1d9-177">UserRole 記憶域</span><span class="sxs-lookup"><span data-stu-id="ef1d9-177">UserRole Storage</span></span>

<span data-ttu-id="ef1d9-178">格納し、どのユーザーに割り当てられたロールを取得します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-178">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="ef1d9-179">例</span><span class="sxs-lookup"><span data-stu-id="ef1d9-179">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="ef1d9-180">**ヒント:**のみ、アプリで使用するクラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-180">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="ef1d9-181">データ アクセス クラスでは、永続化メカニズムのデータ操作を実行するコードを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-181">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="ef1d9-182">たとえば、カスタム プロバイダーにする必要がありますで新しいユーザーを作成するには、次のコード、*格納*クラス。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-182">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="ef1d9-183">ユーザーを作成するための実装ロジックにあり、``_usersTable.CreateAsync``メソッドを次に示すです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-183">The implementation logic for creating the user is in the ``_usersTable.CreateAsync`` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="ef1d9-184">ユーザー クラスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-184">Customize the user class</span></span>

<span data-ttu-id="ef1d9-185">記憶域プロバイダーを実装する場合と同じであるユーザー クラスを作成、 [ `IdentityUser`クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-185">When implementing a storage provider, create a user class which is equivalent to the [`IdentityUser` class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="ef1d9-186">少なくとも、ユーザー クラスを含める必要があります、`Id`と`UserName`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-186">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="ef1d9-187">`IdentityUser`クラス、プロパティを定義する、``UserManager``要求された操作の呼び出しを実行するとき。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-187">The `IdentityUser` class defines the properties that the ``UserManager`` calls when performing requested operations.</span></span> <span data-ttu-id="ef1d9-188">既定の種類、`Id`プロパティは、文字列から継承することができますが、`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`し、別の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-188">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="ef1d9-189">フレームワークでは、データ型変換を処理するストレージの実装が必要です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-189">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="ef1d9-190">ユーザーのストアをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-190">Customize the user store</span></span>

<span data-ttu-id="ef1d9-191">作成、`UserStore`ユーザーに対するすべてのデータ操作のメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-191">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="ef1d9-192">このクラスは、 [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-192">This class is equivalent to the [UserStore<TUser>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="ef1d9-193">`UserStore`クラスを実装`IUserStore<TUser>`と必要とする省略可能なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-193">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="ef1d9-194">実装する省略可能なインターフェイスを選択すると、アプリで提供される機能に基づいています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-194">You select which optional interfaces to implement based on on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="ef1d9-195">省略可能なインターフェイス</span><span class="sxs-lookup"><span data-stu-id="ef1d9-195">Optional interfaces</span></span>

- <span data-ttu-id="ef1d9-196">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span><span class="sxs-lookup"><span data-stu-id="ef1d9-196">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span></span>
- <span data-ttu-id="ef1d9-197">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span><span class="sxs-lookup"><span data-stu-id="ef1d9-197">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span></span>
- <span data-ttu-id="ef1d9-198">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span><span class="sxs-lookup"><span data-stu-id="ef1d9-198">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span></span>
- <span data-ttu-id="ef1d9-199">IUserSecurityStampStore<!-- make these all links and remove / --></span><span class="sxs-lookup"><span data-stu-id="ef1d9-199">IUserSecurityStampStore <!-- make these all links and remove / --></span></span>
- <span data-ttu-id="ef1d9-200">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-200">IUserEmailStore</span></span>
- <span data-ttu-id="ef1d9-201">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-201">IPhoneNumberStore</span></span>
- <span data-ttu-id="ef1d9-202">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-202">IQueryableUserStore</span></span>
- <span data-ttu-id="ef1d9-203">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-203">IUserLoginStore</span></span>
- <span data-ttu-id="ef1d9-204">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-204">IUserTwoFactorStore</span></span>
- <span data-ttu-id="ef1d9-205">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="ef1d9-205">IUserLockoutStore</span></span>

<span data-ttu-id="ef1d9-206">省略可能なインターフェイスを継承`IUserStore`です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-206">The optional interfaces inherit from `IUserStore`.</span></span> <span data-ttu-id="ef1d9-207">部分的に実装されているサンプルのユーザー ストアを確認できます[ここ](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-207">You can see a partially implemented sample user store [here](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="ef1d9-208">内で、`UserStore`クラス、操作を実行するために作成したデータ アクセス クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-208">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="ef1d9-209">依存関係の挿入を使用して渡されます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-209">These are passed in using dependency injection.</span></span> <span data-ttu-id="ef1d9-210">口ひげ実装では、使用して、SQL サーバーなどで、`UserStore`クラスには、`CreateAsync`メソッドのインスタンスを使用する`DapperUsersTable`新しいレコードを挿入します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-210">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="ef1d9-211">ユーザーのストアをカスタマイズするときに実装するインターフェイス</span><span class="sxs-lookup"><span data-stu-id="ef1d9-211">Interfaces to implement when customizing user store</span></span>

- <span data-ttu-id="ef1d9-212">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-212">**IUserStore**</span></span>  
 <span data-ttu-id="ef1d9-213">[IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1)インターフェイスは、のみのインターフェイスで、ユーザーのストアを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-213">The [IUserStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="ef1d9-214">作成、更新、削除、およびユーザーを取得するためのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-214">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
- <span data-ttu-id="ef1d9-215">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-215">**IUserClaimStore**</span></span>  
 <span data-ttu-id="ef1d9-216">[IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1)インターフェイスがユーザーの信頼性情報を有効にするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-216">The [IUserClaimStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="ef1d9-217">追加、削除、およびユーザーの信頼性情報を取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-217">It contains methods for adding, removing and retrieving user claims.</span></span>
- <span data-ttu-id="ef1d9-218">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-218">**IUserLoginStore**</span></span>  
 <span data-ttu-id="ef1d9-219">[IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1)外部認証プロバイダーを有効にするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-219">The [IUserLoginStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="ef1d9-220">追加、削除、およびユーザーのログインとログイン情報に基づいてユーザーを取得するためのメソッドを取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-220">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
- <span data-ttu-id="ef1d9-221">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-221">**IUserRoleStore**</span></span>  
 <span data-ttu-id="ef1d9-222">[IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1)インターフェイス ロールにユーザーをマップするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-222">The [IUserRoleStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="ef1d9-223">追加、削除、およびユーザーのロール、およびユーザーがロールに割り当てられているかどうかをチェックするメソッドを取得するメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-223">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
- <span data-ttu-id="ef1d9-224">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-224">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="ef1d9-225">[IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)インターフェイス ハッシュされたパスワードを永続化するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-225">The [IUserPasswordStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="ef1d9-226">ハッシュされたパスワード、およびユーザーがパスワードを設定するかどうかを示すメソッド取得および設定のためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-226">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
- <span data-ttu-id="ef1d9-227">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-227">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="ef1d9-228">[IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)インターフェイス セキュリティ スタンプを使用して、ユーザーのアカウント情報が変更されたかどうかを示すために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-228">The [IUserSecurityStampStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="ef1d9-229">ユーザーをパスワードを変更または追加または削除のログイン時に、このスタンプが更新されます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-229">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="ef1d9-230">取得およびセキュリティ スタンプを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-230">It contains methods for getting and setting the security stamp.</span></span>
- <span data-ttu-id="ef1d9-231">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-231">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="ef1d9-232">[IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)インターフェイス 2 要素認証をサポートするために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-232">The [IUserTwoFactorStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="ef1d9-233">取得およびユーザーの 2 要素認証が有効になっているかどうかを設定するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-233">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
- <span data-ttu-id="ef1d9-234">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-234">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="ef1d9-235">[IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)インターフェイスがユーザーの電話番号を格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-235">The [IUserPhoneNumberStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="ef1d9-236">電話番号および電話番号が確認されているかどうか取得および設定のためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-236">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
- <span data-ttu-id="ef1d9-237">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-237">**IUserEmailStore**</span></span>  
 <span data-ttu-id="ef1d9-238">[IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1)インターフェイスがユーザーの電子メール アドレスを格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-238">The [IUserEmailStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="ef1d9-239">電子メール アドレスおよび電子メールが確認されているかどうか取得および設定のためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-239">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
- <span data-ttu-id="ef1d9-240">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-240">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="ef1d9-241">[IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)インターフェイスがアカウントのロックに関する情報を格納するために実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-241">The [IUserLockoutStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="ef1d9-242">失敗したアクセス試行とロックアウトを追跡するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-242">It contains methods for tracking failed access attempts and lockouts.</span></span>
- <span data-ttu-id="ef1d9-243">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-243">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="ef1d9-244">[IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)インターフェイスは、クエリ可能なユーザー ストアを提供するメンバーの実装を定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-244">The [IQueryableUserStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members implement to provide a queryable user store.</span></span>

<span data-ttu-id="ef1d9-245">インターフェイスを実装するだけのために必要なアプリでします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-245">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="ef1d9-246">例:</span><span class="sxs-lookup"><span data-stu-id="ef1d9-246">For example:</span></span>

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="ef1d9-247">IdentityUserClaim、IdentityUserLogin、および IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="ef1d9-247">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="ef1d9-248">``Microsoft.AspNet.Identity.EntityFramework``名前空間の実装を含む、 [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、 [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)、および[IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-248">The ``Microsoft.AspNet.Identity.EntityFramework`` namespace contains implementations of the [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="ef1d9-249">これらの機能を使用している場合は、これらのクラスの独自バージョンを作成し、アプリのプロパティを定義することがあります。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-249">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="ef1d9-250">ただし、場合もありますがこれらのエンティティを追加または削除、ユーザーの要求) などの基本的な操作を実行するときにメモリに読み込まれないする方が効率的です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-250">However, sometimes it is more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="ef1d9-251">代わりに、バックエンド ストア クラスは、データ ソースで直接これらの操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-251">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="ef1d9-252">たとえば、``UserStore.GetClaimsAsync``メソッドを呼び出すことができます、``userClaimTable.FindByUserId(user.Id)``メソッドでクエリを実行するテーブルの直接要求の一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-252">For example, the ``UserStore.GetClaimsAsync`` method can call the ``userClaimTable.FindByUserId(user.Id)`` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="ef1d9-253">Role クラスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-253">Customize the role class</span></span>

<span data-ttu-id="ef1d9-254">役割の記憶域プロバイダーを実装する場合は、カスタム ロールの種類を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-254">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="ef1d9-255">特定のインターフェイスを実装していない必要がある必要があります、`Id`し、通常が、`Name`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-255">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="ef1d9-256">ロールのクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-256">The following is an example role class:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="ef1d9-257">ロール ストアをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-257">Customize the role store</span></span>

<span data-ttu-id="ef1d9-258">作成することができます、``RoleStore``ロール上のすべてのデータ操作のメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-258">You can create a ``RoleStore`` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="ef1d9-259">このクラスは、 [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-259">This class is equivalent to the [RoleStore<TRole>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="ef1d9-260">`RoleStore`クラスを実装する、``IRoleStore<TRole>``し、必要に応じて、``IQueryableRoleStore<TRole>``インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-260">In the `RoleStore` class, you implement the ``IRoleStore<TRole>`` and optionally the ``IQueryableRoleStore<TRole>`` interface.</span></span>

- <span data-ttu-id="ef1d9-261">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-261">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="ef1d9-262">[IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1)インターフェイスは、ロール ストアのクラスに実装するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-262">The [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="ef1d9-263">作成、更新、削除、およびロールを取得するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-263">It contains methods for creating, updating, deleting and retrieving roles.</span></span>
- <span data-ttu-id="ef1d9-264">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="ef1d9-264">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="ef1d9-265">カスタマイズする`RoleStore`を実装するクラスを作成、`IRoleStore`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-265">To customize `RoleStore`, create a class that implements the `IRoleStore` interface.</span></span> 

## <a name="reconfigure-app-to-use-new-storage-provider"></a><span data-ttu-id="ef1d9-266">新しい記憶域プロバイダーを使用するアプリを再構成します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-266">Reconfigure app to use new storage provider</span></span>

<span data-ttu-id="ef1d9-267">記憶域プロバイダーを実装した後、それを使用するアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-267">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="ef1d9-268">アプリでは、既定のプロバイダーを使用する場合は、カスタム プロバイダーで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-268">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="ef1d9-269">削除、 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-269">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="ef1d9-270">記憶域プロバイダーが別のプロジェクトまたはパッケージが存在する場合への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-270">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="ef1d9-271">すべての参照を置き換える`Microsoft.AspNetCore.EntityFramework.Identity`を使用して、、記憶域プロバイダーの名前空間のステートメント。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-271">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="ef1d9-272">``ConfigureServices``メソッド、変更、`AddIdentity`カスタム型を使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-272">In the ``ConfigureServices`` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="ef1d9-273">この目的のための独自の拡張メソッドを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-273">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="ef1d9-274">参照してください[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)例についてはします。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-274">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="ef1d9-275">ロールを使用している場合は、更新、`RoleManager`を使用する、`RoleStore`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-275">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="ef1d9-276">アプリの構成に、接続文字列と資格情報を更新します。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-276">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="ef1d9-277">例:</span><span class="sxs-lookup"><span data-stu-id="ef1d9-277">Example:</span></span>

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

## <a name="references"></a><span data-ttu-id="ef1d9-278">参照</span><span class="sxs-lookup"><span data-stu-id="ef1d9-278">References</span></span>

- [<span data-ttu-id="ef1d9-279">ASP.NET Id のカスタムの記憶域プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ef1d9-279">Custom Storage Providers for ASP.NET Identity</span></span>](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- <span data-ttu-id="ef1d9-280">[ASP.NET Core Id](https://github.com/aspnet/identity) -このリポジトリには、ストア プロバイダーを管理するコミュニティへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef1d9-280">[ASP.NET Core Identity](https://github.com/aspnet/identity) - This repository includes links to community maintained store providers.</span></span>
