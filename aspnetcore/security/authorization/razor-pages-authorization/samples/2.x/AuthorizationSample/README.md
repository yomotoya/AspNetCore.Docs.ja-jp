---
ms.openlocfilehash: 28553cff2804438119545a0021c238b2ca1182ce
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345416"
---
# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core の承認例

このサンプルでは、規則での Razor ページの承認の使用を示しています。 このサンプルで説明する機能、 [Razor ページの承認規則](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization)トピック。

このサンプルでは、ユーザーの承認機能の説明で、cookie 認証を使用して、 [ASP.NET Core Identity なしの cookie 認証を使用して](https://docs.microsoft.com/aspnet/core/security/authentication/cookie)トピック。 概念とこのトピックで示す例については、ASP.NET Core Identity を使用するアプリに等しく適用されます。 ASP.NET Core Identity を使用する方法の詳細については、[の ASP.NET core Identity の概要](https://docs.microsoft.com/aspnet/core/security/authentication/identity)を参照してください。

電子メール アドレスを使用して、 **maria.rodriguez@contoso.com**他のパスワードとユーザーを認証します。 における、ユーザーの認証、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。 実際の例では、ユーザーは、データベースに対して認証は。

## <a name="examples-in-this-sample"></a>このサンプルの例

| 機能 | 説明 |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | 追加、 [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)ページ、指定したパスにします。 |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | 追加、 [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスのフォルダー内のページのすべてにします。 |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | 追加、 [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)ページ、指定したパスにします。 |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | 追加、 [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスのフォルダー内のページのすべてにします。 |
