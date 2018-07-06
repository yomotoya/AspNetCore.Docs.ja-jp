---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: ページ (Razor) サイトを ASP.NET web のソーシャル ネットワー キングの追加 |Microsoft Docs
author: tfitzmac
description: この章では、ソーシャル ネットワー キング サービスと、サイトを統合する方法について説明します。 この章では、ユーザー web サイトをブックマーク/リンクできるようにする方法を学習しています.
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e50a35d9770da247d18bbe1b3660b7bd5d46d8e9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822445"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>ソーシャル ネットワー キングを ASP.NET Web ページ (Razor) サイトを追加します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Twitter のフィード、Xbox のゲーマー カードおよび Gravatar イメージに含める方法と、ASP.NET Web Pages (Razor) の web サイトのページに、Facebook、Twitter、Reddit、Digg、ソーシャル ネットワー キングへのリンクを追加する方法について説明します。
> 
> 学習内容。
> 
> - ユーザーに、サイト/リンクをブックマークする方法。
> - Twitter のフィードを追加する方法。
> - Facebook を追加する方法**など**ページにボタンをクリックします。
> - Gravatar.com イメージをレンダリングする方法。
> - Xbox ゲームのカードをサイトに表示する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web ヘルパー ライブラリ (NuGet パッケージ)
>   
> 
> このチュートリアルは、ASP.NET Web ヘルパー ライブラリを使用する部分を除く ASP.NET Web ページの 3 でも動作します。


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>ソーシャル ネットワー キング サイトで web サイトのリンク

ユーザーなどのサイトで何か、友人と共有する場合よくあります。 この簡単な Digg、Reddit、Facebook、Twitter、または類似したサイトのページを共有するユーザーがクリックできるグリフ (アイコン) を表示することで。

これらのグリフを表示するには追加、`LinkSharecode`ページ ヘルパー。 ページの閲覧者は、個々 のグリフをクリックできます。 ソーシャル ネットワーク サイトを持つアカウントがそのサイトでし、ページへのリンクに投稿できます。

![図 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)してください - 既に追加していない場合、という名前のページを作成*ListLinkShare.cshtml*追加。次のマークアップ。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    この例では、ときに、`LinkShare`ヘルパーの実行、ページのタイトルは、パラメーターをソーシャル ネットワー キング サイトにページ タイトルを渡しますとして渡されます。 ただしで任意の文字列を渡す可能性があります。 この例では、一覧に含めるソーシャル ネットワーク サイトを指定します。 サイトに関連するソーシャル ネットワーク サイトを指定できます。
2. 実行、 *ListLinkShare.cshtml*ブラウザーでページ。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。
3. サインアップ、サインインしているサイトのいずれかのグリフをクリックします。 リンク ページに移動、共有できる、選択したソーシャル ネットワーク サイトにリンクします。 たとえば、Reddit リンクをクリックすると表示されます、 `submit to reddit` Reddit web サイトのページ。

     ![2 番目の画像](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>フィード、Twitter を追加します。

Twitter API の現在のバージョンと互換性がある Twitter ヘルパーの使用方法の詳細については、次を参照してください。 [Twitter ヘルパー](../ui-layouts-and-themes/twitter-helper.md)します。 この例では、多くのページからコードを簡単に再利用できるように、独自のヘルパーを記述する方法を示します。

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Facebook を表示する&quot;など&quot;ボタン

場合によっては、最善の対処方法は、ヘルパーに依存するのではなく、ソーシャル ネットワーク プロバイダーから直接コードを取得するがします。 これは、ソーシャル ネットワーク プロバイダーは、ヘルパーが更新されたよりもすばやくそのオプションを更新する場合に特に当てはまります。

コード スニペットを取得するには、サイトを Facebook 機能 (など、Like ボタン) を追加する、 [developers.facebook.com](https://developers.facebook.com/)サイト。 Facebook のサイトでは、そのツールを使用して、サイトに関連するコード スニペットを生成します。

次の強調表示されたコードでは、developers.facebook.com のサイトで、ボタンのようなツールから取得されたコードを示します。 独自のアプリ ID を提供する必要があります。

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Gravatar イメージを表示

A *Gravatar* (、&quot;グローバルに認識されたアバター&quot;) 複数の web サイトで自分のアバターとして使用できるイメージは、&#8212;を表すイメージは、します。 たとえば、Gravatar はフォーラム投稿をブログ コメント内のユーザーを識別しにできます。 (Gravatar web サイトで、独自の Gravatar を登録する[ http://www.gravatar.com/ ](http://www.gravatar.com/))。Web サイト上のユーザーの名前または電子メール アドレスの横にあるイメージを表示するには、Gravatar ヘルパーを使用することができます。

この例では、自分自身を表す 1 つ Gravatar を使用しています。 Gravatar を使用する別の方法では、ユーザーに、サイトで登録するときに、Gravatar アドレスを指定します。 (ユーザーに登録する方法を学習できます[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904))。そのユーザーの情報を表示するたびに、ユーザーの名前を表示する、Gravatar だけ追加できます。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しい web ページを作成する*Gravatar.cshtml*します。
3. ファイルには、次のマークアップを追加します。 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml`メソッドがページに Gravatar イメージを表示します。 イメージのサイズを変更するには、2 番目のパラメーターとして数値を含めることができます。 既定のサイズは、80 です。 番号が 80 未満の作成、イメージより小さい。 80 より大きい数値では、画像を拡大します。
4. `Gravatar.GetHtml`メソッドを置き換える`<Your Gravatar account here>`Gravatar アカウントを使用する電子メール アドレス。 (Gravatar アカウントを持っていない場合を使用できますがユーザーの電子メール アドレス。)
5. お使いのブラウザーでページを実行します。 ページには、指定した電子メール アドレスの 2 つの Gravatar イメージが表示されます。 2 番目のイメージは、最初よりも小さくなります。 

    ![図 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Xbox ゲームのカードを表示します。

各ユーザーが一意の ID を持つユーザーは、Microsoft Xbox オンライン ゲームを再生、ときに 表示、評価、ゲーマー スコア、およびゲームを最近再生したゲーマー、カードの形式では、各プレーヤーでは、統計は保持されます。 ゲーマー Xbox 場合は、サイト内のページ上のゲーマーのカードを表示を使用して、`GamerCard`ヘルパー。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*XboxGamer.cshtml*し、次のマークアップを追加します。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    使用する、`GamerCard.GetHtml`プロパティを表示するゲーマーのカードのエイリアスを指定します。
3. お使いのブラウザーでページを実行します。 ページには、指定した Xbox ゲーマー カードが表示されます。

    ![図 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
