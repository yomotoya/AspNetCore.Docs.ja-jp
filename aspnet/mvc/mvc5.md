---
uid: mvc/mvc5
title: "ASP.NET MVC 5 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC 5 ASP.NET MVC 5 は、安定したデザイン パターンと AS. の電源を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークは."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5 の新機能

### <a name="one-aspnet"></a>1 つの ASP.NET

Web の MVC プロジェクト テンプレートは、新しい 1 つの ASP.NET エクスペリエンスとシームレスに統合します。 MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成することができます。 ASP.NET MVC 5 に入門チュートリアルはあります[ASP.NET MVC 5 の概要](overview/getting-started/introduction/getting-started.md)です。

MVC 5 に MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)です。

### <a name="aspnet-identity"></a>ASP.NET Identity

ASP.NET の Id を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。 提供され、Facebook、Google の認証と新しいメンバーシップ API のチュートリアルが見つかります[Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成する](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[を使用した ASP.NET MVC のセキュリティで保護されたアプリの展開メンバーシップ、OAuth、および Windows Azure Web サイトの SQL データベース](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。

### <a name="bootstrap"></a>ブートス トラップ

使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)光沢のあるや応答性のルック アンド フィールを簡単にカスタマイズすることができますを提供します。 詳細については、次を参照してください。 [Visual Studio 2013 の web プロジェクト テンプレートでブートス トラップ](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)です。

### <a name="authentication-filters"></a>認証フィルター

[認証フィルター](http://www.dotnetcurry.com/showarticle.aspx?ID=957)新しい ASP.NET MVC パイプラインでの承認フィルターの前に実行し、認証ロジックごとのアクションを指定することを ASP.NET MVC でのフィルターの種類は、コント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを提供します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。 参照してください[ASP.NET MVC 5 の認証フィルター](http://www.dotnetcurry.com/showarticle.aspx?ID=957)、 [ASP.NET MVC 5 の認証フィルター](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/)と[最後に、新しい ASP.NET MVC 5 認証フィルター!](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/)です。

### <a name="filter-overrides"></a>上書きをフィルター処理します。

指定して特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドすることが今すぐ、[フィルターをオーバーライド](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)です。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行しないでフィルターの種類のセットを指定します。 これにより、グローバルに適用が、特定のアクションまたはコント ローラーに適用されない特定のグローバル フィルターを除外するフィルターを構成することができます。 参照してください[ASP.NET MVC 5 と ASP.NET Web API 2 の機能の新しいフィルターをオーバーライド](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx)、[機能を使用して、ASP.NET MVC 5 フィルターをオーバーライドする方法](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/)、および[フィルターは、ASP.NET MVC 5 の上書き](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>属性のルーティング

ASP.NET MVC をサポート[属性がルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)、Tim McCall の作成者によって貢献感謝[http://attributerouting.net](http://attributerouting.net)です。 属性のルーティングは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。

## <a name="new-web-project-experience"></a>新しい Web プロジェクトのエクスペリエンス

Visual Studio 2013 で新しい web プロジェクトを作成する場合の動作を拡張しています。 **新しい ASP.NET Web プロジェクト**ダイアログ テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成する、認証オプションを構成し、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。

![新しい ASP.NET プロジェクト](mvc5/_static/image1.png)

新しいダイアログ ボックスでは、多くのテンプレートの既定の認証オプションを変更することができます。 たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。

- 認証なし
- 個々 のユーザー アカウント (ASP.NET メンバーシップまたはのソーシャル プロバイダー ログ)
- 組織アカウント (インターネット アプリケーションでは Active Directory)
- Windows 認証 (イントラネット アプリケーションの Active Directory)

![認証オプション](mvc5/_static/image2.png)

Web プロジェクトを作成するための新しいプロセスの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)です。 新しい認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](../identity/overview/index.md)です。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。 により、簡単にデータ モデルと連携するプロジェクトに定型コードを追加します。

Visual Studio の以前のバージョンでは、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 Visual Studio 2013 で Web フォームを含む、すべての ASP.NET プロジェクトのスキャフォールディングを使用できます。 Visual Studio 2013 はサポートされていません生成のページは、Web フォーム プロジェクトがプロジェクトに MVC 依存関係を追加することで、Web フォームのスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは、今後の更新で追加されます。

スキャフォールディングを使用する場合は、必要なすべての依存関係がプロジェクトにインストールされていることを確認します。 たとえば、ASP.NET Web フォーム プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

MVC のスキャフォールディングを Web フォーム プロジェクトに追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 の依存関係**ダイアログ ウィンドウでします。 MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。 最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。 [完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。

非同期コント ローラーをスキャフォールディングのサポートは、Entity Framework 6 から新しい非同期機能を使用します。

詳細な情報およびチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)です。

### <a name="getting-help-and-reporting-issues"></a>ヘルプを取得して、問題を報告

- [既知の問題と重大な変更の一覧](../visual-studio/overview/2013/release-notes.md#knownissues)
- ヘルプを取得し、ASP.NET MVC 5 の説明、[フォーラム](https://forums.asp.net/1146.aspx)
- [ASP.NET MVC 5 のバグを報告します。](https://github.com/aspnet/AspNetWebStack/issues)
- [機能の要求を行う](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>ASP.NET MVC 4 からのアップグレード

参照してください[ASP.NET MVC 4 にアップグレードし、Web API プロジェクトは、ASP.NET MVC 5 と Web API 2 にする方法](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
