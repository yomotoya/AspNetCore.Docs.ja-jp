## <a name="multiple-authentication-providers"></a>複数の認証プロバイダー

アプリが複数のプロバイダーを必要とする場合、[AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication) の背後にあるプロバイダーの拡張メソッドをチェインします。

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
