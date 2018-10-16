---
uid: mvc/mvc5
title: ASP.NET MVC 5 |Microsoft Docs
author: rick-anderson
description: 安定したデザイン パターンと AS. の電源を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークを ASP.NET MVC 5 の ASP.NET MVC 5 には.
ms.author: riande
ms.date: 10/11/2018
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: c958d39c7eff0d581de6b05890b8e6df8bdb5207
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348261"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5 の新機能新機能

### <a name="one-aspnet"></a>1 つの ASP.NET

Web MVC プロジェクト テンプレートは、1 つの ASP.NET のエクスペリエンスとシームレスに統合します。 MVC プロジェクトをカスタマイズし、1 つの ASP.NET プロジェクトの作成ウィザードを使用して認証を構成します。 ASP.NET MVC 5 の入門のチュートリアルをご覧[ASP.NET MVC 5 の概要](overview/getting-started/introduction/getting-started.md)します。

MVC 5、MVC 4 プロジェクトをアップグレードする方法については、次を参照してください。 [ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)します。

### <a name="aspnet-identity"></a>ASP.NET Identity

ASP.NET Identity を使用して認証と id 管理には、MVC プロジェクト テンプレートが更新されました。 Facebook や Google の認証と新しいメンバーシップ API を備えたチュートリアルをご覧[Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)と[で安全な ASP.NET MVC アプリのデプロイメンバーシップ、OAuth、および Windows Azure の Web サイトへの SQL Database](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。

### <a name="bootstrap"></a>ブートス トラップ

使用する MVC プロジェクト テンプレートが更新されました[ブートス トラップ](http://getbootstrap.com/)や応答性のスマートなルック アンド フィールを簡単にカスタマイズできるを提供します。 詳細については、次を参照してください。 [Visual Studio web プロジェクト テンプレートでのブートス トラップ](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)します。

### <a name="authentication-filters"></a>認証フィルター

[認証フィルター](http://www.dotnetcurry.com/showarticle.aspx?ID=957)新しい ASP.NET MVC パイプライン内の承認フィルターの前に実行し、認証ロジックごとのアクションを指定することは ASP.NET MVC でのフィルターの種類がコント ローラーごとまたはすべてのコント ローラーに対してグローバルにします。 認証フィルターは、要求に資格情報を処理し、対応するプリンシパルを指定します。 認証フィルターは、未承認の要求に応答認証チャレンジを追加することもできます。 参照してください[ASP.NET MVC 5 の認証フィルター](http://www.dotnetcurry.com/showarticle.aspx?ID=957)、 [ASP.NET MVC 5 での認証フィルター](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/)します。

### <a name="filter-overrides"></a>フィルターのオーバーライド

指定することによって、特定のアクション メソッドまたはコント ローラーに適用するフィルターをオーバーライドできるようになりました、[フィルターをオーバーライド](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)します。 オーバーライド フィルターは、特定のスコープ (アクションまたはコント ローラー) で実行するかのフィルターの種類のセットを指定します。 これにより、グローバルに適用されますが、特定のアクションまたはコント ローラーに適用することから特定のグローバル フィルターを除外するフィルターを構成することができます。 参照してください[ASP.NET MVC 5 と ASP.NET Web API 2 での機能の新しいフィルターによる上書き](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx)、 [ASP.NET MVC 5 のフィルターを上書き機能を使用する方法](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/)、および[フィルターは、ASP.NET MVC 5 よりも優先されます](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>属性ルーティング

ASP.NET MVC になりました[属性ルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)、Tim McCall の作成者によって投稿物に協力してくれた[ http://attributerouting.net](http://attributerouting.net)します。 属性ルーティングでは、アクションとコント ローラーに注釈を付けることによって、ルートを指定できます。

## <a name="new-web-project-experience"></a>新しい Web プロジェクト エクスペリエンス

Visual Studio には、Visual Studio 2013 以降で、新しい web プロジェクトの作成のエクスペリエンスが向上します。 **新しい ASP.NET Web プロジェクト**ダイアログ、テクノロジ (Web フォーム、MVC、Web API) の任意の組み合わせを構成、認証オプションの構成、Docker のサポートを追加して、単体テスト プロジェクトを追加、プロジェクトの種類を選択することができます。

![新しい ASP.NET プロジェクト](mvc5/_static/new-aspnet-web-app-dialog.png)

ダイアログ ボックスでは、テンプレートの多くの既定の認証オプションを変更することができます。 たとえば、ASP.NET Web フォーム プロジェクトを作成するときに、次のオプションのいずれかを選択できます。

- 認証なし
- 個々 のユーザー アカウント (ASP.NET メンバーシップまたはソーシャル プロバイダー ログ)
- 職場または学校アカウント (インターネット アプリケーションでは Active Directory)
- Windows 認証 (イントラネット アプリケーションで Active Directory)

![認証オプション](mvc5/_static/change-authentication-dialog.png)

Web プロジェクトを作成するためのプロセスの詳細については、次を参照してください。 [Visual Studio で ASP.NET Web プロジェクトの作成](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)です。 認証オプションの詳細については、次を参照してください。 [ASP.NET Identity](../identity/overview/index.md)します。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET のスキャフォールディング

ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。 これにより、簡単にデータ モデルを操作するプロジェクトに定型コードを追加できます。

2013 より前のバージョンの Visual Studio では、スキャフォールディングは、ASP.NET MVC プロジェクトに制限されていました。 Visual Studio 2013 以降、Web フォームを含む、任意の ASP.NET プロジェクトのスキャフォールディングを使用できます。 Visual Studio はサポートされていないページを生成する Web フォーム プロジェクトの場合は、MVC 依存関係をプロジェクトに追加することで、Web フォームでスキャフォールディングを使用することもできます。 Web フォームのページを生成するためのサポートは、将来のバージョンで追加されます。

スキャフォールディングを使用する場合、プロジェクトの依存関係がインストールされているすべての必須。 たとえば、ASP.NET Web フォーム プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な NuGet パッケージと参照をプロジェクトに自動的に追加されます。

Web フォーム プロジェクトには、MVC のスキャフォールディングを追加するには、追加、**スキャフォールディングされた新しい項目**選択**MVC 5 依存関係**ダイアログ ウィンドウでします。 MVC; をスキャフォールディングするための 2 つのオプションがあります。**最小の依存関係**と**依存関係を完全な**します。 選択した場合**最小の依存関係**、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加します。 選択した場合**完全な依存関係**、MVC プロジェクトの場合は、必要なコンテンツ ファイルと最小の依存関係が追加されます。

![Visual Studio でスキャフォールディング ダイアログ ボックスを追加します。](overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

非同期コント ローラーのスキャフォールディングのサポートは、Entity Framework 6 から非同期機能を使用します。

詳細とチュートリアルについては、次を参照してください。 [ASP.NET スキャフォールディング概要](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)します。

### <a name="get-help-and-report-issues"></a>ヘルプを表示し、問題の報告

- [既知の問題と重大な変更一覧](../visual-studio/overview/2013/release-notes.md#knownissues)
- ヘルプを取得しでの ASP.NET MVC 5 の説明、[フォーラム](https://forums.asp.net/1146.aspx)
- [ASP.NET MVC 5 のバグを報告します。](https://github.com/aspnet/AspNetWebStack/issues)
- [機能要求を作成します。](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrade-from-aspnet-mvc-4"></a>ASP.NET MVC 4 からのアップグレード

参照してください[ASP.NET MVC 4 にアップグレードしてから Web API プロジェクトを ASP.NET MVC 5 と Web API 2 の方法](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
