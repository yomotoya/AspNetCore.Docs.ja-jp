---
uid: mvc/mvc3
title: ASP.NET MVC 3 |Microsoft Docs
author: rick-anderson
description: (2011 年 4 月が含まれています Tools Update)安定したデザイン パターンを使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークを ASP.NET MVC 3 には.
ms.author: aspnetcontent
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 2c2d53734cd62e59ee4f550713b0576a73b0e32b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834771"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(2011 年 4 月が含まれています Tools Update)*
> 
> ASP.NET MVC 3 は、既に確立されている設計パターンと ASP.NET および .NET Framework の機能を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。
> 
> サイド バイ サイドでは、現在を使用して開始するために、ASP.NET MVC 2 でインストールされます。
> 
> ダウンロード、[インストーラーはこちらで](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>上位の特徴

- NuGet を使用して拡張可能な統合のスキャフォールディング システム
- HTML 5 の有効なプロジェクト テンプレート
- 新しい Razor ビュー エンジンを含む、表現力豊かなビュー
- 依存関係の挿入とグローバル アクション フィルターの強力なフック
- 控えめな JavaScript、jQuery の検証、JSON のバインドと豊富な JavaScript のサポート
- *完全な機能の一覧を読み取る[以下](#overview)*

## <a name="top-links"></a>ページ上部のリンク

ASP.NET MVC 3 の新機能新機能

- Phil Haack: [ASP.NET MVC 3 のリリース](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3、WebMatrix、NuGet、IIS Express および Orchard のリリースのコンテキストで、Microsoft 年 1 月の Web リリース](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [ASP.NET MVC 3、IIS Express、SQL CE 4、Web Farm Framework、Orchard、WebMatrix のリリースの発表](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

インストールとヘルプ

- ASP.NET MVC 3 を使用してインストール、 [Web Platform Installer が (推奨)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 を使用してインストール、[インストーラー実行可能ファイル](https://go.microsoft.com/fwlink/?LinkID=208140)
- インストール[ASP.NET MVC 3 for Visual Studio 11 開発者プレビュー](https://go.microsoft.com/fwlink/?LinkID=208140)
- 読み取り、 [ASP.NET MVC 3 のチュートリアルの概要](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- ヘルプを取得し、ASP.NET MVC 3 について説明します、[フォーラム](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 の概要

ASP.NET MVC 3 は、両方にコードを簡略化し、詳細な機能拡張を許可することの優れた機能を追加する ASP.NET MVC 1 と 2 に基づいています。 このトピックでは、数多くのこのリリースでは、次のセクション別に分類に含まれている新しい機能の概要を示します。

- [拡張可能なスキャフォールディング MvcScaffold 統合](#BM_MvcScaffolding)
- [HTML 5 の有効なプロジェクト テンプレート](#BM_HTML5)
- [Razor ビュー エンジン](#BM_TheRazorViewEngine)
- [複数のビュー エンジンのサポート](#BM_Support_for_Multiple_View_Engines)
- [コント ローラーの機能強化](#BM_Controller_Improvements)
- [JavaScript および Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [モデル検証の機能強化](#BM_Model_Validation_Improvements)
- [依存関係の注入機能強化](#BM_Dependency_Injection_Improvements)
- [他の新機能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>拡張可能なスキャフォールディング MvcScaffold 統合

新しいスキャフォールディング システム選択し、フレームワークにまったく新しい場合は、生産的な使用を開始してする経験豊富な場合は、一般的な開発タスクを自動化し、何をしている使い慣れたやすくなります。

新しい NuGet でサポートされる*スキャフォールディング*という名前のパッケージ**MvcScaffolding**します。 多くのソフトウェア テクノロジで「できるソフトウェアの基本的な概要をすばやく生成し、編集およびカスタマイズ」を意味する「スキャフォールディング」が使用される用語です。 ASP.NET mvc を作成しています、スキャフォールディング パッケージは、いくつかのシナリオで大幅に利点があります。

- **ASP.NET MVC を初めて学習している場合**をすばやくを編集して、必要に応じて調整できますし、有益な場合は、作業コードを取得する方法を提供するためです。 空白のページを見るとわかりませんをどこから始めることのトラウマが省けます。
- **ASP.NET MVC をよく知っているし、新しいアドオン テクノロジについて検討していますがかどうか**など、オブジェクト リレーショナル マッパー、ビュー エンジン、ライブラリ、テストなど、そのテクノロジの作成者可能性がありますもパッケージの作成、スキャフォールディングのためです。
- **類似のクラスまたは何らかのファイルを繰り返し作成するかどうか、作業に必要があります**出力テスト フィクスチャ、配置スクリプト、または必要なあらゆるカスタム スキャフォールダーを作成するためです。 カスタムのスキャフォールディングを使用して、チームの全員できます。

MvcScaffolding でその他の機能は次のとおりです。

- C# および VB プロジェクトのサポート
- ASPX、Razor ビュー エンジンをサポートします。
- ASP.NET MVC 区分にスキャフォールディング、およびカスタム ビュー レイアウト/マスターの使用をサポートします。
- T4 テンプレートを編集することによって、出力を簡単にカスタマイズすることができます。
- PowerShell のカスタム ロジックとカスタム T4 テンプレートを使用して、まったく新しいスキャフォールディングを追加することができます。 これら (およびユーザーに許可するすべてのカスタム パラメーター) に自動的にコンソール タブ補完の一覧を表示します。
- さまざまなテクノロジの追加のスキャフォールディングを含む NuGet パッケージを取得できます (例: は、概念実証いずれかの LINQ to SQL のようになりました) を混在させるし、まとめてと一致させる

ASP.NET MVC 3 Tools Update には、このスキャフォールディング システムでは、優れた Visual Studio のサポートにはなどが含まれます。

- コント ローラーのダイアログを作成、読み取り、更新、および削除のコント ローラー アクションと対応するビューの完全な自動スキャフォールディング サポートを追加します。 既定では、EF Code First を使用してデータ アクセス コードをスキャフォールディングします。
- コント ローラーのダイアログのサポートを追加*拡張可能なスキャフォールディング*など、NuGet を使用してパッケージ*MvcScaffolding*します。 これは、カスタムのスキャフォールディングを希望している場合は、ODBCDirect で NHibernate なども JET の他のデータ アクセス テクノロジのスキャフォールディングを作成することができるようにダイアログにプラグインすることができます。

ASP.NET MVC 3 でスキャフォールディングの詳細については、次のリソースを参照してください。

- Steve Sanderson の後の系列を含みます。 

    1. [概要: ASP.NET MVC 3 プロジェクトと MvcScaffolding パッケージのスキャフォールディングします。](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [標準の使用状況: 一般的なユース ケースとオプション](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一対多のリレーションシップ](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [スキャフォールディング アクションと単体テスト](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [T4 テンプレートをオーバーライドします。](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [この投稿: カスタム スキャフォールダーの作成](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- PDC 2010 セッションからの Scott Hanselman の post ["名前のないパッケージの Web Love"Microsoft のブログを構築](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 プロジェクト テンプレート

新しいプロジェクト ダイアログには、プロジェクト テンプレートのチェック ボックスを有効にする HTML 5 バージョンが含まれています。 これらのテンプレートは、下位レベルのブラウザーで HTML 5、CSS 3 の互換性サポートを提供する Modernizr 1.7 を活用します。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor ビュー エンジン

ASP.NET MVC 3 Razor、次の利点を提供するという名前の新しいビュー エンジンが付属します。

- Razor 構文では、クリーンで簡潔では、キーストロークの最小数を必要とします。
- Razor は c# および Visual Basic などの既存の言語に基づいているために一部習得しやすいです。
- Visual Studio には、Razor 構文の IntelliSense およびコードの色付けが含まれています。
- Razor ビューは、単体テスト アプリケーションを実行または web サーバーを起動することを必要とせずに指定できます。

Razor の新機能の一部を以下に示します。

- `@model` ビューに渡される型を指定する構文があります。
- `@* *@` コメントの構文です。
- 既定値を指定する機能 (など`layoutpage`) サイト全体に対して 1 回です。
- `Html.Raw` HTML エンコードせずにテキストを表示するためのメソッドにします。
- 複数のビューの間でコードを共有するためのサポート (*\_viewstart.cshtml*または *\_viewstart.vbhtml*ファイル)。

Razor では、次などの新しい HTML ヘルパーも含まれます。

- `Chart`。 ASP.NET 4 のグラフ コントロールと同じ機能を提供する、グラフを表示します。
- `WebGrid`。 ページングと並べ替え機能を備えたデータ グリッドを表示します。
- `Crypto`。 正常に作成するアルゴリズムのハッシュを使用してでは、ソルトし、パスワードをハッシュします。
- `WebImage`。 イメージを表示します。
- `WebMail`。 電子メールを送信します。

Razor の詳細については、次のリソースを参照してください。

- [Razor Scott Guthrie のブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie のブログ投稿の概要、@modelキーワード](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie のブログ投稿の概要の Razor レイアウト](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API クイック リファレンス](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>複数のビュー エンジンのサポート

**ビューの追加** ダイアログ ボックスで、ASP.NET MVC 3 では、ビュー エンジンを使用すると、作業対象を選択できます。 および**新しいプロジェクト** ダイアログ ボックスを使用して、プロジェクトの既定のビュー エンジンを指定できます。 など、Web フォーム ビュー エンジン (ASPX)、Razor、またはオープン ソースのビュー エンジンを選択できます[Spark](http://sparkviewengine.com/)、 [NHaml](https://code.google.com/p/nhaml/)、または[NDjango](http://ndjango.org/)します。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>コント ローラーの機能強化

### <a name="global-action-filters"></a>グローバル アクション フィルター

アクション メソッドの実行前に、またはアクション メソッドの実行後にロジックを実行することがあります。 これをサポートするには、ASP.NET MVC 2 には、アクション フィルターが用意されています。 アクション フィルターは、宣言型の事前アクションと事後アクションの動作を特定のコント ローラー アクション メソッドに追加する手段を提供するカスタム属性です。 ただし、場合によっては、すべてのアクション メソッドに適用される前のアクションまたは事後アクションの動作を指定する必要があります。 MVC 3 では、追加することによって、グローバル フィルターを指定することができます、`GlobalFilters`コレクション。 グローバル アクション フィルターの詳細については、次のリソースを参照してください。

- [MVC 3 の Preview で Scott Guthrie のブログ](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [ASP.NET MVC でのフィルター処理](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新しい"ViewBag"プロパティ

MVC 2 コント ローラーのサポート、`ViewData`プロパティすると、遅延バインディング ディクショナリ API を使用してビュー テンプレートにデータを渡すことができます。 MVC 3 で、少々 単純な構文を使用することも、`ViewBag`プロパティを同じ目的を達成します。 書き込みの例ではなく`ViewData["Message"]="text"`、書き込める`ViewBag.Message="text"`します。 使用する厳密に型指定されたクラスを定義する必要はありません、`ViewBag`プロパティ。 動的プロパティは、だけ取得できますかプロパティを設定し、解決する動的に実行時にします。 内部的には、`ViewBag`で名前/値ペアとしてプロパティが格納されている、`ViewData`ディクショナリ。 (注: MVC 3 では、ほとんどのプレリリース バージョンで、`ViewBag`プロパティの名前、`ViewModel`プロパティです)。

### <a name="new-actionresult-types"></a>新しい"ActionResult"の種類

次`ActionResult`型と対応するヘルパー メソッドは、新しいまたは MVC 3 で強化されています。

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)します。 クライアントには、HTTP ステータス コード 404 を返します。
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)します。 一時的なリダイレクト (HTTP 302 ステータス コード) またはブール型パラメーターによって永続的なリダイレクト (HTTP 301 状態コード) を返します。 この変更により、組み合わせて、[コント ローラー](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)クラスが永続的なリダイレクトを実行するための 3 つのメソッドを持つようになりました: `RedirectPermanent`、 `RedirectToRoutePermanent`、および`RedirectToActionPermanent`します。 これらのメソッドのインスタンスを返す`RedirectResult`で、`Permanent`プロパティに設定`true`します。
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)します。 ユーザー指定の HTTP ステータス コードを返します。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript と Ajax の機能強化

既定では、MVC 3 の Ajax と検証ヘルパーは、控えめな JavaScript アプローチを使用します。 控えめな JavaScript は、HTML にインライン JavaScript を挿入することを回避できます。 これは、HTML 小さく、煩雑になりの入れ替えや JavaScript ライブラリのカスタマイズを簡単になります。 MVC 3 の検証ヘルパーを使用しても、`jQueryValidate`プラグインが既定でします。 MVC 2 の動作を実行する場合に、控えめな JavaScript を使用して無効にすることができます、 *web.config*ファイルの設定。 JavaScript と Ajax の機能強化の詳細については、次のリソースを参照してください。

- [Wikipedia のサイトでの控えめな JavaScript の基本的な概要](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson の控えめな JavaScript の投稿](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson の控えめな JavaScript 検証の投稿](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Razor と控えめな JavaScript で MVC 3 アプリケーションを作成する](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)(ASP.NET サイトのチュートリアル)
- [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>既定で有効になっているクライアント側の検証

MVC の以前のバージョンでは、明示的に呼び出す必要があります、`Html.EnableClientValidation`クライアント側の検証を有効にするには、ビューからのメソッド。 MVC 3 でこれは必要なくなりましたクライアント側の検証が既定で有効になっているためです。 (を無効にできますで設定を使用して、 *web.config*ファイルです)。

クライアント側検証が動作するためには、引き続き適切な jQuery と jQuery 検証ライブラリ、サイトを参照する必要があります。 これらのライブラリを独自のサーバーでホストしたり、Microsoft または Google から Cdn のようなコンテンツ配信ネットワーク (CDN) からそれらを参照できます。

### <a name="remote-validator"></a>リモート検証コントロール

ASP.NET MVC 3 をサポート、新しい[RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)活用するためには jQuery Validation プラグインできるようにするクラスのリモート検証コントロールのサポート。 これにより、サーバー側のみ実行できる検証ロジックを実行するには、サーバー上で定義するカスタム メソッドを自動的に呼び出すクライアント側の検証ライブラリです。

次の例では、`Remote`属性は、クライアントの検証がという名前のアクションを呼び出すことを指定します`UserNameAvailable`上、`UsersController`クラスを検証するために、`UserName`フィールド。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

次の例は、対応するコント ローラーを示しています。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

使用する方法についての詳細、`Remote`属性は、「[方法: ASP.NET MVC でのリモート検証の実装](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN ライブラリ。

### <a name="json-binding-support"></a>JSON のバインディング サポート

ASP.NET MVC 3 には、JSON でエンコードされたデータを受信し、モデル バインド、アクション メソッドのパラメーターにアクション メソッドを使用する組み込みの JSON バインド サポートが含まれています。 この機能は、クライアントのテンプレートとデータ バインドに関連するシナリオで役立ちます。 (クライアントのテンプレートを使用する書式設定し、クライアント上で実行されるテンプレートを使用して、1 つのデータ項目または一連のデータ項目を表示します。)MVC 3 では、サーバー上の JSON データを送受信するアクション メソッドとクライアント テンプレートを簡単に接続することができます。 JSON のバインディング サポートの詳細については、次を参照してください。、 **JavaScript と AJAX の機能強化**の[Scott Guthrie の MVC 3 Preview ブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)します。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>モデル検証の機能強化

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations"メタデータ属性

ASP.NET MVC 3 では`DataAnnotations`などのメタデータ属性`DisplayAttribute`します。

### <a name="validationattribute-class"></a>"ValidationAttribute"クラス

`ValidationAttribute`クラスの新しいサポートするために .NET Framework 4 で向上`IsValid`など、どのようなオブジェクトが検証されている、現在の検証コンテキストに関する情報を提供するオーバー ロードします。 これによりより豊富なシナリオをモデルの別のプロパティに基づいて、現在の値を検証することができます。 たとえば、新しい`CompareAttribute`属性では、モデルの 2 つのプロパティの値を比較することができます。 次の例では、`ComparePassword`プロパティに一致する必要があります、`Password`有効にするのにはフィールド。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>検証のインターフェイス

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)インターフェイスでは、モデル レベルの検証を実行することができ、全体的なモデルまたはモデル内の 2 つのプロパティの間の状態に固有のエラー メッセージの検証を提供できます. MVC 3 からのエラーを取得、`IValidatableObject`インターフェイスの影響を受ける HTML フォームの組み込みのヘルパーを使用してビュー内のフィールドと、モデル バインディングと自動的にフラグまたは強調表示します。

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)インターフェイスは、実行時に検証コントロールがあるクライアントの検証をサポートするかどうかを検出する ASP.NET MVC を使用できます。 このインターフェイスは、さまざまな検証フレームワークに統合できるように設計されています。

検証のインターフェイスの詳細については、次を参照してください。、**モデル検証の機能強化**の[Scott Guthrie の MVC 3 Preview ブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)します。 (ただし、ブログで"IValidateObject"への参照は"IValidatableObject"である必要があります。)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>依存関係の注入機能強化

ASP.NET MVC 3 は、依存関係挿入 (DI) を適用するため、依存関係の挿入またはコントロールの反転 (IOC) コンテナーを統合するための優れたサポートを提供します。 次の領域で、DI のサポートが追加されました。

- コント ローラーが表示されている (登録とコント ローラー ファクトリ、コント ローラーを挿入する挿入する)。
- ビュー (を登録して、ビュー エンジン、ページの表示への依存関係の挿入を挿入する)。
- アクション フィルター (検索とフィルターを挿入する)。
- モデル バインダー (登録および挿入する)。
- モデル検証プロバイダー (登録および挿入する)。
- モデル メタデータ プロバイダー (登録および挿入する)。
- (登録および挿入する) の値プロバイダー。

MVC 3 のサポート、[共通サービス ロケーター](https://github.com/unitycontainer/commonservicelocator)ライブラリとそのライブラリをサポートするすべての DI コンテナー`IServiceLocator`インターフェイス。 新しいサポートも`IDependencyResolver`インターフェイス DI のフレームワークを統合するが簡単です。

MVC 3 の DI の詳細については、次のリソースを参照してください。

- [Brad Wilson の一連のサービスの場所でのブログ](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>他の新機能

### <a name="nuget-integration"></a>NuGet の統合

ASP.NET MVC 3 は自動的にインストールされ、そのセットアップの一部として NuGet を有効にします。 NuGet は、簡単に検索、インストール、およびプロジェクトで .NET ライブラリとツールを使用する無料のオープン ソース パッケージ マネージャーです。 (ASP.NET Web フォームと ASP.NET MVC を含む) すべての Visual Studio プロジェクトの種類で動作します。

NuGet には、そのライブラリをパッケージ化し、オンライン ギャラリーに登録するオープン ソース プロジェクト (たとえば、Moq、NHibernate、Ninject、StructureMap、NUnit、Windsor、RhinoMocks、Elmah などのプロジェクト) を管理する開発者ができるようにします。 .NET 開発者がこれらのライブラリのいずれかを使用して、パッケージを見つけてで作業するプロジェクトにインストールする場合に簡単です。

ASP.NET の 3 Tools Update のプロジェクト テンプレートには、NuGet 経由で更新されるために JavaScript ライブラリ事前インストールされている NuGet パッケージが含まれます。 Entity Framework Code First はも事前インストールされている NuGet パッケージとして。

NuGet の詳細については、[NuGet のドキュメント](https://docs.microsoft.com/nuget/)を参照してください。

### <a name="partial-page-output-caching"></a>部分ページ出力キャッシュ

ASP.NET MVC は、出力がバージョン 1 以降のページ全体の応答のキャッシュ サポートされています。 MVC 3 もサポートしています部分ページ出力キャッシュ、リージョンを簡単にキャッシュまたは応答のフラグメントにできます。 キャッシュの詳細については、次を参照してください、**部分的なページ出力キャッシュ**の[MVC 3 のリリース候補で Scott Guthrie のブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)、**子アクションの出力キャッシュ。** のセクション、 [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)します。

### <a name="granular-control-over-request-validation"></a>要求の検証をきめ細かく制御

ASP.NET MVC では、XSS や HTML インジェクション攻撃から保護を自動的に役立つ組み込みの要求の検証があります。 ただし、明示的に無効にする要求の検証する場合がある場合などたいユーザー (たとえば、ブログ エントリや CMS コンテンツ) でのコンテンツの HTML を送信できるようにします。 追加できるように、 [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)属性をモデルまたはモデル バインド中に、プロパティごとに、要求の検証を無効にするモデルを表示します。 要求の検証の詳細については、次のリソースを参照してください。

- **控えめな JavaScript と検証**セクション[MVC 3 のリリース候補で Scott Guthrie のブログの投稿](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)します。
- [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>拡張可能な [新しいプロジェクト] ダイアログ ボックス

ASP.NET MVC 3 では、プロジェクト テンプレート、ビュー エンジンを追加することができ、単体テスト プロジェクトのフレームワークを**新しいプロジェクト** ダイアログ ボックス。

### <a name="template-scaffolding-improvements"></a>テンプレートのスキャフォールディング機能強化

ASP.NET MVC 3 のスキャフォールディング テンプレートは、モデルの主キー プロパティを識別してより適切にそれらを処理するには MVC の以前のバージョンの方を実行します。 (たとえば、スキャフォールディング テンプレートことを確認します主キーが編集可能なフォームのフィールドとスキャフォールディングしないことです。)

既定では、Create と Edit のスキャフォールディングを今すぐ使用して、`Html.EditorFor`ヘルパーの代わりに、`Html.TextBoxFor`ヘルパー。 これによりデータの形式でモデルのメタデータのサポートが向上注釈属性の場合に、**ビューの追加** ダイアログ ボックスは、ビューを生成します。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor"と"Html.LabelForModel"の新しいオーバー ロード

新しいメソッドのオーバー ロードが追加されました、`LabelFor`と`LabelForModel`ヘルパー メソッド。 新しいオーバー ロードを使用すると、指定するか、ラベルのテキストを上書きできます。

### <a name="sessionless-controller-support"></a>Sessionless コント ローラーのサポート

ASP.NET MVC 3 の場合は、セッションの状態を使用するコント ローラー クラスを希望するかどうかを指定する、セッション状態するかどうか読み取り/書き込みまたは読み取り専用です。 Sessionless コント ローラーのサポートの詳細については、次を参照してください。 [MVC 3 のリリース ノート](../whitepapers/mvc3-release-notes.md)します。

### <a name="new-additionalmetadataattribute-class"></a>新しい"AdditionalMetadataAttribute"クラス

使用することができます、 [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)を設定する属性、`ModelMetadata.AdditionalValues`モデル プロパティのディクショナリ。 たとえば、ビュー モデルでは、管理者にのみ表示されるプロパティが、注釈プロパティを次の例に示すように。

[!code-csharp[Main](mvc3/samples/sample4.cs)]

このメタデータは、製品のビュー モデルが表示されるときに、表示またはエディターのテンプレートを使用されます。 メタデータ情報を解釈することの責任です。

### <a name="accountcontroller-improvements"></a>AccountController の機能強化

インターネットのプロジェクト テンプレートで AccountController が大幅に改善されました。

### <a name="new-intranet-project-template"></a>新しいイントラネット プロジェクト テンプレート

新しいイントラネット プロジェクト テンプレートが含まれる Windows 認証を有効にして、AccountController を削除します。
