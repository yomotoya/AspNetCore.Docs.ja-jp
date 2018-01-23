# <a name="cookie-sharing-sample-app"></a>Cookie の共有のサンプル アプリ

このサンプルは、cookie の cookie 認証を使用する 3 つのアプリ間で共有を示しています。

| プロジェクト                             | 説明 |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core の Id を使用せず、ASP.NET 2.0 Razor ページの中核となるアプリ |
| CookieAuthWithIdentity.Core         | ASP.NET Core Id を持つ ASP.NET Core 2.0 MVC アプリ |
| CookieAuthWithIdentity.NETFramework | ASP.NET Identity Framework 4.6.1 の ASP.NET MVC アプリ |

方法:

1. CookieAuth.Core アプリを実行します。 ユーザーを登録します。 アプリは、ユーザーが登録されているときにユーザーを認証します。 ユーザーをサインアウトします。
1. 同じブラウザー セッションで CookieAuthWithIdentity.Core アプリを実行します。 コア アプリで使用されると同じユーザーを登録します。 アプリは、ユーザーが登録されているときにユーザーを認証します。 ユーザーをサインアウトします。
1. 同じブラウザー セッションで CookieAuthWithIdentity.NETFramework アプリを実行します。 他のアプリで使用されると同じユーザーを登録します。 アプリは、ユーザーが登録されているときにユーザーを認証します。 ユーザーをサインアウトします。
1. 次の 3 つのアプリのいずれかをユーザーにサインインします。 認証 cookie は、アプリ間で共有されます。 ユーザーは自動的にその他の 2 つのアプリに署名することに注意してください。
1. アプリのいずれかからユーザーをサインアウトします。 ユーザーが他の 2 つのアプリから自動的にサインアウトすることに注意してください。

このサンプルで説明する機能、[アプリ間で cookie を共有](https://docs.microsoft.com/aspnet/core/security/data-protection/compatibility/cookie-sharing)トピックです。
