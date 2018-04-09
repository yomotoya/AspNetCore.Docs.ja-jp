# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core 承認のサンプル

このサンプルでは、規則で Razor ページの認証の使用方法を示します。 このサンプルで説明する機能、 [Razor ページの承認規則](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization)トピックです。

このサンプルではユーザーの承認で説明されている機能の cookie 認証を使用して、 [cookie 認証を使用する ASP.NET Core Id せず](https://docs.microsoft.com/aspnet/core/security/authentication/cookie)トピックです。 ASP.NET Core Id の使用方法の詳細については、トピックを参照して、[認証](https://docs.microsoft.com/aspnet/core/security/authentication/index)ドキュメントのセクションでします。

サンプルを実行する場合は、電子メール アドレスを使用して**maria.rodriguez@contoso.com**ユーザーを認証します。

## <a name="examples-in-this-sample"></a>このサンプルの例

|                                                                              機能                                                                               |                                                                                        説明                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)          |                追加、 [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)ページ、指定したパスにします。                |
|        [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)        |      追加、 [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスのフォルダー内のページのすべてにします。      |
|   [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)   |            追加、 [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)ページ、指定したパスにします。            |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | 追加、 [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスのフォルダー内のページのすべてにします。 |

