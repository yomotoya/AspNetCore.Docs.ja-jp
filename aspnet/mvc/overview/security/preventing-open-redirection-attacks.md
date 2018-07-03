---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: オープン リダイレクト攻撃を防ぐ (c#) |Microsoft Docs
author: jongalloway
description: このチュートリアルでは、ASP.NET MVC アプリケーションでオープン リダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、加えられた変更について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 27921e23d38d34332b81fb85dcc795c8f9ff0352
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375471"
---
<a name="preventing-open-redirection-attacks-c"></a>オープン リダイレクト攻撃を防ぐ (c#)
====================
によって[Jon Galloway](https://github.com/jongalloway)

> このチュートリアルでは、ASP.NET MVC アプリケーションでオープン リダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、ASP.NET MVC 3 で AccountController で加えられた変更について説明し、既存の ASP.NET MVC 1.0 と 2 つのアプリケーションでこれらの変更を適用する方法を示します。


## <a name="what-is-an-open-redirection-attack"></a>オープン リダイレクト攻撃とは何ですか。

悪意のある、外部 URL にユーザーをリダイレクトする任意の web アプリケーションなど、クエリ文字列またはフォーム データ要求で指定されている URL にリダイレクトすることができます改ざんされる可能性があります。 この改ざんは、オープン リダイレクト攻撃と呼ばれます。

アプリケーション ロジックは、指定された URL にリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。 AccountController の既定の ASP.NET MVC 1.0 と ASP.NET MVC 2 の両方に使用されるログインは、開くリダイレクト攻撃に対して脆弱です。 さいわい、簡単に、修正の ASP.NET MVC 3 の Preview を使用する既存のアプリケーションを更新できますが。

この脆弱性を理解するのには、既定の ASP.NET MVC 2 Web アプリケーション プロジェクトでのログイン リダイレクトのしくみを見てみましょう。 このアプリケーションで [Authorize] 属性を持つコント ローラー アクションにアクセスしようとしてにリダイレクトされます承認されていないユーザー/Account/LogOn ビュー。 /Account/LogOn へのリダイレクトをこのユーザーが正常にログインした後、ユーザーが最初に要求された URL に返されるようにする returnUrl クエリ文字列パラメーターが含まれます。

次のスクリーン ショット ログインしていない場合は、/Account/ChangePassword ビューにアクセスしようとすると、/Account/LogOn にリダイレクトでに結果ことがわかりますか。ReturnUrl % 2faccount% 2fChangePassword %2f を = です。

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**図 01**: オープンのリダイレクトされたログイン ページ

ReturnUrl クエリ文字列パラメーターが検証されていないため、攻撃者は、オープン リダイレクト攻撃を実行するパラメーターに任意の URL アドレスを挿入することを変更できます。 これを示すためには、ReturnUrl のパラメーターを変更できます[ http://bing.com](http://bing.com)のため結果として得られるログイン URL になります/アカウント/ログオン、ですか?ReturnUrl =<http://www.bing.com/>します。 正常にサイトにログインをするとにリダイレクトされます。 [ http://bing.com](http://bing.com)します。 このリダイレクトが検証されていないため代わりに、ユーザーをだましてしようとする悪意のあるサイトを指している可能性があります。

### <a name="a-more-complex-open-redirection-attack"></a>複雑なオープン リダイレクト攻撃

オープン リダイレクト攻撃は、攻撃者に対する脆弱性のおかげが特定の web サイトにログインしようとしていることを知っているために、特に危険を[フィッシング攻撃](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)します。 たとえば、攻撃者は、自分のパスワードをキャプチャするために、web サイトのユーザーに悪意のある電子メールを送信でした。 NerdDinner サイトでどのように見てみましょう。 (オープン リダイレクト攻撃から保護するライブ NerdDinner サイトが更新されたことに注意してください)。

最初に、攻撃者リンクが送信私たちが偽造ページへのリダイレクトを含む NerdDinner にログイン ページ。

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

戻り先 URL が word dinner から nerddiner.com で、"n"がありませんを指すことに注意してください。 この例では、これは、攻撃者によって制御されるドメインです。 上記のリンクにアクセスするときに正当な NerdDinner.com ログイン ページに移動します。

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**図 02**: オープンのリダイレクトで NerdDinner ログイン ページ

誤ってログ、ASP.NET MVC AccountController のログオン操作は私たち returnUrl クエリ文字列パラメーターで指定された URL にリダイレクトします。 この場合、これは、攻撃者が入力すると、URL は[ http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)します。 私たちは、攻撃者が必ず確認してくださいされているため特に、これを気付かない可能性がありますが、非常に常時しない限り、偽造ページは、正当なログイン ページとまったく同じように検索します。 このログイン ページには、もう一度ログイン要求するエラー メッセージが含まれています。 少しぎこちない、パスワードをタイプミスしましたする必要があります。

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**図 03**: 偽造 NerdDinner ログイン画面

ユーザー名とパスワードを再入力し、偽造のログイン ページ情報を保存します正当な NerdDinner.com サイトにマイクロソフトへ送信されます。 この時点で NerdDinner.com サイトが既に認証されて、偽造のログイン ページがそのページに直接リダイレクトできるようにします。 最終的な結果は、攻撃者が、ユーザー名とパスワード、そのことも紹介それに対応していません。

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>AccountController ログオン アクションで脆弱なコードを見る

ASP.NET MVC 2 アプリケーションでのログオン操作のコードは、以下に示します。 ログインに成功すると、コント ローラーに返されるリダイレクト、returnUrl に注意してください。 ReturnUrl パラメーターに対して検証を実行しないことを確認できます。

**1 – で ASP.NET MVC 2 のログオン操作を一覧表示します。 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

これで、ASP.NET MVC 3 のログオン操作への変更内容を見てみましょう。 このコードがという名前の System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって returnUrl パラメーターの検証に変更された`IsLocalUrl()`します。

**2 – で ASP.NET MVC 3 のログオン操作を一覧表示します。 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

これが、System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出して、戻り値 URL パラメーターの検証に変更された`IsLocalUrl()`します。

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>ASP.NET MVC 1.0 と MVC 2 を保護するアプリケーション

ReturnUrl パラメーターを検証するログオン アクションの更新を IsLocalUrl() のヘルパー メソッドを追加して、既存の ASP.NET MVC 1.0 と 2 つのアプリケーションで ASP.NET MVC 3 の変更の利用できます。

この検証として、System.Web.WebPages 内のメソッドを呼び出す実際には、UrlHelper IsLocalUrl() メソッドは、ASP.NET Web Pages アプリケーションによっても使用されます。

**3 – ASP.NET MVC 3 の UrlHelper から IsLocalUrl() メソッドを一覧表示します。 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost メソッドには、リスト 4 に示すように、実際の検証ロジックが含まれています。

**4 – System.Web.WebPages RequestExtensions クラスから IsUrlLocalToHost() メソッドを一覧表示します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

ASP.NET MVC 1.0 または 2 つのアプリケーションで、AccountController に IsLocalUrl() メソッドを追加しますが、可能であれば別のヘルパー クラスに追加することをお勧めしていること。 AccountController 内で動作するように IsLocalUrl() の ASP.NET MVC 3 バージョンに 2 つの小さな変更が行ったされます。 最初に、変更しますが、パブリック メソッドから、プライベート メソッドをコント ローラー内のパブリック メソッドをコント ローラー アクションとしてアクセスできますので。 次に、私たちに対してでは、アプリケーション ホスト URL のホストを確認する呼び出しを変更します。 呼び出しは、ローカル RequestContext の使用、UrlHelper クラスのフィールド。 代わりに、これを使用します。RequestContext.HttpContext.Request.Url.Host、これを使用します。Request.Url.Host します。 次のコードでは、ASP.NET MVC 1.0 と 2 つのアプリケーションでコント ローラー クラスで使用するための変更の IsLocalUrl() メソッドを示します。

**この MVC コント ローラーのクラスを使用するため変更が 5 – IsLocalUrl() メソッドを一覧表示します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

IsLocalUrl() メソッドが、これで呼び出せる returnUrl パラメーターを検証する、ログオン アクションから次のコードに示すように。

**6 – returnUrl パラメーターを検証する更新されたログオン メソッドを一覧表示します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

外部の戻り先 URL を使用してログインしようとして、オープン リダイレクト攻撃をテストできます。 /Account/LogOn を使用してみましょうか。ReturnUrl =<http://www.bing.com/>もう一度です。

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**図 04**: 更新されたログオン操作をテストします。

正常にログインした後には、私たちは、外部 URL ではなく Home/index コント ローラー アクションにリダイレクトされます。

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**図 05**: 敗北オープン リダイレクト攻撃

## <a name="summary"></a>まとめ

オープン リダイレクト攻撃は、リダイレクト Url は、アプリケーションの URL をパラメーターとして渡されるときに発生します。 テンプレートにはから保護するためのコードが含まれています、ASP.NET MVC 3 では、リダイレクト攻撃を開きます。 ASP.NET MVC 1.0 と 2 つのアプリケーションを一部変更してこのコードを追加することができます。 オープン リダイレクト攻撃に対する保護のため ASP.NET 1.0 と 2 つのアプリケーションにログインするとき、IsLocalUrl() メソッドを追加し、ログオン中 returnUrl パラメーターを検証します。
