---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: ObjectDataSource のパラメーターの値 (c#) をプログラムによって設定 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DAL および BLL を 1 つの入力パラメーターを受け取り、データを返すメソッドを追加で紹介します。 例は、このパラメーターを設定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 946db6ad8acd5c1229659c6bc5beb454320740ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376337"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a><span data-ttu-id="a9768-104">ObjectDataSource のパラメーターの値 (c#) をプログラムによって設定</span><span class="sxs-lookup"><span data-stu-id="a9768-104">Programmatically Setting the ObjectDataSource's Parameter Values (C#)</span></span>
====================
<span data-ttu-id="a9768-105">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="a9768-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="a9768-106">[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe)または[PDF のダウンロード](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="a9768-106">[Download Sample App](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) or [Download PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)</span></span>

> <span data-ttu-id="a9768-107">このチュートリアルでは、DAL および BLL を 1 つの入力パラメーターを受け取り、データを返すメソッドを追加で紹介します。</span><span class="sxs-lookup"><span data-stu-id="a9768-107">In this tutorial we'll look at adding a method to our DAL and BLL that accepts a single input parameter and returns data.</span></span> <span data-ttu-id="a9768-108">例は、このパラメーターをプログラムによって設定されます。</span><span class="sxs-lookup"><span data-stu-id="a9768-108">The example will set this parameter programmatically.</span></span>


## <a name="introduction"></a><span data-ttu-id="a9768-109">はじめに</span><span class="sxs-lookup"><span data-stu-id="a9768-109">Introduction</span></span>

<span data-ttu-id="a9768-110">説明したように、[前のチュートリアル](declarative-parameters-cs.md)さまざまなオプションは宣言によって ObjectDataSource のメソッドにパラメーター値を渡すために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a9768-110">As we saw in the [previous tutorial](declarative-parameters-cs.md), a number of options are available for declaratively passing parameter values to the ObjectDataSource's methods.</span></span> <span data-ttu-id="a9768-111">ページで、Web コントロールから取得ハード コーディングされたパラメーター値の場合、またはデータ ソースによって読み取り可能なその他のソースでは、`Parameter`オブジェクトなど、コードを記述しなくても値が入力パラメーターにバインドできます。</span><span class="sxs-lookup"><span data-stu-id="a9768-111">If the parameter value is hard-coded, comes from a Web control on the page, or is in any other source that is readable by a data source `Parameter` object, for example, that value can be bound to the input parameter without writing a line of code.</span></span>

<span data-ttu-id="a9768-112">ただし、組み込みのデータ ソースのいずれかで考慮されていないソースからパラメーター値を取得する場合がある可能性があります`Parameter`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a9768-112">There may be times, however, when the parameter value comes from some source not already accounted for by one of the built-in data source `Parameter` objects.</span></span> <span data-ttu-id="a9768-113">私たちのサイトには、ユーザー アカウントがサポートされている場合、現在ログインしている訪問者のユーザー ID に基づくパラメーターを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9768-113">If our site supported user accounts we might want to set the parameter based on the currently logged in visitor's User ID.</span></span> <span data-ttu-id="a9768-114">または、に沿って ObjectDataSource の基になるオブジェクトのメソッドに送信する前に、パラメーターの値をカスタマイズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9768-114">Or we may need to customize the parameter value before sending it along to the ObjectDataSource's underlying object's method.</span></span>

<span data-ttu-id="a9768-115">たびに ObjectDataSource の`Select`メソッドが呼び出される、ObjectDataSource がその[するイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a9768-115">Whenever the ObjectDataSource's `Select` method is invoked the ObjectDataSource first raises its [Selecting event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx).</span></span> <span data-ttu-id="a9768-116">ObjectDataSource の基になるオブジェクトのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a9768-116">The ObjectDataSource's underlying object's method is then invoked.</span></span> <span data-ttu-id="a9768-117">ObjectDataSource の完了後[選択したイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)(図 1 は、このイベントのシーケンスを示しています) が起動します。</span><span class="sxs-lookup"><span data-stu-id="a9768-117">Once that completes the ObjectDataSource's [Selected event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) fires (Figure 1 illustrates this sequence of events).</span></span> <span data-ttu-id="a9768-118">ObjectDataSource の基になるオブジェクトのメソッドに渡されたパラメーター値を設定またはのイベント ハンドラーでカスタマイズできる、`Selecting`イベント。</span><span class="sxs-lookup"><span data-stu-id="a9768-118">The parameter values passed into the ObjectDataSource's underlying object's method can be set or customized in an event handler for the `Selecting` event.</span></span>


<span data-ttu-id="a9768-119">[![ObjectDataSource の選択と選択イベント起動する前に、後の基になるオブジェクトのメソッドが呼び出されます](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-119">[![The ObjectDataSource's Selected and Selecting Events Fire Before and After Its Underlying Object's Method is Invoked](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)</span></span>

<span data-ttu-id="a9768-120">**図 1**:、ObjectDataSource の`Selected`と`Selecting`イベントの発生前に、と後の基になるオブジェクトのメソッドが呼び出されます ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-120">**Figure 1**: The ObjectDataSource's `Selected` and `Selecting` Events Fire Before and After Its Underlying Object's Method is Invoked ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))</span></span>


<span data-ttu-id="a9768-121">このチュートリアルでは、DAL と 1 つの入力パラメーターを受け取る BLL にメソッドを追加することに注目します`Month`、型の`int`を返します、`EmployeesDataTable`雇用、記念日を指定したがある従業員を使用して作成されたオブジェクト`Month`.</span><span class="sxs-lookup"><span data-stu-id="a9768-121">In this tutorial we'll look at adding a method to our DAL and BLL that accepts a single input parameter `Month`, of type `int` and returns an `EmployeesDataTable` object populated with those employees that have their hiring anniversary in the specified `Month`.</span></span> <span data-ttu-id="a9768-122">この例ではプログラムで「従業員の記念日今月」の一覧を表示、現在の月に基づくこのパラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="a9768-122">Our example will set this parameter programmatically based on the current month, showing a list of "Employee Anniversaries This Month."</span></span>

<span data-ttu-id="a9768-123">それでは、始めましょう!</span><span class="sxs-lookup"><span data-stu-id="a9768-123">Let's get started!</span></span>

## <a name="step-1-adding-a-method-toemployeestableadapter"></a><span data-ttu-id="a9768-124">手順 1: 追加する方法`EmployeesTableAdapter`</span><span class="sxs-lookup"><span data-stu-id="a9768-124">Step 1: Adding a Method to`EmployeesTableAdapter`</span></span>

<span data-ttu-id="a9768-125">これらの従業員を取得するための手段を追加する必要があります最初の例の持つ`HireDate`で指定した月が発生しました。</span><span class="sxs-lookup"><span data-stu-id="a9768-125">For our first example we need to add a means to retrieve those employees whose `HireDate` occurred in a specified month.</span></span> <span data-ttu-id="a9768-126">最初のメソッドを作成する必要があります、アーキテクチャに従ってこの機能を提供する`EmployeesTableAdapter`適切な SQL ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="a9768-126">To provide this functionality in accordance with our architecture we need to first create a method in `EmployeesTableAdapter` that maps to the proper SQL statement.</span></span> <span data-ttu-id="a9768-127">これを実現するには、Northwind の型指定されたデータセットを開くことで開始します。</span><span class="sxs-lookup"><span data-stu-id="a9768-127">To accomplish this, start by opening the Northwind Typed DataSet.</span></span> <span data-ttu-id="a9768-128">右クリックし、`EmployeesTableAdapter`ラベルし、クエリの追加 を選択します。</span><span class="sxs-lookup"><span data-stu-id="a9768-128">Right-click on the `EmployeesTableAdapter` label and choose Add Query.</span></span>


<span data-ttu-id="a9768-129">[![新しいクエリ、EmployeesTableAdapter を追加します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-129">[![Add a New Query to the EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)</span></span>

<span data-ttu-id="a9768-130">**図 2**: 新しいクエリを追加、 `EmployeesTableAdapter` ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-130">**Figure 2**: Add a New Query to the `EmployeesTableAdapter` ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))</span></span>


<span data-ttu-id="a9768-131">行を返す SQL ステートメントを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="a9768-131">Choose to add a SQL statement that returns rows.</span></span> <span data-ttu-id="a9768-132">指定に達すると、`SELECT`ステートメントは、既定値を画面`SELECT`のステートメント、`EmployeesTableAdapter`は既に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a9768-132">When you reach the Specify a `SELECT` Statement screen the default `SELECT` statement for the `EmployeesTableAdapter` will already be loaded.</span></span> <span data-ttu-id="a9768-133">追加するだけです、`WHERE`句:`WHERE DATEPART(m, HireDate) = @Month`します。</span><span class="sxs-lookup"><span data-stu-id="a9768-133">Simply add in the `WHERE` clause: `WHERE DATEPART(m, HireDate) = @Month`.</span></span> <span data-ttu-id="a9768-134">[DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) T-SQL 関数の特定の日付部分を返しますです、`datetime`型。 この場合、使用している`DATEPART`の月を返す、`HireDate`列。</span><span class="sxs-lookup"><span data-stu-id="a9768-134">[DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) is a T-SQL function that returns a particular date portion of a `datetime` type; in this case we're using `DATEPART` to return the month of the `HireDate` column.</span></span>


<span data-ttu-id="a9768-135">[![戻り値、行が、HireDate 列のみが未満かと等しい、@HiredBeforeDateパラメーター](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-135">[![Return Only Those Rows Where the HireDate Column is Less Than or Equal to the @HiredBeforeDate Parameter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)</span></span>

<span data-ttu-id="a9768-136">**図 3**: 返すのみこれらの行が、`HireDate`列は、以下に、`@HiredBeforeDate`パラメーター ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-136">**Figure 3**: Return Only Those Rows Where the `HireDate` Column is Less Than or Equal to the `@HiredBeforeDate` Parameter ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))</span></span>


<span data-ttu-id="a9768-137">最後に、変更、`FillBy`と`GetDataBy`メソッド名をする`FillByHiredDateMonth`と`GetEmployeesByHiredDateMonth`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="a9768-137">Finally, change the `FillBy` and `GetDataBy` method names to `FillByHiredDateMonth` and `GetEmployeesByHiredDateMonth`, respectively.</span></span>


<span data-ttu-id="a9768-138">[![FillBy と GetDataBy よりもより適切なメソッド名を選択します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-138">[![Choose More Appropriate Method Names Than FillBy and GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)</span></span>

<span data-ttu-id="a9768-139">**図 4**: 選択より適切なメソッド名よりも`FillBy`と`GetDataBy`([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-139">**Figure 4**: Choose More Appropriate Method Names Than `FillBy` and `GetDataBy` ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))</span></span>


<span data-ttu-id="a9768-140">ウィザードを完了し、データセットのデザイン画面に戻るには、[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9768-140">Click Finish to complete the wizard and return to the DataSet's design surface.</span></span> <span data-ttu-id="a9768-141">`EmployeesTableAdapter`従業員を雇用すると、指定した 1 か月にアクセスするためのメソッドの新しいセットを含める必要がありますようになりました。</span><span class="sxs-lookup"><span data-stu-id="a9768-141">The `EmployeesTableAdapter` should now include a new set of methods for accessing employees hired in a specified month.</span></span>


<span data-ttu-id="a9768-142">[![新しいメソッド、データセットのデザイン画面に表示します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-142">[![The New Methods Appear in the DataSet's Design Surface](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)</span></span>

<span data-ttu-id="a9768-143">**図 5**: [新しいメソッドが表示されます、データセットのデザイン画面で ([フルサイズの画像を表示する] をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-143">**Figure 5**: The New Methods Appear in the DataSet's Design Surface ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))</span></span>


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a><span data-ttu-id="a9768-144">手順 2: 追加、`GetEmployeesByHiredDateMonth(month)`メソッドをビジネス ロジック層</span><span class="sxs-lookup"><span data-stu-id="a9768-144">Step 2: Adding the`GetEmployeesByHiredDateMonth(month)`Method to the Business Logic Layer</span></span>

<span data-ttu-id="a9768-145">ロジックをアプリケーション アーキテクチャは、別のレイヤーのビジネス ロジックとデータにアクセスするために指定した日付より前に採用された従業員を取得するまで、DAL の呼び出しを BLL メソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9768-145">Since our application architecture uses a separate layer for the business logic and data access logic, we need to add a method to our BLL that calls down to the DAL to retrieve employees hired before a specified date.</span></span> <span data-ttu-id="a9768-146">開く、`EmployeesBLL.cs`ファイルを開き、次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9768-146">Open the `EmployeesBLL.cs` file and add the following method:</span></span>


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

<span data-ttu-id="a9768-147">このクラスで、その他のメソッドと同様`GetEmployeesByHiredDateMonth(month)`DAL 呼び出すだけですし、結果を返します。</span><span class="sxs-lookup"><span data-stu-id="a9768-147">As with our other methods in this class, `GetEmployeesByHiredDateMonth(month)` simply calls down into the DAL and returns the results.</span></span>

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a><span data-ttu-id="a9768-148">手順 3: 従業員の雇用の記念日はこの月を表示します。</span><span class="sxs-lookup"><span data-stu-id="a9768-148">Step 3: Displaying Employees Whose Hiring Anniversary Is This Month</span></span>

<span data-ttu-id="a9768-149">この例の最後の手順では、従業員の雇用の記念日はこの月を表示します。</span><span class="sxs-lookup"><span data-stu-id="a9768-149">Our final step for this example is to display those employees whose hiring anniversary is this month.</span></span> <span data-ttu-id="a9768-150">GridView を追加することで開始、`ProgrammaticParams.aspx`ページで、`BasicReporting`フォルダーとそのデータ ソースとして新しい ObjectDataSource を追加します。</span><span class="sxs-lookup"><span data-stu-id="a9768-150">Start by adding a GridView to the `ProgrammaticParams.aspx` page in the `BasicReporting` folder and add a new ObjectDataSource as its data source.</span></span> <span data-ttu-id="a9768-151">構成を使用する ObjectDataSource、`EmployeesBLL`クラス、`SelectMethod`に設定`GetEmployeesByHiredDateMonth(month)`します。</span><span class="sxs-lookup"><span data-stu-id="a9768-151">Configure the ObjectDataSource to use the `EmployeesBLL` class with the `SelectMethod` set to `GetEmployeesByHiredDateMonth(month)`.</span></span>


<span data-ttu-id="a9768-152">[![EmployeesBLL クラスを使用して、](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-152">[![Use the EmployeesBLL Class](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)</span></span>

<span data-ttu-id="a9768-153">**図 6**: 使用して、`EmployeesBLL`クラス ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-153">**Figure 6**: Use the `EmployeesBLL` Class ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))</span></span>


<span data-ttu-id="a9768-154">[![The GetEmployeesByHiredDateMonth(month) から選択メソッド](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-154">[![Select From the GetEmployeesByHiredDateMonth(month) method](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)</span></span>

<span data-ttu-id="a9768-155">**図 7**: Select From、`GetEmployeesByHiredDateMonth(month)`メソッド ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-155">**Figure 7**: Select From the `GetEmployeesByHiredDateMonth(month)` method ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))</span></span>


<span data-ttu-id="a9768-156">最後の画面には、提供するよう求められます、`month`パラメーターの値のソース。</span><span class="sxs-lookup"><span data-stu-id="a9768-156">The final screen asks us to provide the `month` parameter value's source.</span></span> <span data-ttu-id="a9768-157">この値をプログラムで設定しますがため、パラメーター ソースが [なし]、既定値に設定したままにはオプションし、[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9768-157">Since we'll set this value programmatically, leave the Parameter source set to the default None option and click Finish.</span></span>


<span data-ttu-id="a9768-158">[![[なし] に、パラメーター ソースの設定をそのまま使用します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-158">[![Leave the Parameter Source Set to None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)</span></span>

<span data-ttu-id="a9768-159">**図 8**: [なし] にパラメーターのソースの設定のままに ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-159">**Figure 8**: Leave the Parameter Source Set to None ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))</span></span>


<span data-ttu-id="a9768-160">これが作成されます、`Parameter`オブジェクトで ObjectDataSource の`SelectParameters`値が指定されていないコレクション。</span><span class="sxs-lookup"><span data-stu-id="a9768-160">This will create a `Parameter` object in the ObjectDataSource's `SelectParameters` collection that does not have a value specified.</span></span>


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

<span data-ttu-id="a9768-161">この値をプログラムで設定する必要がありますの ObjectDataSource のイベント ハンドラーを作成する`Selecting`イベント。</span><span class="sxs-lookup"><span data-stu-id="a9768-161">To set this value programmatically, we need to create an event handler for the ObjectDataSource's `Selecting` event.</span></span> <span data-ttu-id="a9768-162">これを実現するには、デザイン ビューに移動し、ObjectDataSource をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9768-162">To accomplish this, go to the Design view and double-click the ObjectDataSource.</span></span> <span data-ttu-id="a9768-163">または、ObjectDataSource を選択、[プロパティ] ウィンドウに移動し、稲妻のアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9768-163">Alternatively, select the ObjectDataSource, go to the Properties window, and click the lightning bolt icon.</span></span> <span data-ttu-id="a9768-164">次をダブルクリックするか、テキスト ボックスの横に、`Selecting`イベントまたはイベント ハンドラーを使用する名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="a9768-164">Next, either double-click in the textbox next to the `Selecting` event or type in the name of the event handler you want to use.</span></span>


![Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで稲妻のアイコンをクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

<span data-ttu-id="a9768-166">**図 9**: Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで稲妻のアイコンをクリックします</span><span class="sxs-lookup"><span data-stu-id="a9768-166">**Figure 9**: Click on the Lightning Bolt Icon in the Properties Window to List a Web Control's Events</span></span>


<span data-ttu-id="a9768-167">どちらの方法では、新しいイベント ハンドラーを追加の ObjectDataSource の`Selecting`ページの分離コード クラスにイベント。</span><span class="sxs-lookup"><span data-stu-id="a9768-167">Both approaches add a new event handler for the ObjectDataSource's `Selecting` event to the page's code-behind class.</span></span> <span data-ttu-id="a9768-168">このイベント ハンドラーには、読み取りし、書き込みを使用してパラメーター値できます`e.InputParameters[parameterName]`ここで、 *`parameterName`* の値である、`Name`属性、`<asp:Parameter>`タグ (、`InputParameters`コレクションこともできます序数、としてのインデックス付き`e.InputParameters[index]`)。</span><span class="sxs-lookup"><span data-stu-id="a9768-168">In this event handler we can read and write to the parameter values using `e.InputParameters[parameterName]`, where *`parameterName`* is the value of the `Name` attribute in the `<asp:Parameter>` tag (the `InputParameters` collection can also be indexed ordinally, as in `e.InputParameters[index]`).</span></span> <span data-ttu-id="a9768-169">設定する、`month`パラメーター、現在の月を追加するには、次の`Selecting`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="a9768-169">To set the `month` parameter to the current month, add the following to the `Selecting` event handler:</span></span>


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

<span data-ttu-id="a9768-170">ブラウザーからこのページにアクセスすると、1 つだけの従業員が採用された今月 (3 月) 確認できます Laura Callahan、1994 年以来勤務しています。</span><span class="sxs-lookup"><span data-stu-id="a9768-170">When visiting this page through a browser we can see that only one employee was hired this month (March) Laura Callahan, who's been with the company since 1994.</span></span>


<span data-ttu-id="a9768-171">[![これらの従業員を記念日今月が表示されます。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="a9768-171">[![Those Employees Whose Anniversaries This Month Are Shown](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)</span></span>

<span data-ttu-id="a9768-172">**図 10**: これらの従業員を記念日この 1 か月が表示されます ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))。</span><span class="sxs-lookup"><span data-stu-id="a9768-172">**Figure 10**: Those Employees Whose Anniversaries This Month Are Shown ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))</span></span>


## <a name="summary"></a><span data-ttu-id="a9768-173">まとめ</span><span class="sxs-lookup"><span data-stu-id="a9768-173">Summary</span></span>

<span data-ttu-id="a9768-174">ObjectDataSource のパラメーターの設定できる値は通常ここでは、行のコードを必要とせずに、パラメーター値をプログラムで設定する簡単です。</span><span class="sxs-lookup"><span data-stu-id="a9768-174">While the ObjectDataSource's parameters' values can typically be set declaratively, without requiring a line of code, it's easy to set the parameter values programmatically.</span></span> <span data-ttu-id="a9768-175">ObjectDataSource のイベント ハンドラーを作成を行う必要がありますすべてが`Selecting`、基になるオブジェクトのメソッドが呼び出され、使用して、1 つまたは複数のパラメーターの値を手動で設定する前に発生するイベント、`InputParameters`コレクション。</span><span class="sxs-lookup"><span data-stu-id="a9768-175">All we need to do is create an event handler for the ObjectDataSource's `Selecting` event, which fires before the underlying object's method is invoked, and manually set the values for one or more parameters via the `InputParameters` collection.</span></span>

<span data-ttu-id="a9768-176">このチュートリアルでは、基本レポートのセクションで終了します。</span><span class="sxs-lookup"><span data-stu-id="a9768-176">This tutorial concludes the Basic Reporting section.</span></span> <span data-ttu-id="a9768-177">[次のチュートリアル](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)マスター詳細シナリオのフィルター処理を私たちがデータをフィルター処理の訪問者を許可するための手法を見てとセクション マスター レポートから詳細レポートにドリル ダウンを開始します。</span><span class="sxs-lookup"><span data-stu-id="a9768-177">The [next tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kicks off the Filtering and Master-Details Scenarios section, in which we'll look at techniques for allowing the visitor to filter data and drill down from a master report into a details report.</span></span>

<span data-ttu-id="a9768-178">満足のプログラミングです。</span><span class="sxs-lookup"><span data-stu-id="a9768-178">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="a9768-179">執筆者紹介</span><span class="sxs-lookup"><span data-stu-id="a9768-179">About the Author</span></span>

<span data-ttu-id="a9768-180">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。</span><span class="sxs-lookup"><span data-stu-id="a9768-180">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="a9768-181">Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。</span><span class="sxs-lookup"><span data-stu-id="a9768-181">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="a9768-182">最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。</span><span class="sxs-lookup"><span data-stu-id="a9768-182">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="a9768-183">彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="a9768-183">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="a9768-184">彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。</span><span class="sxs-lookup"><span data-stu-id="a9768-184">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="a9768-185">特別なに感謝します。</span><span class="sxs-lookup"><span data-stu-id="a9768-185">Special Thanks To</span></span>

<span data-ttu-id="a9768-186">このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。</span><span class="sxs-lookup"><span data-stu-id="a9768-186">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="a9768-187">このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。</span><span class="sxs-lookup"><span data-stu-id="a9768-187">Lead reviewer for this tutorial was Hilton Giesenow.</span></span> <span data-ttu-id="a9768-188">今後、MSDN の記事を確認したいですか。</span><span class="sxs-lookup"><span data-stu-id="a9768-188">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="a9768-189">場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="a9768-189">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9768-190">[前へ](declarative-parameters-cs.md)
> [次へ](displaying-data-with-the-objectdatasource-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a9768-190">[Previous](declarative-parameters-cs.md)
[Next](displaying-data-with-the-objectdatasource-vb.md)</span></span>
