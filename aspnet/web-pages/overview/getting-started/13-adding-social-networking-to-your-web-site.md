---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: ページ (Razor) サイトを ASP.NET web のソーシャル ネットワー キングの追加 |Microsoft Docs
author: tfitzmac
description: この章では、ソーシャル ネットワー キング サービスと、サイトを統合する方法について説明します。 この章では、ユーザー web サイトをブックマーク/リンクできるようにする方法を学習しています.
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 684fcfdde0aefeb168398bdf7a42f9fdbd6e48b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825885"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="93825-104">ソーシャル ネットワー キングを ASP.NET Web ページ (Razor) サイトを追加します。</span><span class="sxs-lookup"><span data-stu-id="93825-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="93825-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="93825-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="93825-106">この記事では、Twitter のフィード、Xbox のゲーマー カードおよび Gravatar イメージに含める方法と、ASP.NET Web Pages (Razor) の web サイトのページに、Facebook、Twitter、Reddit、Digg、ソーシャル ネットワー キングへのリンクを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="93825-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="93825-107">学習内容。</span><span class="sxs-lookup"><span data-stu-id="93825-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="93825-108">ユーザーに、サイト/リンクをブックマークする方法。</span><span class="sxs-lookup"><span data-stu-id="93825-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="93825-109">Twitter のフィードを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="93825-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="93825-110">Facebook を追加する方法**など**ページにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="93825-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="93825-111">Gravatar.com イメージをレンダリングする方法。</span><span class="sxs-lookup"><span data-stu-id="93825-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="93825-112">Xbox ゲームのカードをサイトに表示する方法。</span><span class="sxs-lookup"><span data-stu-id="93825-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="93825-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="93825-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="93825-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="93825-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="93825-115">ASP.NET Web ヘルパー ライブラリ (NuGet パッケージ)</span><span class="sxs-lookup"><span data-stu-id="93825-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="93825-116">このチュートリアルは、ASP.NET Web ヘルパー ライブラリを使用する部分を除く ASP.NET Web ページの 3 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="93825-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="93825-117">ソーシャル ネットワー キング サイトで web サイトのリンク</span><span class="sxs-lookup"><span data-stu-id="93825-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="93825-118">ユーザーなどのサイトで何か、友人と共有する場合よくあります。</span><span class="sxs-lookup"><span data-stu-id="93825-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="93825-119">この簡単な Digg、Reddit、Facebook、Twitter、または類似したサイトのページを共有するユーザーがクリックできるグリフ (アイコン) を表示することで。</span><span class="sxs-lookup"><span data-stu-id="93825-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="93825-120">これらのグリフを表示するには追加、`LinkSharecode`ページ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="93825-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="93825-121">ページの閲覧者は、個々 のグリフをクリックできます。</span><span class="sxs-lookup"><span data-stu-id="93825-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="93825-122">ソーシャル ネットワーク サイトを持つアカウントがそのサイトでし、ページへのリンクに投稿できます。</span><span class="sxs-lookup"><span data-stu-id="93825-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![図 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="93825-124">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)してください - 既に追加していない場合、という名前のページを作成*ListLinkShare.cshtml*追加。次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="93825-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="93825-125">この例では、ときに、`LinkShare`ヘルパーの実行、ページのタイトルは、パラメーターをソーシャル ネットワー キング サイトにページ タイトルを渡しますとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="93825-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="93825-126">ただしで任意の文字列を渡す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="93825-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="93825-127">この例では、一覧に含めるソーシャル ネットワーク サイトを指定します。</span><span class="sxs-lookup"><span data-stu-id="93825-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="93825-128">サイトに関連するソーシャル ネットワーク サイトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="93825-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="93825-129">実行、 *ListLinkShare.cshtml*ブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="93825-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="93825-130">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。</span><span class="sxs-lookup"><span data-stu-id="93825-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="93825-131">サインアップ、サインインしているサイトのいずれかのグリフをクリックします。</span><span class="sxs-lookup"><span data-stu-id="93825-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="93825-132">リンク ページに移動、共有できる、選択したソーシャル ネットワーク サイトにリンクします。</span><span class="sxs-lookup"><span data-stu-id="93825-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="93825-133">たとえば、Reddit リンクをクリックすると表示されます、 `submit to reddit` Reddit web サイトのページ。</span><span class="sxs-lookup"><span data-stu-id="93825-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![2 番目の画像](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="93825-135">フィード、Twitter を追加します。</span><span class="sxs-lookup"><span data-stu-id="93825-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="93825-136">Twitter API の現在のバージョンと互換性がある Twitter ヘルパーの使用方法の詳細については、次を参照してください。 [Twitter ヘルパー](../ui-layouts-and-themes/twitter-helper.md)します。</span><span class="sxs-lookup"><span data-stu-id="93825-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="93825-137">この例では、多くのページからコードを簡単に再利用できるように、独自のヘルパーを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="93825-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="93825-138">Facebook を表示する&quot;など&quot;ボタン</span><span class="sxs-lookup"><span data-stu-id="93825-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="93825-139">場合によっては、最善の対処方法は、ヘルパーに依存するのではなく、ソーシャル ネットワーク プロバイダーから直接コードを取得するがします。</span><span class="sxs-lookup"><span data-stu-id="93825-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="93825-140">これは、ソーシャル ネットワーク プロバイダーは、ヘルパーが更新されたよりもすばやくそのオプションを更新する場合に特に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="93825-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="93825-141">コード スニペットを取得するには、サイトを Facebook 機能 (など、Like ボタン) を追加する、 [developers.facebook.com](https://developers.facebook.com/)サイト。</span><span class="sxs-lookup"><span data-stu-id="93825-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="93825-142">Facebook のサイトでは、そのツールを使用して、サイトに関連するコード スニペットを生成します。</span><span class="sxs-lookup"><span data-stu-id="93825-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="93825-143">次の強調表示されたコードでは、developers.facebook.com のサイトで、ボタンのようなツールから取得されたコードを示します。</span><span class="sxs-lookup"><span data-stu-id="93825-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="93825-144">独自のアプリ ID を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="93825-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="93825-145">Gravatar イメージを表示</span><span class="sxs-lookup"><span data-stu-id="93825-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="93825-146">A *Gravatar* (、&quot;グローバルに認識されたアバター&quot;) 複数の web サイトで自分のアバターとして使用できるイメージは、&#8212;を表すイメージは、します。</span><span class="sxs-lookup"><span data-stu-id="93825-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="93825-147">たとえば、Gravatar はフォーラム投稿をブログ コメント内のユーザーを識別しにできます。</span><span class="sxs-lookup"><span data-stu-id="93825-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="93825-148">(Gravatar web サイトで、独自の Gravatar を登録する[ http://www.gravatar.com/ ](http://www.gravatar.com/))。Web サイト上のユーザーの名前または電子メール アドレスの横にあるイメージを表示するには、Gravatar ヘルパーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="93825-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="93825-149">この例では、自分自身を表す 1 つ Gravatar を使用しています。</span><span class="sxs-lookup"><span data-stu-id="93825-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="93825-150">Gravatar を使用する別の方法では、ユーザーに、サイトで登録するときに、Gravatar アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="93825-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="93825-151">(ユーザーに登録する方法を学習できます[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904))。そのユーザーの情報を表示するたびに、ユーザーの名前を表示する、Gravatar だけ追加できます。</span><span class="sxs-lookup"><span data-stu-id="93825-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="93825-152">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="93825-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="93825-153">という名前の新しい web ページを作成する*Gravatar.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="93825-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="93825-154">ファイルには、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="93825-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="93825-155">`Gravatar.GetHtml`メソッドがページに Gravatar イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="93825-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="93825-156">イメージのサイズを変更するには、2 番目のパラメーターとして数値を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="93825-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="93825-157">既定のサイズは、80 です。</span><span class="sxs-lookup"><span data-stu-id="93825-157">The default size is 80.</span></span> <span data-ttu-id="93825-158">番号が 80 未満の作成、イメージより小さい。</span><span class="sxs-lookup"><span data-stu-id="93825-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="93825-159">80 より大きい数値では、画像を拡大します。</span><span class="sxs-lookup"><span data-stu-id="93825-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="93825-160">`Gravatar.GetHtml`メソッドを置き換える`<Your Gravatar account here>`Gravatar アカウントを使用する電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="93825-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="93825-161">(Gravatar アカウントを持っていない場合を使用できますがユーザーの電子メール アドレス。)</span><span class="sxs-lookup"><span data-stu-id="93825-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="93825-162">お使いのブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="93825-162">Run the page in your browser.</span></span> <span data-ttu-id="93825-163">ページには、指定した電子メール アドレスの 2 つの Gravatar イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="93825-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="93825-164">2 番目のイメージは、最初よりも小さくなります。</span><span class="sxs-lookup"><span data-stu-id="93825-164">The second image is smaller than the first.</span></span> 

    ![図 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="93825-166">Xbox ゲームのカードを表示します。</span><span class="sxs-lookup"><span data-stu-id="93825-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="93825-167">各ユーザーが一意の ID を持つユーザーは、Microsoft Xbox オンライン ゲームを再生、ときに</span><span class="sxs-lookup"><span data-stu-id="93825-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="93825-168">表示、評価、ゲーマー スコア、およびゲームを最近再生したゲーマー、カードの形式では、各プレーヤーでは、統計は保持されます。</span><span class="sxs-lookup"><span data-stu-id="93825-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="93825-169">ゲーマー Xbox 場合は、サイト内のページ上のゲーマーのカードを表示を使用して、`GamerCard`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="93825-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="93825-170">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="93825-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="93825-171">という名前の新しいページを作成する*XboxGamer.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="93825-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="93825-172">使用する、`GamerCard.GetHtml`プロパティを表示するゲーマーのカードのエイリアスを指定します。</span><span class="sxs-lookup"><span data-stu-id="93825-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="93825-173">お使いのブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="93825-173">Run the page in your browser.</span></span> <span data-ttu-id="93825-174">ページには、指定した Xbox ゲーマー カードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="93825-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![図 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
