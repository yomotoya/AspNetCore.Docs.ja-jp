---
title: ClaimsPrincipal.Current からの移行します。
author: mjrousos
description: 現在の認証されたユーザーの id および ASP.NET Core 内のクレームを取得する ClaimsPrincipal.Current から離れた場所に移行する方法を説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 477f9fe8f0249bdfc528e1b401f072851f2f0d23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277187"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>ClaimsPrincipal.Current からの移行します。

ASP.NET プロジェクトでは使用する一般的な[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)ユーザーの id と要求を認証するには、現在を取得します。 ASP.NET Core では、このプロパティは設定不要になった。 コードは、別の手段を使用して、現在認証済みユーザーの id を取得するように更新する必要があります。

## <a name="context-specific-data-instead-of-static-data"></a>静的データではなく、コンテキスト固有のデータ

ASP.NET Core、両方の値を使用するときに`ClaimsPrincipal.Current`と`Thread.CurrentPrincipal`よう設定されていません。 これらのプロパティは両方を表す静的状態は、ASP.NET Core を一般的に回避できます。 ASP.NET Core のアーキテクチャは、コンテキスト固有のサービスのコレクションから (現在のユーザーの id) のような依存関係を取得する代わりに、(を使用してその[依存性の注入](xref:fundamentals/dependency-injection)(DI) モデル)。 さらに、`Thread.CurrentPrincipal`スレッドが静的では、一部の非同期のシナリオでの変更を保持可能性がありますされないように (および`ClaimsPrincipal.Current`呼び出すだけ`Thread.CurrentPrincipal`既定)。

問題のスレッドの種類を理解するには、静的メンバー発生する可能性が非同期のシナリオでは、次のコード スニペットを検討してください。

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

上記のサンプル コード セット`Thread.CurrentPrincipal`前に、と後の非同期呼び出しを待機してその値を確認します。 `Thread.CurrentPrincipal` 固有の*スレッド*が設定されているし、メソッドは await の後ろで別のスレッドで実行を再開する可能性があります。 その結果、`Thread.CurrentPrincipal`が最初にチェックが null で呼び出しの後に、ある`await Task.Yield()`です。

アプリの DI サービスのコレクションから現在のユーザーの id を取得がよりテストが容易なすぎます、テスト ユーザーが簡単に挿入されるためです。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>ASP.NET Core アプリケーションの現在のユーザーを取得します

現在の認証済みユーザーを取得するためのいくつかのオプション`ClaimsPrincipal`ASP.NET Core の代わりに`ClaimsPrincipal.Current`:

* **ControllerBase.User**です。 MVC コント ローラーで、現在の認証済みユーザーにアクセスできる、[ユーザー](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)プロパティです。
* **HttpContext.User**です。 現在のアクセス権を持つコンポーネント`HttpContext`(ミドルウェアの例) は、現在のユーザーを取得できます`ClaimsPrincipal`から[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)です。
* **呼び出し元から渡された**です。 現在のアクセス権のないライブラリ`HttpContext`コント ローラーまたはミドルウェア コンポーネントから多くの場合、呼び出され、引数として渡された現在のユーザーの id を持つことができます。
* **IHttpContextAccessor**です。 ASP.NET Core に移行して、ASP.NET プロジェクトは、すべての必要な場所に、現在のユーザーの id を簡単に渡すには大きすぎる可能性があります。 このような場合、 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)回避策として使用できます。 `IHttpContextAccessor` 現在アクセス可能な`HttpContext`(が存在する場合)。 短期的なソリューション内の ASP.NET Core DI ドリブン アーキテクチャと作業をまだ更新されていないコードの現在のユーザーの id を取得するのには次のようになります。

  * ください`IHttpContextAccessor`を呼び出して DI コンテナーで使用できる[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)で`Startup.ConfigureServices`です。
  * インスタンスを取得`IHttpContextAccessor`の起動時に静的変数に格納します。 インスタンスが行われますが、現在のユーザーが静的プロパティから取得することは以前のコードを使用できます。
  * 現在のユーザーの取得`ClaimsPrincipal`を使用して`HttpContextAccessor.HttpContext?.User`です。 このコードは、HTTP 要求のコンテキスト外で使用する場合、`HttpContext`が null です。

最終的なオプションを使用して`IHttpContextAccessor`は、(静的な依存関係を挿入された依存関係を使い続ける理由) ASP.NET Core の基本原則とは対照的です。 最終的に、静的な依存関係を削除しようとしています。`IHttpContextAccessor`ヘルパー。 できますに便利です、ブリッジは、以前使用していた既存の ASP.NET アプリケーションが大きなを移行するときに`ClaimsPrincipal.Current`です。
