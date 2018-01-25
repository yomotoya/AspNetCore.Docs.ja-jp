---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "接続文字列を作成し、SQL Server LocalDB 協力 |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d2abd-102">接続文字列を作成し、SQL Server LocalDB 協力</span><span class="sxs-lookup"><span data-stu-id="d2abd-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="d2abd-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d2abd-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d2abd-104">接続文字列を作成し、SQL Server LocalDB 協力</span><span class="sxs-lookup"><span data-stu-id="d2abd-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d2abd-105">`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d2abd-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d2abd-106">1 つに質問する可能性がありますには、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="d2abd-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d2abd-107">使用するデータベースを指定することも実際には、Entity Framework を使用するのには既定で[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)です。</span><span class="sxs-lookup"><span data-stu-id="d2abd-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="d2abd-108">このセクションで明示的に追加の接続文字列、 *Web.config*アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d2abd-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d2abd-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d2abd-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) SQL Server Express データベース エンジンの要求時に開始され、ユーザー モードで実行される軽量バージョンです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="d2abd-111">LocalDB は、SQL Server Express データベースを対象として使用することができますの特殊な実行モードで実行*.mdf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d2abd-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="d2abd-112">LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="d2abd-113">SQL Server Express は、実稼働 web アプリケーションでの使用は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="d2abd-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="d2abd-114">LocalDB 具体的には指定しないで web アプリケーションと運用環境のため、IIS を使用するものはありません。</span><span class="sxs-lookup"><span data-stu-id="d2abd-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="d2abd-115">ただし、SQL Server または SQL Azure に LocalDB データベースを簡単に移行できます。</span><span class="sxs-lookup"><span data-stu-id="d2abd-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="d2abd-116">Visual Studio 2017、LocalDB は既定では Visual Studio と共にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d2abd-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="d2abd-117">既定では、Entity Framework は、オブジェクト コンテキスト クラスと同じ名前の接続文字列の検索 (`MovieDBContext`このプロジェクトの)。</span><span class="sxs-lookup"><span data-stu-id="d2abd-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="d2abd-118">詳細については、次を参照してください。 [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d2abd-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="d2abd-119">アプリケーションのルートを開く*Web.config*次に示すファイル。</span><span class="sxs-lookup"><span data-stu-id="d2abd-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="d2abd-120">(されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="d2abd-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="d2abd-121">検索、`<connectionStrings>`要素。</span><span class="sxs-lookup"><span data-stu-id="d2abd-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="d2abd-122">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d2abd-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="d2abd-123">次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。</span><span class="sxs-lookup"><span data-stu-id="d2abd-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="d2abd-124">2 つの接続文字列は、非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="d2abd-124">The two connection strings are very similar.</span></span> <span data-ttu-id="d2abd-125">最初の接続文字列が名前付き`DefaultConnection`メンバーシップ データベースがアプリケーションにアクセスできるユーザーを制御するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d2abd-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="d2abd-126">という名前の LocalDB データベースを指定する接続文字列を追加した*Movie.mdf*にある、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="d2abd-127">メンバーシップ、認証およびセキュリティの詳細については、このチュートリアルで、メンバーシップ データベースを使用して、チュートリアルを参照してくださいおされません[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。</span><span class="sxs-lookup"><span data-stu-id="d2abd-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="d2abd-128">接続文字列の名前の名前に一致する必要があります、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="d2abd-129">実際にでは、追加する必要はありません、`MovieDBContext`接続文字列。</span><span class="sxs-lookup"><span data-stu-id="d2abd-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="d2abd-130">Entity Framework が LocalDB データベースを作成するユーザーのディレクトリの完全修飾名で接続文字列を指定しない場合、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス (このケースで`MvcMovie.Models.MovieDBContext`)。</span><span class="sxs-lookup"><span data-stu-id="d2abd-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="d2abd-131">できる任意の名前をデータベースが必要ながある限り、*です。MDF*サフィックス。</span><span class="sxs-lookup"><span data-stu-id="d2abd-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="d2abd-132">たとえば、データベースの名前おでした*MyFilms.mdf*です。</span><span class="sxs-lookup"><span data-stu-id="d2abd-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="d2abd-133">新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="d2abd-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d2abd-134">[前へ](adding-a-model.md)
[次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d2abd-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
