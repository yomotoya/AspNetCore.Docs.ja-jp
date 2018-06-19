---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: ページ (Razor) サイトの ASP.NET Web へのソーシャル ネットワー キングの追加 |Microsoft ドキュメント
author: tfitzmac
description: この章では、ソーシャル ネットワーク サービスをサイトを統合する方法について説明します。 この章では、ブックマーク/リンクの web サイトのユーザーに通知する方法を学習しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899344"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f4c63-104">ソーシャル ネットワークを ASP.NET Web Pages (Razor) サイトを追加します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f4c63-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f4c63-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f4c63-106">この記事では、Twitter フィード、Xbox ゲーマーのカードと Gravatar イメージを含める方法と、ASP.NET Web Pages (Razor) の web サイトのページに、Facebook、Twitter、Reddit、および Digg のソーシャル ネットワークへのリンクを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="f4c63-107">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="f4c63-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f4c63-108">ブックマーク/リンクのサイトのユーザーに通知する方法。</span><span class="sxs-lookup"><span data-stu-id="f4c63-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="f4c63-109">Twitter フィードを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="f4c63-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="f4c63-110">Facebook を追加する方法**など**ページへボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4c63-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="f4c63-111">Gravatar.com 画像を表示する方法。</span><span class="sxs-lookup"><span data-stu-id="f4c63-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="f4c63-112">Xbox ゲーマー カードをサイトに表示する方法。</span><span class="sxs-lookup"><span data-stu-id="f4c63-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4c63-113">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f4c63-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f4c63-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f4c63-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f4c63-115">ASP.NET Web ヘルパー ライブラリ (NuGet パッケージ)</span><span class="sxs-lookup"><span data-stu-id="f4c63-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="f4c63-116">このチュートリアルは、ASP.NET Web ヘルパー ライブラリを使用する部分を除く ASP.NET Web Pages 3 のでも動作します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="f4c63-117">ソーシャル ネットワーク サイトに web サイトをリンク</span><span class="sxs-lookup"><span data-stu-id="f4c63-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="f4c63-118">ユーザーには、サイトで何かと同様に、多くの場合する友人と共有します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="f4c63-119">この簡単な Digg、Reddit、Facebook、Twitter、または類似したサイトのページを共有するユーザーがクリックされるグリフ (アイコン) を表示することによってです。</span><span class="sxs-lookup"><span data-stu-id="f4c63-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="f4c63-120">これらのグリフを表示するには追加、`LinkSharecode`ページ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="f4c63-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="f4c63-121">ページの閲覧者は、個々 のグリフをクリックできます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="f4c63-122">ソーシャル ネットワーク サイトにアカウントを持っている場合は、そのサイトでページへのリンクを投稿したことができますし、します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![図 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="f4c63-124">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)ください - 既に追加していない場合、という名前のページを作成する*ListLinkShare.cshtml*し、追加次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="f4c63-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="f4c63-125">この例では、ときに、`LinkShare`ヘルパーを実行するページのタイトルは、さらに、ソーシャル ネットワーク サイトにページのタイトルを通過するパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="f4c63-126">ただしで使用文字列を渡す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f4c63-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="f4c63-127">この例では、一覧に含める場合はどのソーシャル ネットワー キング サイトも指定します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="f4c63-128">サイトに関連性のあるソーシャル ネットワー キング サイトを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="f4c63-129">実行、 *ListLinkShare.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="f4c63-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="f4c63-130">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。</span><span class="sxs-lookup"><span data-stu-id="f4c63-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="f4c63-131">サインインしているサイトのいずれかのグリフをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4c63-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="f4c63-132">リンクをクリックするページを共有することができます、ソーシャル ネットワークを選択したサイトにリンクします。</span><span class="sxs-lookup"><span data-stu-id="f4c63-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="f4c63-133">たとえば、Reddit リンクをクリックするとが表示される、 `submit to reddit` Reddit web サイトのページです。</span><span class="sxs-lookup"><span data-stu-id="f4c63-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![2 番目の画像](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="f4c63-135">フィード、Twitter の追加</span><span class="sxs-lookup"><span data-stu-id="f4c63-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="f4c63-136">Twitter API の現在のバージョンと互換性がある Twitter ヘルパーの使用方法の詳細については、次を参照してください。[ヘルパーの Twitter](../ui-layouts-and-themes/twitter-helper.md)です。</span><span class="sxs-lookup"><span data-stu-id="f4c63-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="f4c63-137">この例では、多くのページからコードを簡単に再利用できるように、独自のヘルパーを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="f4c63-138">Facebook を表示する&quot;など&quot;ボタン</span><span class="sxs-lookup"><span data-stu-id="f4c63-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="f4c63-139">場合によっては、最善の対処方法でヘルパーに依存するのではなく、ソーシャル ネットワーク プロバイダーから直接コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="f4c63-140">これは、ソーシャル ネットワーク プロバイダーが、ヘルパーが更新されたよりも速く、オプションを更新する場合に特に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="f4c63-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="f4c63-141">Facebook 機能 (など、Like のボタン) をサイトに追加するからのコード スニペットを取得できます、 [developers.facebook.com](https://developers.facebook.com/)サイトです。</span><span class="sxs-lookup"><span data-stu-id="f4c63-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="f4c63-142">Facebook サイトで、ツールを使用して、サイトに関連するコード スニペットを生成します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="f4c63-143">次の強調表示されたコードは、developers.facebook.com サイトで、ボタンのようなツールから取得されたコードです。</span><span class="sxs-lookup"><span data-stu-id="f4c63-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="f4c63-144">独自のアプリ ID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4c63-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="f4c63-145">Gravatar イメージを表示</span><span class="sxs-lookup"><span data-stu-id="f4c63-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="f4c63-146">A *Gravatar* (、&quot;グローバルに認識されているアバター&quot;) は、自分のアバターとして複数の web サイトで使用できるイメージ&#8212;するを表すイメージは、します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="f4c63-147">たとえば、Gravatar は投稿では、フォーラム、ブログのコメントに個人を特定しにできます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="f4c63-148">(Gravatar の web サイトで、独自の Gravatar を登録する[ http://www.gravatar.com/ ](http://www.gravatar.com/))。Web サイトに個人の名前や電子メール アドレスの横にあるイメージを表示する場合は、Gravatar ヘルパーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="f4c63-149">この例では、自分自身を表す単一 Gravatar を使用しています。</span><span class="sxs-lookup"><span data-stu-id="f4c63-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="f4c63-150">Gravatar を使用する別の方法では、サイトに登録するときに、Gravatar アドレスを指定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f4c63-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="f4c63-151">(ユーザーが登録できるようにする方法を学習できます[追加のセキュリティと ASP.NET Web Pages サイトにメンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904))。そのユーザーの情報を表示するたびに、ユーザーの名前を表示する、Gravatar だけ追加できます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="f4c63-152">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f4c63-153">という名前の新しい web ページを作成する*Gravatar.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="f4c63-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="f4c63-154">ファイルに次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="f4c63-155">`Gravatar.GetHtml`メソッドでは、ページ上 Gravatar イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="f4c63-156">イメージのサイズを変更するには、2 番目のパラメーターとして数値を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="f4c63-157">既定のサイズは 80 です。</span><span class="sxs-lookup"><span data-stu-id="f4c63-157">The default size is 80.</span></span> <span data-ttu-id="f4c63-158">番号が 80 未満の作成、イメージ小さくできます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="f4c63-159">80 より大きい数値では、画像を拡大します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="f4c63-160">`Gravatar.GetHtml`メソッド、置換`<Your Gravatar account here>`Gravatar アカウントを使用する電子メール アドレスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="f4c63-161">(Gravatar アカウントを持っていない場合、使用できます人の電子メール アドレスです。)</span><span class="sxs-lookup"><span data-stu-id="f4c63-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="f4c63-162">お使いのブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-162">Run the page in your browser.</span></span> <span data-ttu-id="f4c63-163">指定した電子メール アドレスの 2 つの Gravatar イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="f4c63-164">2 番目のイメージは、最初より小さくなります。</span><span class="sxs-lookup"><span data-stu-id="f4c63-164">The second image is smaller than the first.</span></span> 

    ![図 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="f4c63-166">Xbox ゲーマー カードを表示します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="f4c63-167">ユーザーは、Microsoft Xbox オンライン ゲームを再生するときに、各ユーザーが一意の id。</span><span class="sxs-lookup"><span data-stu-id="f4c63-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="f4c63-168">統計は、各プレーヤー、評価、ゲーマーのスコアが表示され、ゲームを最近再生したゲーム、カードなどの形式で保存されます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="f4c63-169">ゲーマー Xbox でない場合に表示できる、ゲーマー カード サイト内のページを使用して、`GamerCard`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="f4c63-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="f4c63-170">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f4c63-171">という名前の新しいページを作成する*XboxGamer.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="f4c63-172">使用する、`GamerCard.GetHtml`プロパティを表示するゲーマー カードに対して別名を指定します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="f4c63-173">お使いのブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="f4c63-173">Run the page in your browser.</span></span> <span data-ttu-id="f4c63-174">ページには、指定した Xbox ゲーマー カードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f4c63-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![図 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
