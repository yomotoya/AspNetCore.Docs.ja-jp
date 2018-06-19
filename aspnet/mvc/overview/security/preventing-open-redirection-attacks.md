---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: 開いているリダイレクト攻撃 (c#) |Microsoft ドキュメント
author: jongalloway
description: このチュートリアルでは、ASP.NET MVC アプリケーションで開いているリダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、加えられた変更について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: ec1cd1791eb6d32e7c1ea50bc6626929cad2960e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879674"
---
<a name="preventing-open-redirection-attacks-c"></a>開いているリダイレクト攻撃 (c#)
====================
によって[Jon Galloway](https://github.com/jongalloway)

> このチュートリアルでは、ASP.NET MVC アプリケーションで開いているリダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、ASP.NET MVC 3 で AccountController で加えられた変更について説明し、既存の ASP.NET MVC 1.0 と 2 のアプリケーションでこれらの変更を適用する方法を示しています。


## <a name="what-is-an-open-redirection-attack"></a>開いているリダイレクト攻撃とは何ですか。

クエリ文字列またはフォームのデータなどの要求で指定されている URL にリダイレクトする任意の web アプリケーション可能性のある改ざんされるおそれがユーザーをリダイレクトする、悪意のある外部の url。 このによって改ざんされると、開いているリダイレクト攻撃が呼び出されます。

アプリケーション ロジックは、指定された URL へリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。 ASP.NET MVC 1.0 と ASP.NET MVC 2 の両方の既定 AccountController で使用するログインを開くにはリダイレクト攻撃に対して脆弱です。 しかし、ASP.NET MVC 3 Preview からの修正を使用する既存のアプリケーションを更新する簡単です。

この脆弱性を理解するのには、ログインのリダイレクトが、既定の ASP.NET MVC 2 Web アプリケーション プロジェクトでどのように動作するかを見てみましょう。 このアプリケーションで [Authorize] 属性を持つコント ローラーのアクションにアクセスしようとしています。 リダイレクト承認されていないユーザー/Account/LogOn ビューにします。 このリダイレクト/Account/LogOn に、ログインに成功した後、ユーザーが最初に要求された URL に返されるようにする returnUrl querystring パラメーターが含まれます。

次のスクリーン ショット ログオンしていないとき、/Account/ChangePassword ビューにアクセスしようとすると、結果をするリダイレクト/Account/LogOn が確認できますか。ReturnUrl % 2faccount% 2fChangePassword %2f を = です。

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**図 01**: 開いているリダイレクトとログイン ページ

ReturnUrl querystring パラメーターが検証されていないため、攻撃者は、開いているリダイレクト攻撃を実行するためにパラメーターに任意の URL アドレスを挿入することを変更できます。 これを示すためには、ReturnUrl パラメーターを変更できる[ http://bing.com](http://bing.com)ので、結果として得られるログイン URL になります/アカウント/ログオンですか?ReturnUrl =<http://www.bing.com/>です。 ログインすると、サイトに、私たちにリダイレクト[ http://bing.com](http://bing.com)です。このリダイレクトが検証されていないため代わりに、ユーザーを騙そうしようとする悪意のあるサイトを指している可能性があります。

### <a name="a-more-complex-open-redirection-attack"></a>複雑なオープン リダイレクト攻撃

開いているリダイレクト攻撃は、攻撃者がに対する脆弱性と、お客様を特定の web サイトにログインしようとしていることを認識しているために、特に危険な[フィッシング攻撃](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)です。 たとえば、攻撃者で、自分のパスワードをキャプチャするために、web サイトのユーザーに悪意のある電子メールを送信でしたできます。 これを NerdDinner サイトでは機能するしくみを見てみましょう。 (開いているリダイレクト攻撃を防ぐために、ライブ NerdDinner サイトが更新されたことに注意してください)。

最初に、攻撃者は、送信 us リンク NerdDinner が偽造ページへのリダイレクトを含む、ログイン ページにします。

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

戻り先 URL が word dinner からは"n"のない nerddiner.com を指していることを注意してください。 この例では、これは、攻撃者を制御するドメインです。 上記のリンクにアクセスするときに、正当な NerdDinner.com ログイン ページが表示されるおです。

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**図 02**: 開いているリダイレクトと NerdDinner ログイン ページ

ログイン時に正しく、ASP.NET MVC AccountController のログオン操作は私たちを returnUrl querystring パラメーターで指定された URL にリダイレクトします。 この場合、これは、攻撃者が入った URL は[ http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)です。 私たちは、攻撃者ことを確認するように注意されているため特にお気付きませんこれには、多くの場合は、非常に高く万一しない限り、偽造ページは、正当なログイン ページと同じように検索します。 このログイン ページには、もう一度ログインして要求するエラー メッセージが含まれています。 扱いにくい、パスワードを間違ってお必要があります。

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**図 03**: 偽造 NerdDinner ログイン画面

ユーザー名とパスワードを再入力してと偽造ログイン ページ情報を保存することを正当な NerdDinner.com サイトに送信します。 この時点では、NerdDinner.com サイトが既に認証 us, ため、そのページに直接偽造ログイン ページにリダイレクトできます。 最終的な結果は、攻撃者は、ユーザー名とパスワード、および、提供されていますがそれらに対応していないことです。

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>AccountController ログオン アクションで脆弱なコードを見る

ASP.NET MVC 2 アプリケーションのログオン操作のコードは、以下に示します。 ログインに成功すると、コント ローラーに返されるリダイレクト returnUrl に注意してください。 ReturnUrl パラメーターに対して検証を実行するないことを確認できます。

**1 – で ASP.NET MVC 2 のログオン操作を一覧表示します。 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

これで、ASP.NET MVC 3 のログオン操作への変更を見てみましょう。 このコードがという名前の System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって returnUrl パラメーターの検証に変更された`IsLocalUrl()`です。

**2 – で ASP.NET MVC 3 のログオン操作を一覧表示します。 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

これに変わりました System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって、戻り値 URL パラメーターを検証する`IsLocalUrl()`です。

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>ASP.NET MVC 1.0 と MVC 2 を保護するアプリケーション

IsLocalUrl() ヘルパー メソッドを追加し、returnUrl パラメーターを検証するログオン操作を更新して、既存の ASP.NET MVC 1.0 と 2 のアプリケーションで ASP.NET MVC 3 の変更の利用できます。

この検証として、System.Web.WebPages 内のメソッドを呼び出す実際には、UrlHelper IsLocalUrl() メソッドは、ASP.NET Web Pages アプリケーションによっても使用されます。

**3 – ASP.NET MVC 3 UrlHelper から IsLocalUrl() メソッドを一覧表示します。 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost メソッドには、リスト 4 に示すように、実際の検証ロジックが含まれています。

**4 – System.Web.WebPages RequestExtensions クラスから IsUrlLocalToHost() メソッドを一覧表示します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

ASP.NET MVC 1.0 または 2 つのアプリケーションでは IsLocalUrl() メソッドを追加、AccountController が可能であれば別のヘルパー クラスに追加することをお勧めします。 2 つの小さな変更、AccountController 内に動作するように IsLocalUrl() の ASP.NET MVC 3 バージョンに実行されます。 最初に変更しますが、パブリック メソッドからプライベート メソッド コント ローラー内のパブリック メソッドは、コント ローラーのアクションとしてアクセスできるためです。 次に、アプリケーションのホストに対して URL のホストを確認する呼び出しを変更します。 呼び出しがローカル RequestContext を使用すること、UrlHelper クラスのフィールドです。 代わりに、この設定を使用します。RequestContext.HttpContext.Request.Url.Host、これを使用します。Request.Url.Host です。 次のコードは、ASP.NET MVC 1.0 と 2 のアプリケーションで、コント ローラー クラスで使用するため、変更された IsLocalUrl() メソッドを示します。

**5 – IsLocalUrl() メソッドを一覧表示するユーザーが変更、MVC コント ローラー クラスで使用します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

IsLocalUrl() メソッドは、配置では、これでお呼び出すことができます、returnUrl パラメーターを検証する、ログオン アクションから次のコードに示すようにします。

**6 – returnUrl パラメーターを検証する方法で更新されたログオンを一覧表示します。**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

外部戻り先 URL を使用してログインしようとして、開いているリダイレクト攻撃をテストできます。 /Account/LogOn を利用してみましょう。ReturnUrl =<http://www.bing.com/>もう一度です。

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**図 04**: 更新済みのログオン操作のテスト

ログインすると、後に、外部 URL ではなく Home/index コント ローラーのアクションにリダイレクトされます。

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**図 05**: 開いているリダイレクト攻撃を阻止

## <a name="summary"></a>まとめ

リダイレクト Url は、アプリケーションの URL をパラメーターとして渡される、開いているリダイレクト攻撃が発生します。 テンプレートを防ぐためにコードが含まれています。 ASP.NET MVC 3 では、リダイレクト攻撃を開きます。 ASP.NET MVC 1.0 と 2 つのアプリケーションをいくつかの変更でこのコードを追加することができます。 ASP.NET 1.0 と 2 つのアプリケーションにログインするときにを開いているリダイレクト攻撃を防ぐためするには、IsLocalUrl() メソッドを追加し、ログオン アクションで returnUrl パラメーターを検証します。
