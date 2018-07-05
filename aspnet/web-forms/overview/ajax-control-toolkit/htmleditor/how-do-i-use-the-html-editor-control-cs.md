---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: HTML エディター コントロールの使用方法 (C#) |Microsoft Docs
author: microsoft
description: HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ef8fba82a77ed570c800dce0b1c1378fd36ea56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397627"
---
<a name="how-do-i-use-the-html-editor-control-c"></a><span data-ttu-id="1d64e-104">HTML エディター コントロールの使用方法</span><span class="sxs-lookup"><span data-stu-id="1d64e-104">How do I use the HTML Editor Control?</span></span> <span data-ttu-id="1d64e-105">(C#)</span><span class="sxs-lookup"><span data-stu-id="1d64e-105">(C#)</span></span>
====================
<span data-ttu-id="1d64e-106">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1d64e-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1d64e-107">HTMLEditor は、ASP.NET AJAX コントロールを簡単に作成し、ツールバーのボタンを使用して HTML コンテンツを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-107">HTMLEditor is an ASP.NET AJAX Control that allows you to easily create and edit HTML content via buttons in a toolbar.</span></span>


<span data-ttu-id="1d64e-108">このチュートリアルの目的は、AJAX Control Toolkit に含まれている HTML エディター コントロールの概要を提供するためにです。</span><span class="sxs-lookup"><span data-stu-id="1d64e-108">The goal of this tutorial is to provide you with an overview of the HTML Editor control included with the AJAX Control Toolkit.</span></span> <span data-ttu-id="1d64e-109">HTML エディターには、リンクを追加するには、テキストの配置を変更するイメージの追加、フォント サイズを変更する、フォントを選択すると、背景色を変更する、前景色の色を変更するためのオプションと切り取りを実行するには、コピー、および貼り付けの操作 (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="1d64e-109">The HTML Editor includes options for changing font size, selecting a font, changing background color, modifying the foreground color, adding links, adding images, changing text alignment, and performing cut, copy, and paste operations (see Figure 1).</span></span>


<span data-ttu-id="1d64e-110">[![HTML エディター](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1d64e-110">[![The HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="1d64e-111">**図 01**:、HTML エディター ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="1d64e-111">**Figure 01**: The HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image2.png))</span></span>


<span data-ttu-id="1d64e-112">HTML エディターでは、デザイン モードを使用してコンテンツを入力できます。 または、HTML を直接入力することができます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-112">The HTML editor enables you to enter content using a design mode or you can enter HTML directly.</span></span> <span data-ttu-id="1d64e-113">HTML コンテンツをプレビューするオプションも提供されます (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="1d64e-113">You also are provided with the option to preview your HTML content (see Figure 2).</span></span>


<span data-ttu-id="1d64e-114">[![デザイン、HTML、およびプレビュー ボタン](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1d64e-114">[![Design, HTML, and Preview buttons](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)</span></span>

<span data-ttu-id="1d64e-115">**図 02**: デザイン、HTML、およびプレビュー ボタン ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-cs/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="1d64e-115">**Figure 02**: Design, HTML, and Preview buttons([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image4.png))</span></span>


<span data-ttu-id="1d64e-116">このチュートリアルでは、HTML エディターを表示する方法、HTML エディターで表示されるツールバーのボタンをカスタマイズする方法、およびクロス サイト スクリプティング攻撃を回避する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-116">In this tutorial, you learn how to display the HTML Editor, how to customize the toolbar buttons that appear in the HTML Editor, and how to avoid Cross-Site Scripting Attacks.</span></span>

## <a name="displaying-the-html-editor"></a><span data-ttu-id="1d64e-117">HTML エディターを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-117">Displaying the HTML Editor</span></span>

<span data-ttu-id="1d64e-118">HTML エディターを使用するには、ASP.NET ページで、前に、ページに ScriptManager コントロールを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-118">Before you can use the HTML Editor in an ASP.NET page, you must first add a ScriptManager control to the page.</span></span> <span data-ttu-id="1d64e-119">ScriptManager コントロールは、Visual Studio または Visual Web Developer Express ツールボックスの [AJAX Extensions] タブの下に配置します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-119">The ScriptManager control is located beneath the AJAX Extensions tab in the Visual Studio/Visual Web Developer Express toolbox.</span></span>

<span data-ttu-id="1d64e-120">ページ上の他のコントロールの前にページの上部にある、ScriptManager コントロールを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-120">You should place the ScriptManager control at the top of the page before any other controls on the page.</span></span> <span data-ttu-id="1d64e-121">たとえば、することができます下に配置すぐに開始サーバー側&lt;フォーム&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="1d64e-121">For example, you can place it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="1d64e-122">AJAX Control Toolkit のコントロールの残りの部分を使用して、ツールボックスには、HTML エディター コントロールがあります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-122">The HTML Editor control is located in the toolbox with the rest of the AJAX Control Toolkit controls.</span></span> <span data-ttu-id="1d64e-123">エディター コントロールという名前です (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1d64e-123">It is named the Editor control (see Figure 3).</span></span>


<span data-ttu-id="1d64e-124">[![HTML エディター コントロール](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1d64e-124">[![The HTML Editor control](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)</span></span>

<span data-ttu-id="1d64e-125">**図 03**:、HTML エディター コード ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="1d64e-125">**Figure 03**: The HTML Editor control([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image6.png))</span></span>


<span data-ttu-id="1d64e-126">HTML エディターをページにドラッグした後は、プロパティ シートでそのプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-126">After you drag the HTML Editor onto a page, you can set its properties in the property sheet.</span></span> <span data-ttu-id="1d64e-127">たとえば、通常 Width および Height プロパティを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-127">For example, you normally want to set the Width and Height properties.</span></span> <span data-ttu-id="1d64e-128">1 を一覧表示するには、HTML エディターを含む ASP.NET ページのソースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d64e-128">Listing 1 contains the source for an ASP.NET page that contains an HTML editor.</span></span>

<span data-ttu-id="1d64e-129">**1 - SimpleEditor.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1d64e-129">**Listing 1 - SimpleEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

<span data-ttu-id="1d64e-130">リスト 1 で、ページには、HTML エディター コントロール、Button コントロール、およびリテラル コントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d64e-130">The page in Listing 1 contains an HTML Editor control, a Button control, and a Literal control.</span></span> <span data-ttu-id="1d64e-131">リテラル コントロールの HTML エディターの内容が表示されます、ボタンをクリックすると (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="1d64e-131">When you click the button, the contents of the HTML Editor appear in the Literal control (see Figure 4).</span></span>


<span data-ttu-id="1d64e-132">[![HTML エディターでフォームを送信します。](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1d64e-132">[![Submitting a form with an HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)</span></span>

<span data-ttu-id="1d64e-133">**図 04**: HTML エディターでフォームを送信する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-cs/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="1d64e-133">**Figure 04**: Submitting a form with an HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image8.png))</span></span>


<span data-ttu-id="1d64e-134">HTML エディターのコンテンツ プロパティは、HTML エディターに入力した HTML コンテンツの検索に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-134">The HTML Editor Content property is used to retrieve the HTML content entered into the HTML Editor.</span></span> <span data-ttu-id="1d64e-135">この HTML コンテンツが JavaScript を含めることができますに注意します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-135">Be aware that this HTML content can contain JavaScript.</span></span> <span data-ttu-id="1d64e-136">次のセクションでは、JavaScript インジェクション攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-136">In the next section, we discuss how you can prevent JavaScript Injection Attacks.</span></span>

## <a name="customizing-the-html-editor-toolbar"></a><span data-ttu-id="1d64e-137">HTML エディターのツールバーをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="1d64e-137">Customizing the HTML Editor Toolbar</span></span>

<span data-ttu-id="1d64e-138">正確にボタンをカスタマイズすることができます、エディターに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-138">You can customize exactly which buttons appear in the editor.</span></span> <span data-ttu-id="1d64e-139">たとえばをユーザーが HTML エディターを HTML モードに切り替えることを防ぐために、[HTML] タブを削除する場合があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-139">For example, you might want to remove the HTML tab to prevent users from switching the HTML Editor into HTML mode.</span></span> <span data-ttu-id="1d64e-140">または、ユーザーがフォーラムに過度に大きなテキストを作成できないようにするには、フォント サイズ ドロップダウン リストを削除したい場合があります (図 5 参照) を post メッセージします。</span><span class="sxs-lookup"><span data-stu-id="1d64e-140">Or, you might want to remove the font size dropdown list to prevent users from creating overly large text in a forum message post (see Figure 5).</span></span>


<span data-ttu-id="1d64e-141">[![カスタマイズされた HTML エディター](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="1d64e-141">[![A customized HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)</span></span>

<span data-ttu-id="1d64e-142">**図 05**: A HTML エディターのカスタマイズ ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-html-editor-control-cs/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="1d64e-142">**Figure 05**: A customized HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-cs/_static/image10.png))</span></span>


<span data-ttu-id="1d64e-143">ツール バー ボタンをカスタマイズするには、新しい HTML エディターのエディターの基本クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-143">You customize the toolbar buttons by deriving a new HTML Editor from the base Editor class.</span></span> <span data-ttu-id="1d64e-144">たとえば、リスト 2 でのカスタム エディターには、太字と斜体のツール バー ボタンにはのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-144">For example, the custom editor in Listing 2 only contains toolbar buttons for bold and italic.</span></span> <span data-ttu-id="1d64e-145">その他のすべてのツール バー ボタンが削除されました。</span><span class="sxs-lookup"><span data-stu-id="1d64e-145">All other toolbar buttons have been removed.</span></span> <span data-ttu-id="1d64e-146">さらに、エディターの下部にある HTML タブがなくなりました (ただし、デザインおよびプレビュー タブが残っています)。</span><span class="sxs-lookup"><span data-stu-id="1d64e-146">Furthermore, the HTML tab has been removed from the bottom of the editor (but the Design and Preview tabs are still there).</span></span>

<span data-ttu-id="1d64e-147">**2 - アプリの一覧を表示する\_Code\CustomEditor.cs**</span><span class="sxs-lookup"><span data-stu-id="1d64e-147">**Listing 2 - App\_Code\CustomEditor.cs**</span></span>

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

<span data-ttu-id="1d64e-148">アプリに、リスト 2 で、クラスを追加する必要があります\_コード フォルダーのクラスが自動的にコンパイルされるようにします。</span><span class="sxs-lookup"><span data-stu-id="1d64e-148">You must add the class in Listing 2 to your App\_Code folder so that the class will be compiled automatically.</span></span> <span data-ttu-id="1d64e-149">場合、アプリ\_コード フォルダーは、web サイトに存在しませんし、フォルダーを追加することだけです。</span><span class="sxs-lookup"><span data-stu-id="1d64e-149">If the App\_Code folder does not exist in your website then you can simply add the folder.</span></span>

<span data-ttu-id="1d64e-150">カスタム エディターを作成した後追加できますが、ASP.NET ページに同じ方法で、通常の HTML エディター (リスト 3 参照) を追加すると。</span><span class="sxs-lookup"><span data-stu-id="1d64e-150">After you create a custom editor, you can add it to an ASP.NET page in the same way as you add the normal HTML Editor (see Listing 3).</span></span>

<span data-ttu-id="1d64e-151">**3 - ShowCustomEditor.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1d64e-151">**Listing 3 - ShowCustomEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a><span data-ttu-id="1d64e-152">クロス サイト スクリプティング (XSS) 攻撃の回避</span><span class="sxs-lookup"><span data-stu-id="1d64e-152">Avoiding Cross-Site Scripting (XSS) Attacks</span></span>

<span data-ttu-id="1d64e-153">ユーザーからの入力をそのまま使用し、web サイトにその入力を再表示すると、可能性のあるクロスサイト スクリプティング (XSS) 攻撃に web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-153">Whenever you accept input from a user, and redisplay that input on your website, you potentially open your website to Cross-Site Scripting (XSS) Attacks.</span></span> <span data-ttu-id="1d64e-154">理論上は、悪意のあるハッカーは、入力が再表示と実行される JavaScript コードを送信でした。</span><span class="sxs-lookup"><span data-stu-id="1d64e-154">In theory, a malicious hacker could submit JavaScript code that gets executed when the input is redisplayed.</span></span> <span data-ttu-id="1d64e-155">JavaScript は、ユーザーのパスワードやその他の機密情報の盗用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-155">The JavaScript could be used to steal user passwords or other sensitive information.</span></span>

<span data-ttu-id="1d64e-156">通常、html web ページに表示する前に、ユーザーから取得する任意の入力をエンコード XSS 攻撃を倒すことができます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-156">Normally, you can defeat XSS attacks by HTML encoding whatever input you retrieve from a user before displaying it in a web page.</span></span> <span data-ttu-id="1d64e-157">ただし、HTML 出力を HTML エディターのエンコードはのみエンコードしない&lt;スクリプト&gt;タグも、すべての HTML タグをエンコードことはできます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-157">However, HTML encoding the output of the HTML Editor would not only encode &lt;script&gt; tags, it would also encode all HTML tags.</span></span> <span data-ttu-id="1d64e-158">つまり、すべての種類のフォント、フォント サイズ、および背景色などの書式設定が失われます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-158">In other words, you would lose all of the formatting such as the font type, font size, and background color.</span></span>

<span data-ttu-id="1d64e-159">パスワード、クレジット_カード番号、社会保障番号のなどのユーザーから機密情報を収集している場合、HTML エディターでのユーザーから取得したコンテンツのエンコードされていないいない表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d64e-159">If you are collecting sensitive information from your users -- such as passwords, credit-card numbers, and social security numbers - then you should not display un-encoded content that you retrieve from a user with the HTML Editor.</span></span> <span data-ttu-id="1d64e-160">HTML コンテンツを再表示できませんまたは HTML コンテンツを送信していますの状況でのみ、HTML エディターを使用して、信頼されたパーティで web サイトにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-160">You should use the HTML Editor only in situations in which you are not redisplaying the HTML content, or the HTML content is being submitted to your website by a trusted party.</span></span>

<span data-ttu-id="1d64e-161">たとえば、ブログのアプリケーションを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="1d64e-161">Imagine, for example, that you are creating a blog application.</span></span> <span data-ttu-id="1d64e-162">このような状況で理にかなってブログの投稿を作成するときに、HTML エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d64e-162">In this situation, it makes sense to use the HTML Editor when composing blog posts.</span></span> <span data-ttu-id="1d64e-163">ブログの投稿を送信した 1 つのみと、悪意のある JavaScript の送信が自分で信頼の。</span><span class="sxs-lookup"><span data-stu-id="1d64e-163">You are the only one who submits a blog post and, presumably, you can trust yourself not to submit malicious JavaScript.</span></span> <span data-ttu-id="1d64e-164">ただし、匿名ユーザーがコメントの投稿を許可する場合は、HTML エディターを使用しても意味は行いません。</span><span class="sxs-lookup"><span data-stu-id="1d64e-164">However, it does not make sense to use the HTML Editor when allowing anonymous users to post comments.</span></span> <span data-ttu-id="1d64e-165">ユーザーがパスワードなどの機密情報を送信する場合に特に注意が必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-165">You should be especially careful in situations in which users submit sensitive information such as passwords.</span></span> <span data-ttu-id="1d64e-166">場合によっては、悪意のあるユーザーは、パスワードを盗むの適切な JavaScript を含むコメントを投稿する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1d64e-166">Potentially, a malicious user could post a comment that contains the right JavaScript for stealing a password.</span></span>

## <a name="summary"></a><span data-ttu-id="1d64e-167">まとめ</span><span class="sxs-lookup"><span data-stu-id="1d64e-167">Summary</span></span>

<span data-ttu-id="1d64e-168">このチュートリアルでは、AJAX Control Toolkit に含まれる HTML エディター コントロールの概要を簡単にいました。</span><span class="sxs-lookup"><span data-stu-id="1d64e-168">In this tutorial, you were provided with a brief overview of the HTML Editor control included in the AJAX Control Toolkit.</span></span> <span data-ttu-id="1d64e-169">HTML エディターを使用して、ユーザーからの豊富なコンテンツを受け取るし、サーバーにコンテンツを送信する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="1d64e-169">You learned how to use the HTML Editor to accept rich content from a user and submit the content to the server.</span></span> <span data-ttu-id="1d64e-170">HTML エディターによって表示されるツールバー ボタンをカスタマイズする方法についても説明しました。</span><span class="sxs-lookup"><span data-stu-id="1d64e-170">We also discussed how you can customize the toolbar buttons that are displayed by the HTML Editor.</span></span> <span data-ttu-id="1d64e-171">最後に、HTML エディターを使用して、可能性のある悪意のある入力をそのまま使用する場合、クロスサイト スクリプティング攻撃を回避する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="1d64e-171">Finally, you learned how to avoid Cross-Site Scripting Attacks when using the HTML Editor to accept potentially malicious input.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1d64e-172">次へ</span><span class="sxs-lookup"><span data-stu-id="1d64e-172">Next</span></span>](how-do-i-use-the-html-editor-control-vb.md)
