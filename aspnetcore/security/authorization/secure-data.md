---
title: "認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。"
author: rick-anderson
description: "認証によって保護されているユーザー データと Razor ページ アプリケーションを作成する方法を説明します。 SSL、認証、セキュリティ、ASP.NET Core Id が含まれます。"
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 944886a7d55af8966dc51424d16bec5ff58dbc05
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)

このチュートリアルでは、認証によって保護されているユーザー データと ASP.NET Core web アプリを作成する方法を示します。 (登録済み) のユーザーを認証した連絡先の一覧を表示を作成します。 これには次の 3 つのセキュリティ グループがあります。

* **ユーザーが登録されている**承認済みのすべてのデータを表示することができ、自分のデータ編集、または削除できます。
* **マネージャー**保護者の連絡先データを拒否することができます。 許可されている連絡先のみがユーザーに表示されます。
* **管理者**承認または拒否でき、すべてのデータを編集/削除します。

次の図では、ユーザー Rick (`rick@example.com`) 署名します。 Rick は許可されている連絡先だけを表示でき、**編集**/**削除**/**新規作成**連絡先へのリンク。 Rick、ディスプレイによってのみ、最後のレコードが作成された**編集**と**削除**リンクします。 その他のユーザーは、マネージャーまたは管理者は、ステータスが"Approved"に変更されるまで、最後のレコードを表示されません。

![イメージは、前に説明されています。](secure-data/_static/rick.png)

次の図の`manager@contoso.com`とマネージャーの役割では署名します。

![イメージは、前に説明されています。](secure-data/_static/manager1.png)

次の図は、連絡先の詳細ビューをマネージャーには。

![イメージは、前に説明されています。](secure-data/_static/manager.png)

**承認**と**拒否**ボタンは、管理者、および管理者のみ表示されます。

次の図の`admin@contoso.com`では、管理者ロールでは署名します。

![イメージは、前に説明されています。](secure-data/_static/admin.png)

管理者は、すべての特権を持ちます。 彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。

アプリがによって作成された[スキャフォールディング](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)次`Contact`モデル。

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

このサンプルには、次の認証ハンドラーが含まれています。

* `ContactIsOwnerAuthorizationHandler`: ユーザーが、データを編集のみできることを確認します。
* `ContactManagerAuthorizationHandler`: を承認または拒否の連絡先のマネージャーをできるようにします。
* `ContactAdministratorsAuthorizationHandler`: 管理者の連絡先を編集/削除し、承認または連絡先を拒否するようにします。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルを進められます。 理解しておく必要があります。

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [認証](xref:security/authentication/index)
* [アカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
* [承認](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

このチュートリアルの ASP.NET Core の 1.1 バージョンでは[この](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)フォルダーです。 ASP.NET Core サンプルでは、1.1、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)です。

## <a name="the-starter-and-completed-app"></a>Starter および完成したアプリ

[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)アプリ。 [テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解しておくようにします。

### <a name="the-starter-app"></a>スターター アプリ

[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)アプリ。

アプリを実行する、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。

## <a name="secure-user-data"></a>ユーザー データを保護します。

次のセクションでは、セキュリティで保護されたユーザー データのアプリを作成するすべての主要な手順があります。 完成したプロジェクトを参照すると便利です。

### <a name="tie-the-contact-data-to-the-user"></a>ユーザーに連絡先データを関連付ける

ASP.NET を使用して[Identity](xref:security/authentication/identity)のユーザーのユーザー ID を編集できますが、そのデータを他のユーザー データは表示されません。 追加`OwnerID`と`ContactStatus`を`Contact`モデル。

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。 `Status`フィールドは、連絡先が一般のユーザーによって表示可能であるかどうか。

新しい移行を作成し、データベースを更新します。

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a>SSL と認証済みユーザーが必要

追加[IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)に`Startup`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

`ConfigureServices`のメソッド、 *Startup.cs*ファイルに追加し、 [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)承認フィルター。

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

Visual Studio を使用している場合は、SSL を有効にします。

HTTP 要求を HTTPS にリダイレクトするを参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。 Visual Studio のコードを使用してローカルのプラットフォームでテストしている場合は、テスト証明書を SSL 用に含まれていません。

  設定`"LocalTest:skipSSL": true`で、 *appsettings です。Developement.json*ファイル。

### <a name="require-authenticated-users"></a>ユーザーが認証されている必要があります。

ユーザー認証を必要とする既定の認証ポリシーを設定します。 Razor ページ、コントローラ、またはアクション メソッド レベルでの認証を省略することができます、`[AllowAnonymous]`属性。 ユーザー認証を必要とする既定の認証ポリシーの設定と、新しく追加された Razor ページおよびコント ローラーが保護されます。 既定では必要な認証を新しいコント ローラーおよび Razor ページで証明書利用者のより安全ですが、`[Authorize]`属性。 次の追加、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

追加[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)インデックス、および連絡先について、ページを登録する前に、匿名ユーザーはサイトに関する情報を取得できるようにします。 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

追加`[AllowAnonymous]`を[LoginModel と RegisterModel](https://github.com/aspnet/templating/issues/238)です。

### <a name="configure-the-test-account"></a>テスト アカウントを構成します。

`SeedData`クラスは 2 つのアカウントを作成します。 管理者と管理者です。 使用して、[シークレット マネージャー ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。 プロジェクト ディレクトリから、パスワードの設定 (ディレクトリを含む*Program.cs*)。

```console
dotnet user-secrets set SeedUserPW <PW>
```

更新`Main`テスト パスワードを使用します。

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>テスト アカウントを作成し、連絡先の更新

更新プログラム、`Initialize`メソッドで、`SeedData`テスト アカウントを作成するクラス。

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

管理者のユーザー ID を追加し、`ContactStatus`連絡先にします。 「送信済み」と「拒否」の 1 つの連絡先のいずれかを作成します。 すべての連絡先に、ユーザー ID と状態を追加します。 1 人だけが表示されます。

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>所有者、マネージャー、および管理者の承認のハンドラーを作成します。

作成、`ContactIsOwnerAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactIsOwnerAuthorizationHandler`リソースで動作しているユーザーがリソースを所有していることを確認します。

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼び出し[コンテキスト。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)現在の認証済みユーザーがメンバーの所有者である場合。 認証ハンドラー通常。

* 返す`context.Succeed`要件を満たしている場合にします。
* 返す`Task.CompletedTask`要件を満たしていない場合にします。 `Task.CompletedTask`成功または失敗のどちらも&mdash;他の認証ハンドラーを実行することができます。

明示的に失敗する場合は、返す[コンテキスト。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)です。

アプリケーションが独自のデータ編集/削除/作成所有者にお問い合わせください。 できます。 `ContactIsOwnerAuthorizationHandler`要求パラメーターに渡された操作を確認する必要はありません。

### <a name="create-a-manager-authorization-handler"></a>マネージャーの認証ハンドラーを作成します。

作成、`ContactManagerAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactManagerAuthorizationHandler`リソースに対して機能しているユーザーが管理者であることを確認します。 マネージャーだけでは、承認したり、コンテンツの変更 (新しいまたは変更された) を拒否することができます。

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>管理者認証ハンドラーを作成します。

作成、`ContactAdministratorsAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactAdministratorsAuthorizationHandler`リソースに対して機能しているユーザーが管理者であることを確認します。 管理者は、すべての操作を行うことができます。

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>認証ハンドラーを登録します。

Entity Framework のコアを使用してサービスを登録する必要があります[依存性の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)です。 `ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。 ハンドラー コレクションに登録サービスを使用するため、`ContactsController`を通じて[依存性の注入](xref:fundamentals/dependency-injection)です。 末尾に次のコードを追加`ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

`ContactAdministratorsAuthorizationHandler`および`ContactManagerAuthorizationHandler`シングルトンとして追加されます。 シングルトンを務める EF を使用していないし、必要なすべての情報があるため、`Context`のパラメーター、`HandleRequirementAsync`メソッドです。

## <a name="support-authorization"></a>承認をサポートします。

このセクションでは、Razor ページを更新し、操作の要件クラスを追加します。

### <a name="review-the-contact-operations-requirements-class"></a>連絡先の操作の要件のクラスを確認します。

確認、`ContactOperations`クラスです。 このクラスには、要件が含まれています。 アプリケーションがサポートします。

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Razor ページの基本クラスを作成します。

連絡先 Razor ページで使用するサービスを含む基本クラスを作成します。 基本クラスは、1 つの場所でその初期化コードを配置します。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

上のコードでは以下の操作が行われます。

* 追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。
* Id を追加`UserManager`サービス。
* `ApplicationDbContext` を追加します。

### <a name="update-the-createmodel"></a>更新プログラム、CreateModel

使用するモデル コンス トラクターを作成する ページを更新、`DI_BasePageModel`基本クラス。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

更新プログラム、`CreateModel.OnPostAsync`メソッド。

* ユーザー ID を追加、`Contact`モデル。
* ユーザーが連絡先を作成するアクセス許可を確認する認証ハンドラーを呼び出します。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新プログラム、IndexModel

更新プログラム、`OnGetAsync`メソッドため、一般的なユーザーにのみ許可されている連絡先が表示されますの。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新プログラム、EditModel

ユーザーに連絡先を所有していることを確認する認証ハンドラーを追加します。 リソース承認が検証されているため、`[Authorize]`属性では不十分です。 アプリは、属性が評価されるときに、リソースへのアクセスにすることがありません。 リソース ベースの承認は、命令型である必要があります。 ページのモデルに読み込んで、またはハンドラー自体内で読み込むことにより、アプリが、リソースへのアクセスを持つ、チェックを実行する必要があります。 リソース キーを渡すことによって、リソースにアクセスする頻度。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新プログラム、DeleteModel

ユーザーが連絡先の削除アクセス権を持つことを確認する認証ハンドラーを使用して削除ページ モデルを更新します。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>承認サービスをビューに挿入します。

現時点では、UI の表示では、編集し、ユーザーが変更できないデータへのリンクを削除します。 認証ハンドラーをビューに適用することによって、UI が固定されています。

承認サービスを挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに使用可能になるようにします。

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

上記のマークアップに追加のいくつか`using`ステートメントです。

更新プログラム、**編集**と**削除**でリンク*Pages/Contacts/Index.cshtml*のため、適切なアクセス許可を持つユーザー、レンダリング中のみ。

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> データを変更する権限がないユーザーからのリンクを非表示にすると、アプリをセキュリティで保護しません。 リンクを非表示にするにより、アプリをユーザーにわかりやすい唯一の有効なリンクを表示します。 ユーザーは、編集を呼び出すし、自分が所有しないデータの操作を削除するには、生成された Url を切断できます。 Razor ページまたはコント ローラーは、データを保護するアクセス チェックを適用する必要があります。

### <a name="update-details"></a>更新プログラムの詳細

マネージャーが承認または連絡先を拒否するため、詳細ビューを更新します。

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

詳細ページのモデルを更新します。

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>完成したアプリをテストします。

Visual Studio のコードを使用してローカルのプラットフォームでテストしている場合は、テスト証明書を SSL 用に含まれていません。

* 設定`"LocalTest:skipSSL": true`で、 *appsettings です。Developement.json*ファイルを SSL の要件をスキップします。 開発用コンピューターでのみ SSL をスキップします。

場合は、アプリには、連絡先があります。

* 内のすべてのレコードを削除、`Contact`テーブル。
* データベースのシードにアプリを再起動します。

連絡先を参照するためには、ユーザーを登録します。

完成したアプリをテストする簡単な方法では、3 つの異なるブラウザー (または incognito/inprivate ブラウズのバージョン) を起動します。 1 つのブラウザーで、新しいユーザーを登録します (たとえば、 `test@example.com`)。 別のユーザーに各ブラウザーにサインインします。 次の操作を確認します。

* 登録済みのユーザーには、承認されているすべての連絡先データを表示できます。
* 登録済みユーザーを編集/削除が独自のデータ。
* 管理者では、承認したり、連絡先データを拒否することができます。 `Details`ビューには、**承認**と**拒否**ボタン。
* および管理者が承認または却下とすべてのデータを編集/削除します。

| ユーザー| オプション |
| ------------ | ---------|
| test@example.com | データの編集、または削除できます。 |
| manager@contoso.com | 承認または拒否し、編集/削除を所有できるデータ |
| admin@contoso.com | 編集/削除してすべてのデータの承認または却下|

管理者のブラウザーで連絡先を作成します。 削除の URL をコピーし、管理者の連絡先から編集します。 これらのリンクをテスト ユーザーは、これらの操作を実行できないことを確認するテスト ユーザーのブラウザーに貼り付けます。

## <a name="create-the-starter-app"></a>スターター アプリを作成します。

* "ContactManager"をという名前の Razor ページ アプリを作成します。

  * 使用してアプリを作成する**個々 のユーザー アカウント**です。
  * 名前を付けます"ContactManager"ので、名前空間のサンプルで使用される名前空間に一致します。

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld`SQLite ではなく LocalDB を指定します

* 次の追加`Contact`モデル。

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Scaffold、`Contact`モデル。

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* 更新プログラム、 **ContactManager**でアンカー、 *Pages/_Layout.cshtml*ファイル。

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* 初期の移行をスキャフォールディングして、データベースの更新します。

```console
dotnet ef migrations add initial
dotnet ef database update
```

* アプリをテストするには、作成、編集、および連絡先を削除します。

### <a name="seed-the-database"></a>データベースのシード

追加、`SeedData`クラスを*データ*フォルダーです。 サンプル、ダウンロードした場合は、コピー、 *SeedData.cs*ファイルの名前を*データ*のスタート プロジェクトのフォルダーです。

呼び出す`SeedData.Initialize`から`Main`:

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

アプリに、データベースがシード処理をテストします。 DB の連絡先に任意の行がある場合は、シード メソッドは実行されません。

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)です。 このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。
* [ASP.NET Core での承認: 単純な場合、ロール、クレームに基づく、およびカスタム](xref:security/authorization/index)
* [カスタム ポリシー ベースの承認](xref:security/authorization/policies)
