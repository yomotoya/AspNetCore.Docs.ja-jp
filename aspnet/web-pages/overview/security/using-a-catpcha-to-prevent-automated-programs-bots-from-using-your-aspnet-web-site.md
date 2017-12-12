---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "サイトのコンポーネントを使用できるように、ASP.NET Web Razor、CAPTCHA を使用して) |Microsoft ドキュメント"
author: microsoft
description: "ReCaptcha (セキュリティ メジャー) を使用して自動プログラム (bot) がタスクで、ASP.NET Web Pages (Razor) を実行することを防止する方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>サイトのコンポーネントを使用できるように、ASP.NET Web Razor、CAPTCHA を使用)
====================
によって[Microsoft](https://github.com/microsoft)

> この記事では、ReCaptcha (セキュリティ メジャー) を使用して自動プログラム (bot) が ASP.NET Web Pages (Razor) の web サイトにタスクを実行することを防止する方法について説明します。
> 
> **学習する内容。** 
> 
> - CAPTCHA テストをサイトに追加する方法。
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - `ReCaptcha`ヘルパー。
> 
> > [!NOTE]
> > この記事の情報は、ASP.NET Web Pages 1.0 および Web Pages 2 に適用されます。


## <a name="about-captchas"></a>な Captcha について

いつでも、サイトまたはのみでもユーザーに登録を許可するには、URL (ブログのコメントのような) と名前を入力してください、膨大な量の偽の名前を取得する可能性があります。 これらは多くの場合、確認することができるすべての web サイトで Url を保持しようとする自動プログラム (bot) で残されます。 (一般的な動機は販売製品の Url をポストします。)

必ず、ユーザーが実際のユーザーとコンピューター プログラムではなくを使用していただくこと、 *CAPTCHA*登録またはそれ以外の場合、名前とサイトを入力したときにユーザーを検証します。 CAPTCHA をパブリック チューリングを完全に自動化されたテスト コンピューターと人間間隔を表します。 CAPTCHA が、*チャレンジ/レスポンス*を促すメッセージが処理を行うには簡単に個人を実行するプログラムの自動化を行うにはハード テストします。 CAPTCHA の最も一般的な型はゆがんで文字を表示し、それらの入力を求められる配置します。 (ゆがみなって bot 文字を解読するが困難にします。)

## <a name="adding-a-recaptcha-test"></a>ReCaptcha テストを追加します。

ASP.NET ページで、使用することができます、 `ReCaptcha` 、ReCaptcha サービスに基づいている CAPTCHA テストを表示するためのヘルパー ([http://recaptcha.net](http://recaptcha.net))。 `ReCaptcha`ヘルパーはユーザーがページを検証する前に、正しく入力する必要がある 2 つの変形単語のイメージを表示します。 ユーザーの応答が ReCaptcha.Net サービスによって検証されます。

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. ReCaptcha.Net で web サイトを登録 ([http://recaptcha.net](http://recaptcha.net))。 登録が完了したら、公開キーと秘密キーが表示されます。
2. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
3. まだしていない場合、  *\_AppStart.cshtml*ファイル、という名前のファイルを作成、web サイトのルート フォルダーに *\_AppStart.cshtml*です。
4. 次の追加`Recaptcha`ヘルパーの設定、  *\_AppStart.cshtml*ファイル。 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. 設定、`PublicKey`と`PrivateKey`プロパティが、独自のパブリックおよびプライベート キーを使用します。
6. 保存、  *\_AppStart.cshtml*ファイルして、閉じます。
7. という名前の新しいページを作成、web サイトのルート フォルダーに*Recaptcha.cshtml*です。
8. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. 実行、 *Recaptcha.cshtml*ブラウザーのページです。 場合、`PrivateKey`値が有効では、ページは、ReCaptcha コントロールとボタンが表示されます。 キーをグローバルに設定した場合は *\_AppStart.html*ページにエラーが表示されます。 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. テストの単語を入力します。 ReCaptcha テストに合格する場合、メッセージにそれを参照してください。 それ以外の場合、エラー メッセージを表示し、ReCaptcha コントロールが再表示されます。

> [!NOTE]
> コンピューターがプロキシ サーバーを使用しているドメイン上にある場合は、構成する必要があります、`defaultproxy`の要素、 *Web.config*ファイル。 次の例は、 *Web.config*ファイルと、 `defaultproxy` ReCaptcha サービスを動作を有効にするように構成要素。
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [ASP.NET Web Pages サイトのサイト全体の動作のカスタマイズ](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha サイト](https://www.google.com/recaptcha)
