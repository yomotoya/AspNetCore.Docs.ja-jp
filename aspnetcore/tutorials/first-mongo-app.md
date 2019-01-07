---
title: ASP.NET Core と MongoDB で Web API を構築する
author: prkhandelwal
description: このチュートリアルは、MongoDB NoSQL データベースを使用して ASP.NET Core Web API を構築する方法を説明します。
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577839"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="c8a50-103">ASP.NET Core と MongoDB で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="c8a50-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="c8a50-104">作成者: [Pratik Khandelwal](https://twitter.com/K2Prk) および [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c8a50-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c8a50-105">このチュートリアルでは、[MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL データベース上で Create、Read、Update、Delete の各操作 (CRUD) を実行するWeb API を作成します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="c8a50-106">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c8a50-107">MongoDB を構成する</span><span class="sxs-lookup"><span data-stu-id="c8a50-107">Configure MongoDB</span></span>
> * <span data-ttu-id="c8a50-108">MongoDB データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="c8a50-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="c8a50-109">MongoDB のコレクションとスキーマを定義する</span><span class="sxs-lookup"><span data-stu-id="c8a50-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="c8a50-110">Web API から MongoDB CRUD 操作を実行する</span><span class="sxs-lookup"><span data-stu-id="c8a50-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="c8a50-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c8a50-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8a50-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c8a50-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8a50-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8a50-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="c8a50-114">.NET Core SDK 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="c8a50-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="c8a50-115">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) バージョン 15.9 以降</span><span class="sxs-lookup"><span data-stu-id="c8a50-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="c8a50-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c8a50-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8a50-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8a50-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="c8a50-118">.NET Core SDK 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="c8a50-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c8a50-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8a50-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="c8a50-120">Visual Studio Code 用 C#</span><span class="sxs-lookup"><span data-stu-id="c8a50-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="c8a50-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c8a50-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8a50-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8a50-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="c8a50-123">.NET Core SDK 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="c8a50-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c8a50-124">Visual Studio for Mac バージョン 7.7 以降</span><span class="sxs-lookup"><span data-stu-id="c8a50-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="c8a50-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c8a50-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="c8a50-126">MongoDB を構成する</span><span class="sxs-lookup"><span data-stu-id="c8a50-126">Configure MongoDB</span></span>

<span data-ttu-id="c8a50-127">Windows を使用する場合、既定では MongoDB は *C:\Program Files\MongoDB* にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="c8a50-128">*C:\Program Files\MongoDB\Server\<version_number>\bin* を `Path` 環境変数に追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="c8a50-129">この変更により、開発用コンピューターのどこからでも MongoDB にアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="c8a50-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="c8a50-130">次の手順では mongo シェルを使用して、データベースを作成し、コレクションを作成し、ドキュメントを保存します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="c8a50-131">mongo のシェル コマンドについて詳しくは、「[Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)」(mongo シェルの使用) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c8a50-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="c8a50-132">データを格納するために開発用コンピューター上のディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="c8a50-133">たとえば、Windows では *C:\BooksData* です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="c8a50-134">存在しない場合はディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="c8a50-135">mongo シェルでは新しいディレクトリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="c8a50-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="c8a50-136">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-136">Open a command shell.</span></span> <span data-ttu-id="c8a50-137">次のコマンドを実行して、既定のポート 27017 で MongoDB に接続します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="c8a50-138">忘れずに、前の手順で選択したディレクトリで `<data_directory_path>` を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="c8a50-139">別のコマンド シェル インスタンスを開きます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-139">Open another command shell instance.</span></span> <span data-ttu-id="c8a50-140">次のコマンドを実行して、既定のテスト データベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="c8a50-141">コマンド シェルで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="c8a50-142">これがまだ存在していない場合、*BookstoreDb* という名前のデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="c8a50-143">データベースが存在する場合は、トランザクションのために接続されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="c8a50-144">次のコマンドを使用して `Books` コレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="c8a50-145">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="c8a50-146">次のコマンドを使用して、`Books` コレクションのスキーマを定義し、2 つのドキュメントを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="c8a50-147">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="c8a50-148">次のコマンドを使用して、データベース内のドキュメントを表示します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="c8a50-149">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-149">The following result is displayed:</span></span>

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    <span data-ttu-id="c8a50-150">スキーマによって、自動生成された型が `ObjectId` の `_id` プロパティが各ドキュメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="c8a50-151">データベースの準備ができました。</span><span class="sxs-lookup"><span data-stu-id="c8a50-151">The database is ready.</span></span> <span data-ttu-id="c8a50-152">ASP.NET Core Web API の作成を開始できます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="c8a50-153">ASP.NET Core Web API プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="c8a50-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8a50-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8a50-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c8a50-155">**[ファイル]** > **[新規作成]** > **[プロジェクト]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="c8a50-156">**[ASP.NET Core Web アプリケーション]** を選択し、プロジェクト名を「*BooksApi*」として、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="c8a50-157">**[.NET Core]** ターゲット フレームワークと **[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="c8a50-158">**[API]** プロジェクト テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="c8a50-159">**[パッケージ マネージャー コンソール]** ウィンドウで、プロジェクトのルートに移動します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="c8a50-160">次のコマンドを実行して、MongoDB 用の .NET ドライバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8a50-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8a50-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="c8a50-162">コマンド シェルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="c8a50-163">.NET Core をターゲットとする新しい ASP.NET Core Web API プロジェクトが生成され、Visual Studio Code で開きます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="c8a50-164">**[はい]** をクリックするのは、*[Required assets to build and debug are missing from 'BooksApi'.Add them?]\(構築とデバッグに必要なアセットが 'BooksApi' にありません。追加しますか?\)* という通知が表示された場合です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="c8a50-165">**[統合端末]** を開き、プロジェクトのルートに移動します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="c8a50-166">次のコマンドを実行して、MongoDB 用の .NET ドライバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8a50-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8a50-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c8a50-168">**[ファイル]** > **[新しいソリューション]** > **[.NET Core]** > **[アプリ]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="c8a50-169">**[ASP.NET Core Web API]** C# プロジェクト テンプレートを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="c8a50-170">**[ターゲット フレームワーク]** ドロップダウン リストで **[.NET Core 2.2]** を選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="c8a50-171">**[プロジェクト名]** に「*BooksApi*」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="c8a50-172">**[ソリューション]** パッドで、プロジェクトの **[依存関係]** ノードを右クリックし、**[パッケージを追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="c8a50-173">検索ボックスに「*MongoDB.Driver*」と入力し、*MongoDB.Driver* パッケージを選択して、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="c8a50-174">**[ライセンスへの同意]** ダイアログの **[承諾]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="c8a50-175">モデルを追加する</span><span class="sxs-lookup"><span data-stu-id="c8a50-175">Add a model</span></span>

1. <span data-ttu-id="c8a50-176">*Models* ディレクトリをプロジェクトのルートに追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="c8a50-177">次のコードを使用して、`Book` クラスを *Models* ディレクトリに追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="c8a50-178">共通言語ランタイム (CLR) オブジェクトを MongoDB コレクションにマッピングするには、上記のクラスに `Id` プロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="c8a50-179">クラスのその他のプロパティは、`[BsonElement]` 属性で修飾されています。</span><span class="sxs-lookup"><span data-stu-id="c8a50-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="c8a50-180">属性の値は、MongoDB コレクションでのプロパティ名を表します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="c8a50-181">CRUD 操作のクラスを追加する</span><span class="sxs-lookup"><span data-stu-id="c8a50-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="c8a50-182">*Services* ディレクトリをプロジェクトのルートに追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="c8a50-183">次のコードを使用して、`BookService` クラスを *Services* ディレクトリに追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="c8a50-184">MongoDB の接続文字列を *appsettings.json* に追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="c8a50-185">上記の `BookstoreDb` プロパティは `BookService` クラス コンストラクターでアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="c8a50-186">`Startup.ConfigureServices` で、`BookService` クラスを依存関係挿入システムに登録します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="c8a50-187">上記のサービスの登録は、コンシューマー クラスでコンス トラクターの挿入をサポートするために必要です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="c8a50-188">`BookService` クラスは次の `MongoDB.Driver` メンバーを使用して、データベースに対する CRUD 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="c8a50-189">`MongoClient` &ndash; データベース操作を実行するためにサーバー インスタンスを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="c8a50-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="c8a50-190">このクラスのコンストラクターには MongoDB 接続文字列が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="c8a50-191">`IMongoDatabase` &ndash; 操作を実行する Mongo データベースを表します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="c8a50-192">このチュートリアルでは、インターフェイスで汎用の `GetCollection<T>(collection)` メソッドを使用し、特定のコレクション内のデータへのアクセスを取得します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="c8a50-193">CRUD 操作をこのコレクションに対して実行できるのは、このメソッドが呼び出された後です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="c8a50-194">`GetCollection<T>(collection)` メソッドの呼び出しの内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c8a50-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="c8a50-195">`collection` はコレクション名です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="c8a50-196">`T` はコレクションに格納されている CLR オブジェクト型です。</span><span class="sxs-lookup"><span data-stu-id="c8a50-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="c8a50-197">`GetCollection<T>(collection)` は、コレクションを表す `MongoCollection` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="c8a50-198">このチュートリアルでは、コレクションに対して次のメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="c8a50-199">`Find<T>` &ndash; 指定された検索基準と一致するコレクション内のすべてのドキュメントを返します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="c8a50-200">`InsertOne` &ndash; 指定されたオブジェクトを新しいドキュメントとしてコレクションに挿入します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="c8a50-201">`ReplaceOne` &ndash; 指定された検索基準と一致する 1 つのドキュメントを、指定されたオブジェクトで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="c8a50-202">`DeleteOne` &ndash; 指定された検索基準と一致する 1 つのドキュメントを削除します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="c8a50-203">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="c8a50-203">Add a controller</span></span>

1. <span data-ttu-id="c8a50-204">次のコードを使用して、`BooksController` クラスを *Controllers* ディレクトリに追加します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="c8a50-205">上記の Web API コントローラー:</span><span class="sxs-lookup"><span data-stu-id="c8a50-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="c8a50-206">`BookService` クラスを使用して CRUD 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="c8a50-207">GET、POST、PUT、DELETE HTTP 要求をサポートするアクション メソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="c8a50-208">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="c8a50-208">Build and run the app.</span></span>
1. <span data-ttu-id="c8a50-209">ブラウザーで `http://localhost:<port>/api/books` にナビゲートします。</span><span class="sxs-lookup"><span data-stu-id="c8a50-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="c8a50-210">次の JSON 応答が示されます。</span><span class="sxs-lookup"><span data-stu-id="c8a50-210">The following JSON response is displayed:</span></span>

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a><span data-ttu-id="c8a50-211">次の手順</span><span class="sxs-lookup"><span data-stu-id="c8a50-211">Next steps</span></span>

<span data-ttu-id="c8a50-212">ASP.NET Core Web API の構築について詳しくは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c8a50-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
