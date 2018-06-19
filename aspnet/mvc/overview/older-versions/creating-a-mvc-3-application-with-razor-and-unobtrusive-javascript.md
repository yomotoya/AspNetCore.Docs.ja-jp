---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: 作成した MVC 3 Razor および控えめな JavaScript を持つアプリケーション |Microsoft ドキュメント
author: microsoft
description: ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。 サンプル アプリケーション s.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30874695"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="77b15-104">作成した MVC 3 Razor および控えめな JavaScript を持つアプリケーション</span><span class="sxs-lookup"><span data-stu-id="77b15-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="77b15-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="77b15-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="77b15-106">ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="77b15-107">サンプル アプリケーションでは、ASP.NET MVC version 3 での新しい Razor ビュー エンジンおよび Visual Studio 2010 を使用して、作成、表示、編集、削除するユーザーなどの機能が含まれています、架空ユーザー一覧 web サイトを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="77b15-108">このチュートリアルでは、ユーザーの一覧のサンプルの ASP.NET MVC 3 アプリケーションをビルドするために実行された手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="77b15-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="77b15-109">このトピックは、Visual Studio プロジェクトと c# および VB のソース コード:[ダウンロード](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)です。</span><span class="sxs-lookup"><span data-stu-id="77b15-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="77b15-110">このチュートリアルに関して質問がある場合は、投稿してくださいに、 [MVC フォーラム](https://forums.asp.net/1146.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="77b15-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="77b15-111">概要</span><span class="sxs-lookup"><span data-stu-id="77b15-111">Overview</span></span>

<span data-ttu-id="77b15-112">構築するアプリケーションは、単純なユーザー リストの web サイトです。</span><span class="sxs-lookup"><span data-stu-id="77b15-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="77b15-113">ユーザーは入力、表示、およびユーザー情報を更新します。</span><span class="sxs-lookup"><span data-stu-id="77b15-113">Users can enter, view, and update user information.</span></span>

![サンプル サイト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="77b15-115">VB と c# の完成したプロジェクトをダウンロードする[ここ](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)です。</span><span class="sxs-lookup"><span data-stu-id="77b15-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="77b15-116">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="77b15-116">Creating the Web Application</span></span>

<span data-ttu-id="77b15-117">チュートリアルを開始する Visual Studio 2010 を開き、使用して新しいプロジェクトを作成、 *ASP.NET MVC 3 Web アプリケーション*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="77b15-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="77b15-118">アプリケーションの名前&quot;Mvc3Razor&quot;です。</span><span class="sxs-lookup"><span data-stu-id="77b15-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="77b15-119">[![新しい MVC 3 プロジェクト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="77b15-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="77b15-120">**新しい ASP.NET MVC 3 プロジェクト**ダイアログで、**インターネット アプリケーション**Razor ビュー エンジンを選択して、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![新しい ASP.NET MVC 3 プロジェクト] ダイアログ ボックス](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="77b15-122">このチュートリアルではない使用する ASP.NET メンバーシップ プロバイダー ログオンおよびメンバーシップに関連付けられているすべてのファイルを削除できるようにします。</span><span class="sxs-lookup"><span data-stu-id="77b15-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="77b15-123">**ソリューション エクスプ ローラー**、次のファイルとディレクトリを削除します。</span><span class="sxs-lookup"><span data-stu-id="77b15-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="77b15-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="77b15-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="77b15-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="77b15-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="77b15-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="77b15-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="77b15-127">*Views\Account* (およびこのディレクトリ内のすべてのファイル)</span><span class="sxs-lookup"><span data-stu-id="77b15-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="77b15-129">編集、 <em> \_Layout.cshtml</em>ファイルし、内部のマークアップを置換、`<div>`という名前の要素`logindisplay`メッセージ<em> &quot;</em>ログイン無効&quot;.</span><span class="sxs-lookup"><span data-stu-id="77b15-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="77b15-130">次の例では、新しいマークアップを示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="77b15-131">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="77b15-131">Adding the Model</span></span>

<span data-ttu-id="77b15-132">**ソリューション エクスプ ローラー**を右クリックし、*モデル*フォルダーを選択**追加**、クリックして**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![新しいユーザー Mdl クラス](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="77b15-134">クラスに `UserModel` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="77b15-134">Name the class `UserModel`.</span></span> <span data-ttu-id="77b15-135">内容を置き換える、 *UserModel*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="77b15-136">`UserModel`クラスは、ユーザーを表します。</span><span class="sxs-lookup"><span data-stu-id="77b15-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="77b15-137">クラスの各メンバーの注釈が付いて、[必要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性から、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="77b15-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="77b15-138">内の属性、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、web アプリケーションの自動クライアントとサーバー側の検証を提供します。</span><span class="sxs-lookup"><span data-stu-id="77b15-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="77b15-139">開く、`HomeController`クラスを追加、`using`アクセスできるようにするディレクティブ、`UserModel`と`Users`クラス。</span><span class="sxs-lookup"><span data-stu-id="77b15-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="77b15-140">直後、`HomeController`宣言では、次のコメントおよびへの参照を追加、`Users`クラス。</span><span class="sxs-lookup"><span data-stu-id="77b15-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="77b15-141">`Users`クラスはこのチュートリアルで使用するシンプルなメモリ内データ ストアです。</span><span class="sxs-lookup"><span data-stu-id="77b15-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="77b15-142">実際のアプリケーションでは、ユーザー情報を格納するのにデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="77b15-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="77b15-143">最初の数行、`HomeController`ファイルは、次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="77b15-144">ユーザー モデルを次の手順でスキャフォールディング ウィザードを使用できるようにアプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="77b15-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="77b15-145">既定のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="77b15-145">Creating the Default View</span></span>

<span data-ttu-id="77b15-146">次の手順では、アクション メソッドと、ユーザーを表示するビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="77b15-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="77b15-147">既存の削除*Views\Home\Index*ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="77b15-148">新規に作成する、*インデックス*ファイルが、ユーザーを表示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="77b15-149">`HomeController`クラスの内容を交換してください、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="77b15-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="77b15-150">内部を右クリックし、`Index`メソッドをクリックして**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="77b15-152">選択、**厳密に型指定されたビューを作成する**オプション。</span><span class="sxs-lookup"><span data-stu-id="77b15-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="77b15-153">**データ クラスを表示**[ **Mvc3Razor.Models.UserModel**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="77b15-154">(表示されない場合**Mvc3Razor.Models.UserModel**で、**データ クラスを表示**ボックスで、プロジェクトをビルドする必要があります)。ビュー エンジンに設定されていることを確認してください**Razor**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="77b15-155">設定**コンテンツを表示**に**リスト**] をクリックし、**追加**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-155">Set **View content** to **List** and then click **Add**.</span></span>

![インデックス ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="77b15-157">新しいビューが自動的に渡されるユーザー データを scaffolds、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="77b15-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="77b15-158">新しく生成された確認*Views\Home\Index*ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="77b15-159">**新規作成**、**編集**、**詳細**、および**削除**リンクが機能しませんが、ページの残りの部分は機能します。</span><span class="sxs-lookup"><span data-stu-id="77b15-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="77b15-160">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="77b15-160">Run the page.</span></span> <span data-ttu-id="77b15-161">ユーザーの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77b15-161">You see a list of users.</span></span>

![ページをインデックスします。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="77b15-163">開く、 *Index.cshtml*ファイルし、置換、`ActionLink`のマークアップ**編集**、**詳細**、および**削除**を次のコード:</span><span class="sxs-lookup"><span data-stu-id="77b15-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="77b15-164">選択したレコードを検索するユーザー名が ID として使用が、**編集**、**詳細**、および**削除**リンクします。</span><span class="sxs-lookup"><span data-stu-id="77b15-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="77b15-165">詳細ビューの作成</span><span class="sxs-lookup"><span data-stu-id="77b15-165">Creating the Details View</span></span>

<span data-ttu-id="77b15-166">次の手順が追加するには、`Details`アクション メソッドとユーザーの詳細を表示するために表示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="77b15-168">次の追加`Details`メソッドを home コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="77b15-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="77b15-169">内部を右クリックし、`Details`メソッドと、選択<strong>ビューの追加</strong>です。</span><span class="sxs-lookup"><span data-stu-id="77b15-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="77b15-170">いることを確認、<strong>データ クラスを表示</strong>ボックスには<strong>Mvc3Razor.Models.UserModel</strong><em>です。</em></span><span class="sxs-lookup"><span data-stu-id="77b15-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="77b15-171">設定<strong>コンテンツを表示</strong>に<strong>詳細</strong>] をクリックし、<strong>追加</strong>です。</span><span class="sxs-lookup"><span data-stu-id="77b15-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![詳細ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="77b15-173">アプリケーションを実行し、詳細情報のリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="77b15-173">Run the application and select a details link.</span></span> <span data-ttu-id="77b15-174">自動のスキャフォールディングでは、モデルの各プロパティを示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-174">The automatic scaffolding shows each property in the model.</span></span>

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="77b15-176">編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="77b15-176">Creating the Edit View</span></span>

<span data-ttu-id="77b15-177">次の追加`Edit`メソッド、home コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="77b15-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="77b15-178">前の手順と同様にビューを追加するが、設定**コンテンツを表示**に**編集**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![編集ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="77b15-180">アプリケーションを実行し、ユーザーの 1 つの最初と最後の名前を編集します。</span><span class="sxs-lookup"><span data-stu-id="77b15-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="77b15-181">いずれかに違反した場合`DataAnnotation`に適用されている制約、`UserModel`クラス、フォームを送信するときに検証エラーが発生するサーバー コードによって生成されます。</span><span class="sxs-lookup"><span data-stu-id="77b15-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="77b15-182">たとえば、最初の名前を変更する&quot;Ann&quot;に&quot;A&quot;フォームを送信するときに、フォームに次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="77b15-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="77b15-183">このチュートリアルでは、主キーとしてユーザー名を扱うしています。</span><span class="sxs-lookup"><span data-stu-id="77b15-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="77b15-184">そのため、ユーザー名プロパティを変更できません。</span><span class="sxs-lookup"><span data-stu-id="77b15-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="77b15-185">*Edit.cshtml*ファイル直後、`Html.BeginForm`ステートメントでは、隠しフィールドであるユーザー名を設定します。</span><span class="sxs-lookup"><span data-stu-id="77b15-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="77b15-186">これにより、モデルに渡されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="77b15-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="77b15-187">次のコード フラグメントの位置を示しています、`Hidden`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="77b15-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="77b15-188">置換、`TextBoxFor`と`ValidationMessageFor`マークアップをユーザー名と、`DisplayFor`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="77b15-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="77b15-189">`DisplayFor`メソッドは、読み取り専用の要素として、プロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="77b15-190">次の例は、完全なマークアップを示しています。</span><span class="sxs-lookup"><span data-stu-id="77b15-190">The following example shows the completed markup.</span></span> <span data-ttu-id="77b15-191">元の`TextBoxFor`と`ValidationMessageFor`Razor begin コメント終了コメント文字を使って呼び出しをコメント アウト (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="77b15-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="77b15-192">クライアント側の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="77b15-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="77b15-193">ASP.NET MVC 3 でのクライアント側の検証を有効にするには、2 つのフラグを設定する必要があり、次の 3 つの JavaScript ファイルをインクルードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="77b15-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="77b15-194">アプリケーションの開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="77b15-195">確認`that ClientValidationEnabled`と`UnobtrusiveJavaScriptEnabled`に設定されているアプリケーションの設定では true です。</span><span class="sxs-lookup"><span data-stu-id="77b15-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="77b15-196">次のフラグメント ルートから*Web.config*ファイルが、正しい設定を示しています。</span><span class="sxs-lookup"><span data-stu-id="77b15-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="77b15-197">設定`UnobtrusiveJavaScriptEnabled`有効控えめな Ajax および控えめなクライアント検証は、true にします。</span><span class="sxs-lookup"><span data-stu-id="77b15-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="77b15-198">控えめな検証を使用する場合、検証規則が HTML5 属性に変換します。</span><span class="sxs-lookup"><span data-stu-id="77b15-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="77b15-199">HTML5 属性名は、小文字、数字、およびダッシュのみで構成できます。</span><span class="sxs-lookup"><span data-stu-id="77b15-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="77b15-200">設定`ClientValidationEnabled`クライアント側検証の場合は true を有効にします。</span><span class="sxs-lookup"><span data-stu-id="77b15-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="77b15-201">アプリケーションでこれらのキーを設定して*Web.config*ファイル、クライアント検証と控えめな JavaScript アプリケーション全体に対して有効にします。</span><span class="sxs-lookup"><span data-stu-id="77b15-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="77b15-202">有効にするにまたは、個々 のビューまたは次のコードを使用してコント ローラーのメソッドでこれらの設定を無効にすることができますも。</span><span class="sxs-lookup"><span data-stu-id="77b15-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="77b15-203">また、レンダリングされるビューに複数の JavaScript ファイルを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="77b15-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="77b15-204">JavaScript は、すべてのビューを簡単に追加するのには、 *\shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="77b15-205">置換、`<head>`の要素、 * \_Layout.cshtml*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="77b15-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="77b15-206">最初の 2 つの jQuery スクリプトは、Microsoft Ajax コンテンツ配信ネットワーク (CDN) でホストされています。</span><span class="sxs-lookup"><span data-stu-id="77b15-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="77b15-207">Microsoft Ajax CDN を利用して、アプリケーションの初回のパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="77b15-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="77b15-208">アプリケーションを実行し、[編集] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="77b15-208">Run the application and click an edit link.</span></span> <span data-ttu-id="77b15-209">ブラウザーでページのソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-209">View the page's source in the browser.</span></span> <span data-ttu-id="77b15-210">ブラウザー ソースは、フォームの多くの属性を示しています。 `data-val` (のデータ検証)。</span><span class="sxs-lookup"><span data-stu-id="77b15-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="77b15-211">クライアント検証と控えめな JavaScript が有効にすると、クライアント検証規則とは、入力フィールドが含まれて、`data-val="true"`控えめなクライアント検証をトリガーする属性。</span><span class="sxs-lookup"><span data-stu-id="77b15-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="77b15-212">たとえば、 `City` 、モデル内のフィールドがで修飾された、[必要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性は、次の例に示すように html 形式で結果を。</span><span class="sxs-lookup"><span data-stu-id="77b15-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="77b15-213">それぞれのクライアント検証規則では、属性が追加、フォームを含む`data-val-rulename="message"`です。</span><span class="sxs-lookup"><span data-stu-id="77b15-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="77b15-214">使用して、`City`フィールド前の例で必要なクライアント検証規則を生成、`data-val-required`属性と、メッセージ&quot;、市区町村] フィールドは必須&quot;です。</span><span class="sxs-lookup"><span data-stu-id="77b15-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="77b15-215">アプリケーションを実行はユーザーの 1 つを編集およびオフ、`City`フィールドです。</span><span class="sxs-lookup"><span data-stu-id="77b15-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="77b15-216">フィールドから移動するとき、クライアント側の検証エラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="77b15-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![必要な市区町村](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="77b15-218">同様に、クライアント検証規則内の各パラメーターの属性を追加、フォームを含む`data-val-rulename-paramname=paramvalue`です。</span><span class="sxs-lookup"><span data-stu-id="77b15-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="77b15-219">たとえば、`FirstName`プロパティの注釈が付いて、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性および 3 の最小長と 8 の最大長を指定します。</span><span class="sxs-lookup"><span data-stu-id="77b15-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="77b15-220">という名前のデータ検証ルール`length`パラメーターの名前を持つ`max`と 8 のパラメーターの値。</span><span class="sxs-lookup"><span data-stu-id="77b15-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="77b15-221">次に示しますに対して生成される HTML、`FirstName`ユーザーを編集するときにフィールドします。</span><span class="sxs-lookup"><span data-stu-id="77b15-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="77b15-222">控えめなクライアント検証の詳細については、エントリを参照してください。 [ASP.NET MVC 3 の控えめなクライアント検証](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad Wilson のブログでします。</span><span class="sxs-lookup"><span data-stu-id="77b15-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="77b15-223">ASP.NET MVC 3 のベータ版である場合があります、クライアント側の検証を開始するために、フォームを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="77b15-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="77b15-224">これは、最終リリースの変更があります。</span><span class="sxs-lookup"><span data-stu-id="77b15-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="77b15-225">作成ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="77b15-225">Creating the Create View</span></span>

<span data-ttu-id="77b15-226">次の手順が追加するには、`Create`アクション メソッドと、新しいユーザーを作成するユーザーを有効にするために表示します。</span><span class="sxs-lookup"><span data-stu-id="77b15-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="77b15-227">次の追加`Create`メソッドを home コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="77b15-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="77b15-228">前の手順と同様にビューを追加するが、設定**コンテンツを表示**に**作成**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="77b15-230">アプリケーションの実行で、選択、**作成**リンク、および新しいユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="77b15-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="77b15-231">`Create`メソッドが自動的にクライアント側およびサーバー側の検証を活用を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="77b15-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="77b15-232">空白文字を含むユーザー名を入力しようとしています。 &quot;Ben X&quot;です。</span><span class="sxs-lookup"><span data-stu-id="77b15-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="77b15-233">ユーザー名フィールドで、クライアント側の検証エラー外] タブと (`White space is not allowed`) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="77b15-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="77b15-234">Delete メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="77b15-234">Add the Delete method</span></span>

<span data-ttu-id="77b15-235">このチュートリアルを完了するには、次のコードを追加`Delete`メソッドを home コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="77b15-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="77b15-236">追加、`Delete`設定、前の手順と同様にビュー**コンテンツを表示**に**削除**です。</span><span class="sxs-lookup"><span data-stu-id="77b15-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![ビューを削除します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="77b15-238">検証に単純ですが、完全に機能 ASP.NET MVC 3 アプリケーションがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="77b15-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
