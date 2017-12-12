---
uid: mvc/mvc3
title: "ASP.NET MVC 3 |Microsoft ドキュメント"
author: rick-anderson
description: "(2011 年 4 月が含まれています Tools Update)ASP.NET MVC 3 では、安定したデザイン パターンを使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークがしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 1aa059e92b5637b9ba7ce488da4b44322dab6d8e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(2011 年 4 月が含まれています Tools Update)*
> 
> ASP.NET MVC 3 は、安定したデザイン パターンを活用し、ASP.NET と .NET Framework を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。
> 
> 今日を使用して作業を開始するために、ASP.NET MVC 2 がサイド バイ サイド インストールされます。
> 
> ダウンロード、[ここインストーラー](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>上位の特徴

- NuGet 経由で拡張可能な統合のスキャフォールディング システム
- HTML 5 の有効なプロジェクト テンプレート
- 新しい Razor ビュー エンジンを含む、表現力が豊かビュー
- 依存関係挿入とグローバル アクション フィルターで強力なフック関数
- 控えめな JavaScript、jQuery 検証、および JSON バインディング豊富な JavaScript のサポート
- *完全な機能の一覧を読み取る[以下](#overview)*

## <a name="top-links"></a>ページ上部のリンク

ASP.NET MVC 3 の新機能

- Phil Haack: [ASP.NET MVC 3 のリリース](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3、WebMatrix、NuGet、IIS Express、Orchard のリリースのコンテキストで、Microsoft 年 1 月の Web リリース](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Announcing ASP.NET MVC 3、IIS Express、SQL CE 4、Web Farm Framework、Orchard、WebMatrix のリリース](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

インストールとヘルプ

- ASP.NET MVC 3 を使用してインストール、 [Web Platform Installer が (推奨)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 を使用してインストール、[インストーラー実行可能ファイル](https://go.microsoft.com/fwlink/?LinkID=208140)
- インストール[ASP.NET MVC 3 Visual Studio 11 開発者プレビューの](https://go.microsoft.com/fwlink/?LinkID=208140)
- 読み取り、 [ASP.NET MVC 3 のチュートリアル入門](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- ヘルプを取得し、ASP.NET MVC 3 での説明、[フォーラム](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 の概要

ASP.NET MVC 3 は、コードを簡略化しより深い機能拡張を許可する両方の優れた機能を追加することを ASP.NET MVC 1 と 2 でビルドします。 このトピックでは、次のセクションに整理されている、このリリースに含まれている新機能の多くの概要を示します。

- [MvcScaffold 統合でスキャフォールディング拡張](#BM_MvcScaffolding)
- [HTML 5 の有効なプロジェクト テンプレート](#BM_HTML5)
- [Razor ビュー エンジン](#BM_TheRazorViewEngine)
- [複数のビュー エンジンのサポート](#BM_Support_for_Multiple_View_Engines)
- [コント ローラーの機能強化](#BM_Controller_Improvements)
- [JavaScript と Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [モデル検証の機能強化](#BM_Model_Validation_Improvements)
- [依存関係の挿入の機能強化](#BM_Dependency_Injection_Improvements)
- [他の新機能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>MvcScaffold 統合でスキャフォールディング拡張

新しいスキャフォールディング システムを取得し、生産性、framework 全体経験がない場合の使用を開始してをやすくした経験が場合は、一般的な開発タスクを自動化し、あなたが何を知っています。

これは新しい NuGet によってサポートされて*スキャフォールディング*というパッケージ**MvcScaffolding**です。 という用語は、「スキャフォールディング」多数のソフトウェア テクノロジによって「することができます、ソフトウェアの基本的な概要をすばやく生成し、編集およびカスタマイズ」を意味します。 作成している ASP.NET MVC のスキャフォールディング パッケージは、いくつかのシナリオで大幅に利点があります。

- **最初に ASP.NET MVC を学習している場合**をすばやくを編集して、必要に応じて調整することができますし、有益な稼働コードを取得する方法が提供されるため、します。 空白のページを見ると、まったくわからない開始位置を持つ trauma が省けます。
- **ASP.NET MVC をよく理解し、新しいアドオン テクノロジを探索するようになりましたかどうか**など、オブジェクト リレーショナル マッパー、ビュー エンジン、テストのライブラリなど、そのテクノロジの作成者は、可能性がありますもパッケージの作成、スキャフォールディングのためです。
- **繰り返しのようなクラスまたは何らかのファイルを作成するかどうか、作業にも行われます**出力テスト フィクスチャ、配置スクリプト、または必要とされる他のあらゆるカスタム スキャフォールダーを作成できるため、します。 すぎる、カスタム スキャフォールダーを使用して、チームの全員ことができます。

MvcScaffolding でその他の機能は次のとおりです。

- C# および VB プロジェクトのサポート
- ビュー エンジンの Razor と ASPX をサポートします。
- ASP.NET MVC の領域にスキャフォールディングとマスター/カスタム ビューのレイアウトを使用してサポート
- T4 テンプレートを編集することによって、出力を簡単にカスタマイズすることができます。
- PowerShell のカスタム ロジックとカスタム T4 テンプレートを使用して、まったく新しいスキャフォールダーを追加することができます。 これら (およびそれらを許可したすべてのカスタム パラメーター) コンソール タブ補完の一覧に自動的に表示されます。
- 追加スキャフォールダーのさまざまなテクノロジを含む NuGet パッケージを取得することができます (例: は、概念実証のいずれかの LINQ to SQL のようになりました) とを混在させるし、一緒に一致させる

ASP.NET MVC 3 Tools Update には、Visual Studio のこのスキャフォールディング システムの優れたサポートにはなどが含まれます。

- コント ローラー ダイアログでは、作成、読み取り、更新、および削除のコント ローラー アクションと対応するビューの完全な自動スキャフォールディングをサポートを追加します。 既定では、このスキャフォールディング EF Code First を使用してデータ アクセス コード。
- コント ローラー ダイアログのサポートを追加*拡張スキャフォールディング*NuGet 経由でなどのパッケージ*MvcScaffolding*です。 これを希望している場合、ODBCDirect と NHibernate なども JET の他のデータ アクセス テクノロジのスキャフォールディングを作成することができるようにダイアログ ボックスにカスタムのスキャフォールディングにプラグインすることができます。

ASP.NET MVC 3 でスキャフォールディングの詳細については、次のリソースを参照してください。

- Steve Sanderson を含む系列を通知します。 

    1. [概要: スキャフォールディング、ASP.NET MVC 3 プロジェクトと MvcScaffolding パッケージ](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [標準の使用状況: 一般的なユース ケースとオプション](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一対多のリレーションシップ](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [スキャフォールディング アクションと単体テスト](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [T4 テンプレートをオーバーライドします。](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [この投稿: カスタム スキャフォールダーの作成](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- PDC 2010 セッションからの Scott Hanselman 投稿[Microsoft「名前のないパッケージの Web いただける」とブログの構築](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 のリリース ノートします。](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 プロジェクト テンプレート

新しいプロジェクト ダイアログには、プロジェクト テンプレートのチェック ボックスを有効にする HTML 5 バージョンが含まれています。 これらのテンプレートは、ダウン レベルのブラウザーで HTML 5、CSS 3 の互換性サポートを提供する Modernizr 1.7 を活用します。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor ビュー エンジン

ASP.NET MVC 3 Razor で次の利点がありますをという名前の新しいビュー エンジンが付属します。

- Razor 構文では、クリーンと、簡潔なキーストロークの最小数を必要とします。
- Razor では、c# や Visual Basic などの既存の言語に基づいているため一部については、簡単です。
- Visual Studio には、Razor 構文の IntelliSense とコードの色付けが含まれています。
- Razor ビューは、単体テスト アプリケーションを実行するか、web サーバーの起動を必要とせずに指定できます。

Razor の新機能の一部を以下に示します。

- `@model`ビューに渡される型を指定する構文があります。
- `@* *@`コメント構文です。
- 既定値を指定する機能 (など`layoutpage`) サイト全体に対して 1 回です。
- `Html.Raw` HTML エンコードせずにテキストを表示するためのメソッドにします。
- 複数のビューの間でのコードを共有するためのサポート (*\_viewstart.cshtml*または *\_viewstart.vbhtml*ファイル)。

Razor には、次のような新しい HTML ヘルパーも含まれます。

- `Chart`。 ASP.NET 4 で、グラフ コントロールと同じ機能を提供する、グラフを表示します。
- `WebGrid`。 ページングや並べ替えの機能を備えたデータ グリッドを表示します。
- `Crypto`。 ハッシュを正しく作成するアルゴリズムを使用では、ソルトし、パスワードをハッシュします。
- `WebImage`。 イメージを表示します。
- `WebMail`。 電子メール メッセージを送信します。

Razor の詳細については、次のリソースを参照してください。

- [Scott Guthrie のブログの投稿 Razor の概要](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie のブログ投稿の概要、@modelキーワード](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie のブログ投稿 Razor レイアウトの概要](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API クイック リファレンス](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 のリリース ノートします。](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>複数のビュー エンジンのサポート

**ビューの追加**ASP.NET MVC 3 でのダイアログ ボックスで、使用するビュー エンジンを選択できます、**新しいプロジェクト** ダイアログ ボックスでは、プロジェクトの既定のビュー エンジンを指定することができます。 など、Web フォーム ビュー エンジン (ASPX)、Razor、またはオープン ソースのビュー エンジンを選択できます[Spark](http://sparkviewengine.com/)、 [NHaml](https://code.google.com/p/nhaml/)、または[NDjango](http://ndjango.org/)です。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>コント ローラーの機能強化

### <a name="global-action-filters"></a>グローバル アクション フィルター

アクション メソッドの実行前に、または後にアクション メソッドの実行ロジックを実行する場合があります。 そのためには、ASP.NET MVC 2 には、アクション フィルターが用意されています。 アクション フィルターは、特定のコント ローラー アクション メソッドにアクション前とアクション後の動作を追加する宣言型手段を提供するカスタム属性です。 ただし、場合によっては、すべてのアクション メソッドに適用される前のアクションまたはアクション後の動作を指定する必要があります。 MVC 3 に追加することでグローバル フィルターを指定することができます、`GlobalFilters`コレクション。 グローバル アクション フィルターに関する詳細については、次のリソースを参照してください。

- [MVC 3 preview Scott Guthrie のブログ](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC でのフィルタ リング](https://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新しい"ViewBag"プロパティ

MVC 2 コント ローラーのサポート、`ViewData`プロパティすると、遅延バインディングされたディクショナリ API を使用してビュー テンプレートにデータを渡すことができます。 MVC 3 では、多少単純構文も使用できます、`ViewBag`プロパティを同じ目的を達成します。 たとえば、作成する代わりに`ViewData["Message"]="text"`、書き込めること`ViewBag.Message="text"`です。 使用する、厳密に型指定されたクラスを定義する必要はありません、`ViewBag`プロパティです。 動的プロパティは、代わりを取得できますかプロパティを設定してが解決に動的に実行時にします。 内部的には、`ViewBag`プロパティは、名前と値のペアとして格納、`ViewData`ディクショナリ。 (注: MVC 3 のほとんどのプレリリース バージョンで、`ViewBag`プロパティの名前、`ViewModel`プロパティです)。

### <a name="new-actionresult-types"></a>新しい"ActionResult"の種類

次`ActionResult`型と対応するヘルパー メソッドは、新しい MVC 3 で強化されました。

- [HttpNotFoundResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)です。 クライアントに 404 HTTP ステータス コードを返します。
- [RedirectResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.redirectresult(v=VS.98).aspx)です。 一時的なリダイレクト (HTTP 302 状態コード) またはブール型パラメーターによって永続的なリダイレクト (301 の HTTP ステータス コード) を返します。 この変更により、組み合わせて、[コント ローラー](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller(v=VS.98).aspx)クラスが 3 つの永続的なリダイレクトを実行する方法: `RedirectPermanent`、 `RedirectToRoutePermanent`、および`RedirectToActionPermanent`です。 これらのメソッドのインスタンスを返す`RedirectResult`で、`Permanent`プロパティに設定`true`です。
- [HttpStatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)です。 ユーザー指定の HTTP ステータス コードを返します。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript と Ajax 機能強化

既定では、MVC 3 の Ajax と検証ヘルパーは、控えめな JavaScript 手法を使用します。 控えめな JavaScript では、HTML にインライン JavaScript を挿入することで回避できます。 これは、HTML 小さく、煩雑になりをスワップ アウトまたは JavaScript ライブラリをカスタマイズするが容易します。 MVC 3 の検証ヘルパーを使用しても、`jQueryValidate`既定ではプラグインです。 MVC 2 の動作を実行する場合に、控えめな JavaScript を使用して無効にすることができます、 *web.config*ファイルの設定。 JavaScript と Ajax 機能強化の詳細については、次のリソースを参照してください。

- [Wikipedia サイトでの控えめな JavaScript の基本的な概要](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson の控えめな JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson の控えめな JavaScript 検証 Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Razor および控えめな JavaScript と MVC 3 アプリケーションを作成する](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)(ASP.NET サイトのチュートリアル)
- [MVC 3 のリリース ノートします。](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>既定で有効になっているクライアント側の検証

MVC の以前のバージョンでは、明示的に呼び出す必要があります、`Html.EnableClientValidation`クライアント側の検証を有効にするためにビューからのメソッドです。 MVC 3 でこれが不要になった必要なクライアント側の検証が既定で有効にします。 (を無効にできますで設定を使用して、 *web.config*ファイルです)。

クライアント側の検証動作するためには、引き続き適切な jQuery、およびサイトの jQuery 検証ライブラリを参照する必要があります。 独自のサーバーでこれらのライブラリをホストしたり、Microsoft または Google から Cdn のようにコンテンツ配信ネットワーク (CDN) からそれらを参照できます。

### <a name="remote-validator"></a>リモート検証コントロール

ASP.NET MVC 3 をサポートしている新しい[RemoteAttribute](https://msdn.microsoft.com/en-us/library/system.web.mvc.remoteattribute(v=VS.98).aspx)活用するために、jQuery 検証プラグインできるようにするクラスのリモート検証のサポート。 これにより、自動的にメソッドを呼び出すカスタムできるのみ検証ロジックを実行するために、サーバー上で定義するサーバー側のクライアント側検証ライブラリです。

次の例で、`Remote`属性は、クライアントの検証がという名前のアクションを呼び出すことを指定`UserNameAvailable`上、`UsersController`を検証するためにクラス、`UserName`フィールドです。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

次の例は、対応するコント ローラーを示しています。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

使用する方法についての詳細、`Remote`属性は、「[する方法: ASP.NET MVC でのリモート検証の実装](https://msdn.microsoft.com/en-us/library/gg508808(VS.98).aspx)、MSDN ライブラリです。

### <a name="json-binding-support"></a>バインディングの JSON サポート

ASP.NET MVC 3 には、JSON でエンコードされたデータを受信し、モデル バインディングがアクション メソッド パラメーターにアクション メソッドを使用する組み込みの JSON バインディング サポートが含まれています。 この機能は、クライアントのテンプレートおよびデータ バインディングに関連するシナリオで役に立ちます。 (クライアント テンプレートを使用する書式設定し、クライアントで実行するテンプレートを使用して、単一のデータ項目または一連のデータ項目を表示することです。)MVC 3 では、JSON データの送受信をサーバー上のアクション メソッドとクライアント テンプレートを簡単に接続することができます。 バインディングの JSON サポートの詳細については、次を参照してください。、 **JavaScript と AJAX 機能強化**のセクション[Scott Guthrie の MVC 3 Preview ブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)です。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>モデル検証の機能強化

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations"メタデータの属性

ASP.NET MVC 3 をサポートしている`DataAnnotations`などのメタデータが属性`DisplayAttribute`です。

### <a name="validationattribute-class"></a>"ValidationAttribute"クラス

`ValidationAttribute`クラスが強化され、新しいサポートするために .NET Framework 4`IsValid`の詳細については、どのようなオブジェクトが検証されているなど、現在の検証コンテキストを提供するオーバー ロードします。 これにより、高度なシナリオが、モデルの別のプロパティに基づく現在の値を検証することができます。 たとえば、新しい`CompareAttribute`属性では、モデルの 2 つのプロパティの値を比較することができます。 次の例で、`ComparePassword`プロパティに一致する必要があります、`Password`有効にするのにはフィールドです。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>検証インターフェイス

[IValidatableObject](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)インターフェイスでは、モデル レベル検証を実行することができ、全体的なモデルまたはモデル内の 2 つのプロパティの間の状態に固有のエラー メッセージの検証を提供することができます. MVC 3 からのエラーを取得、`IValidatableObject`インターフェイスの影響を受ける組み込み HTML フォーム ヘルパーを使用してビュー内のフィールドと、モデル バインディングと自動的に、フラグまたは強調表示します。

[IClientValidatable](https://msdn.microsoft.com/en-us/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)インターフェイスにより、ASP.NET MVC 検証コントロールがあるクライアントの検証をサポートするかどうかを実行時に検出します。 このインターフェイスは、検証フレームワークのさまざまな統合できるように設計されています。

検証のインターフェイスの詳細については、次を参照してください。、**モデル検証の機能強化**のセクション[Scott Guthrie の MVC 3 Preview ブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)です。 (ただし、ブログで"IValidateObject"への参照は"IValidatableObject"である必要があります。)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>依存関係の挿入の機能強化

ASP.NET MVC 3 は、依存関係の挿入 (DI) を適用するためと依存関係の挿入またはコントロールの反転 (IOC) コンテナーと統合するためより適切なサポートを提供します。 DI のサポートは、次の領域に追加されました。

- コント ローラー (を登録して、コント ローラーを挿入することは、コント ローラー ファクトリを提供する)。
- ビュー (を登録して、ページの表示に依存関係を挿入するビュー エンジンを挿入すること)。
- (検索とフィルターを提供する) のアクション フィルター。
- モデル バインダー (登録および挿入すること)。
- モデル検証プロバイダー (登録および挿入) です。
- モデル メタデータ プロバイダー (登録および挿入) です。
- (登録および送り込んで) の値プロバイダー。

MVC 3 をサポートしている、[共通サービス ロケーター](https://github.com/unitycontainer/commonservicelocator)ライブラリとそのライブラリをサポートする任意の DI コンテナー`IServiceLocator`インターフェイスです。 サポートします。 新しい`IDependencyResolver`インターフェイス DI フレームワークに統合するのが容易です。

MVC 3 の DI の詳細については、次のリソースを参照してください。

- [サービスの場所のブログ投稿の Brad Wilson のシリーズ](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 のリリース ノートします。](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>他の新機能

### <a name="nuget-integration"></a>NuGet の統合

ASP.NET MVC 3 は自動的にインストールし、そのセットアップの一部として NuGet を有効にします。 NuGet は、簡単に検索、インストール、およびプロジェクトでの .NET ライブラリとツールを使用する無料のオープン ソース パッケージ マネージャーです。 (ASP.NET Web フォームと ASP.NET MVC を含む) すべての Visual Studio プロジェクトの種類で動作します。

NuGet では、それらのライブラリをパッケージ化し、それらをオンライン ギャラリーで登録するオープン ソース プロジェクト (たとえば、Moq、NHibernate、Ninject、StructureMap、NUnit、Windsor、RhinoMocks、Elmah など) を管理する開発者ができるようにします。 使用してこれらのライブラリのいずれかのパッケージを見つけるし、しているプロジェクトにインストールする .NET 開発者を簡単には、です。

ASP.NET の 3 Tools Update でプロジェクト テンプレートには、NuGet 経由で更新されるように JavaScript ライブラリ事前インストールされている NuGet パッケージが含まれます。 Entity Framework Code First はも事前インストールされている NuGet パッケージとして。

NuGet の詳細については、[NuGet のドキュメント](https://docs.microsoft.com/nuget/)を参照してください。

### <a name="partial-page-output-caching"></a>部分ページ出力キャッシュ

ASP.NET MVC は出力が 1 のバージョン以降のページ全体の応答のキャッシュをサポートされています。 MVC 3 もサポートしています部分ページ出力キャッシュを簡単にキャッシュ領域または応答の断片です。 キャッシュの詳細については、次を参照してください、**部分ページ出力キャッシュ**のセクション[MVC 3 のリリース候補に Scott Guthrie のブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)と**子アクションの出力キャッシュ。**のセクションで、 [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)です。

### <a name="granular-control-over-request-validation"></a>要求の検証をきめ細かく制御

ASP.NET MVC には、組み込みの要求の検証に自動的に保護するのに役立つ XSS や HTML インジェクション攻撃があります。 ただし、明示的に無効にする要求の検証する必要がある場合などする (たとえば、ブログ記事や CMS コンテンツ) 内のコンテンツの HTML を送信できるようにします。 追加できるように、 [AllowHtml](https://msdn.microsoft.com/en-us/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)モデルに属性またはプロパティごとにモデル バインド中に、要求の検証を無効にするモデルを表示します。 要求の検証の詳細については、次のリソースを参照してください。

- **控えめな JavaScript と検証**」の「 [MVC 3 のリリース候補に Scott Guthrie のブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)です。
- [MVC 3 のリリース ノートします。](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>拡張可能な""新しいプロジェクト ダイアログ ボックス

ASP.NET MVC 3 では、プロジェクト テンプレート、ビュー エンジンを追加することができ、単体テストをプロジェクトのフレームワーク、**新しいプロジェクト** ダイアログ ボックス。

### <a name="template-scaffolding-improvements"></a>テンプレートのスキャフォールディング機能強化

ASP.NET MVC 3 のスキャフォールディング テンプレートは、モデルの主キー プロパティを識別して、MVC の以前のバージョンでより適切に処理の向上ジョブを実行します。 (たとえば、スキャフォールディング テンプレートことを確認、主キーが編集可能なフォームのフィールドとしてスキャフォールディングされたいないこと。)

既定では、作成および編集のスキャフォールディングを今すぐ使用して、`Html.EditorFor`ヘルパーの代わりに、`Html.TextBoxFor`ヘルパー。 これにより、モデル データの形式でのメタデータのサポートが向上注釈属性の場合、**ビューの追加** ダイアログ ボックスは、ビューを生成します。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor"と"Html.LabelForModel"の新しいオーバー ロード

新しいメソッドのオーバー ロードが追加されて、`LabelFor`と`LabelForModel`ヘルパー メソッドです。 新しいオーバー ロードを使用すると、指定するか、ラベルのテキストをオーバーライドできます。

### <a name="sessionless-controller-support"></a>Sessionless コント ローラーのサポート

ASP.NET MVC 3 の場合は、セッション状態を使用するコント ローラー クラスを希望するかどうかを指定する、セッション状態するかどうか読み取り/書き込みまたは読み取り専用です。 Sessionless コント ローラーのサポートの詳細については、次を参照してください。 [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)です。

### <a name="new-additionalmetadataattribute-class"></a>新しい"AdditionalMetadataAttribute"クラス

使用することができます、 [AdditionalMetadata](https://msdn.microsoft.com/en-us/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)を設定する属性、`ModelMetadata.AdditionalValues`モデル プロパティのディクショナリ。 たとえば、ビュー モデルに、管理者にのみ表示されるプロパティがある場合は、注釈を付けて、そのプロパティ次の例で示すように。

[!code-csharp[Main](mvc3/samples/sample4.cs)]

製品ビュー モデルが表示される場合に、このメタデータは、表示またはエディターのテンプレートを使用可能になります。 メタデータ情報を解釈する責任です。

### <a name="accountcontroller-improvements"></a>AccountController の機能強化

インターネットのプロジェクト テンプレートで AccountController が大幅に強化されました。

### <a name="new-intranet-project-template"></a>新しいイントラネット プロジェクト テンプレート

新しいイントラネット プロジェクト テンプレートが含まれるこれ Windows 認証を有効になり、AccountController を削除します。
