---
uid: web-pages/overview/data/working-with-files
title: ASP.NET Web Pages (Razor) サイトでファイルを扱う |Microsoft Docs
author: tfitzmac
description: この章では、読み取り、書き込み、追加、削除、およびファイルをアップロードする方法について説明します。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 20fdbfda7d3e4b214a8e201c4a815a29c5bb7061
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834322"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="31e38-103">ASP.NET Web ページ (Razor) サイトでファイルの使用</span><span class="sxs-lookup"><span data-stu-id="31e38-103">Working with Files in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="31e38-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="31e38-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="31e38-105">この記事では、読み取り、書き込み、追加、削除、および ASP.NET Web Pages (Razor) サイトでファイルをアップロードする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="31e38-105">This article explains how to read, write, append, delete, and upload files in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="31e38-106">イメージをアップロードし、それらを操作する場合 (反転またはサイズ変更など) を参照してください[ASP.NET Web ページ サイトでのイメージを操作](https://go.microsoft.com/fwlink/?LinkId=202897)します。</span><span class="sxs-lookup"><span data-stu-id="31e38-106">If you want to upload images and manipulate them (for example, flip or resize them), see [Working with Images in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202897).</span></span>
> 
> 
> <span data-ttu-id="31e38-107">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="31e38-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="31e38-108">テキスト ファイルを作成し、データを書き込む方法。</span><span class="sxs-lookup"><span data-stu-id="31e38-108">How to create a text file and write data to it.</span></span>
> - <span data-ttu-id="31e38-109">既存のファイルにデータを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="31e38-109">How to append data to an existing file.</span></span>
> - <span data-ttu-id="31e38-110">ファイルを読み取り、そこから表示する方法。</span><span class="sxs-lookup"><span data-stu-id="31e38-110">How to read a file and display from it.</span></span>
> - <span data-ttu-id="31e38-111">Web サイトからファイルを削除する方法。</span><span class="sxs-lookup"><span data-stu-id="31e38-111">How to delete files from a website.</span></span>
> - <span data-ttu-id="31e38-112">ユーザーが 1 つまたは複数のファイルをアップロードできるようにする方法。</span><span class="sxs-lookup"><span data-stu-id="31e38-112">How to let users upload one file or multiple files.</span></span>
> 
> <span data-ttu-id="31e38-113">この記事で導入された機能のプログラミング、ASP.NET を次に示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-113">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="31e38-114">`File`オブジェクトで、ファイルを管理する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="31e38-114">The `File` object, which provides a way to manage files.</span></span>
> - <span data-ttu-id="31e38-115">`FileUpload`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="31e38-115">The `FileUpload` helper.</span></span>
> - <span data-ttu-id="31e38-116">`Path`オブジェクトで、パスとファイル名を操作できるメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="31e38-116">The `Path` object, which provides methods that let you manipulate path and file names.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31e38-117">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="31e38-117">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="31e38-118">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="31e38-118">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="31e38-119">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="31e38-119">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="31e38-120">このチュートリアルは、WebMatrix 3 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="31e38-120">This tutorial also works with WebMatrix 3.</span></span>


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a><span data-ttu-id="31e38-121">テキスト ファイルを作成し、データを書き込む</span><span class="sxs-lookup"><span data-stu-id="31e38-121">Creating a Text File and Writing Data to It</span></span>

<span data-ttu-id="31e38-122">に加えて、web サイトのデータベースを使用して、ファイルを操作する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-122">In addition to using a database in your website, you might work with files.</span></span> <span data-ttu-id="31e38-123">たとえば、サイトのデータを格納する簡単な方法としてテキスト ファイルを使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-123">For example, you might use text files as a simple way to store data for the site.</span></span> <span data-ttu-id="31e38-124">(データの格納に使用されるテキスト ファイルとも呼ばれます、*フラット ファイル*)。テキスト ファイルがなどのさまざまな形式であることができます *.txt*、 *.xml*、または *.csv* (コンマ区切り値)。</span><span class="sxs-lookup"><span data-stu-id="31e38-124">(A text file that's used to store data is sometimes called a *flat file*.) Text files can be in different formats, like *.txt*, *.xml*, or *.csv* (comma-delimited values).</span></span>

<span data-ttu-id="31e38-125">テキスト ファイルにデータを格納する場合は、使用、`File.WriteAllText`メソッドを作成するファイルを指定して、データへの書き込みをします。</span><span class="sxs-lookup"><span data-stu-id="31e38-125">If you want to store data in a text file, you can use the `File.WriteAllText` method to specify the file to create and the data to write to it.</span></span> <span data-ttu-id="31e38-126">この手順では、3 つの単純なフォームを含むページを作成します`input`要素 (名、姓、名、および電子メール アドレス) と**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e38-126">In this procedure, you'll create a page that contains a simple form with three `input` elements (first name, last name, and email address) and a **Submit** button.</span></span> <span data-ttu-id="31e38-127">ユーザーがフォームを送信するときにテキスト ファイルに、ユーザーの入力を格納します。</span><span class="sxs-lookup"><span data-stu-id="31e38-127">When the user submits the form, you'll store the user's input in a text file.</span></span>

1. <span data-ttu-id="31e38-128">という名前の新しいフォルダーを作成する*アプリ\_データ*が既に存在しない場合、します。</span><span class="sxs-lookup"><span data-stu-id="31e38-128">Create a new folder named *App\_Data*, if it doesn't exist already.</span></span>
2. <span data-ttu-id="31e38-129">という名前の新しいファイルを作成、web サイトのルート ディレクトリにある*UserData.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-129">At the root of your website, create a new file named *UserData.cshtml*.</span></span>
3. <span data-ttu-id="31e38-130">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="31e38-130">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    <span data-ttu-id="31e38-131">HTML マークアップでは、3 つのテキスト ボックスで、フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="31e38-131">The HTML markup creates the form with the three text boxes.</span></span> <span data-ttu-id="31e38-132">コードを使用して、`IsPost`処理を開始する前に、ページが送信されたかどうかを決定するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="31e38-132">In the code, you use the `IsPost` property to determine whether the page has been submitted before you start processing.</span></span>

    <span data-ttu-id="31e38-133">最初のタスクが、ユーザー入力を取得し、変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="31e38-133">The first task is to get the user input and assign it to variables.</span></span> <span data-ttu-id="31e38-134">次に、コードは 1 つコンマで区切られた文字列は、別の変数に格納されます別個の変数の値を連結します。</span><span class="sxs-lookup"><span data-stu-id="31e38-134">The code then concatenates the values of the separate variables into one comma-delimited string, which is then stored in a different variable.</span></span> <span data-ttu-id="31e38-135">コンマ区切り記号が引用符内に含まれる文字列 (「,」)、コンマを作成している大きな文字列に埋め込む文字どおりためです。</span><span class="sxs-lookup"><span data-stu-id="31e38-135">Notice that the comma separator is a string contained in quotation marks (","), because you're literally embedding a comma into the big string that you're creating.</span></span> <span data-ttu-id="31e38-136">連結するデータの最後で追加する`Environment.NewLine`します。</span><span class="sxs-lookup"><span data-stu-id="31e38-136">At the end of the data that you concatenate together, you add `Environment.NewLine`.</span></span> <span data-ttu-id="31e38-137">これには、改行 (改行文字) が追加されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-137">This adds a line break (a newline character).</span></span> <span data-ttu-id="31e38-138">このすべての連結で作成しているは、次のような文字列です。</span><span class="sxs-lookup"><span data-stu-id="31e38-138">What you're creating with all this concatenation is a string that looks like this:</span></span>

    [!code-css[Main](working-with-files/samples/sample2.css)]

    <span data-ttu-id="31e38-139">(Break を使用する非表示の行、最後にします。)</span><span class="sxs-lookup"><span data-stu-id="31e38-139">(With an invisible line break at the end.)</span></span>

    <span data-ttu-id="31e38-140">変数を作成し (`dataFile`) 内のデータを格納するファイルの名前と場所を格納しています。</span><span class="sxs-lookup"><span data-stu-id="31e38-140">You then create a variable (`dataFile`) that contains the location and name of the file to store the data in.</span></span> <span data-ttu-id="31e38-141">場所の設定には、いくつかの特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="31e38-141">Setting the location requires some special handling.</span></span> <span data-ttu-id="31e38-142">ような絶対パスをコードで参照する不適切な手法は、web サイトで*C:\Folder\File.txt* web サーバー上のファイル。</span><span class="sxs-lookup"><span data-stu-id="31e38-142">In websites, it's a bad practice to refer in code to absolute paths like *C:\Folder\File.txt* for files on the web server.</span></span> <span data-ttu-id="31e38-143">Web サイトを移動すると、絶対パスは問題になります。</span><span class="sxs-lookup"><span data-stu-id="31e38-143">If a website is moved, an absolute path will be wrong.</span></span> <span data-ttu-id="31e38-144">さらに、ホストされているサイトの (、お使いコンピューターではなく) 通常もわからない、正しいパスが、コードを記述するときに。</span><span class="sxs-lookup"><span data-stu-id="31e38-144">Moreover, for a hosted site (as opposed to on your own computer) you typically don't even know what the correct path is when you're writing the code.</span></span>

    <span data-ttu-id="31e38-145">場合によっては (現時点では、ファイルを書き込むため) などは、完全なパスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-145">But sometimes (like now, for writing a file) you do need a complete path.</span></span> <span data-ttu-id="31e38-146">ソリューションは、使用する、`MapPath`のメソッド、`Server`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="31e38-146">The solution is to use the `MapPath` method of the `Server` object.</span></span> <span data-ttu-id="31e38-147">これは、web サイトへの完全なパスを返します。</span><span class="sxs-lookup"><span data-stu-id="31e38-147">This returns the complete path to your website.</span></span> <span data-ttu-id="31e38-148">ユーザーが、web サイトのルートのパスを取得する、`~`演算子 (represen には、サイトの仮想ルート) に`MapPath`します。</span><span class="sxs-lookup"><span data-stu-id="31e38-148">To get the path for the website root, you user the `~` operator (to represen the site's virtual root) to `MapPath`.</span></span> <span data-ttu-id="31e38-149">(など、サブフォルダー名を渡すこともできます *~/App\_データ/*、そのサブフォルダーのパスを取得します)。完全なパスを作成するにはどのようなメソッドを返します上に追加の情報を連結できます。</span><span class="sxs-lookup"><span data-stu-id="31e38-149">(You can also pass a subfolder name to it, like *~/App\_Data/*, to get the path for that subfolder.) You can then concatenate additional information onto whatever the method returns in order to create a complete path.</span></span> <span data-ttu-id="31e38-150">この例では、ファイル名を追加します。</span><span class="sxs-lookup"><span data-stu-id="31e38-150">In this example, you add a file name.</span></span> <span data-ttu-id="31e38-151">(詳細でファイルとフォルダーのパスを操作する方法について[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths))。</span><span class="sxs-lookup"><span data-stu-id="31e38-151">(You can read more about how to work with file and folder paths in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)</span></span>

    <span data-ttu-id="31e38-152">ファイルを保存、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="31e38-152">The file is saved in the *App\_Data* folder.</span></span> <span data-ttu-id="31e38-153">このフォルダーは」の説明に従って、データ ファイルの格納に使用する asp.net の特別なフォルダー [ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=195209)します。</span><span class="sxs-lookup"><span data-stu-id="31e38-153">This folder is a special folder in ASP.NET that's used to store data files, as described in [Introduction to Working with a Database in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=195209).</span></span>

    <span data-ttu-id="31e38-154">`WriteAllText`のメソッド、`File`オブジェクトは、データをファイルに書き込みます。</span><span class="sxs-lookup"><span data-stu-id="31e38-154">The `WriteAllText` method of the `File` object writes the data to the file.</span></span> <span data-ttu-id="31e38-155">このメソッドは 2 つのパラメーターを受け取ります。 書き込み、ファイルと書き込む実際のデータの名前 (パス) を使用します。</span><span class="sxs-lookup"><span data-stu-id="31e38-155">This method takes two parameters: the name (with path) of the file to write to, and the actual data to write.</span></span> <span data-ttu-id="31e38-156">最初のパラメーターの名前に注意してください、`@`をプレフィックスとして文字。</span><span class="sxs-lookup"><span data-stu-id="31e38-156">Notice that the name of the first parameter has an `@` character as a prefix.</span></span> <span data-ttu-id="31e38-157">これは verbatim 文字列リテラルを提供しているなどの文字を ASP.NET に指示「/」を特別な方法で解釈する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-157">This tells ASP.NET that you're providing a verbatim string literal, and that characters like "/" should not be interpreted in special ways.</span></span> <span data-ttu-id="31e38-158">(詳細については、次を参照してください[Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)。</span><span class="sxs-lookup"><span data-stu-id="31e38-158">(For more information, see [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)</span></span>

    > [!NOTE]
    > <span data-ttu-id="31e38-159">コード内のファイルを保存するために、*アプリ\_データ*フォルダー、アプリケーション必要がある読み取り/書き込みフォルダーのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="31e38-159">In order for your code to save files in the *App\_Data* folder, the application needs read-write permissions for that folder.</span></span> <span data-ttu-id="31e38-160">開発用コンピューターにこれは通常、問題ありません。</span><span class="sxs-lookup"><span data-stu-id="31e38-160">On your development computer this is not typically an issue.</span></span> <span data-ttu-id="31e38-161">ただし、ホスティング プロバイダーの web サーバーにサイトを発行するときは、これらのアクセス許可を明示的に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-161">However, when you publish your site to a hosting provider's web server, you might need to explicitly set those permissions.</span></span> <span data-ttu-id="31e38-162">ホスティング プロバイダーのサーバーでこのコードを実行し、エラーが発生する場合は、これらのアクセス許可を設定する方法を確認するホスティング プロバイダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="31e38-162">If you run this code on a hosting provider's server and get errors, check with the hosting provider to find out how to set those permissions.</span></span>

- <span data-ttu-id="31e38-163">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-163">Run the page in a browser.</span></span> 

    ![](working-with-files/_static/image1.jpg)
- <span data-ttu-id="31e38-164">フィールドに値を入力し、クリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-164">Enter values into the fields and then click **Submit**.</span></span>
- <span data-ttu-id="31e38-165">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="31e38-165">Close the browser.</span></span>
- <span data-ttu-id="31e38-166">プロジェクトに戻り、表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="31e38-166">Return to the project and refresh the view.</span></span>
- <span data-ttu-id="31e38-167">開く、 *data.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="31e38-167">Open the *data.txt* file.</span></span> <span data-ttu-id="31e38-168">形式で送信されたデータは、ファイルです。</span><span class="sxs-lookup"><span data-stu-id="31e38-168">The data you submitted in the form is in the file.</span></span> 

    ![[イメージ]](working-with-files/_static/image2.jpg)
- <span data-ttu-id="31e38-170">閉じる、 *data.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="31e38-170">Close the *data.txt* file.</span></span>

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a><span data-ttu-id="31e38-171">既存のファイルにデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="31e38-171">Appending Data to an Existing File</span></span>

<span data-ttu-id="31e38-172">前の例では使用して`WriteAllText`が 1 つだけのデータを取得するテキスト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="31e38-172">In the previous example, you used `WriteAllText` to create a text file that's got just one piece of data in it.</span></span> <span data-ttu-id="31e38-173">メソッドをもう一度呼び出しし、同じファイル名を渡して、既存のファイルは完全に上書きされます。</span><span class="sxs-lookup"><span data-stu-id="31e38-173">If you call the method again and pass it the same file name, the existing file is completely overwritten.</span></span> <span data-ttu-id="31e38-174">ただし、ファイルを作成した後に多くの場合するファイルの末尾に新しいデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="31e38-174">However, after you've created a file you often want to add new data to the end of the file.</span></span> <span data-ttu-id="31e38-175">使用して行うことができます、`AppendAllText`のメソッド、`File`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="31e38-175">You can do that using the `AppendAllText` method of the `File` object.</span></span>

1. <span data-ttu-id="31e38-176">Web サイトでのコピーを作成、 *UserData.cshtml*ファイルを開き、コピーを名前*UserDataMultiple.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-176">In the website, make a copy of the *UserData.cshtml* file and name the copy *UserDataMultiple.cshtml*.</span></span>
2. <span data-ttu-id="31e38-177">開始する前に、コード ブロックを置き換える`<!DOCTYPE html>`次のコード ブロックを持つタグ。</span><span class="sxs-lookup"><span data-stu-id="31e38-177">Replace the code block before the opening `<!DOCTYPE html>` tag with the following code block:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    <span data-ttu-id="31e38-178">このコードでは、前の例では、1 つの変更を持っています。</span><span class="sxs-lookup"><span data-stu-id="31e38-178">This code has one change in it from the previous example.</span></span> <span data-ttu-id="31e38-179">使用する代わりに`WriteAllText`を使用して`the AppendAllText`メソッド。</span><span class="sxs-lookup"><span data-stu-id="31e38-179">Instead of using `WriteAllText`, it uses `the AppendAllText` method.</span></span> <span data-ttu-id="31e38-180">メソッドは、のようなことを除いて`AppendAllText`ファイルの末尾にデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="31e38-180">The methods are similar, except that `AppendAllText` adds the data to the end of the file.</span></span> <span data-ttu-id="31e38-181">同様`WriteAllText`、`AppendAllText`存在しない場合は、ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="31e38-181">As with `WriteAllText`, `AppendAllText` creates the file if it doesn't already exist.</span></span>
3. <span data-ttu-id="31e38-182">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-182">Run the page in a browser.</span></span>
4. <span data-ttu-id="31e38-183">フィールドの値を入力し、クリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-183">Enter values for the fields and then click **Submit**.</span></span>
5. <span data-ttu-id="31e38-184">多くのデータを追加し、もう一度、フォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="31e38-184">Add more data and submit the form again.</span></span>
6. <span data-ttu-id="31e38-185">プロジェクトに戻ります、プロジェクト フォルダーを右クリックし、クリックして**更新**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-185">Return to your project, right-click the project folder, and then click **Refresh**.</span></span>
7. <span data-ttu-id="31e38-186">開く、 *data.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="31e38-186">Open the *data.txt* file.</span></span> <span data-ttu-id="31e38-187">これで、入力した新しいデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="31e38-187">It now contains the new data that you just entered.</span></span> 

    ![[イメージ]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a><span data-ttu-id="31e38-189">読み取りとファイルからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-189">Reading and Displaying Data from a File</span></span>

<span data-ttu-id="31e38-190">場合でも、テキスト ファイルにデータを記述する必要はありません、いずれかからデータを読み取るする必要がありますをおそらく場合があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-190">Even if you don't need to write data to a text file, you'll probably sometimes need to read data from one.</span></span> <span data-ttu-id="31e38-191">これを行うには、もう一度使用できます、`File`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="31e38-191">To do this, you can again use the `File` object.</span></span> <span data-ttu-id="31e38-192">使用することができます、`File`各行を個別に読み取るオブジェクト (改行で区切られた) や、それらの区切る方法に関係なく、個々 の項目の読み取り。</span><span class="sxs-lookup"><span data-stu-id="31e38-192">You can use the `File` object to read each line individually (separated by line breaks) or to read individual item no matter how they're separated.</span></span>

<span data-ttu-id="31e38-193">この手順では、読み取り、前の例で作成したデータを表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-193">This procedure shows you how to read and display the data that you created in the previous example.</span></span>

1. <span data-ttu-id="31e38-194">という名前の新しいファイルを作成、web サイトのルート ディレクトリにある*DisplayData.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-194">At the root of your website, create a new file named *DisplayData.cshtml*.</span></span>
2. <span data-ttu-id="31e38-195">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="31e38-195">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    <span data-ttu-id="31e38-196">という名前の変数に、前の例で作成したファイルを読み取ることによって、コードの先頭`userData`、このメソッドの呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="31e38-196">The code starts by reading the file that you created in the previous example into a variable named `userData`, using this method call:</span></span>

    [!code-css[Main](working-with-files/samples/sample5.css)]

    <span data-ttu-id="31e38-197">これを行うコードが内部では、`if`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="31e38-197">The code to do this is inside an `if` statement.</span></span> <span data-ttu-id="31e38-198">使用することをお勧めはファイルを確認するには、ときに、`File.Exists`最初に、ファイルが使用できるかどうかを確認するメソッド。</span><span class="sxs-lookup"><span data-stu-id="31e38-198">When you want to read a file, it's a good idea to use the `File.Exists` method to determine first whether the file is available.</span></span> <span data-ttu-id="31e38-199">コードでは、ファイルが空かどうかも確認します。</span><span class="sxs-lookup"><span data-stu-id="31e38-199">The code also checks whether the file is empty.</span></span>

    <span data-ttu-id="31e38-200">ページの本文には 2 つ`foreach`ループ、他の内部で入れ子になった 1 つ。</span><span class="sxs-lookup"><span data-stu-id="31e38-200">The body of the page contains two `foreach` loops, one nested inside the other.</span></span> <span data-ttu-id="31e38-201">外側`foreach`ループは、データ ファイルから一度に 1 つの行を取得します。</span><span class="sxs-lookup"><span data-stu-id="31e38-201">The outer `foreach` loop gets one line at a time from the data file.</span></span> <span data-ttu-id="31e38-202">ファイル内の改行で行を定義するこの例では、&#8212;これは、データ項目ごとに独自の行があります。</span><span class="sxs-lookup"><span data-stu-id="31e38-202">In this case, the lines are defined by line breaks in the file &#8212; that is, each data item is on its own line.</span></span> <span data-ttu-id="31e38-203">外側のループが新しい項目を作成します (`<li>`要素)、順序付きリスト内で (`<ol>`要素)。</span><span class="sxs-lookup"><span data-stu-id="31e38-203">The outer loop creates a new item (`<li>` element) inside an ordered list (`<ol>` element).</span></span>

    <span data-ttu-id="31e38-204">内側のループでは、区切り記号としてコンマを使用して項目 (フィールド) に各データ行を分割します。</span><span class="sxs-lookup"><span data-stu-id="31e38-204">The inner loop splits each data line into items (fields) using a comma as a delimiter.</span></span> <span data-ttu-id="31e38-205">(前の例に基づいて、つまり、行ごとに 3 つのフィールドが含まれている&#8212;をコンマで区切られた各名、姓、名、および電子メール アドレス、.)内側のループを作成も、`<ul>`データ行の各フィールドの項目のリストと 1 つの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-205">(Based on the previous example, this means that each line contains three fields &#8212; the first name, last name, and email address, each separated by a comma.) The inner loop also creates a `<ul>` list and displays one list item for each field in the data line.</span></span>

    <span data-ttu-id="31e38-206">コードでは、2 つのデータ型、配列を使用する方法を示しています。 および`char`データ型。</span><span class="sxs-lookup"><span data-stu-id="31e38-206">The code illustrates how to use two data types, an array and the `char` data type.</span></span> <span data-ttu-id="31e38-207">配列が必要な`File.ReadAllLines`メソッドは配列としてデータを返します。</span><span class="sxs-lookup"><span data-stu-id="31e38-207">The array is required because the `File.ReadAllLines` method returns data as an array.</span></span> <span data-ttu-id="31e38-208">`char`ため、データ型が必要ですが、`Split`メソッドを返します。、`array`型の各要素が`char`します。</span><span class="sxs-lookup"><span data-stu-id="31e38-208">The `char` data type is required because the `Split` method returns an `array` in which each element is of the type `char`.</span></span> <span data-ttu-id="31e38-209">(配列については、次を参照してください[Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)。)。</span><span class="sxs-lookup"><span data-stu-id="31e38-209">(For information about arrays, see [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)</span></span>
3. <span data-ttu-id="31e38-210">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-210">Run the page in a browser.</span></span> <span data-ttu-id="31e38-211">前の例については、入力したデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-211">The data you entered for the previous examples is displayed.</span></span> 

    ![[イメージ]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> <span data-ttu-id="31e38-213">**Microsoft Excel のコンマ区切りファイルからデータを表示します。**</span><span class="sxs-lookup"><span data-stu-id="31e38-213">**Displaying Data from a Microsoft Excel Comma-Delimited File**</span></span>
> 
> <span data-ttu-id="31e38-214">Microsoft Excel を使用して、コンマ区切りファイルとしてのスプレッドシートに含まれるデータを保存することができます (*.csv*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="31e38-214">You can use Microsoft Excel to save the data contained in a spreadsheet as a comma-delimited file (*.csv* file).</span></span> <span data-ttu-id="31e38-215">この場合、Excel 形式ではなく、プレーン テキストで、ファイルが保存されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-215">When you do, the file is saved in plain text, not in Excel format.</span></span> <span data-ttu-id="31e38-216">スプレッドシート内の各行は、テキスト ファイル内での改行で区切られます、各データ項目はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="31e38-216">Each row in the spreadsheet is separated by a line break in the text file, and each data item is separated by a comma.</span></span> <span data-ttu-id="31e38-217">コード内で、データ ファイルの名前を変更するだけで、Excel のコンマ区切りファイルを読み取るには、前の例に示すようにコードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="31e38-217">You can use the code shown in the previous example to read an Excel comma-delimited file just by changing the name of the data file in your code.</span></span>


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a><span data-ttu-id="31e38-218">ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="31e38-218">Deleting Files</span></span>

<span data-ttu-id="31e38-219">使用することができます、web サイトからファイルを削除する、`File.Delete`メソッド。</span><span class="sxs-lookup"><span data-stu-id="31e38-219">To delete files from your website, you can use the `File.Delete` method.</span></span> <span data-ttu-id="31e38-220">この手順は、ユーザーがイメージを削除できるようにする方法を示します (*.jpg*ファイル) から、*イメージ*フォルダー、ファイルの名前がわかっている場合。</span><span class="sxs-lookup"><span data-stu-id="31e38-220">This procedure shows how to let users delete an image (*.jpg* file) from an *images* folder if they know the name of the file.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="31e38-221">**重要な**を運用環境の web サイトを通常できるユーザーを制限、データを変更します。</span><span class="sxs-lookup"><span data-stu-id="31e38-221">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="31e38-222">サイトのタスクを実行するユーザーを承認する方法についてとメンバーシップを設定する方法については、次を参照してください。[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)します。</span><span class="sxs-lookup"><span data-stu-id="31e38-222">For information about how to set up membership and about ways to authorize users to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="31e38-223">という名前のサブフォルダーを作成、web サイトで*イメージ*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-223">In the website, create a subfolder named *images*.</span></span>
2. <span data-ttu-id="31e38-224">1 つまたは複数のコピー *.jpg*へのファイル、*イメージ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="31e38-224">Copy one or more *.jpg* files into the *images* folder.</span></span>
3. <span data-ttu-id="31e38-225">という名前の新しいファイルを作成、web サイトのルートに*FileDelete.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-225">In the root of the website, create a new file named *FileDelete.cshtml*.</span></span>
4. <span data-ttu-id="31e38-226">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="31e38-226">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    <span data-ttu-id="31e38-227">このページには、イメージ ファイルの名前を入力できるフォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="31e38-227">This page contains a form where users can enter the name of an image file.</span></span> <span data-ttu-id="31e38-228">入力がない、 *.jpg*ファイル名拡張子。 このようなファイル名を制限することでサイト上の任意のファイルを削除していますからユーザーを回避するヘルプ。</span><span class="sxs-lookup"><span data-stu-id="31e38-228">They don't enter the *.jpg* file-name extension; by restricting the file name like this, you help prevents users from deleting arbitrary files on your site.</span></span>

    <span data-ttu-id="31e38-229">コードでは、ユーザーが入力したし、完全なパスを構築し、ファイル名を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="31e38-229">The code reads the file name that the user has entered and then constructs a complete path.</span></span> <span data-ttu-id="31e38-230">コードをパスを作成するには、現在の web サイトのパスを使用して (によって返される、`Server.MapPath`メソッド)、*イメージ*フォルダー名、ユーザーが指定すると、名前、およびリテラル文字列として".jpg"。</span><span class="sxs-lookup"><span data-stu-id="31e38-230">To create the path, the code uses the current website path (as returned by the `Server.MapPath` method), the *images* folder name, the name that the user has provided, and ".jpg" as a literal string.</span></span>

    <span data-ttu-id="31e38-231">コードでは、ファイルを削除する、`File.Delete`メソッドを作成した完全なパスを渡します。</span><span class="sxs-lookup"><span data-stu-id="31e38-231">To delete the file, the code calls the `File.Delete` method, passing it the full path that you just constructed.</span></span> <span data-ttu-id="31e38-232">マークアップの末尾には、コードには、ファイルが削除されたことの確認メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-232">At the end of the markup, code displays a confirmation message that the file was deleted.</span></span>
5. <span data-ttu-id="31e38-233">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-233">Run the page in a browser.</span></span> 

    ![[イメージ]](working-with-files/_static/image5.jpg)
6. <span data-ttu-id="31e38-235">削除し、をクリックし、ファイルの名前を入力**送信**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-235">Enter the name of the file to delete and then click **Submit**.</span></span> <span data-ttu-id="31e38-236">ファイルが削除された場合は、ページの下部にあるファイルの名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-236">If the file was deleted, the name of the file is displayed at the bottom of the page.</span></span>

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a><span data-ttu-id="31e38-237">ファイルをできるようにするためのユーザーのアップロード</span><span class="sxs-lookup"><span data-stu-id="31e38-237">Letting Users Upload a File</span></span>

<span data-ttu-id="31e38-238">`FileUpload`ヘルパーにより、ユーザーが自分の web サイトにファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="31e38-238">The `FileUpload` helper lets users upload files to your website.</span></span> <span data-ttu-id="31e38-239">次の手順では、ユーザーが 1 つのファイルをアップロードできるようにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-239">The procedure below shows you how to let users upload a single file.</span></span>

1. <span data-ttu-id="31e38-240">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)以前追加しなかった場合、します。</span><span class="sxs-lookup"><span data-stu-id="31e38-240">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you didn't add it previously.</span></span>
2. <span data-ttu-id="31e38-241">*アプリ\_データ*フォルダーは、新しいフォルダーを作成し、名前を付けます*UploadedFiles*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-241">In the *App\_Data* folder, create a new a folder and name it *UploadedFiles*.</span></span>
3. <span data-ttu-id="31e38-242">という名前の新しいファイルの作成のルートに*FileUpload.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-242">In the root, create a new file named *FileUpload.cshtml*.</span></span>
4. <span data-ttu-id="31e38-243">次のように、ページで、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="31e38-243">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    <span data-ttu-id="31e38-244">ページの本文部分を使用して、`FileUpload`アップロード ボックスとおそらく慣れているボタンを作成するヘルパー。</span><span class="sxs-lookup"><span data-stu-id="31e38-244">The body portion of the page uses the `FileUpload` helper to create the upload box and buttons that you're probably familiar with:</span></span>

    ![[イメージ]](working-with-files/_static/image6.jpg)

    <span data-ttu-id="31e38-246">設定したプロパティ、`FileUpload`ヘルパーは、アップロードするファイルの 1 つの箱をして送信 ボタンの読み取りをすることを指定**アップロード**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-246">The properties that you set for the `FileUpload` helper specify that you want a single box for the file to upload and that you want the submit button to read **Upload**.</span></span> <span data-ttu-id="31e38-247">(ボックスの詳細については、記事の後半で追加します)。</span><span class="sxs-lookup"><span data-stu-id="31e38-247">(You'll add more boxes later in the article.)</span></span>

    <span data-ttu-id="31e38-248">ユーザーがクリックすると**アップロード**ページの上部にあるコード ファイルを取得および保存します。</span><span class="sxs-lookup"><span data-stu-id="31e38-248">When the user clicks **Upload**, the code at the top of the page gets the file and saves it.</span></span> <span data-ttu-id="31e38-249">`Request`フォームのフィールドから値を取得するために通常使用オブジェクトがあります、`Files`ファイル (またはファイル) を含む配列がアップロードされています。</span><span class="sxs-lookup"><span data-stu-id="31e38-249">The `Request` object that you normally use to get values from form fields also has a `Files` array that contains the file (or files) that have been uploaded.</span></span> <span data-ttu-id="31e38-250">配列内の特定の位置から、個々 のファイルを取得できます&#8212;取得する最初のアップロードされたファイルを取得するには、たとえば、 `Request.Files[0]`2 番目のファイルを取得するには、取得する`Request.Files[1]`など。</span><span class="sxs-lookup"><span data-stu-id="31e38-250">You can get individual files out of specific positions in the array &#8212; for example, to get the first uploaded file, you get `Request.Files[0]`, to get the second file, you get `Request.Files[1]`, and so on.</span></span> <span data-ttu-id="31e38-251">(プログラミングでは、通常はカウントが開始されるゼロに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="31e38-251">(Remember that in programming, counting usually starts at zero.)</span></span>

    <span data-ttu-id="31e38-252">変数に格納するアップロードされたファイルをフェッチするとき (ここでは、 `uploadedFile`) それを操作することです。</span><span class="sxs-lookup"><span data-stu-id="31e38-252">When you fetch an uploaded file, you put it in a variable (here, `uploadedFile`) so that you can manipulate it.</span></span> <span data-ttu-id="31e38-253">取得するだけのアップロードされたファイルの名前を判断するには、その`FileName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="31e38-253">To determine the name of the uploaded file, you just get its `FileName` property.</span></span> <span data-ttu-id="31e38-254">ただし、ユーザーが、ファイルをアップロードすると`FileName`パス全体を含む、ユーザーの元名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="31e38-254">However, when the user uploads a file, `FileName` contains the user's original name, which includes the entire path.</span></span> <span data-ttu-id="31e38-255">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="31e38-255">It might look like this:</span></span>

    <span data-ttu-id="31e38-256">*C:\Users\Public\Sample.txt*</span><span class="sxs-lookup"><span data-stu-id="31e38-256">*C:\Users\Public\Sample.txt*</span></span>

    <span data-ttu-id="31e38-257">ただし、サーバーではなく、ユーザーのコンピューター上のパスであるためです、すべてのパス情報は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="31e38-257">You don't want all that path information, though, because that's the path on the user's computer, not for your server.</span></span> <span data-ttu-id="31e38-258">実際のファイル名 (*Sample.txt*)。</span><span class="sxs-lookup"><span data-stu-id="31e38-258">You just want the actual file name (*Sample.txt*).</span></span> <span data-ttu-id="31e38-259">パスからのファイルだけを除去するを使用して、`Path.GetFileName`メソッドは、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="31e38-259">You can strip out just the file from a path by using the `Path.GetFileName` method, like this:</span></span>

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    <span data-ttu-id="31e38-260">`Path`オブジェクトは、ユーティリティがいくつかのパスを削除、パスを結合およびなどに使用できる次のようなメソッドです。</span><span class="sxs-lookup"><span data-stu-id="31e38-260">The `Path` object is a utility that has a number of methods like this that you can use to strip paths, combine paths, and so on.</span></span>

    <span data-ttu-id="31e38-261">アップロードされたファイルの名前を取得した後は、web サイトにアップロードされたファイルを格納する新しいパスを構築できます。</span><span class="sxs-lookup"><span data-stu-id="31e38-261">Once you've gotten the name of the uploaded file, you can build a new path for where you want to store the uploaded file in your website.</span></span> <span data-ttu-id="31e38-262">この場合、結合する`Server.MapPath`、フォルダー名 (*アプリ\_データ/UploadedFiles*)、および新しいパスを作成する新しく削除されたファイル名。</span><span class="sxs-lookup"><span data-stu-id="31e38-262">In this case, you combine `Server.MapPath`, the folder names (*App\_Data/UploadedFiles*), and the newly stripped file name to create a new path.</span></span> <span data-ttu-id="31e38-263">呼び出して、アップロードされたファイルの`SaveAs`メソッドを実際には、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="31e38-263">You can then call the uploaded file's `SaveAs` method to actually save the file.</span></span>
5. <span data-ttu-id="31e38-264">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-264">Run the page in a browser.</span></span> 

    ![[イメージ]](working-with-files/_static/image7.jpg)
6. <span data-ttu-id="31e38-266">クリックして**参照**しアップロードするファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="31e38-266">Click **Browse** and then select a file to upload.</span></span> 

    ![[イメージ]](working-with-files/_static/image8.jpg)

    <span data-ttu-id="31e38-268">横にあるテキスト ボックス、**参照**ボタンは、パスとファイルの場所が含まれます。</span><span class="sxs-lookup"><span data-stu-id="31e38-268">The text box next to the **Browse** button will contain the path and file location.</span></span>

    ![[イメージ]](working-with-files/_static/image9.jpg)
7. <span data-ttu-id="31e38-270">**[アップロード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e38-270">Click **Upload**.</span></span>
8. <span data-ttu-id="31e38-271">Web サイトで、プロジェクト フォルダーを右クリックしをクリックして**更新**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-271">In the website, right-click the project folder and then click **Refresh**.</span></span>
9. <span data-ttu-id="31e38-272">開く、 *UploadedFiles*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="31e38-272">Open the *UploadedFiles* folder.</span></span> <span data-ttu-id="31e38-273">アップロードしたファイルは、フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="31e38-273">The file that you uploaded is in the folder.</span></span> 

    ![[イメージ]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a><span data-ttu-id="31e38-275">通知ユーザーは、複数のファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="31e38-275">Letting Users Upload Multiple Files</span></span>

<span data-ttu-id="31e38-276">前の例では、ユーザーが 1 つのファイルのアップロードができるようにします。</span><span class="sxs-lookup"><span data-stu-id="31e38-276">In the previous example, you let users upload one file.</span></span> <span data-ttu-id="31e38-277">使用することができますが、`FileUpload`一度に 1 つ以上のファイルをアップロードするヘルパー。</span><span class="sxs-lookup"><span data-stu-id="31e38-277">But you can use the `FileUpload` helper to upload more than one file at a time.</span></span> <span data-ttu-id="31e38-278">これは、一度に 1 つのファイルのアップロードは面倒な写真をアップロードするようなシナリオに便利です。</span><span class="sxs-lookup"><span data-stu-id="31e38-278">This is handy for scenarios like uploading photos, where uploading one file at a time is tedious.</span></span> <span data-ttu-id="31e38-279">(で写真のアップロードについて[ASP.NET Web ページ サイトでのイメージを操作](https://go.microsoft.com/fwlink/?LinkId=202897))。この例では、アップロードしたものを同じ手法を使用できますが、アップロード時、2 つのユーザーをできるようにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="31e38-279">(You can read about uploading photos in [Working with Images in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202897).) This example shows how to let users upload two at a time, although you can use the same technique to upload more than that.</span></span>

1. <span data-ttu-id="31e38-280">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="31e38-280">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="31e38-281">という名前の新しいページを作成する*FileUploadMultiple.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="31e38-281">Create a new page named *FileUploadMultiple.cshtml*.</span></span>
3. <span data-ttu-id="31e38-282">次のように、ページで、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="31e38-282">Replace the existing content in the page with the following:</span></span>  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    <span data-ttu-id="31e38-283">この例で、`FileUpload`ヘルパー ページの本文には、既定で 2 つのファイルをアップロードしてもらうために構成されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-283">In this example, the `FileUpload` helper in the body of the page is configured to let users upload two files by default.</span></span> <span data-ttu-id="31e38-284">`allowMoreFilesToBeAdded`に設定されている`true`ヘルパーがユーザーのアップロードを追加できるリンクをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="31e38-284">Because `allowMoreFilesToBeAdded` is set to `true`, the helper renders a link that lets user add more upload boxes:</span></span>

    ![[イメージ]](working-with-files/_static/image11.jpg)

    <span data-ttu-id="31e38-286">コードをユーザーにアップロードするファイルを処理するには、前の例で使用したのと同じ基本的なテクニックを使用して&#8212;からファイルを取得`Request.Files`し保存します。</span><span class="sxs-lookup"><span data-stu-id="31e38-286">To process the files that the user uploads, the code uses the same basic technique that you used in the previous example &#8212; get a file from `Request.Files` and then save it.</span></span> <span data-ttu-id="31e38-287">(さまざまな考慮事項を含む必要が適切なファイル名とパスを取得するため)。イノベーションこの時間は、ユーザーが複数のファイルをアップロードする場合があります多くが不明です。</span><span class="sxs-lookup"><span data-stu-id="31e38-287">(Including the various things you need to do to get the right file name and path.) The innovation this time is that the user might be uploading multiple files and you don't know many.</span></span> <span data-ttu-id="31e38-288">取得することができますを確認する、`Request.Files.Count`します。</span><span class="sxs-lookup"><span data-stu-id="31e38-288">To find out, you can get `Request.Files.Count`.</span></span>

    <span data-ttu-id="31e38-289">この番号を入手したら、使用してをループすることができます`Request.Files`、さらに、各ファイルのフェッチおよび保存します。</span><span class="sxs-lookup"><span data-stu-id="31e38-289">With this number in hand, you can loop through `Request.Files`, fetch each file in turn, and save it.</span></span> <span data-ttu-id="31e38-290">コレクションを既知回数をループするときに使用できます、`for`ループは、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="31e38-290">When you want to loop a known number of times through a collection, you can use a `for` loop, like this:</span></span>

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    <span data-ttu-id="31e38-291">変数`i`はどのような上限を設定するには 0 から、ある一時カウンターだけです。</span><span class="sxs-lookup"><span data-stu-id="31e38-291">The variable `i` is just a temporary counter that will go from zero to whatever upper limit you set.</span></span> <span data-ttu-id="31e38-292">この場合、上限値には、ファイル数です。</span><span class="sxs-lookup"><span data-stu-id="31e38-292">In this case, the upper limit is the number of files.</span></span> <span data-ttu-id="31e38-293">1 つ未満のファイルの数に上限を実際には、カウンターは、0 の場合は、ASP.NET のシナリオのカウントの一般的なから開始するためです。</span><span class="sxs-lookup"><span data-stu-id="31e38-293">But because the counter starts at zero, as is typical for counting scenarios in ASP.NET, the upper limit is actually one less than the file count.</span></span> <span data-ttu-id="31e38-294">(3 つのファイルがアップロードされている場合、カウントがゼロを 2 にします。)</span><span class="sxs-lookup"><span data-stu-id="31e38-294">(If three files are uploaded, the count is zero to 2.)</span></span>

    <span data-ttu-id="31e38-295">`uploadedCount`変数が正常にアップロードおよび保存されるすべてのファイルを合計します。</span><span class="sxs-lookup"><span data-stu-id="31e38-295">The `uploadedCount` variable totals all the files that are successfully uploaded and saved.</span></span> <span data-ttu-id="31e38-296">このコードは、必要なファイルをアップロードすることができない可能性がある可能性をアカウントします。</span><span class="sxs-lookup"><span data-stu-id="31e38-296">This code accounts for the possibility that an expected file may not be able to be uploaded.</span></span>
4. <span data-ttu-id="31e38-297">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="31e38-297">Run the page in a browser.</span></span> <span data-ttu-id="31e38-298">ブラウザーでは、ページと 2 つのアップロード ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-298">The browser displays the page and its two upload boxes.</span></span>
5. <span data-ttu-id="31e38-299">アップロードする 2 つのファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="31e38-299">Select two files to upload.</span></span>
6. <span data-ttu-id="31e38-300">クリックして**別のファイルを追加**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-300">Click **Add another file**.</span></span> <span data-ttu-id="31e38-301">ページには、新しいアップロードのボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="31e38-301">The page displays a new upload box.</span></span> 

    ![[イメージ]](working-with-files/_static/image12.jpg)
7. <span data-ttu-id="31e38-303">**[アップロード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e38-303">Click **Upload**.</span></span>
8. <span data-ttu-id="31e38-304">Web サイトで、プロジェクト フォルダーを右クリックしをクリックして**更新**します。</span><span class="sxs-lookup"><span data-stu-id="31e38-304">In the website, right-click the project folder and then click **Refresh**.</span></span>
9. <span data-ttu-id="31e38-305">開く、 *UploadedFiles*フォルダーに正常にアップロードしたファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="31e38-305">Open the *UploadedFiles* folder to see the successfully uploaded files.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="31e38-306">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="31e38-306">Additional Resources</span></span>


[<span data-ttu-id="31e38-307">ASP.NET Web ページ サイトでの画像の操作</span><span class="sxs-lookup"><span data-stu-id="31e38-307">Working with Images in an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkId=202897)

[<span data-ttu-id="31e38-308">CSV ファイルにエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="31e38-308">Exporting to a CSV File</span></span>](https://msdn.microsoft.com/library/ms155919.aspx)
