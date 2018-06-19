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
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>ソーシャル ネットワークを ASP.NET Web Pages (Razor) サイトを追加します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Twitter フィード、Xbox ゲーマーのカードと Gravatar イメージを含める方法と、ASP.NET Web Pages (Razor) の web サイトのページに、Facebook、Twitter、Reddit、および Digg のソーシャル ネットワークへのリンクを追加する方法について説明します。
> 
> 学習する内容。
> 
> - ブックマーク/リンクのサイトのユーザーに通知する方法。
> - Twitter フィードを追加する方法。
> - Facebook を追加する方法**など**ページへボタンをクリックします。
> - Gravatar.com 画像を表示する方法。
> - Xbox ゲーマー カードをサイトに表示する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web ヘルパー ライブラリ (NuGet パッケージ)
>   
> 
> このチュートリアルは、ASP.NET Web ヘルパー ライブラリを使用する部分を除く ASP.NET Web Pages 3 のでも動作します。


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>ソーシャル ネットワーク サイトに web サイトをリンク

ユーザーには、サイトで何かと同様に、多くの場合する友人と共有します。 この簡単な Digg、Reddit、Facebook、Twitter、または類似したサイトのページを共有するユーザーがクリックされるグリフ (アイコン) を表示することによってです。

これらのグリフを表示するには追加、`LinkSharecode`ページ ヘルパー。 ページの閲覧者は、個々 のグリフをクリックできます。 ソーシャル ネットワーク サイトにアカウントを持っている場合は、そのサイトでページへのリンクを投稿したことができますし、します。

![図 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)ください - 既に追加していない場合、という名前のページを作成する*ListLinkShare.cshtml*し、追加次のマークアップ。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    この例では、ときに、`LinkShare`ヘルパーを実行するページのタイトルは、さらに、ソーシャル ネットワーク サイトにページのタイトルを通過するパラメーターとして渡されます。 ただしで使用文字列を渡す可能性があります。 この例では、一覧に含める場合はどのソーシャル ネットワー キング サイトも指定します。 サイトに関連性のあるソーシャル ネットワー キング サイトを指定することができます。
2. 実行、 *ListLinkShare.cshtml*ブラウザーのページです。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。
3. サインインしているサイトのいずれかのグリフをクリックします。 リンクをクリックするページを共有することができます、ソーシャル ネットワークを選択したサイトにリンクします。 たとえば、Reddit リンクをクリックするとが表示される、 `submit to reddit` Reddit web サイトのページです。

     ![2 番目の画像](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>フィード、Twitter の追加

Twitter API の現在のバージョンと互換性がある Twitter ヘルパーの使用方法の詳細については、次を参照してください。[ヘルパーの Twitter](../ui-layouts-and-themes/twitter-helper.md)です。 この例では、多くのページからコードを簡単に再利用できるように、独自のヘルパーを記述する方法を示します。

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Facebook を表示する&quot;など&quot;ボタン

場合によっては、最善の対処方法でヘルパーに依存するのではなく、ソーシャル ネットワーク プロバイダーから直接コードを取得します。 これは、ソーシャル ネットワーク プロバイダーが、ヘルパーが更新されたよりも速く、オプションを更新する場合に特に当てはまります。

Facebook 機能 (など、Like のボタン) をサイトに追加するからのコード スニペットを取得できます、 [developers.facebook.com](https://developers.facebook.com/)サイトです。 Facebook サイトで、ツールを使用して、サイトに関連するコード スニペットを生成します。

次の強調表示されたコードは、developers.facebook.com サイトで、ボタンのようなツールから取得されたコードです。 独自のアプリ ID を指定する必要があります。

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Gravatar イメージを表示

A *Gravatar* (、&quot;グローバルに認識されているアバター&quot;) は、自分のアバターとして複数の web サイトで使用できるイメージ&#8212;するを表すイメージは、します。 たとえば、Gravatar は投稿では、フォーラム、ブログのコメントに個人を特定しにできます。 (Gravatar の web サイトで、独自の Gravatar を登録する[ http://www.gravatar.com/ ](http://www.gravatar.com/))。Web サイトに個人の名前や電子メール アドレスの横にあるイメージを表示する場合は、Gravatar ヘルパーを使用することができます。

この例では、自分自身を表す単一 Gravatar を使用しています。 Gravatar を使用する別の方法では、サイトに登録するときに、Gravatar アドレスを指定できるようにします。 (ユーザーが登録できるようにする方法を学習できます[追加のセキュリティと ASP.NET Web Pages サイトにメンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904))。そのユーザーの情報を表示するたびに、ユーザーの名前を表示する、Gravatar だけ追加できます。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しい web ページを作成する*Gravatar.cshtml*です。
3. ファイルに次のマークアップを追加します。 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml`メソッドでは、ページ上 Gravatar イメージを表示します。 イメージのサイズを変更するには、2 番目のパラメーターとして数値を含めることができます。 既定のサイズは 80 です。 番号が 80 未満の作成、イメージ小さくできます。 80 より大きい数値では、画像を拡大します。
4. `Gravatar.GetHtml`メソッド、置換`<Your Gravatar account here>`Gravatar アカウントを使用する電子メール アドレスを使用します。 (Gravatar アカウントを持っていない場合、使用できます人の電子メール アドレスです。)
5. お使いのブラウザーでページを実行します。 指定した電子メール アドレスの 2 つの Gravatar イメージを表示します。 2 番目のイメージは、最初より小さくなります。 

    ![図 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Xbox ゲーマー カードを表示します。

ユーザーは、Microsoft Xbox オンライン ゲームを再生するときに、各ユーザーが一意の id。 統計は、各プレーヤー、評価、ゲーマーのスコアが表示され、ゲームを最近再生したゲーム、カードなどの形式で保存されます。 ゲーマー Xbox でない場合に表示できる、ゲーマー カード サイト内のページを使用して、`GamerCard`ヘルパー。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*XboxGamer.cshtml*し、次のマークアップを追加します。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    使用する、`GamerCard.GetHtml`プロパティを表示するゲーマー カードに対して別名を指定します。
3. お使いのブラウザーでページを実行します。 ページには、指定した Xbox ゲーマー カードが表示されます。

    ![図 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
