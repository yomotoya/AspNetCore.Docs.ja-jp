---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "ASP.NET Web API で HTML フォームのデータを送信する: ファイルのアップロードとマルチパート MIME |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3df59aab2a0c43f4a4f5c59530b0655f68d95cc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="cb506-102">ASP.NET Web API で HTML フォームのデータを送信する: ファイルのアップロードとマルチパート MIME</span><span class="sxs-lookup"><span data-stu-id="cb506-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="cb506-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cb506-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="cb506-104">パート 2: ファイルのアップロードとマルチパート MIME</span><span class="sxs-lookup"><span data-stu-id="cb506-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="cb506-105">このチュートリアルでは、web API にファイルをアップロードする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cb506-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="cb506-106">マルチパート MIME データを処理する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="cb506-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="cb506-107">[完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)です。</span><span class="sxs-lookup"><span data-stu-id="cb506-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="cb506-108">ファイルをアップロードするための HTML フォームの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="cb506-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="cb506-109">このフォームには、テキスト入力コントロールとファイルの入力コントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb506-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="cb506-110">フォームには、ファイルの入力コントロールが含まれている場合、 **enctype**属性は常に&quot;マルチパート フォーム データ&quot;フォームをマルチパート MIME メッセージとして送信されることを指定します。</span><span class="sxs-lookup"><span data-stu-id="cb506-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="cb506-111">マルチパート MIME メッセージの形式は、要求の例を調べることで理解する最も簡単なです。</span><span class="sxs-lookup"><span data-stu-id="cb506-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="cb506-112">このメッセージは 2 つに分かれています*パーツ*、フォーム コントロールごとに 1 つです。</span><span class="sxs-lookup"><span data-stu-id="cb506-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="cb506-113">ダッシュで始まる行では、部品の境界が示されます。</span><span class="sxs-lookup"><span data-stu-id="cb506-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="cb506-114">一部の境界には、ランダムなコンポーネントが含まれています (&quot;41184676334&quot;) を境界文字列が表示されないこと誤ってメッセージ部分の内部を確認します。</span><span class="sxs-lookup"><span data-stu-id="cb506-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="cb506-115">各メッセージ部分には、パーツのコンテンツを続けて、1 つまたは複数のヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb506-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="cb506-116">Content-disposition ヘッダーには、コントロールの名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb506-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="cb506-117">ファイル、そのファイル名も含まれます。</span><span class="sxs-lookup"><span data-stu-id="cb506-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="cb506-118">Content-type ヘッダーには、一部のデータについて説明します。</span><span class="sxs-lookup"><span data-stu-id="cb506-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="cb506-119">このヘッダーを省略すると、既定値はテキスト/プレーンにします。</span><span class="sxs-lookup"><span data-stu-id="cb506-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="cb506-120">前の例では、ユーザーがコンテンツの種類の画像/jpeg 形式; GrandCanyon.jpg をという名前のファイルをアップロードテキスト入力の値と&quot;夏休み&quot;です。</span><span class="sxs-lookup"><span data-stu-id="cb506-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="cb506-121">ファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="cb506-121">File Upload</span></span>

<span data-ttu-id="cb506-122">これでマルチパート MIME メッセージからファイルを読み取り、Web API コント ローラーを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="cb506-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="cb506-123">コント ローラーが非同期的に、ファイルを読み取る。</span><span class="sxs-lookup"><span data-stu-id="cb506-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="cb506-124">Web API を使用して非同期のアクションをサポートしている、[タスク ベース プログラミング モデル](https://msdn.microsoft.com/library/dd460693.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="cb506-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="cb506-125">最初に、次のコードは、サポートする .NET Framework 4.5 を対象としている場合、 **async**と**await**キーワード。</span><span class="sxs-lookup"><span data-stu-id="cb506-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="cb506-126">コント ローラーのアクションが、すべてのパラメーターを受け取らないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cb506-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="cb506-127">現在、メディア タイプ フォーマッタを呼び出すことがなく、アクション内の要求本文を処理するためです。</span><span class="sxs-lookup"><span data-stu-id="cb506-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="cb506-128">**IsMultipartContent**メソッドでは、マルチパート MIME メッセージが要求に含まれているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="cb506-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="cb506-129">それ以外の場合は、コント ローラーは、HTTP 状態コード 415 (サポートされていないメディアの種類) を返します。</span><span class="sxs-lookup"><span data-stu-id="cb506-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="cb506-130">**MultipartFormDataStreamProvider**クラスは、アップロードされたファイルのファイル ストリームの割り当てを行うためのヘルパー オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="cb506-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="cb506-131">マルチパート MIME メッセージを読み取りを呼び出して、 **ReadAsMultipartAsync**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="cb506-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="cb506-132">このメソッドは、すべてのメッセージ部分を抽出し、によって提供されるストリームに書き込みます、 **MultipartFormDataStreamProvider**です。</span><span class="sxs-lookup"><span data-stu-id="cb506-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="cb506-133">メソッドが完了したら、ファイルからの情報を取得できます、 **FileData**プロパティは、これは、コレクションの**MultipartFileData**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="cb506-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="cb506-134">**MultipartFileData.FileName**ファイルの保存場所、サーバーでローカル ファイル名を指定します。</span><span class="sxs-lookup"><span data-stu-id="cb506-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="cb506-135">**MultipartFileData.Headers**パーツのヘッダーが含まれています (*いない*要求ヘッダー)。</span><span class="sxs-lookup"><span data-stu-id="cb506-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="cb506-136">これを使用するには、コンテンツにアクセスする\_廃棄、Content-type ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="cb506-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="cb506-137">名前からわかるように、 **ReadAsMultipartAsync**非同期メソッドです。</span><span class="sxs-lookup"><span data-stu-id="cb506-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="cb506-138">メソッドが終了した後で作業を実行する、[継続タスク](https://msdn.microsoft.com/en-us/library/ee372288.aspx)(.NET 4.0 の場合) または**await**キーワード (.NET 4.5)。</span><span class="sxs-lookup"><span data-stu-id="cb506-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="cb506-139">.NET Framework 4.0 バージョンの前のコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="cb506-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="cb506-140">フォーム コントロールのデータの読み取り</span><span class="sxs-lookup"><span data-stu-id="cb506-140">Reading Form Control Data</span></span>

<span data-ttu-id="cb506-141">前に示した HTML フォームには、テキスト入力コントロールが必要があります。</span><span class="sxs-lookup"><span data-stu-id="cb506-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="cb506-142">コントロールの値を取得することができます、 **FormData**のプロパティ、 **MultipartFormDataStreamProvider**です。</span><span class="sxs-lookup"><span data-stu-id="cb506-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="cb506-143">**FormData**は、 **NameValueCollection**フォーム コントロールの名前/値ペアを格納します。</span><span class="sxs-lookup"><span data-stu-id="cb506-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="cb506-144">コレクションには、重複するキーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="cb506-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="cb506-145">このフォームを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="cb506-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="cb506-146">要求本文は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cb506-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="cb506-147">その場合は、 **FormData**コレクションには、次のキー/値ペアにが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cb506-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="cb506-148">トリップ: ラウンドト リップ</span><span class="sxs-lookup"><span data-stu-id="cb506-148">trip: round-trip</span></span>
- <span data-ttu-id="cb506-149">オプション: 無着陸</span><span class="sxs-lookup"><span data-stu-id="cb506-149">options: nonstop</span></span>
- <span data-ttu-id="cb506-150">オプション: 日付</span><span class="sxs-lookup"><span data-stu-id="cb506-150">options: dates</span></span>
- <span data-ttu-id="cb506-151">接続クライアント数: ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="cb506-151">seat: window</span></span>
