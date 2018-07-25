# <a name="cookie-sharing-sample-app"></a>Cookie の共有のサンプル アプリ

このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。

| プロジェクト                             | 説明 |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core Identity を使用せずに ASP.NET Core Razor ページ アプリ |
| CookieAuthWithIdentity.Core         | ASP.NET Core Identity を使用して ASP.NET Core MVC アプリ |
| CookieAuthWithIdentity.NETFramework | ASP.NET Identity での ASP.NET フレームワーク MVC アプリ |

方法:

1. CookieAuth.Core アプリを実行します。 ユーザーを登録します。 アプリは、ユーザーが登録されているときに、ユーザーを認証します。 ユーザーをサインアウトします。
1. 同じブラウザー セッションでは、CookieAuthWithIdentity.Core アプリを実行します。 Core アプリで使用したのと同じユーザーを登録します。 アプリは、ユーザーが登録されているときに、ユーザーを認証します。 ユーザーをサインアウトします。
1. 同じブラウザー セッションでは、CookieAuthWithIdentity.NETFramework アプリを実行します。 その他のアプリで使用したのと同じユーザーを登録します。 アプリは、ユーザーが登録されているときに、ユーザーを認証します。 ユーザーをサインアウトします。
1. 次の 3 つのアプリのいずれかをユーザーにサインインします。 認証 cookie は、アプリ間で共有されます。 ユーザーは自動的に他の 2 つのアプリに署名することに注意してください。
1. 任意のアプリからユーザーをサインアウトします。 その他の 2 つのアプリからのユーザーが自動的にサインアウトに注意してください。

このサンプルで説明する機能、[アプリ間での cookie の共有](https://docs.microsoft.com/aspnet/core/security/cookie-sharing)トピック。
