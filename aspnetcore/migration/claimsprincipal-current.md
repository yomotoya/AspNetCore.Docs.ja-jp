---
title: ClaimsPrincipal.Current からの移行します。
author: mjrousos
description: 現在の認証済みユーザーの id と ASP.NET Core でのクレームを取得する ClaimsPrincipal.Current から移行する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 35c3389798041e141c45bf0a76fa9d7285212768
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828122"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>ClaimsPrincipal.Current からの移行します。

ASP.NET 4.x プロジェクトでは使用する一般的な[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)現在を取得するユーザーの id と要求を認証します。 ASP.NET Core では、このプロパティは設定不要になった。 それに依存していたコードは、さまざまな手段を通じて現在の認証済みユーザーの id を取得するように更新する必要があります。

## <a name="context-specific-data-instead-of-static-data"></a>静的データではなく、コンテキスト固有のデータ

ASP.NET Core では、両方の値を使用する場合`ClaimsPrincipal.Current`と`Thread.CurrentPrincipal`が設定されていません。 これらのプロパティは両方を表す静的状態は、ASP.NET Core は一般的に回避できます。 ASP.NET Core のアーキテクチャは、コンテキスト固有のサービスのコレクションから (現在のユーザーの id) などの依存関係を取得する代わりに、(を使用してその[依存関係の注入](xref:fundamentals/dependency-injection)(DI) モデル)。 さらに、`Thread.CurrentPrincipal`はいくつかの非同期のシナリオでの変更を無効になること可能性がありますので、スレッドの静的な (と`ClaimsPrincipal.Current`呼び出すだけです`Thread.CurrentPrincipal`既定で)。

問題のスレッドの種類を理解するには、静的メンバーを非同期では、次のコード スニペットを検討してください。 発生する可能性します。

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

上記のサンプル コード セット`Thread.CurrentPrincipal`前に、と後の非同期呼び出しを待機しています。 その値を確認します。 `Thread.CurrentPrincipal` 固有では、*スレッド*を設定すると、およびメソッドが await の後に別のスレッドで実行を再開する可能性があります。 その結果、`Thread.CurrentPrincipal`が最初にチェックが null で呼び出しの後に、ある`await Task.Yield()`します。

アプリの DI サービスのコレクションから現在のユーザーの id の取得がしやすい、すぎます、テスト ユーザーを簡単に挿入されるためです。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>ASP.NET Core アプリの現在のユーザーを取得します。

現在の認証済みユーザーを取得するためのいくつかのオプションがある`ClaimsPrincipal`の代わりに ASP.NET Core で`ClaimsPrincipal.Current`:

* **ControllerBase.User**します。 MVC コント ローラーを現在の認証済みユーザーにアクセスできる、[ユーザー](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)プロパティ。
* **HttpContext.User**します。 現在のアクセス権を持つコンポーネント`HttpContext`(ミドルウェアの例) は、現在のユーザーを取得できます`ClaimsPrincipal`から[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)します。
* **呼び出し元から渡された**します。 現在のアクセス権を持たないライブラリ`HttpContext`コント ローラーまたはミドルウェア コンポーネントから多くの場合、呼び出され、引数として渡された現在のユーザーの id を持つことができます。
* **IHttpContextAccessor**します。 ASP.NET Core に移行しているプロジェクトは、簡単に、現在のユーザーの id を渡すために必要なすべての場所には大きすぎる可能性があります。 このような場合、 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)回避策として使用できます。 `IHttpContextAccessor` 現在アクセスできる`HttpContext`(存在する場合。 ASP.NET Core の DI 駆動アーキテクチャで作業をまだ更新されていないコードで、現在のユーザーの id を取得する短期的なソリューションは次のようになります。

  * ように`IHttpContextAccessor`呼び出して DI コンテナーで使用できる[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)で`Startup.ConfigureServices`します。
  * インスタンスを取得`IHttpContextAccessor`スタートアップ中に静的変数に格納します。 インスタンスが行われたが、現在のユーザーが静的プロパティから取得することは以前のコードを使用できます。
  * 現在のユーザーの取得`ClaimsPrincipal`を使用して`HttpContextAccessor.HttpContext?.User`します。 このコードは、HTTP 要求のコンテキスト外で使用されている場合、`HttpContext`が null です。

最後のオプションを使用して`IHttpContextAccessor`、ASP.NET Core の原則 (静的な依存関係に依存関係の挿入されたよりも優先されます) とは対照的です。 最終的に、静的な依存関係を削除する予定`IHttpContextAccessor`ヘルパー。 できます、ブリッジに便利ですが、以前を使用して既存の ASP.NET アプリが大きなを移行するときに`ClaimsPrincipal.Current`します。
