---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: (VB) のパスワードの強度をテスト |Microsoft ドキュメント
author: wenz
description: パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。 ASP で PasswordStrength コントロールです。N..
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="f90ef-104">テスト (VB) のパスワードの強度</span><span class="sxs-lookup"><span data-stu-id="f90ef-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="f90ef-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f90ef-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f90ef-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f90ef-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="f90ef-107">パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f90ef-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="f90ef-108">ASP.NET AJAX コントロールのツールキットで PasswordStrength コントロールは、適切な方法は、パスワードが確認できます。</span><span class="sxs-lookup"><span data-stu-id="f90ef-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="f90ef-109">概要</span><span class="sxs-lookup"><span data-stu-id="f90ef-109">Overview</span></span>

<span data-ttu-id="f90ef-110">パスワードが要求されて、どこにでもほぼレイジー ユーザーから傾向を容易に実行を中断する単純なパスワードを選択できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f90ef-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="f90ef-111">`PasswordStrength` ASP.NET AJAX コントロール Toolkit でコントロールを適切な方法は、パスワードが確認できます。</span><span class="sxs-lookup"><span data-stu-id="f90ef-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="f90ef-112">手順</span><span class="sxs-lookup"><span data-stu-id="f90ef-112">Steps</span></span>

<span data-ttu-id="f90ef-113">`PasswordStrength`コントロールがテキスト ボックスを拡張し、パスワードが満足できるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="f90ef-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="f90ef-114">さまざまな属性のオプションが提供しています次にそれらの一部を示します。</span><span class="sxs-lookup"><span data-stu-id="f90ef-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="f90ef-115">`MinimumNumericCharacters` パスワードに必要な数字の最小数</span><span class="sxs-lookup"><span data-stu-id="f90ef-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="f90ef-116">`MinimumSymbolCharacters` パスワードに必要な記号文字 (文字と数字ではない) の最小数</span><span class="sxs-lookup"><span data-stu-id="f90ef-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="f90ef-117">`PreferredPasswordLength` パスワードの最小の長さ</span><span class="sxs-lookup"><span data-stu-id="f90ef-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="f90ef-118">`RequiresUpperAndLowerCaseCharacters` かどうか、パスワードは、大文字と小文字の両方を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f90ef-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="f90ef-119">`StrengthIndicatorType`にテキストとして、パスワードの強度を表示する方法を説明 (値`"Text"`) または進行状況バーの一種として (値`"BarIndicator"`)。</span><span class="sxs-lookup"><span data-stu-id="f90ef-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="f90ef-120">`DisplayPosition`情報が表示される場所を構成する属性。</span><span class="sxs-lookup"><span data-stu-id="f90ef-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="f90ef-121">ASP.NET AJAX を含む、完全な例を次に示します`ScriptManager`コントロール、`PasswordStrength`コントロール、そしてもちろん、テキスト ボックス、ユーザーがパスワードを入力可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f90ef-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="f90ef-122">デモについては、ここでは、後者フォーム フィールドを通常のテキスト フィールド、パスワード フィールドではありません、入力中の開発時に認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f90ef-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="f90ef-123">ページを実行し、休暇から入力: 英小文字、大文字、数字および記号を入力した後、パスワードと見なされるに分割できないのみです。</span><span class="sxs-lookup"><span data-stu-id="f90ef-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="f90ef-124">[![今すぐパスワードをお勧め (非常に)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f90ef-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="f90ef-125">(パスワード非常) に優れているようになりました ([フルサイズのイメージを表示するをクリックして](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f90ef-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f90ef-126">前へ</span><span class="sxs-lookup"><span data-stu-id="f90ef-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
