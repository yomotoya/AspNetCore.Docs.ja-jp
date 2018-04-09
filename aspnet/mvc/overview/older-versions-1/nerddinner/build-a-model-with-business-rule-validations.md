---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: ビジネス ルールの検証とモデルの作成 |Microsoft ドキュメント
author: microsoft
description: 手順 3 では、両方のクエリを使用してしたり NerdDinner アプリケーション データベースを更新でくモデルを作成する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="build-a-model-with-business-rule-validations"></a><span data-ttu-id="09841-103">ビジネス ルールの検証とモデルを構築します。</span><span class="sxs-lookup"><span data-stu-id="09841-103">Build a Model with Business Rule Validations</span></span>
====================
<span data-ttu-id="09841-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="09841-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="09841-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="09841-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="09841-106">これは、無料の手順 3 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="09841-106">This is step 3 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="09841-107">手順 3 では、両方のクエリを使用してしたり NerdDinner アプリケーション データベースを更新でくモデルを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09841-107">Step 3 shows how to create a model that we can use to both query and update the database for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="09841-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="09841-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-3-building-the-model"></a><span data-ttu-id="09841-109">NerdDinner 手順 3: モデルの構築</span><span class="sxs-lookup"><span data-stu-id="09841-109">NerdDinner Step 3: Building the Model</span></span>

<span data-ttu-id="09841-110">モデル ビュー コント ローラーのフレームワークでは、「モデル」という用語と統合する検証ルールとビジネス ルールに対応するドメイン ロジックと同様に、アプリケーションのデータを表すオブジェクトを指します。</span><span class="sxs-lookup"><span data-stu-id="09841-110">In a model-view-controller framework the term "model" refers to the objects that represent the data of the application, as well as the corresponding domain logic that integrates validation and business rules with it.</span></span> <span data-ttu-id="09841-111">モデルでは、さまざまな方法で「心臓部」MVC ベースのアプリケーションし、ドライブの動作を後で根本的にわかりのようです。</span><span class="sxs-lookup"><span data-stu-id="09841-111">The model is in many ways the "heart" of an MVC-based application, and as we'll see later fundamentally drives the behavior of it.</span></span>

<span data-ttu-id="09841-112">ASP.NET MVC フレームワークでは、任意のデータ アクセス テクノロジを使用してサポートしているし、開発者は、さまざまなを含む、モデルを実装する機能豊富な .NET データ オプションから選択できます。 LINQ to Entities、LINQ to SQL、NHibernate、LLBLGen Pro、SubSonic、WilsonORM、または単なる生 ADO です。NET DataReaders やデータセットです。</span><span class="sxs-lookup"><span data-stu-id="09841-112">The ASP.NET MVC framework supports using any data access technology, and developers can choose from a variety of rich .NET data options to implement their models including: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM, or just raw ADO.NET DataReaders or DataSets.</span></span>

<span data-ttu-id="09841-113">NerdDinner アプリケーションに対して行うを LINQ to SQL を使用して、データベースの設計にはかなり密接に対応しており、一部のカスタム検証ロジックとビジネス ルールを追加する単純なモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="09841-113">For our NerdDinner application we are going to use LINQ to SQL to create a simple model that corresponds fairly closely to our database design, and adds some custom validation logic and business rules.</span></span> <span data-ttu-id="09841-114">概要を離れたときに役立つリポジトリ クラスをテスト単位簡単にすることにより、アプリケーションでは、残りの部分からデータの永続性の実装お実装は。</span><span class="sxs-lookup"><span data-stu-id="09841-114">We will then implement a repository class that helps abstract away the data persistence implementation from the rest of the application, and enables us to easily unit test it.</span></span>

### <a name="linq-to-sql"></a><span data-ttu-id="09841-115">LINQ to SQL</span><span class="sxs-lookup"><span data-stu-id="09841-115">LINQ to SQL</span></span>

<span data-ttu-id="09841-116">LINQ to SQL は、.NET 3.5 の一部として同梱されている ORM (オブジェクト リレーショナル マッパーです)。</span><span class="sxs-lookup"><span data-stu-id="09841-116">LINQ to SQL is an ORM (object relational mapper) that ships as part of .NET 3.5.</span></span>

<span data-ttu-id="09841-117">LINQ to SQL では、.NET クラスに対してコードを記述できることをデータベース テーブルにマップする簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="09841-117">LINQ to SQL provides an easy way to map database tables to .NET classes we can code against.</span></span> <span data-ttu-id="09841-118">NerdDinner アプリケーションの Dinner および RSVP のクラスに、データベース内で、ディナーおよび RSVP テーブルにマップするのにに使用されます。</span><span class="sxs-lookup"><span data-stu-id="09841-118">For our NerdDinner application we'll use it to map the Dinners and RSVP tables within our database to Dinner and RSVP classes.</span></span> <span data-ttu-id="09841-119">ディナーおよび RSVP のテーブルの列は、Dinner および RSVP クラスのプロパティに対応します。</span><span class="sxs-lookup"><span data-stu-id="09841-119">The columns of the Dinners and RSVP tables will correspond to properties on the Dinner and RSVP classes.</span></span> <span data-ttu-id="09841-120">Dinner および RSVP の各オブジェクトは、データベース内のディナーまたは RSVP テーブル内の個別の行を表します。</span><span class="sxs-lookup"><span data-stu-id="09841-120">Each Dinner and RSVP object will represent a separate row within the Dinners or RSVP tables in the database.</span></span>

<span data-ttu-id="09841-121">LINQ to SQL では、SQL ステートメントを取得および Dinner および RSVP の更新を手動で作成する必要があるにより、データベースのデータ オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="09841-121">LINQ to SQL allows us to avoid having to manually construct SQL statements to retrieve and update Dinner and RSVP objects with database data.</span></span> <span data-ttu-id="09841-122">代わりに、それらに対応付ける方法、およびデータベース、およびそれらのリレーションシップから Dinner および RSVP のクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="09841-122">Instead, we'll define the Dinner and RSVP classes, how they map to/from the database, and the relationships between them.</span></span> <span data-ttu-id="09841-123">LINQ to SQL では、対話は、それらを使用すると、実行時に使用する適切な SQL の実行ロジックを生成するを引き継ぎます。</span><span class="sxs-lookup"><span data-stu-id="09841-123">LINQ to SQL will then takes care of generating the appropriate SQL execution logic to use at runtime when we interact and use them.</span></span>

<span data-ttu-id="09841-124">VB と c# 内での LINQ の言語サポートを使用して Dinner および RSVP を取得する優れたクエリを記述、データベースからのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="09841-124">We can use the LINQ language support within VB and C# to write expressive queries that retrieve Dinner and RSVP objects from the database.</span></span> <span data-ttu-id="09841-125">これには、データのコードを記述する必要がありますの量が最小化し、実際にクリーンなアプリケーションを構築することができます。</span><span class="sxs-lookup"><span data-stu-id="09841-125">This minimizes the amount of data code we need to write, and allows us to build really clean applications.</span></span>

### <a name="adding-linq-to-sql-classes-to-our-project"></a><span data-ttu-id="09841-126">追加する LINQ to SQL クラスをプロジェクト</span><span class="sxs-lookup"><span data-stu-id="09841-126">Adding LINQ to SQL Classes to our project</span></span>

<span data-ttu-id="09841-127">このプロジェクト内で"Models"フォルダーを右クリックして開始がうまくを選択し、**追加 -&gt;新しい項目の**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="09841-127">We'll begin by right-clicking on the "Models" folder within our project, and select the **Add-&gt;New Item** menu command:</span></span>

![](build-a-model-with-business-rule-validations/_static/image1.png)

<span data-ttu-id="09841-128">これは、"新しい項目の追加 ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09841-128">This will bring up the "Add New Item" dialog.</span></span> <span data-ttu-id="09841-129">「データ」カテゴリでフィルタ リング、内に「SQL クラスを LINQ」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="09841-129">We'll filter by the "Data" category and select the "LINQ to SQL Classes" template within it:</span></span>

![](build-a-model-with-business-rule-validations/_static/image2.png)

<span data-ttu-id="09841-130">項目"NerdDinner"という名前を「追加」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="09841-130">We'll name the item "NerdDinner" and click the "Add" button.</span></span> <span data-ttu-id="09841-131">Visual Studio は、当社 \Models ディレクトリにある NerdDinner.dbml ファイルを追加し、LINQ to SQL オブジェクト リレーショナル デザイナーを開きます。</span><span class="sxs-lookup"><span data-stu-id="09841-131">Visual Studio will add a NerdDinner.dbml file under our \Models directory, and then open the LINQ to SQL object relational designer:</span></span>

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a><span data-ttu-id="09841-132">LINQ to SQL でデータ モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09841-132">Creating Data Model Classes with LINQ to SQL</span></span>

<span data-ttu-id="09841-133">LINQ to SQL では、既存のデータベース スキーマからデータ モデル クラスをすばやく作成できます。</span><span class="sxs-lookup"><span data-stu-id="09841-133">LINQ to SQL enables us to quickly create data model classes from existing database schema.</span></span> <span data-ttu-id="09841-134">タスクでモデル化するこのおを NerdDinner データベース、サーバー エクスプ ローラーを開き、テーブルを選択します。</span><span class="sxs-lookup"><span data-stu-id="09841-134">To-do this we'll open the NerdDinner database in the Server Explorer, and select the Tables we want to model in it:</span></span>

![](build-a-model-with-business-rule-validations/_static/image4.png)

<span data-ttu-id="09841-135">おことができますし、テーブル、LINQ の上に SQL デザイナー画面にドラッグします。</span><span class="sxs-lookup"><span data-stu-id="09841-135">We can then drag the tables onto the LINQ to SQL designer surface.</span></span> <span data-ttu-id="09841-136">ときにこの LINQ to SQL 行うと、Dinner が自動的に作成し、(データベース テーブルの列にマップされるクラス プロパティ) を持つテーブルのスキーマを使用して、RSVP クラス。</span><span class="sxs-lookup"><span data-stu-id="09841-136">When we do this LINQ to SQL will automatically create Dinner and RSVP classes using the schema of the tables (with class properties that map to the database table columns):</span></span>

![](build-a-model-with-business-rule-validations/_static/image5.png)

<span data-ttu-id="09841-137">既定では、LINQ to SQL デザイナーに自動的に「を複数化」テーブルおよび列名、データベース スキーマに基づくクラスの作成時にします。</span><span class="sxs-lookup"><span data-stu-id="09841-137">By default the LINQ to SQL designer automatically "pluralizes" table and column names when it creates classes based on a database schema.</span></span> <span data-ttu-id="09841-138">例: 前の例「ディナー」テーブル"Dinner"クラスが発生しました。</span><span class="sxs-lookup"><span data-stu-id="09841-138">For example: the "Dinners" table in our example above resulted in a "Dinner" class.</span></span> <span data-ttu-id="09841-139">このクラスの名前付けには、作成、モデルが .NET の名前付け規則と一致が使用して、その発生しているデザイナーの修正プログラムこの便利なを (特に多くのテーブルの追加) 通常の検索します。</span><span class="sxs-lookup"><span data-stu-id="09841-139">This class naming helps make our models consistent with .NET naming conventions, and I usually find that having the designer fix this up convenient (especially when adding lots of tables).</span></span> <span data-ttu-id="09841-140">クラスや、常にこのメソッドをオーバーライドし、任意の名前に変更できますが、デザイナーにより生成されたプロパティの名前を取り消す場合。</span><span class="sxs-lookup"><span data-stu-id="09841-140">If you don't like the name of a class or property that the designer generates, though, you can always override it and change it to any name you want.</span></span> <span data-ttu-id="09841-141">こうことに、エンティティ/プロパティ名で行のデザイナー内での編集するか、プロパティ グリッドを使用して変更します。</span><span class="sxs-lookup"><span data-stu-id="09841-141">You can do this either by editing the entity/property name in-line within the designer or by modifying it via the property grid.</span></span>

<span data-ttu-id="09841-142">既定では、LINQ to SQL デザイナーはまた、テーブルの主キー/外部キーのリレーションシップを検査し、それに基づいて自動的に関連付けの作成既定"リレーションシップ"を作成する別のモデル クラス間でします。</span><span class="sxs-lookup"><span data-stu-id="09841-142">By default the LINQ to SQL designer also inspects the primary key/foreign key relationships of the tables, and based on them automatically creates default "relationship associations" between the different model classes it creates.</span></span> <span data-ttu-id="09841-143">たとえばと、ディナーをドラッグして、LINQ to SQL デザイナー上に 2 つの一対多リレーションシップの関連テーブルを RSVP 推定された、という事実に基づいて RSVP テーブルには、ディナー テーブルへの外部キーが必要がある (これは、矢印によって示されます、デザイナーの):</span><span class="sxs-lookup"><span data-stu-id="09841-143">For example, when we dragged the Dinners and RSVP tables onto the LINQ to SQL designer a one-to-many relationship association between the two was inferred based on the fact that the RSVP table had a foreign-key to the Dinners table (this is indicated by the arrow in the designer):</span></span>

![](build-a-model-with-business-rule-validations/_static/image6.png)

<span data-ttu-id="09841-144">上記の関連付けには、sql 開発者特定 RSVP に関連付けられている Dinner へのアクセスに使用できる RSVP クラスを厳密に型指定された"Dinner"プロパティを追加する LINQ が発生します。</span><span class="sxs-lookup"><span data-stu-id="09841-144">The above association will cause LINQ to SQL to add a strongly typed "Dinner" property to the RSVP class that developers can use to access the Dinner associated with a given RSVP.</span></span> <span data-ttu-id="09841-145">開発者の取得し、特定夕食に関連付けられている RSVP オブジェクトを更新することができる"RSVPs"コレクション プロパティを持つ Dinner クラスにもなります。</span><span class="sxs-lookup"><span data-stu-id="09841-145">It will also cause the Dinner class to have a "RSVPs" collection property that enables developers to retrieve and update RSVP objects associated with a particular Dinner.</span></span>

<span data-ttu-id="09841-146">次に示す新しい RSVP オブジェクトを作成し、Dinner の RSVPs コレクションに追加する場合は、Visual Studio 内での intellisense の使用例を確認できます。</span><span class="sxs-lookup"><span data-stu-id="09841-146">Below you can see an example of intellisense within Visual Studio when we create a new RSVP object and add it to a Dinner's RSVPs collection.</span></span> <span data-ttu-id="09841-147">どのように LINQ to SQL 自動的に追加された"RSVPs"コレクション Dinner オブジェクトでに注意してください。</span><span class="sxs-lookup"><span data-stu-id="09841-147">Notice how LINQ to SQL automatically added a "RSVPs" collection on the Dinner object:</span></span>

![](build-a-model-with-business-rule-validations/_static/image7.png)

<span data-ttu-id="09841-148">RSVP オブジェクト コレクションに追加して Dinner の RSVPs 指示することに LINQ to SQL に Dinner とデータベースに RSVP 行の間の外部キー リレーションシップを関連付けるには。</span><span class="sxs-lookup"><span data-stu-id="09841-148">By adding the RSVP object to the Dinner's RSVPs collection we are telling LINQ to SQL to associate a foreign-key relationship between the Dinner and the RSVP row in our database:</span></span>

![](build-a-model-with-business-rule-validations/_static/image8.png)

<span data-ttu-id="09841-149">デザイナーのモデルまたはテーブルの関連付けをという名前の方法に満足できない場合は、メソッドをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="09841-149">If you don't like how the designer has modeled or named a table association, you can override it.</span></span> <span data-ttu-id="09841-150">デザイナー内での関連付けの矢印をクリックして、名前の変更、削除または変更するには、プロパティ グリッドを使用してそのプロパティにアクセスだけです。</span><span class="sxs-lookup"><span data-stu-id="09841-150">Just click on the association arrow within the designer and access its properties via the property grid to rename, delete or modify it.</span></span> <span data-ttu-id="09841-151">NerdDinner アプリケーションでは、既定のアソシエーション ルールが構築して、データ モデル クラスの適切に動作して、次の既定の動作を使用できますが、です。</span><span class="sxs-lookup"><span data-stu-id="09841-151">For our NerdDinner application, though, the default association rules work well for the data model classes we are building and we can just use the default behavior.</span></span>

### <a name="nerddinnerdatacontext-class"></a><span data-ttu-id="09841-152">NerdDinnerDataContext Class</span><span class="sxs-lookup"><span data-stu-id="09841-152">NerdDinnerDataContext Class</span></span>

<span data-ttu-id="09841-153">Visual Studio では、モデルおよび LINQ to SQL デザイナーを使用して定義されているデータベースのリレーションシップを表す .NET クラスを自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="09841-153">Visual Studio will automatically create .NET classes that represent the models and database relationships defined using the LINQ to SQL designer.</span></span> <span data-ttu-id="09841-154">各 LINQ to SQL デザイナー ファイルがソリューションに追加の LINQ to SQL DataContext クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="09841-154">A LINQ to SQL DataContext class is also generated for each LINQ to SQL designer file added to the solution.</span></span> <span data-ttu-id="09841-155">当社の LINQ to SQL クラスの項目"NerdDinner"という名前を付けて、ために、作成された DataContext クラスには、"NerdDinnerDataContext"が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="09841-155">Because we named our LINQ to SQL class item "NerdDinner", the DataContext class created will be called "NerdDinnerDataContext".</span></span> <span data-ttu-id="09841-156">この NerdDinnerDataContext クラスは、プライマリ データベースと対話できます。</span><span class="sxs-lookup"><span data-stu-id="09841-156">This NerdDinnerDataContext class is the primary way we will interact with the database.</span></span>

<span data-ttu-id="09841-157">当社 NerdDinnerDataContext クラスは、データベース内でモデル化して 2 つのテーブルを表す「ディナー」と"RSVPs"- - 2 つのプロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="09841-157">Our NerdDinnerDataContext class exposes two properties - "Dinners" and "RSVPs" - that represent the two tables we modeled within the database.</span></span> <span data-ttu-id="09841-158">C# を使用すると、クエリと取得の Dinner および RSVP のオブジェクトをデータベースからこれらのプロパティに対する LINQ クエリを書き込む。</span><span class="sxs-lookup"><span data-stu-id="09841-158">We can use C# to write LINQ queries against those properties to query and retrieve Dinner and RSVP objects from the database.</span></span>

<span data-ttu-id="09841-159">次のコードでは、NerdDinnerDataContext オブジェクトをインスタンス化し、今後の予定ディナーのシーケンスが得られるようにに対して LINQ クエリを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09841-159">The following code demonstrates how to instantiate a NerdDinnerDataContext object and perform a LINQ query against it to obtain a sequence of Dinners that occur in the future.</span></span> <span data-ttu-id="09841-160">Visual Studio は、LINQ クエリを作成するときにフル intellisense を提供し、そこから返されるオブジェクトが厳密に型指定も intellisense をサポートします。</span><span class="sxs-lookup"><span data-stu-id="09841-160">Visual Studio provides full intellisense when writing the LINQ query, and the objects returned from it are strongly-typed and also support intellisense:</span></span>

![](build-a-model-with-business-rule-validations/_static/image9.png)

<span data-ttu-id="09841-161">Dinner と RSVP のオブジェクトのクエリを実行することを許可するだけでなく、NerdDinnerDataContext も自動的に変更を取得するまでお Dinner と RSVP オブジェクトに、その後に加えるおを追跡します。</span><span class="sxs-lookup"><span data-stu-id="09841-161">In addition to allowing us to query for Dinner and RSVP objects, a NerdDinnerDataContext also automatically tracks any changes we subsequently make to the Dinner and RSVP objects we retrieve through it.</span></span> <span data-ttu-id="09841-162">データベースの明示的な SQL 更新プログラム コードを記述するのにことがなく簡単に変更を保存するのにこの機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="09841-162">We can use this functionality to easily save the changes back to the database - without having to write any explicit SQL update code.</span></span>

<span data-ttu-id="09841-163">たとえば、次のコードでは、LINQ クエリを使用して単一 Dinner オブジェクトをデータベースから取得、2 つの Dinner プロパティを更新およびデータベースに変更を保存する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="09841-163">For example, the code below demonstrates how to use a LINQ query to retrieve a single Dinner object from the database, update two of the Dinner properties, and then save the changes back to the database:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

<span data-ttu-id="09841-164">前のコードで NerdDinnerDataContext オブジェクトは、そこから取得お Dinner オブジェクトに対して行われたプロパティの変更を自動的に追跡します。</span><span class="sxs-lookup"><span data-stu-id="09841-164">The NerdDinnerDataContext object in the code above automatically tracked the property changes made to the Dinner object we retrieved from it.</span></span> <span data-ttu-id="09841-165">私たちには、"SubmitChanges()"メソッドが呼び出されるには、適切な SQL ステートメントを実行"UPDATE"、データベースに、更新された値を保持します。</span><span class="sxs-lookup"><span data-stu-id="09841-165">When we called the "SubmitChanges()" method, it will execute an appropriate SQL "UPDATE" statement to the database to persist the updated values back.</span></span>

### <a name="creating-a-dinnerrepository-class"></a><span data-ttu-id="09841-166">DinnerRepository クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09841-166">Creating a DinnerRepository Class</span></span>

<span data-ttu-id="09841-167">小規模なアプリケーションでは、LINQ to SQL DataContext クラスと直接やり取りし、コント ローラー内での LINQ クエリを埋め込むコント ローラーがある問題があります。</span><span class="sxs-lookup"><span data-stu-id="09841-167">For small applications it is sometimes fine to have Controllers work directly against a LINQ to SQL DataContext class, and embed LINQ queries within the Controllers.</span></span> <span data-ttu-id="09841-168">大規模なアプリケーションと、ただし、この方法は複雑になりますを維持し、テストします。</span><span class="sxs-lookup"><span data-stu-id="09841-168">As applications get larger, though, this approach becomes cumbersome to maintain and test.</span></span> <span data-ttu-id="09841-169">複数の場所に同じ LINQ クエリを複製することになることができますも。</span><span class="sxs-lookup"><span data-stu-id="09841-169">It can also lead to us duplicating the same LINQ queries in multiple places.</span></span>

<span data-ttu-id="09841-170">簡単にすることができますのアプリケーションを管理およびテストする方法の 1 つでは、"repository"パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="09841-170">One approach that can make applications easier to maintain and test is to use a "repository" pattern.</span></span> <span data-ttu-id="09841-171">リポジトリ クラスにより、クエリを実行するデータと永続性ロジック、および概要を離れたアプリケーションからデータの永続性の実装の詳細をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="09841-171">A repository class helps encapsulate data querying and persistence logic, and abstracts away the implementation details of the data persistence from the application.</span></span> <span data-ttu-id="09841-172">アプリケーション コードを見やすくするだけでなくリポジトリ パターンを使用することができますしやすく、将来データ ストレージの実装を変更し、単体テストの実際のデータベースを必要とせず、アプリケーションを容易にするために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="09841-172">In addition to making application code cleaner, using a repository pattern can make it easier to change data storage implementations in the future, and it can help facilitate unit testing an application without requiring a real database.</span></span>

<span data-ttu-id="09841-173">NerdDinner アプリケーションについてを持つ DinnerRepository クラスを定義します、署名の下。</span><span class="sxs-lookup"><span data-stu-id="09841-173">For our NerdDinner application we'll define a DinnerRepository class with the below signature:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

<span data-ttu-id="09841-174">*メモ: 後の「おをこのクラスから IDinnerRepository インターフェイスを抽出および、コント ローラーと依存関係挿入有効にします。まず始めに、ただしはここを簡単に開始し、DinnerRepository クラスを直接操作だけです。*</span><span class="sxs-lookup"><span data-stu-id="09841-174">*Note: Later in this chapter we'll extract an IDinnerRepository interface from this class and enable dependency injection with it on our Controllers. To begin with, though, we are going to start simple and just work directly with the DinnerRepository class.*</span></span>

<span data-ttu-id="09841-175">"Models"フォルダーを右クリックしてがうまくを選択し、このクラスを実装する、**追加 -&gt;新しい項目の**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="09841-175">To implement this class we'll right-click on our "Models" folder and choose the **Add-&gt;New Item** menu command.</span></span> <span data-ttu-id="09841-176">"新しい項目の追加 ダイアログ ボックスでは、"Class"テンプレートを選択し、"DinnerRepository.cs"ファイルの名前おします。</span><span class="sxs-lookup"><span data-stu-id="09841-176">Within the "Add New Item" dialog we'll select the "Class" template and name the file "DinnerRepository.cs":</span></span>

![](build-a-model-with-business-rule-validations/_static/image10.png)

<span data-ttu-id="09841-177">次のコードを使用して、DinnerRespository クラスお実装できます。</span><span class="sxs-lookup"><span data-stu-id="09841-177">We can then implement our DinnerRespository class using the code below:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a><span data-ttu-id="09841-178">取得する、更新、挿入および削除 DinnerRepository クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="09841-178">Retrieving, Updating, Inserting and Deleting using the DinnerRepository class</span></span>

<span data-ttu-id="09841-179">これで、DinnerRepository クラスを作成しましたは、一般的なタスクのことができるかを示す、いくつかのコード例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="09841-179">Now that we've created our DinnerRepository class, let's look at a few code examples that demonstrate common tasks we can do with it:</span></span>

#### <a name="querying-examples"></a><span data-ttu-id="09841-180">例のクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="09841-180">Querying Examples</span></span>

<span data-ttu-id="09841-181">次のコードは、DinnerID 値を使用して単一 Dinner を取得します。</span><span class="sxs-lookup"><span data-stu-id="09841-181">The code below retrieves a single Dinner using the DinnerID value:</span></span>


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

<span data-ttu-id="09841-182">次のコードは、上にすべての今後のディナーとループを取得します。</span><span class="sxs-lookup"><span data-stu-id="09841-182">The code below retrieves all upcoming dinners and loops over them:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a><span data-ttu-id="09841-183">挿入と更新プログラムの例</span><span class="sxs-lookup"><span data-stu-id="09841-183">Insert and Update Examples</span></span>

<span data-ttu-id="09841-184">次のコードでは、次の 2 つの新しいディナーを追加することを示します。</span><span class="sxs-lookup"><span data-stu-id="09841-184">The code below demonstrates adding two new dinners.</span></span> <span data-ttu-id="09841-185">リポジトリへの追加や変更は、"Save()"メソッドの呼び出しはまでデータベースにコミットされません。</span><span class="sxs-lookup"><span data-stu-id="09841-185">Additions/modifications to the repository aren't committed to the database until the "Save()" method is called on it.</span></span> <span data-ttu-id="09841-186">LINQ to SQL を使用 – データベースのトランザクションですべての変更が発生するか、全くリポジトリを保存するのすべての変更が自動的に折り返さ。</span><span class="sxs-lookup"><span data-stu-id="09841-186">LINQ to SQL automatically wraps all changes in a database transaction – so either all changes happen or none of them do when our repository saves:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

<span data-ttu-id="09841-187">次のコードでは、既存の Dinner オブジェクトを取得しに 2 つのプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="09841-187">The code below retrieves an existing Dinner object, and modifies two properties on it.</span></span> <span data-ttu-id="09841-188">リポジトリで"Save()"メソッドが呼び出された場合は、変更をデータベースにコミットします。</span><span class="sxs-lookup"><span data-stu-id="09841-188">The changes are committed back to the database when the "Save()" method is called on our repository:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

<span data-ttu-id="09841-189">次のコードは、dinner を取得し、RSVP を追加します。</span><span class="sxs-lookup"><span data-stu-id="09841-189">The code below retrieves a dinner and then adds an RSVP to it.</span></span> <span data-ttu-id="09841-190">これは、(データベースに 2 つのプライマリのキー/外部キー リレーションシップがある) ために、LINQ to SQL をご利用の米国作成 Dinner オブジェクトのコレクションの RSVPs を使用します。</span><span class="sxs-lookup"><span data-stu-id="09841-190">It does this using the RSVPs collection on the Dinner object that LINQ to SQL created for us (because there is a primary-key/foreign-key relationship between the two in the database).</span></span> <span data-ttu-id="09841-191">この変更新しい RSVP テーブルの行としてデータベースに永続化リポジトリで"Save()"メソッドが呼び出されたときに。</span><span class="sxs-lookup"><span data-stu-id="09841-191">This change is persisted back to the database as a new RSVP table row when the "Save()" method is called on the repository:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a><span data-ttu-id="09841-192">削除の例</span><span class="sxs-lookup"><span data-stu-id="09841-192">Delete Example</span></span>

<span data-ttu-id="09841-193">次のコードでは、既存の Dinner オブジェクトを取得し、削除のマークを付けます。</span><span class="sxs-lookup"><span data-stu-id="09841-193">The code below retrieves an existing Dinner object, and then marks it to be deleted.</span></span> <span data-ttu-id="09841-194">リポジトリで"Save()"メソッドが呼び出された場合は、削除をデータベースにコミットします。</span><span class="sxs-lookup"><span data-stu-id="09841-194">When the "Save()" method is called on the repository it will commit the delete back to the database:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a><span data-ttu-id="09841-195">モデルのクラスと検証とビジネス ルール ロジックの統合</span><span class="sxs-lookup"><span data-stu-id="09841-195">Integrating Validation and Business Rule Logic with Model Classes</span></span>

<span data-ttu-id="09841-196">検証とビジネス ルールのロジックは、データを操作するすべてのアプリケーションの重要な部分を統合しています。</span><span class="sxs-lookup"><span data-stu-id="09841-196">Integrating validation and business rule logic is a key part of any application that works with data.</span></span>

#### <a name="schema-validation"></a><span data-ttu-id="09841-197">スキーマの検証</span><span class="sxs-lookup"><span data-stu-id="09841-197">Schema Validation</span></span>

<span data-ttu-id="09841-198">モデル クラスを LINQ to SQL デザイナーを使用して定義したら、データ モデル クラスのプロパティのデータ型は、データベース テーブルのデータ型に対応します。</span><span class="sxs-lookup"><span data-stu-id="09841-198">When model classes are defined using the LINQ to SQL designer, the datatypes of the properties in the data model classes correspond to the datatypes of the database table.</span></span> <span data-ttu-id="09841-199">例: 型 (ある組み込みの .NET データ型)"DateTime"ディナー テーブル内の"EventDate"列が、"datetime"の場合は、LINQ to SQL によって作成されたデータ モデル クラスであります。</span><span class="sxs-lookup"><span data-stu-id="09841-199">For example: if the "EventDate" column in the Dinners table is a "datetime", the data model class created by LINQ to SQL will be of type "DateTime" (which is a built-in .NET datatype).</span></span> <span data-ttu-id="09841-200">つまりにして、コードから整数またはブール値を割り当てようとすると、エラーが発生、自動的に実行時に有効でない文字列型を暗黙的に変換しようとする場合、コンパイル エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09841-200">This means you will get compile errors if you attempt to assign an integer or boolean to it from code, and it will raise an error automatically if you attempt to implicitly convert a non-valid string type to it at runtime.</span></span>

<span data-ttu-id="09841-201">保護するのに役立つする SQL インジェクション攻撃からそれを使用する場合、文字列を使用するときに LINQ to SQL はのも自動的にエスケープ SQL の値を処理します。</span><span class="sxs-lookup"><span data-stu-id="09841-201">LINQ to SQL will also automatically handles escaping SQL values for you when using strings - which helps protect you against SQL injection attacks when using it.</span></span>

#### <a name="validation-and-business-rule-logic"></a><span data-ttu-id="09841-202">検証とビジネス ルール ロジック</span><span class="sxs-lookup"><span data-stu-id="09841-202">Validation and Business Rule Logic</span></span>

<span data-ttu-id="09841-203">スキーマの検証は最初の手順として役に立ちますが、ほとんどだけで十分です。</span><span class="sxs-lookup"><span data-stu-id="09841-203">Schema validation is useful as a first step, but is rarely sufficient.</span></span> <span data-ttu-id="09841-204">ほとんどの現実のシナリオが複数のプロパティにまたがる、コードを実行したりできる多くの場合、モデルの状態を認識している豊富な検証ロジックを指定することが必要 (例: を作成/更新/削除、またはドメイン固有の状態like「アーカイブ」)。</span><span class="sxs-lookup"><span data-stu-id="09841-204">Most real-world scenarios require the ability to specify richer validation logic that can span multiple properties, execute code, and often have awareness of a model's state (for example: is it being created /updated/deleted, or within a domain-specific state like "archived").</span></span> <span data-ttu-id="09841-205">さまざまなパターンやを定義し、モデルのクラスに検証規則を適用するために使用するフレームワークのさまざまながあるし、いくつかの .NET ベース フレームワークがこれを支援するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="09841-205">There are a variety of different patterns and frameworks that can be used to define and apply validation rules to model classes, and there are several .NET based frameworks out there that can be used to help with this.</span></span> <span data-ttu-id="09841-206">ASP.NET MVC アプリケーション内でそれらのほぼすべての操作を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="09841-206">You can use pretty much any of them within ASP.NET MVC applications.</span></span>

<span data-ttu-id="09841-207">NerdDinner アプリケーションの目的で、使用されます、比較的単純な単純なパターンを Dinner モデル オブジェクトに IsValid プロパティおよび GetRuleViolations() メソッドを公開しています。</span><span class="sxs-lookup"><span data-stu-id="09841-207">For the purposes of our NerdDinner application, we'll use a relatively simple and straight-forward pattern where we expose an IsValid property and a GetRuleViolations() method on our Dinner model object.</span></span> <span data-ttu-id="09841-208">True または false、検証規則とビジネス規則はすべて有効かどうかに応じて IsValid プロパティを返します。</span><span class="sxs-lookup"><span data-stu-id="09841-208">The IsValid property will return true or false depending on whether the validation and business rules are all valid.</span></span> <span data-ttu-id="09841-209">GetRuleViolations() メソッドでは、ルール エラーの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="09841-209">The GetRuleViolations() method will return a list of any rule errors.</span></span>

<span data-ttu-id="09841-210">おを実装 IsValid と GetRuleViolations() Dinner モデルの「クラスを部分」をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="09841-210">We'll implement IsValid and GetRuleViolations() for our Dinner model by adding a "partial class" to our project.</span></span> <span data-ttu-id="09841-211">部分クラス (LINQ to SQL デザイナーによって生成された Dinner クラス) などの VS デザイナーで保持されているクラスにメソッド/プロパティ/イベントを追加するために使用して、コードを及ぼしたりからツールを回避できるようにします。</span><span class="sxs-lookup"><span data-stu-id="09841-211">Partial classes can be used to add methods/properties/events to classes maintained by a VS designer (like the Dinner class generated by the LINQ to SQL designer) and help avoid the tool from messing with our code.</span></span> <span data-ttu-id="09841-212">\Models フォルダーを右クリックして、プロジェクトに新しい部分クラスを追加したり、「新しい項目の追加」メニュー コマンドを選択できます。</span><span class="sxs-lookup"><span data-stu-id="09841-212">We can add a new partial class to our project by right-clicking on the \Models folder, and then select the "Add New Item" menu command.</span></span> <span data-ttu-id="09841-213">"新しい項目の追加 ダイアログ ボックスで「クラスが」テンプレートを選択しを Dinner.cs という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="09841-213">We can then choose the "Class" template within the "Add New Item" dialog and name it Dinner.cs.</span></span>

![](build-a-model-with-business-rule-validations/_static/image11.png)

<span data-ttu-id="09841-214">[追加] ボタンをクリックすると、Dinner.cs ファイルをプロジェクトに追加および IDE 内で開きます。</span><span class="sxs-lookup"><span data-stu-id="09841-214">Clicking the "Add" button will add a Dinner.cs file to our project and open it within the IDE.</span></span> <span data-ttu-id="09841-215">基本的なルール/検証強制フレームワークを使用して、実装することができますし、コードの下。</span><span class="sxs-lookup"><span data-stu-id="09841-215">We can then implement a basic rule/validation enforcement framework using the below code:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

<span data-ttu-id="09841-216">上記のコードをいくつか説明します。</span><span class="sxs-lookup"><span data-stu-id="09841-216">A few notes about the above code:</span></span>

- <span data-ttu-id="09841-217">Dinner クラスはそれに含まれるコードを LINQ to SQL デザイナーによって生成される管理クラスと組み合わせるし、1 つのクラスにコンパイルされます意味 –"partial"はキーワードで始まります。</span><span class="sxs-lookup"><span data-stu-id="09841-217">The Dinner class is prefaced with a "partial" keyword – which means the code contained within it will be combined with the class generated/maintained by the LINQ to SQL designer and compiled into a single class.</span></span>
- <span data-ttu-id="09841-218">RuleViolation クラスは、規則違反に関する詳細を提供することができるプロジェクトに追加のヘルパー クラスです。</span><span class="sxs-lookup"><span data-stu-id="09841-218">The RuleViolation class is a helper class we'll add to the project that allows us to provide more details about a rule violation.</span></span>
- <span data-ttu-id="09841-219">Dinner.GetRuleViolations() メソッドにより、検証とビジネス ルールに評価される (間もなく実装にします)。</span><span class="sxs-lookup"><span data-stu-id="09841-219">The Dinner.GetRuleViolations() method causes our validation and business rules to be evaluated (we'll implement them shortly).</span></span> <span data-ttu-id="09841-220">返します戻る規則エラーの詳細を提供する RuleViolation オブジェクトのシーケンス。</span><span class="sxs-lookup"><span data-stu-id="09841-220">It then returns back a sequence of RuleViolation objects that provide more details about any rule errors.</span></span>
- <span data-ttu-id="09841-221">Dinner.IsValid プロパティでは、Dinner オブジェクトが、アクティブな RuleViolations を持つかどうかを示す便利なヘルパー プロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="09841-221">The Dinner.IsValid property provides a convenient helper property that indicates whether the Dinner object has any active RuleViolations.</span></span> <span data-ttu-id="09841-222">Dinner オブジェクトを使用して、いつでも、開発者で事前にチェックすることができます (および例外は発生しません)。</span><span class="sxs-lookup"><span data-stu-id="09841-222">It can be proactively checked by a developer using the Dinner object at anytime (and does not raise an exception).</span></span>
- <span data-ttu-id="09841-223">Dinner.OnValidate() 部分メソッドは、Dinner オブジェクトがデータベース内で永続化しようとしてあれば、いつでも通知により、LINQ to SQL を提供するフックがします。</span><span class="sxs-lookup"><span data-stu-id="09841-223">The Dinner.OnValidate() partial method is a hook that LINQ to SQL provides that allows us to be notified anytime the Dinner object is about to be persisted within the database.</span></span> <span data-ttu-id="09841-224">上記 OnValidate() 実装により、Dinner はない RuleViolations 保存されるまで。</span><span class="sxs-lookup"><span data-stu-id="09841-224">Our OnValidate() implementation above ensures that the Dinner has no RuleViolations before it is saved.</span></span> <span data-ttu-id="09841-225">無効な状態にある場合は、これにより LINQ to SQL トランザクションを中止する、例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="09841-225">If it is in an invalid state it raises an exception, which will cause LINQ to SQL to abort the transaction.</span></span>

<span data-ttu-id="09841-226">この方法は、検証ルールとビジネス ルールに統合できます単純なフレームワークを提供します。</span><span class="sxs-lookup"><span data-stu-id="09841-226">This approach provides a simple framework that we can integrate validation and business rules into.</span></span> <span data-ttu-id="09841-227">ここでは追加、当社 GetRuleViolations() メソッドへの規則の下。</span><span class="sxs-lookup"><span data-stu-id="09841-227">For now let's add the below rules to our GetRuleViolations() method:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

<span data-ttu-id="09841-228">C# の「の戻り値の生成」機能を使用お任意 RuleViolations のシーケンスを返します。</span><span class="sxs-lookup"><span data-stu-id="09841-228">We are using the "yield return" feature of C# to return a sequence of any RuleViolations.</span></span> <span data-ttu-id="09841-229">上記の最初の 6 つのルール チェックは、null または空の文字列のプロパティを Dinner がすることはできませんを強制します。</span><span class="sxs-lookup"><span data-stu-id="09841-229">The first six rule checks above simply enforce that string properties on our Dinner cannot be null or empty.</span></span> <span data-ttu-id="09841-230">興味深いは、前回のルールと呼び出すことに追加できることを確認する、プロジェクト、ContactPhone PhoneValidator.IsValidNumber() ヘルパー メソッド番号形式と一致する Dinner の国。</span><span class="sxs-lookup"><span data-stu-id="09841-230">The last rule is a little more interesting, and calls a PhoneValidator.IsValidNumber() helper method that we can add to our project to verify that the ContactPhone number format matches the Dinner's country.</span></span>

<span data-ttu-id="09841-231">使用できます。この電話検証のサポートを実装する NET の正規表現のサポート。</span><span class="sxs-lookup"><span data-stu-id="09841-231">We can use .NET's regular expression support to implement this phone validation support.</span></span> <span data-ttu-id="09841-232">追加できる単純な PhoneValidator 実装を次に示します国別の正規表現パターンのチェックを追加することにより、プロジェクトに。</span><span class="sxs-lookup"><span data-stu-id="09841-232">Below is a simple PhoneValidator implementation that we can add to our project that enables us to add country-specific Regex pattern checks:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a><span data-ttu-id="09841-233">検証とビジネス ロジックの違反の処理</span><span class="sxs-lookup"><span data-stu-id="09841-233">Handling Validation and Business Logic Violations</span></span>

<span data-ttu-id="09841-234">したを追加したので、上記の検証とビジネス ルールのコードを作成または、夕食を更新しようといつでも、当社の規則ロジックが評価され、適用されます。</span><span class="sxs-lookup"><span data-stu-id="09841-234">Now that we've added the above validation and business rule code, any time we try to create or update a Dinner, our validation logic rules will be evaluated and enforced.</span></span>

<span data-ttu-id="09841-235">開発者は、Dinner オブジェクトは、有効なかどうかを事前に決定する以下のようなコードし、すべての例外は生成せずにすべての違反の一覧の取得を作成できます。</span><span class="sxs-lookup"><span data-stu-id="09841-235">Developers can write code like below to proactively determine if a Dinner object is valid, and retrieve a list of all violations in it without raising any exceptions:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

<span data-ttu-id="09841-236">無効な状態で、夕食を保存するおしようとすると、DinnerRepository で Save() メソッドを呼び出すと、例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="09841-236">If we attempt to save a Dinner in an invalid state, an exception will be raised when we call the Save() method on the DinnerRepository.</span></span> <span data-ttu-id="09841-237">これは、LINQ to SQL は、Dinner の変更を保存して、夕食に規則違反が存在しない場合、例外を発生させる Dinner.OnValidate() にコードを追加した前に自動的にマイクロソフト Dinner.OnValidate() 部分メソッドを呼び出すために発生します。</span><span class="sxs-lookup"><span data-stu-id="09841-237">This occurs because LINQ to SQL automatically calls our Dinner.OnValidate() partial method before it saves the Dinner's changes, and we added code to Dinner.OnValidate() to raise an exception if any rule violations exist in the Dinner.</span></span> <span data-ttu-id="09841-238">この例外をキャッチして、事後対応的違反の修正の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="09841-238">We can catch this exception and reactively retrieve a list of the violations to fix:</span></span>

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

<span data-ttu-id="09841-239">当社モデル レイヤー内および UI レイヤー内ではなく、検証規則とビジネス規則が実装されているためにが適用され、アプリケーション内のすべてのシナリオで使用します。</span><span class="sxs-lookup"><span data-stu-id="09841-239">Because our validation and business rules are implemented within our model layer, and not within the UI layer, they will be applied and used across all scenarios within our application.</span></span> <span data-ttu-id="09841-240">後で変更またはビジネス ルールを追加したりできます Dinner オブジェクトとの連携を無視するすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="09841-240">We can later change or add business rules and have all code that works with our Dinner objects honor them.</span></span>

<span data-ttu-id="09841-241">1 か所でビジネス ルールを変更する柔軟性、これらの変更、アプリケーションと UI ロジック全体に広がる必要はありませんが、適切に記述されたアプリケーションと、MVC フレームワークにより、推奨の効果の記号です。</span><span class="sxs-lookup"><span data-stu-id="09841-241">Having the flexibility to change business rules in one place, without having these changes ripple throughout the application and UI logic, is a sign of a well-written application, and a benefit that an MVC framework helps encourage.</span></span>

### <a name="next-step"></a><span data-ttu-id="09841-242">次の手順</span><span class="sxs-lookup"><span data-stu-id="09841-242">Next Step</span></span>

<span data-ttu-id="09841-243">モデルのクエリし、データベースの更新に使用できるようになりましたしました。</span><span class="sxs-lookup"><span data-stu-id="09841-243">We've now got a model that we can use to both query and update our database.</span></span>

<span data-ttu-id="09841-244">みましょういくつかのコント ローラーとビューに追加、HTML UI エクスペリエンスをテーブルの周囲に使用できるプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="09841-244">Let's now add some controllers and views to the project that we can use to build an HTML UI experience around it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="09841-245">[前へ](create-a-database.md)
> [次へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)</span><span class="sxs-lookup"><span data-stu-id="09841-245">[Previous](create-a-database.md)
[Next](use-controllers-and-views-to-implement-a-listingdetails-ui.md)</span></span>
