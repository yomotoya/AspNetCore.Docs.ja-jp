---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: (VB) のパスワードの強度をテスト |Microsoft Docs
author: wenz
description: パスワードが限定的なユーザーが簡単に解読される単純なパスワードを選択する傾向があるように、ほぼどこにでも必要です。 ASP で PasswordStrength コントロール。N..
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 7073a06186ba3ff6c6a12d558445d75d9301589a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805902"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="1ba54-104">(VB) のパスワードの強度をテスト</span><span class="sxs-lookup"><span data-stu-id="1ba54-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="1ba54-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1ba54-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1ba54-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1ba54-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="1ba54-107">パスワードが限定的なユーザーが簡単に解読される単純なパスワードを選択する傾向があるように、ほぼどこにでも必要です。</span><span class="sxs-lookup"><span data-stu-id="1ba54-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="1ba54-108">PasswordStrength コントロール、ASP.NET AJAX Control Toolkit では、パスワードは、どの程度適切かを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1ba54-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="1ba54-109">概要</span><span class="sxs-lookup"><span data-stu-id="1ba54-109">Overview</span></span>

<span data-ttu-id="1ba54-110">パスワードが限定的なユーザーが簡単に解読される単純なパスワードを選択する傾向があるように、ほぼどこにでも必要です。</span><span class="sxs-lookup"><span data-stu-id="1ba54-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="1ba54-111">`PasswordStrength` ASP.NET AJAX Control Toolkit でコントロールをどの程度適切かパスワードが確認できます。</span><span class="sxs-lookup"><span data-stu-id="1ba54-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="1ba54-112">手順</span><span class="sxs-lookup"><span data-stu-id="1ba54-112">Steps</span></span>

<span data-ttu-id="1ba54-113">`PasswordStrength`コントロールがテキスト ボックスを拡張し、パスワードで十分かどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="1ba54-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="1ba54-114">豊富な属性を使用してオプションを提供していますそれらのほんの一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1ba54-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="1ba54-115">`MinimumNumericCharacters` パスワードに必要な数字の最小数</span><span class="sxs-lookup"><span data-stu-id="1ba54-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="1ba54-116">`MinimumSymbolCharacters` パスワードに必要な記号文字 (文字と数字ではない) の最小数</span><span class="sxs-lookup"><span data-stu-id="1ba54-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="1ba54-117">`PreferredPasswordLength` パスワードの最小の長さ</span><span class="sxs-lookup"><span data-stu-id="1ba54-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="1ba54-118">`RequiresUpperAndLowerCaseCharacters` かどうか、パスワードは、大文字と小文字の両方の文字を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1ba54-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="1ba54-119">`StrengthIndicatorType`にテキストとして、パスワードの強度を表示する方法の情報を提供します (値`"Text"`) または進行状況バーの一種として (値`"BarIndicator"`)。</span><span class="sxs-lookup"><span data-stu-id="1ba54-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="1ba54-120">`DisplayPosition`情報が表示される場所を構成する属性。</span><span class="sxs-lookup"><span data-stu-id="1ba54-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="1ba54-121">ASP.NET AJAX を含む、完全な例を次に示します`ScriptManager`コントロール、`PasswordStrength`コントロールとテキスト ボックス、ユーザーがパスワードを入力できます。</span><span class="sxs-lookup"><span data-stu-id="1ba54-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="1ba54-122">デモンストレーションのため、後者フォーム フィールドは通常のテキスト フィールドと [パスワード] フィールドではない入力内容を開発中に表示できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1ba54-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="1ba54-123">ページを実行し、離れた入力: のみ、小文字、大文字、数字および記号を入力したら、パスワードは、unbreakable と判断します。</span><span class="sxs-lookup"><span data-stu-id="1ba54-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="1ba54-124">[![パスワードが (かなり) 良いようになりました](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ba54-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="1ba54-125">パスワードは (かなり) 良い ([フルサイズの画像を表示する をクリックします](testing-the-strength-of-a-password-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="1ba54-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1ba54-126">前へ</span><span class="sxs-lookup"><span data-stu-id="1ba54-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
