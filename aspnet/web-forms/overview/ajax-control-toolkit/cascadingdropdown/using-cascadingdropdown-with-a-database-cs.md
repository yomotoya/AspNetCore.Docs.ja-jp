---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: (C#) データベースで CascadingDropDown を使用する |Microsoft Docs
author: wenz
description: CascadingDropDown コントロール、AJAX Control Toolkit では、anoth 内の値が 1 つの DropDownList の読み込みの変更に関連付けられているように DropDownList コントロールを拡張しています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: ed5057ee942ce57503b038cbd856fefaa3d287ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838895"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="7eda3-103">(C#) データベースで CascadingDropDown を使用します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="7eda3-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7eda3-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7eda3-105">[コードのダウンロード](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7eda3-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="7eda3-106">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="7eda3-107">これが機能するためには、特別な web サービスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="7eda3-108">概要</span><span class="sxs-lookup"><span data-stu-id="7eda3-108">Overview</span></span>

<span data-ttu-id="7eda3-109">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="7eda3-110">(たとえば、1 つのリストは、私たちの状態の一覧を提供します。 し、[次へ] の一覧は、その状態の主な都市で埋められます)。これが機能するためには、特別な web サービスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="7eda3-111">手順</span><span class="sxs-lookup"><span data-stu-id="7eda3-111">Steps</span></span>

<span data-ttu-id="7eda3-112">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="7eda3-112">First of all, a data source is required.</span></span> <span data-ttu-id="7eda3-113">このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="7eda3-114">データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="7eda3-115">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="7eda3-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="7eda3-116">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="7eda3-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="7eda3-117">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="7eda3-118">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="7eda3-119">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、 &lt; `form` &gt;要素)。</span><span class="sxs-lookup"><span data-stu-id="7eda3-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="7eda3-120">次の手順では、2 つの DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="7eda3-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="7eda3-121">このサンプルでは、AdventureWorks のベンダーと連絡先情報を使用、ため利用可能なベンダーと使用できる連絡先のいずれか 1 つのリストを作成します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="7eda3-122">次に、2 つの CascadingDropDown エクステンダーは、ページに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="7eda3-123">最初の (ベンダー) リストを塗りつぶす 1 つと、もう 1 つの入力 (連絡先) の 2 番目の一覧。</span><span class="sxs-lookup"><span data-stu-id="7eda3-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="7eda3-124">次の属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-124">The following attributes must be set:</span></span>

- <span data-ttu-id="7eda3-125">`ServicePath`: 一覧のエントリを提供する web サービスの URL</span><span class="sxs-lookup"><span data-stu-id="7eda3-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="7eda3-126">`ServiceMethod`: Web メソッドの一覧のエントリを提供します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="7eda3-127">`TargetControlID`。 ドロップダウン リストの ID</span><span class="sxs-lookup"><span data-stu-id="7eda3-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="7eda3-128">`Category`: Web メソッドが呼び出されたときに送信されたカテゴリの情報</span><span class="sxs-lookup"><span data-stu-id="7eda3-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="7eda3-129">`PromptText`: リストのデータをサーバーから非同期的に読み込むときに表示されるテキスト</span><span class="sxs-lookup"><span data-stu-id="7eda3-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="7eda3-130">`ParentControlID`: (省略可能) 親ドロップダウン リストの現在のリストには、そのトリガー読み込み</span><span class="sxs-lookup"><span data-stu-id="7eda3-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="7eda3-131">使用するプログラミング言語によっては、問題のある web サービスの名前が変更が他のすべての属性値が同じ。</span><span class="sxs-lookup"><span data-stu-id="7eda3-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="7eda3-132">最初のドロップダウン リストの CascadingDropDown 要素を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="7eda3-133">2 番目の一覧については、コントロール エクステンダーを設定する必要があります、`ParentControlID`属性連絡先リスト内の要素が関連付けられている読み込みベンダー リスト トリガー内のエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="7eda3-134">実際の作業はし、次のように設定された web サービスで行います。</span><span class="sxs-lookup"><span data-stu-id="7eda3-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="7eda3-135">なお、`[ScriptService]`属性を使用すると、それ以外の場合、ASP.NET AJAX はクライアント側スクリプト コードから web メソッドにアクセスする JavaScript プロキシを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="7eda3-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="7eda3-136">CascadingDropDown によって呼び出される web メソッドのシグネチャは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="7eda3-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="7eda3-137">戻り値は型の配列である必要がありますように`CascadingDropDownNameValue`Control Toolkit で定義されています。</span><span class="sxs-lookup"><span data-stu-id="7eda3-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="7eda3-138">`GetVendors()`メソッドは非常に簡単に実装できます。 コードは、AdventureWorks データベースに接続し、最初の 25 のベンダーのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="7eda3-139">最初のパラメーター、`CascadingDropDownNameValue`コンス トラクターの値を一覧のエントリ、2 つ目のキャプションには (html の value 属性&lt; `option` &gt;要素)。</span><span class="sxs-lookup"><span data-stu-id="7eda3-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="7eda3-140">次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="7eda3-141">仕入先の関連付けられている連絡先の取得 (メソッド名: `GetContactsForVendor()`) 少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="7eda3-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="7eda3-142">まず、最初のドロップダウン リストで選択されている仕入先を決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="7eda3-143">Control Toolkit には、そのタスク用のヘルパー メソッドが定義されています。`ParseKnownCategoryValuesString()`メソッドを返します。 を`StringDictionary`ドロップダウン リストのデータを持つ要素。</span><span class="sxs-lookup"><span data-stu-id="7eda3-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="7eda3-144">セキュリティ上の理由から、このデータを最初に検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="7eda3-145">ベンダーのエントリがある場合は (ため、 `Category` CascadingDropDown の最初の要素のプロパティに設定されて`"Vendor"`)、選択したベンダーの ID を取得することがあります。</span><span class="sxs-lookup"><span data-stu-id="7eda3-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="7eda3-146">メソッドの残りの部分では、かなりの単純明快をしは。</span><span class="sxs-lookup"><span data-stu-id="7eda3-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="7eda3-147">ベンダーの ID は、そのベンダーのすべての関連付けられている連絡先を取得する SQL クエリのパラメーターとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="7eda3-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="7eda3-148">型の配列を返します、もう一度`CascadingDropDownNameValue`します。</span><span class="sxs-lookup"><span data-stu-id="7eda3-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="7eda3-149">ASP.NET ページを読み込むし、後しばらくすると、ベンダーの一覧が 25 のエントリが入力されます。</span><span class="sxs-lookup"><span data-stu-id="7eda3-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="7eda3-150">1 つのエントリを選択し、データが 2 つ目のドロップダウン リストを格納する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="7eda3-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="7eda3-151">[![最初のリストが自動的に入力します。](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7eda3-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="7eda3-152">最初のリストが自動的に入力されます ([フルサイズの画像を表示する をクリックします](using-cascadingdropdown-with-a-database-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="7eda3-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="7eda3-153">[![2 番目のリストが最初のリストの選択に応じて入力します。](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7eda3-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="7eda3-154">2 番目のリストが最初のリストの選択に応じて入力 ([フルサイズの画像を表示する をクリックします](using-cascadingdropdown-with-a-database-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="7eda3-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7eda3-155">[前へ](filling-a-list-using-cascadingdropdown-cs.md)
> [次へ](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7eda3-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
