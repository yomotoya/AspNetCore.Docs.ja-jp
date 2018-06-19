---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: ASP.NET Web Pages 2 の主要な機能 |Microsoft ドキュメント
author: microsoft
description: このトピックでは、ASP.NET Web Pages 2、WebMatr に含まれているかつ軽量の web プログラミング フレームワークで、主な新機能の概要を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899383"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>ASP.NET Web Pages 2 で、上位の特徴
====================
によって[Microsoft](https://github.com/microsoft)

> この記事は、ASP.NET Web Pages 2 の rc で軽量の web プログラミングのフレームワークに含まれている最上位の新機能の概要を説明[Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/)です。
> 
> **新機能が含まれています。** 
> 
> - [WebMatrix をインストールします。](#install)
> - [新機能および強化された機能](#New_and_Enhanced_Features)
> 
>     - [RC のリリースの変更](#Changes_for_the_RC_Version)
>     - [ベータ リリースの変更](#Changes_for_the_Beta_Version)
>     - [新規および更新されたサイト テンプレートを使用します。](#templates)
>     - [ユーザー入力の検証](#validation)
>     - [Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。](#oauthsetup)
>     - [マップのヘルパーを使用して、マップを追加します。](#maphelper)
>     - [サイド バイ サイドの Web ページ アプリケーションを実行します。](#sidebyside)
>     - [モバイル デバイスのページのレンダリング](#mobile)
> - [その他のリソース](#resources)
> 
> > [!NOTE]
> > このトピックでは、ASP.NET Web Pages 2 のコードで動作する WebMatrix を使用するいると仮定します。 ただし、Web ページ 1 では、Visual Studio を使用して Web Pages 2 の web サイトを作成することとを提供する強化された IntelliSense 機能とデバッグします。 Visual Studio で Web ページを操作するには、まず、Visual Studio 2010 SP1、Visual Web Developer Express 2010 SP1、または Visual Studio 11 Beta をインストールする必要があります。 次のテンプレートと Visual Studio で ASP.NET MVC 4 および Web Pages 2 アプリケーションを作成するためのツールを含む ASP.NET MVC 4 Beta をインストールします。
> 
> 
> *最終更新: 18 2012 年 6 月*


<a id="install"></a>
## <a name="installing-webmatrix"></a>WebMatrix をインストールします。

Web ページをインストールするには無料のアプリケーションを容易にインストールして web に関連するテクノロジを構成するものは、Microsoft Web Platform Installer を使用できます。 Web ページ 2 のベータ版を含む WebMatrix 2 Beta をインストールします。

1. Web Platform Installer の最新バージョンのインストール ページを参照してください。

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > WebMatrix 1 がインストールされて既にある場合は、このインストールにより WebMatrix 2 のベータ版に更新します。 同じコンピューターにバージョン 1 または 2 を使用して作成された web サイトを実行することができます。 詳細についてを参照してください[サイド バイ サイド実行 Web Pages アプリケーション](#sidebyside)です。
2. 選択**を今すぐインストール**です。 

    Internet Explorer を使用する場合は、次の手順に進みます。 Google Chrome、Mozilla Firefox などの別のブラウザーを使用する場合は、保存されたら、 *Webmatrix.exe*ファイルをコンピューターにします。 ファイルを保存し、クリックしてインストーラーを起動します。
3. インストーラーを実行して、**インストール**ボタンをクリックします。 WebMatrix および Web ページがインストールされます。

## <a id="New_and_Enhanced_Features"></a>  新機能および強化された機能

### <a id="Changes_for_the_RC_Version"></a>  RC 版 (2012 年 6 月) の変更

2012 年 6 月の RC バージョンのリリースでは、2012 年 3 月にリリースされたベータ バージョンの更新からのいくつかの変更がします。 これらの変更は次のとおりです。

- A`Validation.AddFormError`にメソッドが追加された、`Validation`ヘルパー。 これは、手動で検証を実行する場合に便利です (クエリ文字列で渡される値を検証するなど) によって表示されるエラー メッセージを追加して、`Html.ValidationSummary`メソッドです。 詳細については、セクションを参照して[を検証するデータをしないもの直接からユーザー](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)で[ASP.NET Web Pages (Razor) サイトにおけるユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)です。
- バンドルと縮小の機能は、ASP.NET Web Pages 2 のコア アセンブリから削除されました。 結果として、`Assets`このドキュメントで後述するヘルパーを使用できません。 代わりに、インストールする必要があります、 [ASP.NET 最適化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet パッケージです。 詳細については、次を参照してください。 [Bundling and ASP.NET Web Pages (Razor) サイトに縮小するアセット](https://go.microsoft.com/fwlink/?LinkId=255373)です。
- ASP.NET Web Pages 2 をサポートするために追加のアセンブリが追加されました。 この変更ののみ大きな影響がサイトの複数のアセンブリを表示することがあります*bin*サイトを作成またはホスティング プロバイダーに、サイトを展開すると、そのフォルダーです。

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>ベータ版 (2012 年 2 月) の変更

ベータ バージョン 2012 年 2 月にリリースには、2011 年 12 月にリリースされたベータ版からいくつかの変更のみです。 これらの変更は次のとおりです。

- Razor は、条件付き属性をサポートしています。 Html 要素の値に属性を設定した場合の解決結果にサーバーのコードを`false`または`null`ASP.NET が属性をまったく表示されません。 たとえば、 チェック ボックスを次のマークアップがあるとします。

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    場合の値`checked1`に解決される`false`または`null`、`checked`属性は表示されません。 これは、互換性に影響する変更点です。
- `Validation.GetHtml`メソッドの名前は`Validation.For`します。 これは、互換性に影響する変更です。`Validation.GetHtml`ベータ リリースでは機能しません。
- できるようになりました、`~`マークアップでの演算子を使用せずにサイトのルートを参照する、`Href`関数。 (つまり、Razor パーサーできますを検索して解決するには、`~`演算子への明示的なメソッド呼び出しを必要とせず`Href`)。`Href`メソッドが正常に動作、重大な変更ではありません。

    たとえば、次のようにマークアップを持っていたとします。

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    次のようにマークアップを使用できます。

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts`資産 (リソース) の管理のヘルパーが置き換えられて、`Assets`ヘルパーで、次のよう、少し異なる方法があります。

  - `Scripts.Add`、使用します。 `Assets.AddScript`
  - `Scripts.GetScriptTags`、使用します。 `Assets.GetScripts`

    これは、互換性に影響する変更です。`Scripts`クラスは、ベータ リリースで使用できません。 この変更により、コード例では、このドキュメント資産管理を使用するが更新されました。

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>新規および更新されたサイト テンプレートを使用します。

**スターター サイト**テンプレート Web Pages 2 上で実行されるため、既定によって更新されました。 次の新機能も含まれます。

- モバイル対応のページにレンダリングします。 CSS スタイルを使用して、`@media`セレクター、**スターター サイト**などのモバイル デバイスの画面の小さい画面にページのレンダリングを向上を提供します。
- メンバーシップと認証のオプションが向上します。 Twitter、Facebook、および Windows Live などの他のソーシャル ネットワー キング サイトから自分のアカウントを使用して、サイトにユーザーがログインをさせることができます。 詳細については、次を参照してください。、 [Facebook OAuth および OpenID を使用してその他のサイトからログインを有効にすると](#oauthsetup)セクションです。
- HTML5 の要素です。

新しい**個人用サイト**テンプレートでは、個人用ブログ、写真のページでは、および Twitter ページを含む web サイトを作成することができます。 基にして、サイトをカスタマイズすることができます、**個人用サイト**次の手順を実行してテンプレート。

- レイアウト ファイルを編集することによって、サイトの外観を変更 (*\_SiteLayout.cshtml*) とスタイル ファイル (*Site.css*)。
- NuGet パッケージの機能を追加するには、サイトをインストールします。 パッケージをインストールする方法については、ASP.NET Web Helpers Library などを参照してください、チュートリアル[ヘルパーをインストールする](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)です。

アクセスする、**個人用サイト**テンプレートを選択して**テンプレート**、WebMatrix で**クイック スタート**画面。

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

**テンプレート** ダイアログ ボックスで、選択、**個人用サイト**テンプレート。

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

ランディング ページ、**個人用サイト**Twitter ページ、および写真ページのテンプレートでは、自分のブログを設定するリンクをクリックすることができます。

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>ユーザー入力の検証

Web ページ 1 でに送信されたフォームは、ユーザー入力の検証に使用する、`System.Web.WebPages.Html.ModelState`クラスです。 (「Web ページ 1 のチュートリアルではいくつかのコード サンプルに示す[データを扱う](../data/5-working-with-data.md)。)この方法は、Web Pages 2 で使用することもできます。 ただし、Web Pages 2 では、ユーザー入力を検証するための強化されたツールも提供しています。

- 含む、新しい検証クラス`System.Web.WebPages.ValidationHelper`と`System.Web.WebPages.Validator`数行のコードを使用して検証の強力なタスクを実行できるようにします。
- 必要に応じて、クライアント側の検証は、検証エラーを確認するサーバーへのラウンド トリップを必要とするのではなく、ユーザーに即時のフィードバックを提供します。 (セキュリティ上の理由から、検証は実行、サーバー上のチェックで、クライアントであらかじめ実行された場合でも。)

新しい検証機能を使用するには、次の操作を行います。

ページのコードでのメソッドを使用して検証する要素を登録、`Validation`ヘルパー: `Validation.RequireField`、 `Validation.RequireFields` (を登録する必要になる複数の要素)、または`Validation.Add`です。 `Add`メソッドを選択し、他の種類のデータ型のチェック、文字列長のチェック、別のフィールド内のエントリを比較すると同様に、検証チェックを指定することができます (正規表現を使用した) パターンします。 次にいくつかの例を示します。

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

フィールド固有のエラーを表示するには、呼び出す`Html.ValidationMessage`検証対象の各要素のマークアップで。

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

概要を表示する (`<ul>`一覧) ページで、すべてのエラーの`Html.ValidationSummary`マークアップで。

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

次の手順では、サーバー側の検証を実装することがします。 クライアント側の検証を追加する場合は、次の操作をさらにします。

内の次のスクリプト ファイル参照を追加、 `<head>` web ページのセクションです。 最初の 2 つのスクリプト参照は、コンテンツ配信ネットワーク (CDN) サーバー上のリモート ファイルをポイントします。 3 番目の参照は、ローカルのスクリプト ファイルを指します。 実稼働アプリでは、CDN が利用できない場合、フォールバックを実装する必要があります。 フォールバックをテストします。

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

ローカル コピーを入手する最も簡単な方法、 *jquery.validate.unobtrusive.min.js*ライブラリは、(スターター サイト) などのサイト テンプレートのいずれかに基づいて、新しい Web Pages サイトを作成します。 テンプレートによって作成されたサイトが含まれています*jquery.validate.unobtrusive.js*元となることができますにコピーする、サイト、そのスクリプト フォルダー内のファイルです。

Web サイトで使用する場合、<em>\_SiteLayout</em>ページ レイアウトを制御します ページで、検証は、すべてのコンテンツ ページを利用できるように、そのページにこれらのスクリプト参照を含めることができます。 特定のページに対してのみ検証を実行する場合は、これらのページにスクリプトを登録する資産マネージャーを使用できます。 これを行うには、呼び出す`Assets.AddScript(path)`を検証し、各スクリプト ファイルを参照するページ。 呼び出しを追加`Assets.GetScripts`で、  <em>\_SiteLayout</em> 、登録されている表示するためにページ`<script>`タグ。 詳細については、セクションを参照して[資産マネージャーを使用してスクリプトを登録する](#resmanagement)です。

個々 の要素のマークアップで呼び出して、`Validation.For`メソッドです。 このメソッドは、属性を出力、jQuery クライアント側の検証を提供するためにフックすることができます。 例えば:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

次の例では、ユーザーがフォームに入力を検証するページを示します。 を実行してこの検証コードをテストするには、これの操作を行います。

1. 含む WebMatrix 2 サイト テンプレートのいずれかを使用して新しい web サイトを作成、*スクリプト*フォルダーなど、**スターター サイト**テンプレート。
2. 新しいサイトで作成、新しい *.cshtml*ページ、およびページの内容を次のコードに置き換えます。
3. ブラウザーでページを実行します。 検証の効果を確認する有効および無効な値を入力します。 たとえば、必須フィールドを空白のままにまたはの文字を入力、**クレジット**フィールドです。


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

ユーザーが有効な入力を送信するときに、ページを次に示します。

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

ユーザーが必要なフィールドを空のままで送信すると、ページを次に示します。

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

ユーザーを送信することで整数以外のもので、ページをここでは、**クレジット**フィールド。

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

詳細については、次のブログの投稿を参照してください。

- [V2 の Web ページでの検証を更新](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)検証を使用して追加する基本的な`Validation`ヘルパー (サーバー側のみ)
- [Web ページ バージョン 2、第 2 部での検証を更新](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)クライアント側の検証を追加します。
- [Web ページ バージョン 2、第 3 部での検証を更新](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)妥当性確認エラーを書式設定します。

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>資産マネージャーを使用してスクリプトを登録します。

資産マネージャーは、登録して、クライアント スクリプトを描画するサーバー コードで使用できる新しい機能です。 この機能は、実行時に 1 つのページに結合されます (レイアウト ページ、コンテンツ ページ、ヘルパーなど) などの複数のファイルからコードを使用している場合に便利です。 資産マネージャーでは、スクリプト ファイルが正しく参照されていると効率的に表示されたページ上からコード ファイルに関係なく呼び出されるか、確認呼び出される回数をソース ファイルを調整します。 資産マネージャー レンダリング`<script>`にすぐに (レンダリング中にスクリプトをダウンロードする)、ページが読み込むことができをレンダリングする前にスクリプトが呼び出された場合に発生する可能性があるエラーを回避するのには、完全なように適切な場所にタグを付けます。

たとえば、JavaScript ファイルを呼び出すカスタム ヘルパーを作成することと、コンテンツ ページ コードで次の 3 つの異なる場所でこのヘルパーを呼び出します。 3 つの異なるヘルパーでスクリプトを呼び出すを登録する資産マネージャーを使用しない場合`<script>`タグが、同じスクリプト ファイルへのすべてのポイントが、レンダリングされたページに表示されます。 さらに、場所によって、`<script>`タグが表示されたページに挿入される、スクリプトが、ページが完全に読み込まれる前に特定のページ要素にアクセスしようとしたかどうかはエラーが発生する可能性があります。 スクリプトを登録するには、資産マネージャーを使用する場合は、これらの問題を回避します。

これにより、資産マネージャでスクリプトを登録できます。

- スクリプトを参照する必要があるコードで呼び出して、`Assets.AddScript`メソッドです。
-  *\_SiteLayout*  ページで、呼び出し、`Assets.GetScripts`メソッドを表示するために、`<script>`タグ。 

    > [!NOTE]
    > 呼び出しを配置`Assets.GetScripts`最後の項目として、`<body>`の要素、  *\_SiteLayout*ページ。 これにより、ページ読み込みも高速とスクリプトのエラーを防ぐことができます。

次の例では、資産マネージャーのしくみを示しています。 コードには、次の項目が含まれています。

- という名前のカスタム ヘルパー`MakeNote`です。 このヘルパーは、ラップすることで、ボックス内の文字列を表示、`div`周り要素を境界線および追加することによってスタイルが適用&quot;注:&quot;にします。 ヘルパーには、メモに実行時の動作を追加する JavaScript ファイルも呼び出します。 使用してスクリプトを参照するのではなく、`<script>`タグ、ヘルパーが呼び出すことによって、スクリプトを登録`Assets.AddScript`です。
- JavaScript ファイル。 これは、ファイルは、ヘルパーによって呼び出されると、中にメモ アイテムのフォント サイズを一時的に増加する`mouseover`イベント。
- 参照するコンテンツ ページ、<em>\_SiteLayout</em>  ページで、本文、一部のコンテンツを表示してから、`MakeNote`ヘルパー。
- A  *\_SiteLayout*ページ。 このページは、共通のヘッダーとページ レイアウト構造を提供します。 呼び出しも含まれています。 `Assets.GetScripts`、ページ内を呼び出すスクリプトの表示方法は、資産マネージャであります。

このサンプルを実行します。

1. 空の Web Pages 2 の web サイトを作成します。 WebMatrix を使用する**空サイト**このテンプレート。
2. という名前のフォルダーを作成する*スクリプト*サイトにします。
3. *スクリプト*フォルダー、という名前のファイルを作成する*Test.js*、コピー、 *Test.js*例からにコンテンツし、ファイルを保存.
4. という名前のフォルダーを作成する*アプリ\_コード*サイトにします。
5. *アプリ\_コード*フォルダー、という名前のファイルを作成する*Helpers.cshtml*、そこにコード例をコピーし、という名前のフォルダーに保存、*アプリ\_コード*ルート フォルダーにします。
6. という名前のファイルを作成、サイトのルート フォルダーで *\_SiteLayout.cshtml、* 、そこに例をコピーし、ファイルを保存します。
7. サイト ルートでは、という名前のファイルを作成する*ContentPage.cshtml*例のコードを追加し、保存します。
8. 実行*コンテンツ ページ*ブラウザーでします。 渡された文字列、`MakeNote`ヘルパーは、ボックス化されたノートとしてレンダリングされます。
9. マウス ポインターをメモに通過します。 スクリプトは、注釈のフォント サイズを一時的に増加します。
10. 表示するページのソースを表示します。 呼び出しを配置したため`Assets.GetScripts`、レンダリングされた`<script>`を呼び出すタグ*Test.js*ページの本文の最後の項目。

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

次のスクリーン ショットでは*ContentPage.cshtml*メモ上マウス ポインターを置いたときにブラウザーで。

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。

Web Pages 2 では、メンバーシップと認証の拡張オプションを提供します。 メインの機能強化されている新しい[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。 これらのプロバイダーを使用して、Facebook、Twitter、Windows Live、Google、yahoo から、既存の資格情報を使用して、サイトにユーザーがログインをさせることができます。 たとえば、Facebook アカウントを使用してログイン、ユーザーだけを選択できます Facebook アイコンには、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。 Facebook ログインをサイトで各自のアカウント、関連付けることができますし、します。 Web ページのメンバーシップ機能に関連する拡張機能です。 ユーザーが (ソーシャル ネットワー キング サイトからのログインを含む) 複数のログインに関連付けることができる web サイトで 1 つのアカウント。

この図では、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、または Windows Live のアイコンを選択できます。

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

数行のコードを使用して OAuth および OpenID のメンバーシップを有効にすることができます。 メソッドとプロパティを使用する作業で、OAuth および OpenID プロバイダーでは、`WebMatrix.Security.OAuthWebSecurity`クラスです。

しかし、他のサイトからのログインを有効にするコードを使用する代わりに新しいプロバイダーを開始することをお勧め、使用する新しい**スターター サイト**WebMatrix 2 のベータ版に含まれているテンプレートです。 **スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースのすべてのコードと完了する必要がローカルの資格情報または別のサイトからこれらを使用してサイトにユーザーがログインを使用できます.

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>OAuth および OpenID プロバイダーを使用してログインを有効にする方法

このセクションでは、基になっているサイトにユーザーの外部のサイト (Facebook、Twitter、Windows Live、Google、または Yahoo) からにログインできるようにする方法の例を説明、**スターター サイト**テンプレート。 スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。

- OAuth プロバイダー (Facebook、Twitter、および Windows Live) を使用しているサイトでは、外部のサイトにアプリケーションを作成します。 これにより、それらのサイトのログインの機能を実行するために必要となるアプリケーション キー。 (Google、yahoo!) の OpenID プロバイダーを使用するサイトでは、アプリケーションを作成するはありません。 ログインするために、開発者向けアプリケーションを作成して、これらのサイトのすべてのアカウントが必要です。 

    > [!NOTE]
    > アプリケーションの Windows Live では、作業用の web サイトの URL をライブのみ承認し、ログインをテストするため、ローカルの web サイトの URL を使用することはできません。
- 適切な認証プロバイダーを指定するために、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。

**Google、yahoo のログインを有効にする**:

1. Web サイトには、編集、  *\_AppStart.cshtml*ページおよび Razor コード ブロックで呼び出しの後に次の 2 つのコード行を追加、`WebSecurity.InitializeDatabaseConnection`メソッドです。 このコードにより、プロバイダーは、Google と Yahoo OpenID です。 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. *~/Account/Login.cshtml*  ページで、次のコメントを解除して`<fieldset>`ページの末尾付近のマークアップのブロックです。 コードのコメントを解除、削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 結果として得られるコード ブロックは、このようになります。

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. 追加、`<input>`要素には、Google や yahoo のプロバイダー、`<fieldset>`グループにおいて、 *~/Account/Login.cshtml*ページ。 更新された`<fieldset>`グループ`<input>`Google と yahoo の両方の検索用の要素などの次の例。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. *~/Account/AssociateServiceAccount.cshtml*  ページで、追加`<input>`Google や yahoo への要素、`<fieldset>`ファイルの末尾付近のグループ化します。 同じコピーすることができます`<input>`に追加した要素、 `<fieldset>` 」の「、 *~/Account/Login.cshtml*ページ。 

    *~/Account/AssociateServiceAccount.cshtml*を web サイト上の単一のアカウントの他のサイトからの複数のログインを関連付けることができます ページを作成する場合、スターター サイト テンプレート内のページを使用できます。

今すぐ Google、yahoo のログインをテストすることができます。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタン。 この例では、Google ログインを使用します。 

    Web ページは、Google ログイン ページに、要求をリダイレクトします。

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. 既存の Google アカウントの資格情報を入力します。
4. Google は、するには、アカウントからの情報を使用して、をクリックして Localhost を許可するかどうかを求める場合**許可**です。

    コードでは、Google トークンを使用して、ユーザーを認証し、web サイトでこのページに返します。 このページでは、Google ログインを web サイト上の既存のアカウントに関連付けるユーザーまたは新しいアカウントに外部ログインとを関連付けるには、サイトに登録します。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. 選択、**関連付ける**ボタンをクリックします。 ブラウザーは、アプリケーションのホーム ページに戻ります。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Facebook ログインを有効にする**:

1. 移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(ログインされていないログインしている場合)。
2. 選択、 **Create New App**ボタンをクリックし、指示に従って名前を指定し、新しいアプリケーションを作成します。
3. セクションで**アプリを Facebook と統合する方法を選択**、選択、 **web サイト**セクションです。
4. 入力、**サイトの URL**フィールドをサイトの URL (たとえば、 [ `http://www.example.com` ](http://www.example.com))。 **ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。 

    > [!NOTE]
    > URL を使用して、ローカル コンピューターにサイトを実行している場合と同様に`http://localhost:12345`(数は、ローカル ポート番号)、するには、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールドです。 ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイト URL**アプリケーションのフィールドです。
5. 選択、 **Save Changes**ボタンをクリックします。
6. 選択、**アプリ**もう一度、タブし、アプリケーションのスタート ページを表示します。
7. コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードで Facebook プロバイダーに渡されます。
8. Facebook の開発者向けサイトを終了します。

今すぐを変更する 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用して、サイトにログインすることです。

1. Web サイトには、編集、  *\_AppStart.cshtml*ページおよび Facebook OAuth プロバイダーのコードのコメントを解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、 `consumerKey` (引用符を除く) のパラメーターです。
3. コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`consumerSecret`パラメーターの値。
4. ファイルを保存して閉じます。
5. 編集、 *~/Account/Login.cshtml*ページおよびからコメントを削除、`<fieldset>`ページの末尾付近のブロックです。 コードのコメントを解除、削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 コメント付きコード ブロックでは、次のような削除されます。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. ファイルを保存して閉じます。

Facebook ログインをテストできます。

1. サイトの実行*default.cshtml*ページし、選択、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Facebook**アイコン。 

    Web ページは、Facebook のログイン ページに、要求をリダイレクトします。

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Facebook アカウントにログインします。 

    コードでは、Facebook トークンを使用してユーザーを認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますページに返します。 ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールドです。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーのホーム ページに返しに記録されます。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Twitter ログインを有効にします。** 

1. 参照、 [Twitter 開発者サイト](https://dev.twitter.com/)です。
2. 選択、**アプリの作成**リンクし、サイトにログインします。
3. **アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールドです。
4. **Web サイト**フィールドに、サイトの URL を入力してください (たとえば、 [ `http://www.example.com` ](http://www.example.com))。 

    > [!NOTE]
    > ローカル サイトをテストする場合は (のように URL を使用して`http://localhost:12345`)、Twitter、URL を受け付けないことがあります。 ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。 これには、アプリケーションをローカルでのテストのプロセスが簡略化します。 ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新、 **web サイト**アプリケーションのフィールドです。
5. **コールバック URL**フィールドに、ユーザーが Twitter にログインした後に返される web サイト内のページの URL を入力します。 たとえば、ユーザーを (それらのログインの状態が認識されます) をスターター サイトのホーム ページに送信 を同じ URL を入力に入力した、 **web サイト**フィールドです。
6. 条項に同意し、選択、 **、Twitter アプリケーションを作成**ボタンをクリックします。
7. **マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。
8. **詳細** タブで、一番下までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。
9. **詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードで Twitter プロバイダーに渡すします。
10. Twitter のサイトを終了します。

今すぐを変更する 2 つのページ、web サイトのユーザーが自分の Twitter アカウントを使用して、サイトにログインできるようにします。

1. Web サイトには、編集、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードのコメントを解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. コピー、**コンシューマー キー**の値として Twitter アプリケーションからの値、 `consumerKey` (引用符を除く) のパラメーターです。
3. コピー、**コンシューマー シークレット**の値として Twitter アプリケーションからの値、`consumerSecret`パラメーター。
4. ファイルを保存して閉じます。
5. 編集、 *~/Account/Login.cshtml*ページおよびからコメントを削除、`<fieldset>`ページの末尾付近のブロックです。 コードのコメントを解除、削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 コメント付きコード ブロックでは、次のような削除されます。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. ファイルを保存して閉じます。

Twitter ログインをテストできます。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Twitter**アイコン。 

    Web ページは、作成したアプリケーションの Twitter ログイン ページに、要求をリダイレクトします。

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Twitter アカウントにログインします。
4. コードは、Twitter のトークンを使用してユーザーを認証し、返すページに関連付けることができます、web サイトのアカウントでログイン。 名前や電子メール アドレスを格納、**電子メール**フォームのフィールドです。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーのホーム ページに返しに記録されます。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>マップのヘルパーを使用して、マップを追加します。

Web Pages 2 では、Web Pages サイト用のアドインのパッケージは、ASP.NET Web Helpers Library への追加を含まれています。 によって提供されるマッピング コンポーネントの 1 つが、`Microsoft.Web.Helpers.Maps`クラスです。 使用することができます、`Maps`アドレスまたは緯度と経度座標のセットに基づいてマップを生成するクラス。 `Maps`クラス Bing、Google、MapQuest、yahoo などの一般的なマップ エンジンを直接呼び出すことができます。

使用して、新しい`Maps`クラス、web サイトの Web Helpers Library のバージョン 2 を最初にインストールする必要があります。 これを行うには、現在のリリース版をインストールするための手順を参照してください、 [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)とバージョン 2 をインストールします。

ページへのマッピングを追加する手順は、呼び出すマップ エンジンに関係なく同じです。 マップ ページに JavaScript ファイル参照を追加し、レンダリングの呼び出しを追加、`<script>`ページ上にタグを付けます。 [マッピング] ページで、使用する場合、マップ エンジンを呼び出します。

次の例では、アドレスに基づくマップを表示するページと経度と緯度座標に基づくマップを表示する別のページを作成する方法を示します。 アドレス マッピングの例は、Google マップを使用し、座標のマッピングの例が Bing Maps を使用します。 次の要素をコードを注意してください。

- 呼び出し`Assets.AddScript`2 つのマップ ページの上部にあります。 このメソッドへの参照の追加、 *jquery 1.6.2.min.js*に含まれているファイル、**スターター サイト**テンプレートで必要となると、`Maps`クラスです。
- 呼び出し、`Assets.GetScripts`レイアウト ファイル内のメソッドです。 このメソッドを表示、 `<script>` 2 つのマップ ページのタグ付けします。
- 呼び出し、`@Maps.GetGoogleHtml`と`@Maps.GetBingHtml`マッピング ページ内のメソッドです。 アドレスをマップするには、アドレス文字列を渡す必要があります。 座標をマップするには、経度と緯度を渡す必要があります座標。 Bing Maps エンジンの場合は、キーも渡す必要があります (無料でサインアップすることで取得したもの、 [Bing Maps 開発者サイト](https://www.microsoft.com/maps/developers/web.aspx))。 機能の他のマップ エンジンのメソッドは同様の方法で (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。

マッピング ページを作成します。

1. に基づいて web サイトを作成、**スターター サイト**テンプレート。
2. という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。 このページに渡されたアドレスに基づくマップが生成されます。
3. 既存のコンテンツを上書きして、ファイルに次のコードをコピーします。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. という名前のファイルを作成する *\_MapLayout.cshtml*サイトのルートにします。 このページは 2 つのマップ ページのレイアウト ページになります。
5. 既存のコンテンツを上書きして、ファイルに次のコードをコピーします。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. という名前のファイルを作成する*MapCoordinates.cshtml*サイトのルートにします。 このページに渡す座標のセットに基づくマップが生成されます。
7. 既存のコンテンツを上書きして、ファイルに次のコードをコピーします。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

マッピングのページをテストします。

1. ページの実行*MapAddress.cshtml*ファイル。
2. 番地、州または province、および郵便番号、含む完全なアドレス文字列を入力しを選択し、 **Map It**ボタンをクリックします。 ページは、Google マップからのマップを表示します。 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. 特定の場所の緯度と経度座標のセットを検索します。
4. ページの実行*MapCoordinates.cshtml*です。 座標を入力し、、 **Map It**ボタンをクリックします。 ページは、Bing マップからのマップを表示します。 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>サイド バイ サイドの Web ページ アプリケーションを実行します。

Web Pages 2 では、サイド バイ サイドのアプリケーションを実行する機能を追加します。 これにより引き続き、Web ページ 1 のアプリケーションの実行、Web Pages 2 アプリケーションを新規作成、およびそれらのすべてを同じコンピューターで実行できます。

WebMatrix で Web ページ 2 のベータ版をインストールする際にいくつかの点を次に示します。

- 既定では、既存の Web Pages アプリケーションは、コンピューターにバージョン 2 のアプリケーションとして実行されます。 (バージョン 2 のアセンブリが GAC にインストールされているし、自動的に使用されます。)
- (既定値、前のポイントと同様に) ではなく、Web ページ バージョン 1 を使用してサイトを実行する場合を行うには、サイトを構成することができます。 サイトを持っていない場合、 *web.config*サイトのルートにファイル、新たに作成し、既存のコンテンツを上書きするのには、次の XML をコピーします。 サイトが既に含まれている場合、 *web.config*ファイルに追加し、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクションです。

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-でバージョンが指定されていない場合、 *web.config*ファイル、サイトはバージョン 2 のサイトとして展開します。 (バージョン 2 のアセンブリにコピー、 *bin*配置済みのサイトのフォルダーです)。
- Beta 2 がサイトの Web ページ バージョン 2 のアセンブリを含める Web Matrix バージョンでサイト テンプレートを使って作成する新しいアプリケーション*bin*フォルダーです。

NuGet を使用して、サイトに適切なアセンブリをインストールすることによって、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダーです。 パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)です。

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>モバイル デバイスのページのレンダリング

Web Pages 2 では、モバイル デバイスまたはその他のデバイスでのコンテンツ表示のカスタムの表示を作成できます。

`System.Web.WebPages`名前空間には、表示モードで機能するのに便利な次のクラスが含まれています: `DefaultDisplayMode`、 `DisplayInfo`、および`DisplayModes`です。 これらのクラスを直接使用でき、特定のデバイス用の正しい出力を表示するコードを記述することができます。

または、次のようにファイルの名前付けパターンを使用してデバイス固有のページを作成することができます:<em>ファイル名</em>。<em>Mobile</em><em>.cshtml</em>です。 たとえば、ページ、という名前の 2 つのバージョンを作成することができます<em>MyFile.cshtml</em>という名前の 1 つ<em>MyFile.Mobile.cshtml</em>です。 実行時に、モバイル デバイスを要求したとき<em>MyFile.cshtml</em>、Web ページからコンテンツを表示する<em>MyFile.Mobile.cshtml</em>です。 それ以外の場合、 <em>MyFile.cshtml</em>が表示されます。

次の例では、モバイル デバイスのコンテンツ ページを追加することで、モバイルのレンダリングを有効にする方法を示します。 *Page1.cshtml*コンテンツにはナビゲーション サイド バーが含まれています。 *Page1.Mobile.cshtml*が同じコンテンツが含まれていますが、サイドバーを省略します。

ビルドし、コード サンプルを実行します。

1. サイトでは Web ページ、という名前のファイルを作成する*Page1.cshtml*をコピーし、 *Page1.cshtml*例からコンテンツにします。
2. という名前のファイルを作成する*Page1.Mobile.cshtml*をコピーし、 *Page1.Mobile.cshtml*例からコンテンツにします。 ページのモバイル バージョンが小さい画面できれいに表示の [ナビゲーション] セクションを省略することに注意してください。
3. デスクトップのブラウザーを実行しを参照*Page1.cshtml*です。
4. モバイル ブラウザー (または、モバイル デバイス エミュレーター) を実行しを参照*Page1.cshtml*です。 この時間の Web ページがページのモバイル バージョンを表示することを確認します。 

    > [!NOTE]
    > モバイル ページをテストするには、デスクトップ コンピューターで実行されているモバイル デバイス シミュレーターを使用できます。 このツールを使用して、それらのモバイル デバイスの表示は、web ページをテストできます (つまり、通常より小さな領域を表示)。 シミュレーターの 1 つの例は、[ユーザー エージェント スイッチャー アドオン](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox に対応することができます Firefox のデスクトップ バージョンからのさまざまなモバイル ブラウザーをエミュレートします。

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml*デスクトップ ブラウザーで表示されます。

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Firefox ブラウザーでは、Apple iPhone シミュレーター ビューに表示されます。 要求内容がにもかかわらず*Page1.cshtml*、アプリケーションのレンダリング*Page1.Mobile.cshtml*です。

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>その他のリソース

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 のリソース

> [!NOTE]
> ほとんどの Web ページ 1 のプログラミングと API のリソースは、Web Pages 2 にも適用されます。

- [ASP.NET Web Pages のプログラミングの概要](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix のリソース

- [WebMatrix 2 新機能](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Microsoft WebMatrix で Web 開発の開始](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(前のサンプルの Web ページのアプリケーションが含まれています)
