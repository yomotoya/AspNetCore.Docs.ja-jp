---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 接続文字列を作成し、SQL Server LocalDB の使用 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d6e04de9b1d71a77b4b56b04417d6d4bc24cbd72
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361669"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="15786-102">接続文字列を作成し、SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="15786-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="15786-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="15786-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="15786-104">接続文字列を作成し、SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="15786-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="15786-105">`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="15786-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="15786-106">疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="15786-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="15786-107">使用するデータベースを指定することも実際には、Entity Framework は既定で使用する[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)します。</span><span class="sxs-lookup"><span data-stu-id="15786-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="15786-108">このセクションでは明示的に追加します内の接続文字列、 *Web.config*アプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="15786-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="15786-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="15786-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="15786-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)は、SQL Server Express データベース エンジンの要求時に開始し、ユーザー モードで実行される軽量バージョンです。</span><span class="sxs-lookup"><span data-stu-id="15786-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="15786-111">LocalDB は SQL Server Express データベースとして使用することができますの特殊な実行モードで実行 *.mdf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="15786-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="15786-112">LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="15786-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="15786-113">SQL Server Express は、実稼働 web アプリケーションでの使用は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="15786-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="15786-114">LocalDB 特に使わないで web アプリケーションを運用環境のため、IIS を使用するものではありません。</span><span class="sxs-lookup"><span data-stu-id="15786-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="15786-115">ただし、SQL Server または SQL Azure に LocalDB データベースを簡単に移行できます。</span><span class="sxs-lookup"><span data-stu-id="15786-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="15786-116">Visual Studio 2017 では、LocalDB は、Visual Studio では既定でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="15786-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="15786-117">既定では、Entity Framework は、オブジェクト コンテキスト クラスと同じ名前の接続文字列の検索 (`MovieDBContext`このプロジェクトの)。</span><span class="sxs-lookup"><span data-stu-id="15786-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="15786-118">詳細については、次を参照してください。 [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="15786-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="15786-119">アプリケーションのルートを開く*Web.config*次に示すファイル。</span><span class="sxs-lookup"><span data-stu-id="15786-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="15786-120">(いない、 *Web.config*ファイル、*ビュー*フォルダー)。</span><span class="sxs-lookup"><span data-stu-id="15786-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="15786-121">検索、`<connectionStrings>`要素。</span><span class="sxs-lookup"><span data-stu-id="15786-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="15786-122">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="15786-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="15786-123">次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="15786-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="15786-124">2 つの接続文字列は、非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="15786-124">The two connection strings are very similar.</span></span> <span data-ttu-id="15786-125">最初の接続文字列が名前付き`DefaultConnection`し、アプリケーションにアクセスできるユーザーを制御するメンバーシップ データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="15786-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="15786-126">追加した接続文字列をという名前の LocalDB データベースを指定します*Movie.mdf*内にある、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="15786-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="15786-127">メンバーシップ、認証およびセキュリティの詳細については、このチュートリアルでは、メンバーシップ データベースを使用して、拙著のチュートリアルを参照してくださいしません[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。</span><span class="sxs-lookup"><span data-stu-id="15786-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="15786-128">接続文字列の名前の名前に一致する必要があります、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス。</span><span class="sxs-lookup"><span data-stu-id="15786-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="15786-129">実際に追加する必要はありません、`MovieDBContext`接続文字列。</span><span class="sxs-lookup"><span data-stu-id="15786-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="15786-130">Entity Framework がユーザーのディレクトリの完全修飾名に LocalDB データベースを作成、接続文字列を指定しない場合、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス (ここで`MvcMovie.Models.MovieDBContext`)。</span><span class="sxs-lookup"><span data-stu-id="15786-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="15786-131">ことがデータベースの名前、好きなデザインがある限り、*します。MDF*サフィックス。</span><span class="sxs-lookup"><span data-stu-id="15786-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="15786-132">たとえば、データベースを名前*MyFilms.mdf*します。</span><span class="sxs-lookup"><span data-stu-id="15786-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="15786-133">新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="15786-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15786-134">[前へ](adding-a-model.md)
> [次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="15786-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
