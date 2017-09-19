---
title: "認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。"
author: rick-anderson
keywords: "ASP.NET Core、MVC、承認、ロール、セキュリティ、管理者"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 889fe24b21f2d5cb6439b16e8f0c5c6adc9485f8
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)

このチュートリアルでは、認証によって保護されているユーザー データと web アプリを作成する方法を示します。 (登録済み) のユーザーを認証した連絡先の一覧を表示を作成します。 これには次の 3 つのセキュリティ グループがあります。

* 登録済みのユーザーには、承認されているすべての連絡先データを表示できます。
* 登録済みユーザーを編集/削除が独自のデータ。 
* 管理者では、承認したり、連絡先データを拒否することができます。 許可されている連絡先のみがユーザーに表示されます。
* および管理者が承認または却下とすべてのデータを編集/削除します。

次の図では、ユーザー Rick (`rick@example.com`) 署名します。 ユーザー Rick ことができますのみビュー連絡先を承認し、連絡先を編集/削除します。 のみ、最後のレコードは、Rick によって作成され、編集を表示のリンクを削除

![上記で説明したイメージ](secure-data/_static/rick.png)

次の図の`manager@contoso.com`とマネージャーの役割では署名されています。 

![上記で説明したイメージ](secure-data/_static/manager1.png)

次の図は、連絡先の詳細ビューをマネージャーには。

![上記で説明したイメージ](secure-data/_static/manager.png)

管理者と管理者のみ、承認があるし、ボタンを拒否します。

次の図の`admin@contoso.com`と管理者の役割では署名されています。 

![上記で説明したイメージ](secure-data/_static/admin.png)

管理者は、すべての特権を持ちます。 彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。

アプリがによって作成された[スキャフォールディング](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)次`Contact`モデル。

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

A`ContactIsOwnerAuthorizationHandler`承認ハンドラーにより、ユーザーがそのデータを編集できるのみです。 A`ContactManagerAuthorizationHandler`承認ハンドラーは、承認または却下の連絡先のマネージャーを使用します。  A`ContactAdministratorsAuthorizationHandler`承認ハンドラーは、管理者の承認または拒否の連絡先を編集/削除の連絡先を使用します。 

## <a name="prerequisites"></a>必須コンポーネント

これは最初のチュートリアルではありません。 理解しておく必要があります。

* [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter および完成したアプリ

[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final)アプリ。 [テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解しておくようにします。 

### <a name="the-starter-app"></a>スターター アプリ

完成したサンプル コードを比較することをお勧めします。

[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter)アプリ。 

参照してください[スターター アプリを作成する](#create-the-starter-app)をゼロから作成したい場合。

データベースを更新します。

```none
   dotnet ef database update
```

アプリを実行する、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。

このチュートリアルでは、すべての主要な手順、セキュリティで保護されたユーザー データのアプリを作成するには。 完成したプロジェクトを参照すると便利です。

## <a name="modify-the-app-to-secure-user-data"></a>ユーザー データを保護するアプリを変更します。

次のセクションでは、セキュリティで保護されたユーザー データのアプリを作成するすべての主要な手順があります。 完成したプロジェクトを参照すると便利です。

### <a name="tie-the-contact-data-to-the-user"></a>ユーザーに連絡先データを関連付ける

ASP.NET を使用して[Identity](xref:security/authentication/identity)のユーザーのユーザー ID を編集できますが、そのデータを他のユーザー データは表示されません。 追加`OwnerID`を`Contact`モデル。

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。 `Status`フィールドは、連絡先が一般のユーザーによって表示可能であるかどうか。 

新しい移行をスキャフォールディングして、データベースの更新します。

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>SSL と認証済みユーザーが必要

`ConfigureServices`のメソッド、 *Startup.cs*ファイルに追加し、 [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)承認フィルター。

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Visual Studio を使用している場合は、次を参照してください。 [SSL または HTTPS の IIS Express をセットアップ](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps)です。 HTTP 要求を HTTPS にリダイレクトするを参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。 Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。

- 設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。

### <a name="require-authenticated-users"></a>ユーザーが認証されている必要があります。

ユーザー認証を必要とする既定の認証ポリシーを設定します。 選択を解除とコント ローラーまたはアクション メソッドに認証する、`[AllowAnonymous]`属性。 この方法で、新しいコント ローラーを追加が自動的に認証が必要に含める新しいコント ローラーに依存するよりも安全である、`[Authorize]`属性。 次の追加、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

追加`[AllowAnonymous]`home コント ローラーを登録する前に、匿名ユーザーはサイトに関する情報を取得できるようにします。

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>テスト アカウントを構成します。

`SeedData`クラスは、2 つのアカウント、管理者およびマネージャーを作成します。 使用して、[シークレット マネージャー ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。 これは、プロジェクト ディレクトリから実行 (ディレクトリを含む*Program.cs*)。

```console
dotnet user-secrets set SeedUserPW <PW>
```

更新`Configure`テスト パスワードを使用します。

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

管理者のユーザー ID を追加し、`Status = ContactStatus.Approved`連絡先にします。 1 人だけが表示されると、すべての連絡先にユーザー ID を追加します。

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>所有者、マネージャー、および管理者の承認のハンドラーを作成します。

作成、`ContactIsOwnerAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactIsOwnerAuthorizationHandler`はリソースで動作しているユーザーは、リソースを所有していることを確認します。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼び出し`context.Succeed`現在の認証済みユーザーがメンバーの所有者である場合。 認証ハンドラーが通常返す`context.Succeed`要件を満たしている場合にします。 返される`Task.FromResult(0)`要件が満たされないときにします。 `Task.FromResult(0)`成功または失敗のどちらもその他の認証ハンドラーを実行することができます。 明示的に失敗する場合は、返す`context.Fail()`です。

ここには、ため、要件のパラメーターに渡す操作を確認する必要はありませんに自分のデータを編集/削除する連絡先の所有者ができるようにします。

### <a name="create-a-manager-authorization-handler"></a>マネージャーの認証ハンドラーを作成します。

作成、`ContactManagerAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactManagerAuthorizationHandler`リソースに対して機能しているユーザーは、マネージャーが確認されます。 マネージャーだけでは、承認したり、コンテンツの変更 (新しいまたは変更された) を拒否することができます。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>管理者認証ハンドラーを作成します。

作成、`ContactAdministratorsAuthorizationHandler`クラス内で、*承認*フォルダーです。 `ContactAdministratorsAuthorizationHandler`リソースに対して機能しているユーザーは管理者が確認されます。 管理者は、すべての操作を行うことができます。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>認証ハンドラーを登録します。

Entity Framework のコアを使用してサービスを登録する必要があります[依存性の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)です。 `ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。 ハンドラー コレクションに登録サービスをで使用できるように、`ContactsController`を通じて[依存性の注入](xref:fundamentals/dependency-injection)です。 末尾に次のコードを追加`ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`および`ContactManagerAuthorizationHandler`シングルトンとして追加されます。 これらはシングルトンの EF を使用していないし、では、必要なすべての情報が、`Context`のパラメーター、`HandleRequirementAsync`メソッドです。

完全な`ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>承認をサポートするコードを更新します。

このセクションでは、コント ローラーとビューを更新し、操作の要件クラスを追加します。

### <a name="update-the-contacts-controller"></a>連絡先のコント ローラーを更新します。

更新プログラム、`ContactsController`コンス トラクター。

* 追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。 
* 追加、 `Identity` `UserManager`サービス。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>連絡先の操作の要件のクラスを追加します。

追加、`ContactOperationsRequirements`クラスを*承認*フォルダーです。 このクラスは、要件を含む、アプリケーションがサポートします。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>更新プログラムを作成します。

更新プログラム、`HTTP POST Create`メソッド。

* ユーザー ID を追加、`Contact`モデル。
* ユーザーに連絡先を所有していることを確認する認証ハンドラーを呼び出します。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>更新プログラムの編集

両方を更新`Edit`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。 リソースの承認を実行しているために使用できません、`[Authorize]`属性。 属性が評価されるときに、リソースへのアクセスはありません。 リソース ベースの承認は、命令型である必要があります。 コント ローラーに読み込むか、ハンドラー自体内で読み込むことにより、リソースへのアクセスを取得したら、チェックを実行する必要があります。 頻繁には、リソースをリソース キーを渡すことによってアクセスします。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Update、Delete メソッド

両方を更新`Delete`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>承認サービスをビューに挿入します。

現在、UI に示すは編集し、ユーザーが変更できないデータへのリンクを削除します。 認証ハンドラーをビューに適用することによって修正されます。

承認サービスでの挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに利用できるようにします。

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

更新プログラム、 *Views/Contacts/Index.cshtml* Razor ビューのみを表示、編集し編集/削除の連絡先がユーザー用のリンクを削除します。

追加`@using ContactManager.Authorization;`

更新プログラム、`Edit`と`Delete`リンクを編集および連絡先を削除するアクセス許可を持つユーザーのみレンダリングされるようにします。

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

警告: データ編集または削除するアクセス許可がないユーザーからのリンクを非表示にしても、アプリは保護しません。 リンクを非表示にするはアプリよりユーザー フレンドリによって唯一の有効なリンクを表示します。 ユーザーは、編集を呼び出すし、自分が所有しないデータの操作を削除するには、生成された Url を切断できます。  コント ローラーは、アクセスをセキュリティで保護するチェックを繰り返す必要があります。

### <a name="update-the-details-view"></a>更新プログラムの詳細ビュー

マネージャーが承認または連絡先を拒否するため、詳細ビューを更新します。

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>完成したアプリをテストします。

Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。

- 設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。

アプリを実行した連絡先がある場合は、内のすべてのレコードを削除、`Contact`テーブルが表示され、データベースのシードにアプリを再起動します。 Visual Studio を使用している場合は、終了して、データベースのシードに IIS Express を再起動する必要があります。

連絡先を参照するユーザーを登録します。

完成したアプリをテストする簡単な方法では、3 つの異なるブラウザー (または incognito/inprivate ブラウズのバージョン) を起動します。 1 つのブラウザーで新しいユーザーの例では、登録`test@example.com`です。 別のユーザーに各ブラウザーにサインインします。 次のことを検証します。

* 登録済みのユーザーには、承認されているすべての連絡先データを表示できます。
* 登録済みユーザーを編集/削除が独自のデータ。 
* 管理者では、承認したり、連絡先データを拒否することができます。 `Details`ビューには、**承認**と**拒否**ボタン。 
* および管理者が承認または却下とすべてのデータを編集/削除します。

| ユーザー| オプション |
| ------------ | ---------|
| test@example.com | データの編集、または削除できます。 |
| manager@contoso.com | 承認または拒否し、編集/削除を所有できるデータ  |
| admin@contoso.com | 編集/削除してすべてのデータの承認または却下|

管理者のブラウザーで連絡先を作成します。 削除の URL をコピーし、管理者の連絡先から編集します。 これらのリンクをテスト ユーザーは、これらの操作を実行できないことを確認するテスト ユーザーのブラウザーに貼り付けます。

## <a name="create-the-starter-app"></a>スターター アプリを作成します。

スタート アプリケーションを作成する手順に従います。

* 作成、 **ASP.NET Core Web アプリケーション**を使用して[Visual Studio 2017](https://www.visualstudio.com/) "ContactManager"という名前

  * 使用してアプリを作成する**個々 のユーザー アカウント**です。
  * 名前を付けます"ContactManager"名前空間は、名前空間で使用するサンプルと一致するようにします。

* 次の追加`Contact`モデル。

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Scaffold、`Contact`エンティティ フレームワークのコアを使用するモデルと`ApplicationDbContext`データ コンテキスト。 すべてのスキャフォールディング既定値をそのまま使用します。 使用して`ApplicationDbContext`データ コンテキストのクラス、contact テーブルに、 [Identity](xref:security/authentication/identity)データベース。 参照してください[モデルを追加する](xref:tutorials/first-mvc-app/adding-model)詳細についてはします。

* 更新プログラム、 **ContactManager**でアンカー、 *Views/Shared/_Layout.cshtml*ファイルから`asp-controller="Home"`に`asp-controller="Contacts"`ためをタップすると、 **ContactManager**リンク連絡先コント ローラーを呼び出します。 元のマークアップ。

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

更新されたマークアップ:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* 初期の移行をスキャフォールディングして、データベースの更新

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* アプリをテストするには、作成、編集、および連絡先を削除します。

### <a name="seed-the-database"></a>データベースのシード

追加、`SeedData`クラスを*データ*フォルダーです。 サンプル、ダウンロードした場合は、コピー、 *SeedData.cs*ファイルの名前を*データ*のスタート プロジェクトのフォルダーです。

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

末尾に強調表示されたコードを追加、`Configure`メソッドで、 *Startup.cs*ファイル。

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

アプリに、データベースがシード処理をテストします。 Seed メソッドは、DB の連絡先のすべての行がある場合に実行されません。

### <a name="create-a-class-used-in-the-tutorial"></a>このチュートリアルで使用されるクラスを作成します。

* という名前のフォルダーを作成する*承認*です。
* コピー、 *Authorization\ContactOperations.cs*完成したプロジェクトのダウンロードからファイルまたは次のコードをコピーします。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)です。 このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。
* [ASP.NET Core での承認: 単純な場合、ロール、および信頼性情報ベースのカスタム](index.md)
* [カスタム ポリシー ベースの承認](policies.md)
