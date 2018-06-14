---
title: 追加、ダウンロード、および ASP.NET Core プロジェクトの Id にカスタム ユーザー データの削除
author: rick-anderson
description: Id に、ASP.NET Core プロジェクトにカスタム ユーザー データを追加する方法を説明します。 GDPR ごとのデータを削除します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 3ebb709cc40f6c2477ac57325d035b9b461e2eaf
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2018
ms.locfileid: "35414995"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>追加、ダウンロード、および ASP.NET Core プロジェクトの Id にカスタム ユーザー データの削除

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ここで説明する方法。

* カスタム ユーザー データを ASP.NET Core web アプリに追加します。
* 装飾できるでは、カスタム ユーザー データのモデル、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性のダウンロードと削除に自動的に使用可能になるようにします。 満たすデータがダウンロードされ、削除できないようにするのに役立ちます[GDPR](xref:security/gdpr)要件です。

プロジェクトのサンプルは、Razor ページの web アプリから作成でも、手順は ASP.NET Core MVC web アプリケーションに似ています。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。 プロジェクトに名前を**WebApp1**にする場合の名前空間と一致、[サンプルをダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)コード。
* 選択**ASP.NET Core Web アプリケーション** > **OK**
* 選択**ASP.NET Core 2.1**ドロップダウン リストに
* 選択**Web アプリケーション**  > **OK**
* プロジェクトをビルドして実行します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a>Identity scaffolder を実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ソリューション エクスプ ローラー**、プロジェクトを右クリックして >**追加** > **スキャフォールディングされた新しい項目**です。
* 左側のウィンドウから、**追加 Scaffold**ダイアログで、 **Identity** > **追加**です。
* **追加 Identity**ダイアログ ボックスで、次のオプション。
  * 既存のレイアウト ファイルを選択して *~/Pages/Shared/_Layout.cshtml*
  * オーバーライドする次のファイルを選択します。
    * **アカウントまたは登録**
    * **アカウント//インデックスの管理**
  * 選択、 **+** を新規作成するにはボタン**データ コンテキスト クラス**です。 種類を受諾 (**WebApp1.Models.WebApp1Context**プロジェクトの名前を付けた場合**WebApp1**)。
  * 選択、 **+** を新規作成するにはボタン**ユーザー クラス**です。 種類を受諾 (**WebApp1User**プロジェクトの名前を付けた場合**WebApp1**) >**追加**です。
* 選択**追加**です。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET scaffolder を以前インストールしていない場合は、今すぐインストールします。

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

パッケージの参照を追加[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)プロジェクト (.csproj) ファイルにします。 プロジェクト ディレクトリに、次のコマンドを実行します。

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Identity scaffolder オプションの一覧を次のコマンドを実行します。

```cli
dotnet aspnet-codegenerator identity -h
```

プロジェクト フォルダーでは、Identity scaffolder を実行します。

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

手順に従って[移行、UseAuthentication、およびレイアウト](xref:security/authentication/scaffold-identity#efm)を次の手順を実行します。

* 移行を作成し、データベースを更新します。
* `UseAuthentication` に `Startup.Configure` を追加します。
* 追加`<partial name="_LoginPartial" />`レイアウト ファイルにします。
* アプリをテストします。
  * ユーザーを登録する
  * 新しいユーザー名を選択 (横、**ログアウト**リンク)。 ウィンドウを拡大またはユーザー名およびその他のリンクを表示する、ナビゲーション バーのアイコンを選択する必要があります。
  * 選択、**個人データ**タブです。
  * 選択、**ダウンロード**ボタンをクリックし、調査、 *PersonalData.json*ファイル。
  * テスト、**削除**ボタンで、ユーザーのログオンを削除します。

## <a name="add-custom-user-data-to-the-identity-db"></a>Identity DB にカスタム ユーザー データを追加します。

更新プログラム、`IdentityUser`カスタム プロパティを持つクラスを派生します。 ファイルの名前は WebApp1 プロジェクトの名前を付けた場合*Areas/Identity/Data/WebApp1User.cs*です。 次のコード ファイルを更新します。

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

修飾されたプロパティ、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性は。

* 削除されたときに、 *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor ページを呼び出す`UserManager.Delete`です。
* によってダウンロードされたデータに含まれる、 *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor ページ。

### <a name="update-the-accountmanageindexcshtml-page"></a>Account/Manage/Index.cshtml ページを更新します。

更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*次のようにコードを強調表示されます。

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

更新プログラム、 *Areas/Identity/Pages/Account/Manage/Index.cshtml*次の強調表示されているマークアップ。

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Account/Register.cshtml ページを更新します。

更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Register.cshtml.cs*次のようにコードを強調表示されます。

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

更新プログラム、 *Areas/Identity/Pages/Account/Register.cshtml*次の強調表示されているマークアップ。

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

プロジェクトをビルドします。

### <a name="add-a-migration-for-the-custom-user-data"></a>カスタム ユーザー データの移行を追加します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio で**Package Manager Console**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>テストを作成、表示、ダウンロード、カスタム ユーザー データの削除

アプリをテストします。

* 新しいユーザーを登録します。
* カスタム ユーザー データを表示、`/Identity/Account/Manage`ページ。
* ダウンロードしてから、ユーザーの個人データを表示、`/Identity/Account/Manage/PersonalData`ページ。
