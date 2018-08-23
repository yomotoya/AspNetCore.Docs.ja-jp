---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: サイトの CAPTCHA を使用して、ボットが、ASP.NET Web Razor を使用するを防ぐために) |Microsoft Docs
author: microsoft
description: 自動化されたプログラム (ボット) がタスクで、ASP.NET Web Pages (Razor) を実行することを防ぐために、ReCaptcha (セキュリティ メジャー) を使用する方法について説明します.
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: dc014f42490327743764787d58c613b7caa89f1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827384"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>サイトの CAPTCHA を使用して、ボットが、ASP.NET Web Razor を使用するを防ぐために)
====================
によって[Microsoft](https://github.com/microsoft)

> この記事では、ReCaptcha (セキュリティ メジャー) を使用して、自動プログラム (ボット) が ASP.NET Web Pages (Razor) の web サイトにタスクを実行することを防止する方法について説明します。
> 
> **学習内容。** 
> 
> - サイトに CAPTCHA テストを追加する方法。
> 
> この記事で導入された ASP.NET 機能を次に示します。
> 
> - `ReCaptcha`ヘルパー。
> 
> > [!NOTE]
> > この記事の情報は、ASP.NET Web Pages 1.0 と Web ページ 2 に適用されます。


## <a name="about-captchas"></a>な Captcha について

サイトで、または単にユーザーの登録ができるように、いつは、名前と URL (ブログのコメントのように) を入力してください。、偽名を大量に取得する可能性があります。 Url のままにすべての web サイトを検索できることにしようとする自動化されたプログラム (ボット) によってこれらのままには多くの場合。 (一般的な動機は、製品を販売目的の Url を投稿するです)。

ユーザーが実際のユーザーとコンピューター プログラムではなくを使用しているかどうかを確認できます、 *CAPTCHA*登録またはそれ以外の場合、名前とサイトを入力したときにユーザーを検証します。 CAPTCHA のコンピューターと人間離れているかを知らせるパブリック チューリングを完全に自動テストの略です。 CAPTCHA が、*チャレンジ/レスポンス*は簡単に行う人が実行するプログラムの自動ハード テストが、ユーザーは何かを求められます。 CAPTCHA の最も一般的な種類は、歪んで表示される一部の文字を参照してください、ユーザーが入力を求められます。 (ゆがみは、ボットの文字を解読するが困難と想定されます)。

## <a name="adding-a-recaptcha-test"></a>ReCaptcha テストを追加します。

使用することができます、ASP.NET ページで、 `ReCaptcha` ReCaptcha サービスに基づいている CAPTCHA テストを表示するためにヘルパー ([http://recaptcha.net](http://recaptcha.net))。 `ReCaptcha`ヘルパーはユーザーがページを検証する前に、正しく入力する必要がある歪んで表示される単語が 2 つのイメージを表示します。 ユーザーの応答は、recaptcha. Net サービスによって検証されます。

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Web サイトには、recaptcha. Net の登録 ([http://recaptcha.net](http://recaptcha.net))。 登録が完了したら、公開キーと秘密キーが表示されます。
2. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
3. まだ持っていない場合、  *\_AppStart.cshtml*という名前のファイルを作成、web サイトのルート フォルダーにファイルを *\_AppStart.cshtml*します。
4. 次の追加`Recaptcha`ヘルパーの設定、  *\_AppStart.cshtml*ファイル。 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. 設定、`PublicKey`と`PrivateKey`プロパティは、独自のパブリックおよびプライベート キーを使用します。
6. 保存、  *\_AppStart.cshtml*ファイルして閉じます。
7. Web サイトのルート フォルダーに作成するという名前の新しいページ*Recaptcha.cshtml*します。
8. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. 実行、 *Recaptcha.cshtml*ブラウザーでページ。 場合、`PrivateKey`値が有効では、ページには、ReCaptcha コントロールとボタンが表示されます。 内でグローバルにキーを設定する必要があるされていない場合 *\_AppStart.html*ページにエラーが表示されます。 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. テストの単語を入力します。 ReCaptcha テストを通過する場合、それに対応するメッセージを参照してください。 それ以外の場合、エラー メッセージを表示し、ReCaptcha コントロールが再表示されます。

> [!NOTE]
> 構成する必要がありますが、コンピューターがプロキシ サーバーを使用しているドメインにある場合、`defaultproxy`の要素、 *Web.config*ファイル。 次の例は、 *Web.config*ファイルと、 `defaultproxy` ReCaptcha サービスを動作を有効にするように構成要素。
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [ASP.NET Web Pages サイトのサイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha サイト](https://www.google.com/recaptcha)
