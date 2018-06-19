---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: HTML エディター コントロールの使用方法 (VB) |Microsoft ドキュメント
author: microsoft
description: HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873301"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a><span data-ttu-id="17fbb-104">HTML エディター コントロールの使用方法</span><span class="sxs-lookup"><span data-stu-id="17fbb-104">How do I use the HTML Editor Control?</span></span> <span data-ttu-id="17fbb-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="17fbb-105">(VB)</span></span>
====================
<span data-ttu-id="17fbb-106">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="17fbb-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="17fbb-107">HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-107">HTMLEditor is an ASP.NET AJAX Control that allows you to easily create and edit HTML content via buttons in a toolbar.</span></span>


<span data-ttu-id="17fbb-108">このチュートリアルの目的は、AJAX コントロールのツールキットに含まれている HTML エディター コントロールの概要が提供するためです。</span><span class="sxs-lookup"><span data-stu-id="17fbb-108">The goal of this tutorial is to provide you with an overview of the HTML Editor control included with the AJAX Control Toolkit.</span></span> <span data-ttu-id="17fbb-109">HTML エディターには、イメージを追加するテキストの配置を変更する、リンクを追加するフォント サイズを変更する、フォントを選択すると、背景色を変更する、前景の色を変更するためのオプションが含まれています、切り取りを実行するには、コピー、貼り付けの操作 (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="17fbb-109">The HTML Editor includes options for changing font size, selecting a font, changing background color, modifying the foreground color, adding links, adding images, changing text alignment, and performing cut, copy, and paste operations (see Figure 1).</span></span>


<span data-ttu-id="17fbb-110">[![HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17fbb-110">[![The HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="17fbb-111">**図 01**:、HTML エディター ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="17fbb-111">**Figure 01**: The HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="17fbb-112">HTML エディターでは、デザイン モードを使用してコンテンツを入力できます。 または、HTML を直接入力することができます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-112">The HTML editor enables you to enter content using a design mode or you can enter HTML directly.</span></span> <span data-ttu-id="17fbb-113">HTML コンテンツをプレビューするためのオプションも提供されます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="17fbb-113">You also are provided with the option to preview your HTML content (see Figure 2).</span></span>


<span data-ttu-id="17fbb-114">[![デザイン、HTML、およびプレビュー ボタン](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="17fbb-114">[![Design, HTML, and Preview buttons](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="17fbb-115">**図 02**: ボタンのデザイン、HTML、およびプレビュー ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="17fbb-115">**Figure 02**: Design, HTML, and Preview buttons([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="17fbb-116">このチュートリアルでは、HTML エディターを表示する方法、HTML エディターで、表示されるツールバーのボタンをカスタマイズする方法、およびクロス サイト スクリプト攻撃を回避する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-116">In this tutorial, you learn how to display the HTML Editor, how to customize the toolbar buttons that appear in the HTML Editor, and how to avoid Cross-Site Scripting Attacks.</span></span>

## <a name="displaying-the-html-editor"></a><span data-ttu-id="17fbb-117">HTML エディターを表示します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-117">Displaying the HTML Editor</span></span>

<span data-ttu-id="17fbb-118">HTML エディターを使用するには、ASP.NET ページで、前に、ページに ScriptManager コントロールを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-118">Before you can use the HTML Editor in an ASP.NET page, you must first add a ScriptManager control to the page.</span></span> <span data-ttu-id="17fbb-119">ScriptManager コントロールは、Visual Studio または Visual Web Developer Express ツールボックスの [AJAX Extensions] タブの下にあります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-119">The ScriptManager control is located beneath the AJAX Extensions tab in the Visual Studio/Visual Web Developer Express toolbox.</span></span>

<span data-ttu-id="17fbb-120">ページの他のコントロールの前にページの上部にある ScriptManager コントロールを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-120">You should place the ScriptManager control at the top of the page before any other controls on the page.</span></span> <span data-ttu-id="17fbb-121">などに配置してすぐに始めサーバー側の下&lt;フォーム&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="17fbb-121">For example, you can place it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="17fbb-122">HTML エディター コントロールは、他の AJAX コントロール Toolkit コントロールのツールボックスに配置されます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-122">The HTML Editor control is located in the toolbox with the rest of the AJAX Control Toolkit controls.</span></span> <span data-ttu-id="17fbb-123">エディター コントロールという名前です (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="17fbb-123">It is named the Editor control (see Figure 3).</span></span>


<span data-ttu-id="17fbb-124">[![HTML エディター コントロール](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="17fbb-124">[![The HTML Editor control](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="17fbb-125">**図 03**:、HTML エディター コントロール ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="17fbb-125">**Figure 03**: The HTML Editor control([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="17fbb-126">HTML エディターをページにドラッグした後は、プロパティ シートでそのプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-126">After you drag the HTML Editor onto a page, you can set its properties in the property sheet.</span></span> <span data-ttu-id="17fbb-127">たとえば、通常する幅と高さのプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-127">For example, you normally want to set the Width and Height properties.</span></span> <span data-ttu-id="17fbb-128">1 を一覧表示するには、HTML エディターを含む ASP.NET ページのソースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-128">Listing 1 contains the source for an ASP.NET page that contains an HTML editor.</span></span>

<span data-ttu-id="17fbb-129">**1 - SimpleEditor.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="17fbb-129">**Listing 1 - SimpleEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

<span data-ttu-id="17fbb-130">1 のリスト内のページには、HTML エディター コントロール、ボタン コントロール、およびリテラルのコントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="17fbb-130">The page in Listing 1 contains an HTML Editor control, a Button control, and a Literal control.</span></span> <span data-ttu-id="17fbb-131">Literal コントロールに HTML エディターの内容が表示されるボタンをクリックすると (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="17fbb-131">When you click the button, the contents of the HTML Editor appear in the Literal control (see Figure 4).</span></span>


<span data-ttu-id="17fbb-132">[![HTML エディターでフォームを送信します。](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17fbb-132">[![Submitting a form with an HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="17fbb-133">**図 04**: HTML エディターでフォームを送信する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="17fbb-133">**Figure 04**: Submitting a form with an HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="17fbb-134">HTML エディターの Content プロパティを使用して、HTML エディターに入力された HTML コンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-134">The HTML Editor Content property is used to retrieve the HTML content entered into the HTML Editor.</span></span> <span data-ttu-id="17fbb-135">この HTML コンテンツが JavaScript を含めることができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="17fbb-135">Be aware that this HTML content can contain JavaScript.</span></span> <span data-ttu-id="17fbb-136">次のセクションでは、JavaScript インジェクション攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-136">In the next section, we discuss how you can prevent JavaScript Injection Attacks.</span></span>

## <a name="customizing-the-html-editor-toolbar"></a><span data-ttu-id="17fbb-137">HTML エディター ツールバーをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="17fbb-137">Customizing the HTML Editor Toolbar</span></span>

<span data-ttu-id="17fbb-138">正確にボタンをカスタマイズすることができます、エディターに表示されます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-138">You can customize exactly which buttons appear in the editor.</span></span> <span data-ttu-id="17fbb-139">たとえば、ユーザーが HTML モードに HTML エディターを切り替えることを防止する [HTML] タブを削除する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-139">For example, you might want to remove the HTML tab to prevent users from switching the HTML Editor into HTML mode.</span></span> <span data-ttu-id="17fbb-140">または、ユーザーがフォーラムに過度に大きなテキストを作成することを防止する、フォント サイズのドロップダウン リストを削除する場合があります (図 5 を参照してください) を post メッセージします。</span><span class="sxs-lookup"><span data-stu-id="17fbb-140">Or, you might want to remove the font size dropdown list to prevent users from creating overly large text in a forum message post (see Figure 5).</span></span>


<span data-ttu-id="17fbb-141">[![カスタマイズされた HTML エディター](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="17fbb-141">[![A customized HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="17fbb-142">**図 05**: A HTML エディターのカスタマイズ ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-html-editor-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="17fbb-142">**Figure 05**: A customized HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="17fbb-143">ツールバーのボタンをカスタマイズするには、新しい HTML エディターのエディターの基本クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-143">You customize the toolbar buttons by deriving a new HTML Editor from the base Editor class.</span></span> <span data-ttu-id="17fbb-144">たとえば、カスタム エディター一覧表示する 2 にはでは、太字や斜体のツール バー ボタンにはのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-144">For example, the custom editor in Listing 2 only contains toolbar buttons for bold and italic.</span></span> <span data-ttu-id="17fbb-145">その他のすべてのツール バー ボタンが削除されました。</span><span class="sxs-lookup"><span data-stu-id="17fbb-145">All other toolbar buttons have been removed.</span></span> <span data-ttu-id="17fbb-146">さらに、[HTML] タブは、エディターの下部から削除されました (ただし、デザインおよびプレビュー タブは引き続き行わ)。</span><span class="sxs-lookup"><span data-stu-id="17fbb-146">Furthermore, the HTML tab has been removed from the bottom of the editor (but the Design and Preview tabs are still there).</span></span>

<span data-ttu-id="17fbb-147">**2 - アプリを一覧表示する\_Code\CustomEditor.vb**</span><span class="sxs-lookup"><span data-stu-id="17fbb-147">**Listing 2 - App\_Code\CustomEditor.vb**</span></span>

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

<span data-ttu-id="17fbb-148">アプリを一覧表示する 2 クラスに追加する必要があります\_コード フォルダーのクラスを自動的にコンパイルできるようにします。</span><span class="sxs-lookup"><span data-stu-id="17fbb-148">You must add the class in Listing 2 to your App\_Code folder so that the class will be compiled automatically.</span></span> <span data-ttu-id="17fbb-149">場合、アプリ\_コード フォルダーは、web サイトに存在しませんし、単に、フォルダーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-149">If the App\_Code folder does not exist in your website then you can simply add the folder.</span></span>

<span data-ttu-id="17fbb-150">カスタム エディターを作成した後することができますに追加 ASP.NET ページと同様に、標準の HTML エディター (3 のリストを参照) を追加するとします。</span><span class="sxs-lookup"><span data-stu-id="17fbb-150">After you create a custom editor, you can add it to an ASP.NET page in the same way as you add the normal HTML Editor (see Listing 3).</span></span>

<span data-ttu-id="17fbb-151">**3 - ShowCustomEditor.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="17fbb-151">**Listing 3 - ShowCustomEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a><span data-ttu-id="17fbb-152">クロス サイト スクリプト (XSS) 攻撃の回避</span><span class="sxs-lookup"><span data-stu-id="17fbb-152">Avoiding Cross-Site Scripting (XSS) Attacks</span></span>

<span data-ttu-id="17fbb-153">ユーザーからの入力を受け付ける、web サイトでその入力を再表示して、ときに可能性のあるクロスサイト スクリプト (XSS) 攻撃に web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-153">Whenever you accept input from a user, and redisplay that input on your website, you potentially open your website to Cross-Site Scripting (XSS) Attacks.</span></span> <span data-ttu-id="17fbb-154">理論上は、悪意のあるハッカーは、入力が再表示されるときに実行される JavaScript コードを送信でした。</span><span class="sxs-lookup"><span data-stu-id="17fbb-154">In theory, a malicious hacker could submit JavaScript code that gets executed when the input is redisplayed.</span></span> <span data-ttu-id="17fbb-155">JavaScript は、ユーザーのパスワードや他の機密情報を盗むために使用できます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-155">The JavaScript could be used to steal user passwords or other sensitive information.</span></span>

<span data-ttu-id="17fbb-156">通常は、HTML web ページに表示する前に、ユーザーから取得する任意の入力をエンコーディング XSS 攻撃が損なわためことができます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-156">Normally, you can defeat XSS attacks by HTML encoding whatever input you retrieve from a user before displaying it in a web page.</span></span> <span data-ttu-id="17fbb-157">ただし、HTML、HTML エディターの出力のエンコードはのみエンコードされない&lt;スクリプト&gt;タグも、すべての HTML タグをエンコードことができます。</span><span class="sxs-lookup"><span data-stu-id="17fbb-157">However, HTML encoding the output of the HTML Editor would not only encode &lt;script&gt; tags, it would also encode all HTML tags.</span></span> <span data-ttu-id="17fbb-158">つまり、すべての種類のフォント、フォント サイズ、および背景色などの書式設定が失われるとします。</span><span class="sxs-lookup"><span data-stu-id="17fbb-158">In other words, you would lose all of the formatting such as the font type, font size, and background color.</span></span>

<span data-ttu-id="17fbb-159">パスワード、クレジット_カード番号や社会保障番号 - などのユーザーの機密情報を収集している場合は、HTML エディターを使用して、ユーザーから取得されたエンコードされていないコンテンツをいない表示しています。</span><span class="sxs-lookup"><span data-stu-id="17fbb-159">If you are collecting sensitive information from your users -- such as passwords, credit-card numbers, and social security numbers - then you should not display un-encoded content that you retrieve from a user with the HTML Editor.</span></span> <span data-ttu-id="17fbb-160">HTML コンテンツを再表示されませんまたは HTML コンテンツが送信される場合にのみ、HTML エディターを使用すると、信頼されたパーティから web サイトにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-160">You should use the HTML Editor only in situations in which you are not redisplaying the HTML content, or the HTML content is being submitted to your website by a trusted party.</span></span>

<span data-ttu-id="17fbb-161">たとえば、ブログ アプリケーションを作成している想像してみてください。</span><span class="sxs-lookup"><span data-stu-id="17fbb-161">Imagine, for example, that you are creating a blog application.</span></span> <span data-ttu-id="17fbb-162">このような状況は、ブログの投稿を作成するときに、HTML エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-162">In this situation, it makes sense to use the HTML Editor when composing blog posts.</span></span> <span data-ttu-id="17fbb-163">ブログの投稿を送信した 1 つだけと、悪意のある JavaScript の送信が自分自身を信頼する多くの場合、します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-163">You are the only one who submits a blog post and, presumably, you can trust yourself not to submit malicious JavaScript.</span></span> <span data-ttu-id="17fbb-164">ただし、コメントを投稿する匿名ユーザーを許可する場合に、HTML エディターを使用する意味は行いません。</span><span class="sxs-lookup"><span data-stu-id="17fbb-164">However, it does not make sense to use the HTML Editor when allowing anonymous users to post comments.</span></span> <span data-ttu-id="17fbb-165">ユーザーがパスワードなどの機密情報を送信する場合に特に注意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-165">You should be especially careful in situations in which users submit sensitive information such as passwords.</span></span> <span data-ttu-id="17fbb-166">可能性のある、悪意のあるユーザーはパスワードを盗むの右側の JavaScript を含むコメントを投稿する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="17fbb-166">Potentially, a malicious user could post a comment that contains the right JavaScript for stealing a password.</span></span>

## <a name="summary"></a><span data-ttu-id="17fbb-167">まとめ</span><span class="sxs-lookup"><span data-stu-id="17fbb-167">Summary</span></span>

<span data-ttu-id="17fbb-168">このチュートリアルでは、AJAX コントロールのツールキットに含まれている HTML エディター コントロールの概要で提供されました。</span><span class="sxs-lookup"><span data-stu-id="17fbb-168">In this tutorial, you were provided with a brief overview of the HTML Editor control included in the AJAX Control Toolkit.</span></span> <span data-ttu-id="17fbb-169">HTML エディターを使用して、ユーザーからの豊富なコンテンツを受け取るし、サーバーにコンテンツを送信する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="17fbb-169">You learned how to use the HTML Editor to accept rich content from a user and submit the content to the server.</span></span> <span data-ttu-id="17fbb-170">HTML エディターによって表示されるツールバーのボタンをカスタマイズする方法についても説明しました。</span><span class="sxs-lookup"><span data-stu-id="17fbb-170">We also discussed how you can customize the toolbar buttons that are displayed by the HTML Editor.</span></span> <span data-ttu-id="17fbb-171">最後に、悪意のある入力を受け付ける HTML エディターを使用する場合、クロスサイト スクリプティング攻撃を回避する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="17fbb-171">Finally, you learned how to avoid Cross-Site Scripting Attacks when using the HTML Editor to accept potentially malicious input.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="17fbb-172">前へ</span><span class="sxs-lookup"><span data-stu-id="17fbb-172">Previous</span></span>](how-do-i-use-the-html-editor-control-cs.md)
