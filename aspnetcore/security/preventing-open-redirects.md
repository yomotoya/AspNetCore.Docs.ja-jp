---
title: ASP.NET Core で開いているリダイレクト攻撃を防止します。
author: ardalis
description: ASP.NET Core アプリケーションに対して開くリダイレクト攻撃を防止する方法を示します
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 75591e37753c24bc959b3a96a54abebb51728364
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278299"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>ASP.NET Core で開いているリダイレクト攻撃を防止します。

クエリ文字列またはフォームのデータなどの要求で指定されている URL にリダイレクトする web アプリ可能性のある改ざんされるおそれがユーザーをリダイレクトする、悪意のある外部の url。 このによって改ざんされると、開いているリダイレクト攻撃が呼び出されます。

アプリケーション ロジックは、指定された URL へリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。 ASP.NET Core では、アプリを開いているリダイレクト (開いているとも呼ばれるリダイレクト) 攻撃から保護するための組み込みの機能を持ちます。

## <a name="what-is-an-open-redirect-attack"></a>開いているリダイレクト攻撃とは何ですか。

Web アプリケーションは、認証を必要とするリソースにアクセスするとき頻繁にユーザーとログイン ページにリダイレクトします。 リダイレクト typlically が含まれています、 `returnUrl` querystring パラメーターが正常にログインした後、ユーザーが最初に要求された URL に返されるようにします。 ユーザーが認証されると、ユーザーが最初に要求した URL にリダイレクトしているされます。

リンク先の URL が指定されて、要求のクエリ文字列の悪意のあるユーザーが、クエリ文字列を改ざん可能性があります。 改ざんされたクエリ文字列には、外部、悪意のあるサイトにユーザーをリダイレクトするサイトを許可できます。 この手法は、開いているリダイレクト (またはリダイレクト) 攻撃と呼ばれます。

### <a name="an-example-attack"></a>例攻撃

悪意のあるユーザーは、悪意のあるユーザー、ユーザーの資格情報やアプリで機密情報へのアクセスを許可するためのもので、攻撃を開発できます。 サイトのログイン ページへのリンクをクリックして、ユーザーを誘導する攻撃を開始する、 `returnUrl` querystring 値の URL に追加します。 たとえば、 [NerdDinner.com](http://nerddinner.com) (ASP.NET MVC の記述) サンプル アプリケーションには、このようなログイン ページは、ここが含まれています:`http://nerddinner.com/Account/LogOn?returnUrl=/Home/About`です。 攻撃には、これらの手順を実行し、次に示します。

1. ユーザーへのリンクをクリックした`http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`(に注意してください、2 番目の URL は nerddi**n**er、いない nerddi**nn**er)。
2. ユーザーが正常にログインします。
3. ユーザーがリダイレクト (サイト) によって`http://nerddiner.com/Account/LogOn`(実際のサイトのような悪意のあるサイト)。
4. ユーザーがもう一度ログイン (自分の資格情報をサイト悪意のあること) と実際のサイトにリダイレクトします。

ユーザーは可能性がありますと考えるログインに、最初の試行が失敗したし、その 2 つ目が成功しました。 対応していない可能性があります残ります自分の資格情報が侵害されていることができます。

![開いているリダイレクト攻撃プロセス](preventing-open-redirects/_static/open-redirection-attack-process.png)

ログイン ページだけでなく、一部のサイトはリダイレクト ページまたはエンドポイントを提供します。 アプリが開いているリダイレクトでは、含まれているページを想像`/Home/Redirect`です。 攻撃者を作成して、たとえばに移動する電子メールのリンク`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`です。 一般的なユーザーは、URL を見るし、サイト名で始まるを参照してください。 信頼する、リンクがクリックしてされます。 開いているリダイレクトは自分のものと同じでは、フィッシング詐欺サイトにユーザーに送信し、およびユーザーがそうなと考える内容へのログインは、サイトです。

## <a name="protecting-against-open-redirect-attacks"></a>開いているリダイレクト攻撃から保護します。

Web アプリケーションを開発する場合は、すべてのユーザーが入力したデータを信頼できないとして扱います。 アプリケーションに、URL の内容に基づく、ユーザーをリダイレクトする機能がある場合は、このようなリダイレクトはのみ行うことローカルで、アプリ内で (または、既知の url、クエリ文字列で指定することがありますいない任意の URL) を確認します。

### <a name="localredirect"></a>LocalRedirect

使用して、`LocalRedirect`ベースからのヘルパー メソッド`Controller`クラス。

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` 非ローカル URL が指定されている場合、例外がスローされます。 それ以外の場合、動作と同じように、`Redirect`メソッドです。

### <a name="islocalurl"></a>IsLocalUrl

使用して、 [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)をリダイレクトする前に Url をテストするメソッド。

次の例では、リダイレクトする前に、URL がローカルかどうかをチェックする方法を示します。

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

`IsLocalUrl`メソッドは、悪意のあるサイトにリダイレクトされない誤ってユーザーを保護します。 ローカルの URL を予期したような状況で非ローカル URL が指定されている場合に指定された URL の詳細を記録できます。 ログイン リダイレクト Url ではリダイレクト攻撃を診断する際に役立ちます。
