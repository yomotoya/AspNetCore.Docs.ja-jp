---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: ASP.NET Web Pages 2 での主要な機能 |Microsoft Docs
author: microsoft
description: このトピックでは、ASP.NET の Web ページ 2、WebMatr に含まれている軽量な web プログラミング フレームワークの最上位の新機能の概要を提供します。
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 6e20dedd19ae458b9881973570f23b5d77dda654
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827402"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>ASP.NET Web Pages 2 の主な機能
====================
によって[Microsoft](https://github.com/microsoft)

> この記事では、ASP.NET Web Pages 2 rc では、軽量な web プログラミングのフレームワークに含まれている新機能のトップの概説[Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/)します。
> 
> **何が含まれます。** 
> 
> - [WebMatrix をインストールします。](#install)
> - [新しい強化された機能](#New_and_Enhanced_Features)
> 
>     - [RC のリリースの変更](#Changes_for_the_RC_Version)
>     - [ベータ リリースの変更](#Changes_for_the_Beta_Version)
>     - [新規および更新されたサイト テンプレートを使用します。](#templates)
>     - [ユーザー入力の検証](#validation)
>     - [Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。](#oauthsetup)
>     - [マップ ヘルパーを使用してマップを追加します。](#maphelper)
>     - [サイド バイ サイドの Web ページ アプリケーションを実行します。](#sidebyside)
>     - [モバイル デバイス用ページのレンダリング](#mobile)
> - [その他のリソース](#resources)
> 
> > [!NOTE]
> > このトピックでは、ASP.NET Web Pages 2、コードを操作する WebMatrix を使用していることを前提としています。 ただし、ように Web ページ 1 の場合は、Visual Studio を使用して Web ページ 2 の web サイトを作成することは IntelliSense の機能とデバッグに拡張を使用できます。 Visual Studio で Web ページを操作するには、まず、Visual Studio 2010 SP1、Visual Web Developer Express 2010 SP1、または Visual Studio 11 Beta をインストールする必要があります。 テンプレートと Visual Studio で ASP.NET MVC 4 と Web ページ 2 アプリケーションを作成するためのツールを含む ASP.NET MVC 4 Beta をインストールします。
> 
> 
> *最終更新: 2012 年 6 月の 18*


<a id="install"></a>
## <a name="installing-webmatrix"></a>WebMatrix をインストールします。

Web ページをインストールするにをインストールして web 関連のテクノロジを構成するが容易にする無料のアプリケーションは、Microsoft Web プラットフォーム インストーラーを使用できます。 Web ページ 2 のベータ版を含む WebMatrix 2 の Beta をインストールします。

1. Web Platform Installer の最新バージョンのインストール ページを参照してください。

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > WebMatrix 1 がインストールされて既にある場合は、このインストールにより WebMatrix 2 ベータ版に更新します。 同じコンピューター上のバージョン 1 または 2 を使用して作成された web サイトを実行できます。 詳細については、上のセクションを参照してください。[実行 Web ページ アプリケーション サイド バイ サイド](#sidebyside)します。
2. 選択**を今すぐインストール**します。 

    Internet Explorer を使用する場合は、次の手順に移動します。 Mozilla Firefox または Google Chrome などの別のブラウザーを使用する場合は、保存されたら、 *Webmatrix.exe*ファイルをコンピューターにします。 ファイルを保存し、クリックしてインストーラーを起動します。
3. インストーラーを実行し、選択、**インストール**ボタンをクリックします。 WebMatrix および Web ページがインストールされます。

## <a id="New_and_Enhanced_Features"></a>  新しい強化された機能

### <a id="Changes_for_the_RC_Version"></a>  RC のバージョン (2012 年 6 月) の変更

2012 年 6 月の RC バージョンのリリースでは、2012 年 3 月にリリースされた Beta バージョンの更新からのいくつかの変更があります。 これらの変更は次のとおりです。

- A`Validation.AddFormError`にメソッドが追加された、`Validation`ヘルパー。 これは手動で検証を実行する場合に便利です (クエリ文字列で渡される値を検証するなど) で表示できるエラー メッセージを追加して、`Html.ValidationSummary`メソッド。 詳細については、セクションをご覧ください。[を検証するデータをしないは直接からユーザー](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)で[ASP.NET Web Pages (Razor) サイトでユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)です。
- バンドルと縮小の機能は、ASP.NET Web Pages 2 のコア アセンブリから削除されました。 結果として、`Assets`このドキュメントで後述するヘルパーを使用できません。 代わりに、インストールする必要があります、 [ASP.NET 最適化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet パッケージ。 詳細については、次を参照してください。[バンドルと ASP.NET Web Pages (Razor) サイトで資産を縮小する](https://go.microsoft.com/fwlink/?LinkId=255373)します。
- ASP.NET Web Pages 2 をサポートする追加のアセンブリが追加されました。 この変更の唯一明らかな影響は、サイトの複数のアセンブリを表示することがあります*bin*フォルダー、サイトを作成するか、サイトをホスティング プロバイダーをデプロイします。

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>ベータ版 (2012 年 2 月) の変更

2012 年 2 月にリリースされた Beta バージョンは、2011 年 12 月にリリースされたベータ版からいくつかの変更のみです。 これらの変更は次のとおりです。

- Razor は、条件付き属性をサポートします。 Html 要素の値に属性を設定する場合は、解決にサーバー コードで`false`または`null`ASP.NET が、属性をまったく表示されません。 たとえば、チェック ボックスを次のマークアップがある場合を考えてみましょう。

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    場合の値`checked1`に解決される`false`または`null`、`checked`属性は表示されません。 これは、重大な変更です。
- `Validation.GetHtml`メソッドの名前は`Validation.For`します。 これは重大な変更です。`Validation.GetHtml`ベータ リリースでは機能しません。
- ここで含めることができます、`~`マークアップでの演算子を使用せず、サイトのルートを参照する、`Href`関数。 (つまり、Razor パーサーできますを検索して解決するには、`~`演算子への明示的なメソッド呼び出しを必要とせず`Href`)。`Href`メソッドは引き続き機能しますので、重大な変更はありません。

    たとえば、このようなマークアップを持っていたとします。

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    このようなマークアップを使用できます。

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts`資産 (リソース) の管理のためのヘルパーに置き換えられましたが、`Assets`ヘルパーで、次などの若干異なる方法があります。

  - `Scripts.Add`を使用して、 `Assets.AddScript`
  - `Scripts.GetScriptTags`を使用して、 `Assets.GetScripts`

    これは重大な変更です。`Scripts`クラスは、ベータ版のリリースで使用することはありません。 この変更により、資産管理を使用して、このドキュメントのコード例が更新されました。

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>新規および更新されたサイト テンプレートを使用します。

**スターター サイト**テンプレート Web ページ 2 で実行されるよう既定によって更新されました。 次の新機能も含まれています。

- モバイル対応のページにレンダリングします。 CSS スタイルを使用して、`@media`セレクター、**スターター サイト**などのモバイル デバイスの画面の小さい画面にページの表示が改善されましたを提供します。
- メンバーシップと認証のオプションを強化します。 Twitter、Facebook、および Windows Live などの他のソーシャル ネットワーク サイトから自分のアカウントを使用して、サイトには、ユーザーがログインをさせることができます。 詳細については、次を参照してください。、 [Facebook および OAuth および OpenID を使用してその他のサイトからログインを有効にする](#oauthsetup)セクション。
- HTML5 の要素。

新しい**個人用サイト**テンプレートを使用して、個人のブログ、写真のページでは、および Twitter のページを含む web サイトを作成できます。 に基づいてサイトをカスタマイズすることができます、**個人用サイト**次の手順に従ってテンプレート。

- レイアウト ファイルを編集して、サイトの外観を変更 (*\_SiteLayout.cshtml*) と、スタイル ファイル (*Site.css*)。
- サイトに機能を追加する NuGet パッケージをインストールします。 パッケージをインストールする方法については、ASP.NET Web Helpers Library を参照してください、チュートリアル[ヘルパーをインストールする](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)します。

アクセスする、**個人用サイト**テンプレート、選択**テンプレート**、WebMatrix で**クイック スタート**画面。

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

**テンプレート** ダイアログ ボックスで、選択、**個人用サイト**テンプレート。

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

ランディング ページ、**個人用サイト**ページとページの写真を Twitter、ブログ記事へのリンクをフォローするテンプレートを使用できます。

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>ユーザー入力の検証

Web ページの 1 に送信されたフォームは、ユーザー入力の検証に使用する、`System.Web.WebPages.Html.ModelState`クラス。 (「Web ページ 1 のチュートリアルではいくつかのコード サンプルに示す[データを扱う](../data/5-working-with-data.md)。)この方法は、Web ページ 2 でも使用できます。 ただし、Web ページ 2 では、ユーザー入力の検証のためのツールも提供しています。

- などの新しい検証クラス`System.Web.WebPages.ValidationHelper`と`System.Web.WebPages.Validator`数行のコードで強力な検証タスクを実行することができます。
- 必要に応じて、クライアント側の検証は、検証エラーの確認対象のサーバーへのラウンド トリップを必要とするのではなくユーザーへの迅速なフィードバックを提供します。 (セキュリティ上の理由から、検証は実行サーバーで、チェックをクライアントであらかじめ実行されているが場合でもです。)

新しい検証機能を使用するには、次の操作を行います。

ページのコードでのメソッドを使用して検証する要素を登録、`Validation`ヘルパー: `Validation.RequireField`、 `Validation.RequireFields` (を必須にする複数の要素を登録するには)、または`Validation.Add`します。 `Add`メソッドは、他の種類のデータ型のチェック、文字列の長さのチェック、別のフィールド内のエントリを比較するように、検証チェックを指定することができ、パターン (正規表現を使用)。 次にいくつかの例を示します。

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

フィールド固有のエラーを表示するには、呼び出す`Html.ValidationMessage`検証対象の各要素のマークアップで。

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

概要を表示する (`<ul>`一覧) ページで、すべてのエラーの`Html.ValidationSummary`マークアップで。

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

これらの手順は、サーバー側の検証を実装するために十分です。 クライアント側の検証を追加するには、さらに、以下を実行する場合は。

内では、次のスクリプト ファイル参照を追加、 `<head>` web ページのセクション。 最初の 2 つのスクリプト参照は、コンテンツ配信ネットワーク (CDN) サーバー上のリモート ファイルをポイントします。 3 番目の参照は、ローカル スクリプト ファイルを指します。 実稼働アプリは、CDN が利用できない場合、フォールバックを実装する必要があります。 フォールバックをテストします。

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

ローカル コピーを取得する最も簡単な方法、 *jquery.validate.unobtrusive.min.js*ライブラリは、サイト テンプレート (スターター サイト) などのいずれかに基づく新しい Web ページ サイトを作成します。 テンプレートによって作成されたサイトが含まれています*jquery.validate.unobtrusive.js* 、元にコピーできますが、サイトの Scripts フォルダー内のファイル。

Web サイトで使用する場合、<em>\_SiteLayout</em>ページ レイアウトを制御するページで、検証をすべてのコンテンツ ページを使用できるように、そのページでこれらのスクリプト参照を含めることができます。 特定のページに対してのみ検証を実行する場合は、これらのページだけでスクリプトを登録する資産マネージャーを使用できます。 これを行うには、呼び出す`Assets.AddScript(path)`ページを検証し、各スクリプト ファイルを参照することにします。 呼び出しを追加し、`Assets.GetScripts`で、  <em>\_SiteLayout</em>登録を表示するためにページ`<script>`タグ。 詳細については、セクションをご覧ください。[資産マネージャーを使用してスクリプトを登録する](#resmanagement)します。

個々 の要素のマークアップを呼び出して、`Validation.For`メソッド。 このメソッドは、属性を出力する jQuery をクライアント側の検証を提供するためにフックできます。 例えば:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

次の例では、ユーザーがフォームに入力を検証するページを示します。 この検証コードを実行し、テスト、これを行います。

1. 含む WebMatrix 2 サイト テンプレートのいずれかを使用して新しい web サイトを作成、*スクリプト*フォルダーなど、**スターター サイト**テンプレート。
2. 新しいサイトの作成、新しい *.cshtml*ページ、およびページの内容を次のコードに置き換えます。
3. ブラウザーでページを実行します。 検証の効果を確認の有効および無効な値を入力します。 たとえば、必須フィールドを空白のままにやの文字を入力してください、**クレジット**フィールド。


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

ユーザーが有効な入力を送信するときに次のページに示します。

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

ユーザーが、必須フィールドが空のままで送信すると次のページに示します。

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

ユーザーが以外の整数で送信すると、ページをここでは、**クレジット**フィールド。

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

詳細については、次のブログの投稿を参照してください。

- [Web ページ v2 での検証を更新](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)検証を使用して追加の基本、`Validation`ヘルパー (サーバー側のみ)
- [Web ページ v2、第 2 部での検証を更新](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)クライアント側の検証を追加します。
- [Web ページ v2、パート 3 での検証を更新](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)検証エラーの書式を設定します。

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>資産マネージャーを使用してスクリプトの登録

資産マネージャーは、登録して、クライアント スクリプトをレンダリングするサーバー コードで使用できる新しい機能です。 この機能は、実行時に 1 つのページに結合されます (レイアウト ページ、コンテンツ ページ、ヘルパーなど) などの複数のファイルからコードを使用する場合に便利です。 資産マネージャーは、ソース ファイルまたは確認することのスクリプト ファイルが正しく参照されていると効率的に表示されたページ上から呼び出されたコード ファイルに関係なく呼び出される回数を調整します。 資産マネージャーもレンダリング`<script>`にすぐに (レンダリング中にスクリプトをダウンロードするには)、ページを読み込むことができ、レンダリングの前にスクリプトが呼び出された場合に発生する可能性があるエラーを回避するためには、完全なように適切な場所にタグを付けます。

たとえばを呼び出す JavaScript ファイルでは、カスタム ヘルパーを作成することと、次の 3 つの異なる場所にコンテンツ ページのコードでこのヘルパーを呼び出すことです。 ヘルパーの 3 つの異なる登録資産マネージャーを使用しない場合、スクリプトでは呼び出し`<script>`タグを同じスクリプト ファイルにすべてのポイントは、レンダリングされたページに表示されます。 また、位置によって、`<script>`タグがレンダリングされたページに挿入される、スクリプトが、ページが完全に読み込まれる前に特定のページ要素にアクセスしようとしたかどうかはエラーが発生する可能性があります。 資産マネージャーを使用してスクリプトを登録する場合は、これらの問題を回避します。

これにより、資産マネージャーとスクリプトを登録できます。

- スクリプトを参照する必要があるコードで呼び出し、`Assets.AddScript`メソッド。
- *\_SiteLayout*  ページで、呼び出し、`Assets.GetScripts`をレンダリングするメソッド、`<script>`タグ。 

    > [!NOTE]
    > 呼び出し配置`Assets.GetScripts`の非常に最後の項目として、`<body>`の要素、  *\_SiteLayout*ページ。 これにより、ページ読み込みも高速とスクリプトのエラーを防ぐことができます。

次の例では、資産マネージャーのしくみを示しています。 コードには、次のものが含まれています。

- という名前のカスタム ヘルパー`MakeNote`します。 このヘルパーは、ラップすることで、ボックス内の文字列をレンダリングします。、`div`その周りの要素を境界線でとを追加してスタイルが適用&quot;に注意してください:&quot;にします。 ヘルパーには、ノートの実行時の動作を追加する JavaScript ファイルも呼び出します。 スクリプトへの参照ではなく、`<script>`タグ ヘルパーは、呼び出すことによって、スクリプトを登録`Assets.AddScript`します。
- JavaScript ファイル。 これは、ファイル、ヘルパーによって呼び出され、中にメモ アイテムのフォント サイズを一時的に増加する`mouseover`イベント。
- コンテンツ ページを参照して、<em>\_SiteLayout</em>  ページは、本文の一部のコンテンツをレンダリングしを呼び出して、`MakeNote`ヘルパー。
- A  *\_SiteLayout*ページ。 このページは、一般的なヘッダーとページ レイアウト構造体を提供します。 呼び出しも含まれています。 `Assets.GetScripts`、ページの呼び出しは資産マネージャーでのスクリプトの表示方法。

サンプルを実行するには

1. 空の Web ページ 2 の web サイトを作成します。 WebMatrix を使用する**空のサイト**このテンプレート。
2. という名前のフォルダーを作成する*スクリプト*サイト。
3. *スクリプト*フォルダー、という名前のファイルを作成する*Test.js*、コピー、 *Test.js*からこの例では、そこにコンテンツし、ファイルを保存.
4. という名前のフォルダーを作成する*アプリ\_コード*サイト。
5. *アプリ\_コード*フォルダー、という名前のファイルを作成する*Helpers.cshtml*例のコードをコピー、および、という名前のフォルダーに保存*アプリ\_コード*ルート フォルダーにします。
6. サイトのルート フォルダーでは、という名前のファイルを作成 *\_SiteLayout.cshtml、* 例をコピーし、ファイルを保存します。
7. という名前のファイルを作成、サイトのルートで*ContentPage.cshtml*例のコードを追加し、保存します。
8. 実行*ContentPage*ブラウザーでします。 渡される文字列、`MakeNote`ヘルパーはボックス化されたメモとしてレンダリングされます。
9. メモをマウス ポインターを渡します。 スクリプトは、注釈のフォント サイズを一時的に増加します。
10. 表示するページのソースを表示します。 呼び出しを配置したため`Assets.GetScripts`、レンダリングされた`<script>`を呼び出すタグ*Test.js*ページの本文の最後の項目。

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

次のスクリーン ショット*ContentPage.cshtml*マウス ポインターをメモに置くと、ブラウザーで。

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。

Web ページ 2 では、メンバーシップと認証のオプションの強化を提供します。 主な機能強化は、新しい[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。 これらのプロバイダーを使用して、Facebook、Twitter、Windows Live、Google、yahoo から既存の資格情報を使用して、サイトに、ユーザーがログインをさせることができます。 たとえば、Facebook アカウントを使用してログイン、ユーザーことができます選択 Facebook アイコンは、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。 サイトでユーザーのアカウント、Facebook ログインに関連付けることができます。 Web ページのメンバーシップ機能に関連する拡張機能は、web サイトの 1 つのアカウントを持つユーザーが複数回のログイン (ソーシャル ネットワーク サイトからのログインを含む) を関連付けることができます。

この図は、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、または Windows Live のアイコンを選択できます。

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

数行のコードを使用して OAuth と OpenID のメンバーシップを有効にすることができます。 OAuth を使用するように使用するメソッドとプロパティおよび OpenID プロバイダーはで、`WebMatrix.Security.OAuthWebSecurity`クラス。

しかし、他のサイトからのログインを有効にするコードを使用する代わりに新しいプロバイダーを使用することをお勧め、使用する新しい**スターター サイト**WebMatrix 2 ベータ版に含まれているテンプレートです。 **スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースでは、すべてのコードと完了する必要がありますをローカルの資格情報または別のサイトからこれらのいずれかを使用してサイトに、ユーザーがログインできるようにするには.

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>OAuth および OpenID プロバイダーを使用してログインを有効にする方法

このセクションでは、ユーザーから外部のサイト (Facebook、Twitter、Windows Live、Google、または Yahoo) ログインできるようにする方法の例を示しますに基づいているサイトに、**スターター サイト**テンプレート。 スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。

- OAuth プロバイダー (Facebook、Twitter、および Windows Live) を使用するサイトでは、外部のサイトにアプリケーションを作成します。 これにより、それらのサイトのログイン機能を起動するために必要となるアプリケーションのキー。 (Google、yahoo!)、OpenID プロバイダーを使用して、サイトでは、アプリケーションを作成する必要はありません。 これらのサイトのすべてにログインするために、開発者のアプリケーションを作成して、アカウントが必要です。 

    > [!NOTE]
    > Windows Live アプリケーションでは、ログインをテストするため、ローカルの web サイトの URL を使用することはできませんので作業用の web サイトでライブ URL のみ受け入れます。
- 適切な認証プロバイダーを指定するには、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。

**Google、Yahoo のログインを有効にする**:

1. Web サイト、編集、  *\_AppStart.cshtml*ページし、呼び出しの後の Razor コード ブロックに次の 2 行のコードを追加、`WebSecurity.InitializeDatabaseConnection`メソッド。 このコードにより、Google と Yahoo OpenID プロバイダーをできます。 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. *~/Account/Login.cshtml*  ページで、次のコメントを解除して`<fieldset>`ページの末尾付近のマークアップのブロック。 コードのコメントを解除するには削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 そのコード ブロックのようになります。

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. 追加、`<input>`要素プロバイダーは、Google、Yahoo、`<fieldset>`グループに、 *~/Account/Login.cshtml*ページ。 更新された`<fieldset>`グループ`<input>`などの次の例を Google と Yahoo の両方の外観の要素。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. *~/Account/AssociateServiceAccount.cshtml*ページで、追加`<input>`Google、Yahoo の要素、`<fieldset>`ファイルの末尾付近のグループ化します。 同じをコピーする`<input>`に追加した要素、`<fieldset>`セクション、 *~/Account/Login.cshtml*ページ。 

    *~/Account/AssociateServiceAccount.cshtml*を web サイト上の 1 つのアカウントでの他のサイトから複数のログインを関連付けることができます ページを作成する場合、スターター サイト テンプレートのページを使用できます。

Google、Yahoo のログインをテストできます。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**別のサービスを使用してログイン**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタンをクリックします。 この例では、Google ログインを使用します。 

    Web ページは、Google ログイン ページに、要求をリダイレクトします。

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. 既存の Google アカウントの資格情報を入力します。
4. Google は、Localhost をクリックして、アカウントからの情報の使用を許可するかどうかを確認する場合**許可**します。

    コードでは、Google トークンを使用して、ユーザーを認証して、web サイトにこのページに戻ります。 このページでは、Google ログインを web サイト、既存のアカウントに関連付けるユーザーまたはで外部ログインに関連付けるサイトで新しいアカウントを登録することができます。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. 選択、**関連付ける**ボタンをクリックします。 ブラウザーは、アプリケーションのホーム ページを返します。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Facebook ログインを有効にする**:

1. 移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(まだログインしている場合ログ記録)。
2. 選択、 **Create New App**ボタンをクリックし、新しいアプリケーションを作成して名前を指示に従います。
3. セクションで**アプリを Facebook と統合する方法を選択します。**、選択、 **web サイト**セクション。
4. 入力、**サイトの URL**フィールドに、サイトの URL (たとえば、 [ `http://www.example.com` ](http://www.example.com))。 **ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。 

    > [!NOTE]
    > などの URL を使用してローカル コンピューターにサイトを実行している場合`http://localhost:12345`(番号が、ローカル ポート番号)、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールド。 ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイトの URL**アプリケーションのフィールド。
5. 選択、**変更の保存**ボタンをクリックします。
6. 選択、**アプリ**タブをもう一度、し、アプリケーションのスタート ページを表示します。
7. コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードに Facebook プロバイダーに渡されます。
8. Facebook の開発者向けサイトを終了します。

今すぐに変更を加える 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用してサイトにログインすること。

1. Web サイト、編集、  *\_AppStart.cshtml*ページし、Facebook OAuth プロバイダーのコードをコメント解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、`consumerKey`パラメーター (引用符を除く)。
3. コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`consumerSecret`パラメーターの値。
4. ファイルを保存して閉じます。
5. 編集、 *~/Account/Login.cshtml*ページおよびからコメントを削除、`<fieldset>`ページの末尾付近のブロック。 コードのコメントを解除するには削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 コメントを使用したコード ブロックには、次のように見えるが削除されます。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. ファイルを保存して閉じます。

Facebook ログインをテストできます。

1. サイトの実行*default.cshtml*ページ、**ログイン**ボタンをクリックします。
2. *ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Facebook**アイコン。 

    Web ページは、Facebook のログイン ページに、要求をリダイレクトします。

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Facebook アカウントにログインします。 

    コードでは、Facebook トークンを使用して、認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますをページに返します。 ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールド。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーがホーム ページに戻るしに記録されます。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Twitter ログインを有効にします。** 

1. 参照、 [Twitter 開発者サイト](https://dev.twitter.com/)します。
2. 選択、**アプリを作成する**リンクし、サイトにログインします。
3. **アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールド。
4. **Web サイト**フィールドに、サイトの URL を入力します (たとえば、 [ `http://www.example.com` ](http://www.example.com))。 

    > [!NOTE]
    > サイトをローカルでテストする場合 (ような URL を使用して`http://localhost:12345`)、Twitter の URL を受け付けないことがあります。 ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。 これには、アプリケーションをローカルでのテストのプロセスが簡略化します。 ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新する、 **web サイト**アプリケーションのフィールド。
5. **コールバック URL**フィールドに、ユーザーが Twitter にログインした後に戻り、web サイトのページの URL を入力します。 たとえば、スターター サイト (これは、ログイン ステータスが認識されます) のホーム ページにユーザーを送信するで入力したのと同じ URL を入力、 **web サイト**フィールド。
6. 条項に同意し、選択、 **Twitter アプリケーションを作成**ボタンをクリックします。
7. **マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。
8. **詳細** タブで、下部までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。
9. **詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。 Web サイトのコードで、Twitter プロバイダーにこれらの値を渡すします。
10. Twitter サイトを終了します。

今すぐに変更を加える 2 つのページ、web サイトでユーザーが自分の Twitter アカウントを使用してサイトにログインできるようにします。

1. Web サイト、編集、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードをコメント解除します。 コメント解除されたコード ブロックのようになります。 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. コピー、**コンシューマー キー**の値としての Twitter アプリケーションからの値、`consumerKey`パラメーター (引用符を除く)。
3. コピー、**コンシューマー シークレット**の値としての Twitter アプリケーションからの値、`consumerSecret`パラメーター。
4. ファイルを保存して閉じます。
5. 編集、 *~/Account/Login.cshtml*ページおよびからコメントを削除、`<fieldset>`ページの末尾付近のブロック。 コードのコメントを解除するには削除、`@*`直前および直後の文字、`<fieldset>`ブロックします。 コメントを使用したコード ブロックには、次のように見えるが削除されます。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. ファイルを保存して閉じます。

Twitter ログインをテストできます。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Twitter**アイコン。 

    Web ページは、要求を作成したアプリケーションの Twitter ログイン ページにリダイレクトします。

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Twitter アカウントにログインします。
4. コードは、ユーザーの認証に Twitter のトークンを使用し、返すページに関連付けることができます、web サイトのアカウントでログイン。 名前または電子メール アドレスを格納、**電子メール**フォームのフィールド。

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーがホーム ページに戻るしに記録されます。

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>マップ ヘルパーを使用してマップを追加します。

Web ページ 2 には、ASP.NET Web Helpers Library への追加には、Web ページ サイト用のアドインのパッケージが含まれています。 によって提供されるマッピング コンポーネントの 1 つが、`Microsoft.Web.Helpers.Maps`クラス。 使用することができます、`Maps`アドレスまたは経度と緯度の座標のセットのいずれかに基づくマップを生成するクラス。 `Maps`クラス Bing、Google、MapQuest、Yahoo などの人気のあるマップ エンジンを直接呼び出すことができます。

使用して、新しい`Maps`クラス、web サイトで、Web Helpers Library のバージョン 2 を最初にインストールする必要があります。 これを行うには、現在リリースされているバージョンのインストール手順に移動、 [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)とバージョン 2 をインストールします。

ページへのマッピングを追加する手順は、呼び出すことのマップ エンジンに関係なく同じです。 マップ ページに JavaScript ファイル参照を追加し、表示する呼び出しを追加する、`<script>`ページにタグを付けます。 マッピング ページで、使用するマップ エンジンを呼び出します。

次の例では、アドレスに基づくマップを表示するページおよび経度と緯度の座標に基づくマップを表示する別のページを作成する方法を示します。 アドレスのマッピングの例は、Google マップを使用し、座標のマッピングの例は、Bing Maps を使用しています。 コードでは、次の要素に注意してください。

- 呼び出し`Assets.AddScript`マッピングの 2 つのページの上部にあります。 このメソッドへの参照の追加、 *jquery 1.6.2.min.js*ファイルに含まれている、**スターター サイト**テンプレートで必要な`Maps`クラス。
- 呼び出し、`Assets.GetScripts`レイアウト ファイル内のメソッド。 このメソッドを表示、`<script>`マッピングの 2 つのページでタグ付けします。
- 呼び出し、`@Maps.GetGoogleHtml`と`@Maps.GetBingHtml`マッピング ページ内のメソッド。 アドレスをマップするには、アドレス文字列を渡す必要があります。 座標をマップするには、緯度と経度を渡す必要があります座標。 Bing Maps のエンジンは、キーも渡す必要があります (入手を無料でサインアップすることにより、 [Bing Maps 開発者サイト](https://www.microsoft.com/maps/developers/web.aspx))。 その他のマップ エンジンのメソッドと同様の方法では動作 (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。

マッピングのページを作成します。

1. に基づいて web サイトを作成、**スターター サイト**テンプレート。
2. という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。 このページに渡すことのできる、アドレスに基づくマップが生成されます。
3. 既存のコンテンツを上書き、ファイルに次のコードをコピーします。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. という名前のファイルを作成する *\_MapLayout.cshtml*サイトのルートにします。 このページは、このページは、2 つのマップ ページのレイアウト ページになります。
5. 既存のコンテンツを上書き、ファイルに次のコードをコピーします。 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. という名前のファイルを作成する*MapCoordinates.cshtml*サイトのルートにします。 このページに渡す座標のセットに基づくマップが生成されます。
7. 既存のコンテンツを上書き、ファイルに次のコードをコピーします。 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

マップ ページをテストします。

1. ページの実行*MapAddress.cshtml*ファイル。
2. 番地、状態または都道府県、郵便番号などの完全なアドレス文字列を入力してから、 **Map It**ボタンをクリックします。 ページは、Google マップからマップを表示します。 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. 特定の場所の緯度と経度の座標のセットを検索します。
4. ページの実行*MapCoordinates.cshtml*します。 座標を入力してから、 **Map It**ボタンをクリックします。 ページは、Bing マップからマップを表示します。 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>サイド バイ サイドの Web ページ アプリケーションを実行します。

Web ページ 2 では、サイド バイ サイドのアプリケーションを実行する機能を追加します。 これにより、Web ページ 1 アプリケーションを実行、新しい Web ページ 2 アプリケーションを構築およびそれらのすべてを同じコンピューターで実行を続行できます。

WebMatrix を使用した Web ページ 2 ベータ版をインストールするときの注意点を次に示します。

- 既定では、既存の Web ページ アプリケーションは、コンピューターにバージョン 2 のアプリケーションとして実行されます。 (バージョン 2 のアセンブリは、GAC にインストールおよび自動的に使用されます)。
- (前のポイントのように既定) ではなく、Web ページ バージョン 1 を使用してサイトを実行する場合は、そのためには、サイトを構成できます。 サイトを持っていない場合、 *web.config*サイトのルートにファイル、新しいものを作成および既存のコンテンツを上書きするのには、次の XML をコピーします。 サイトが既に含まれている場合、 *web.config*ファイルを追加、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクション。

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-で、バージョンを指定しない場合、 *web.config*ファイル、サイトはバージョン 2 のサイトとして配置されます。 (バージョン 2 のアセンブリにコピー、 *bin*デプロイされたサイトのフォルダーです)。
- Web Matrix のバージョン 2 ベータ版に含まれる Web ページのバージョン 2 アセンブリで、サイトのサイト テンプレートを使って作成する新しいアプリケーション*bin*フォルダー。

サイトに適切なアセンブリをインストールする NuGet を使用して、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダー。 パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)します。

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>モバイル デバイス用ページのレンダリング

Web ページ 2 では、モバイル デバイスまたはその他のデバイスでコンテンツの描画のカスタムの表示を作成できます。

`System.Web.WebPages`名前空間には表示モードを使用することができます、次のクラスが含まれています: `DefaultDisplayMode`、 `DisplayInfo`、および`DisplayModes`します。 これらのクラスを直接使用し、特定のデバイスの右側の出力をレンダリングするコードを記述できます。

または、このようなファイルの名前付けパターンを使用してデバイスに固有のページを作成できます<em>ファイル名。</em> 。<em>Mobile</em><em>.cshtml</em>します。 たとえば、ページ、という名前の 2 つのバージョンを作成できます<em>MyFile.cshtml</em>という名前の 1 つ<em>MyFile.Mobile.cshtml</em>します。 実行時に、モバイル デバイスを要求したとき<em>MyFile.cshtml</em>、Web ページからコンテンツをレンダリングする<em>MyFile.Mobile.cshtml</em>します。 それ以外の場合、 <em>MyFile.cshtml</em>を表示します。

次の例では、モバイル デバイスのコンテンツ ページを追加して、モバイルのレンダリングを有効にする方法を示します。 *Page1.cshtml*コンテンツにはナビゲーション サイド バーが含まれています。 *Page1.Mobile.cshtml* 、同じコンテンツが含まれていますが、サイドバーが省略されます。

ビルドして、コード サンプルを実行します。

1. という名前のファイルを作成、Web ページ サイトで*Page1.cshtml*をコピーし、 *Page1.cshtml*の例では、そこにコンテンツ。
2. という名前のファイルを作成する*Page1.Mobile.cshtml*をコピーし、 *Page1.Mobile.cshtml*の例では、そこにコンテンツ。 モバイル バージョンのページが小さい画面できれいに表示のナビゲーション セクションを省略することに注意してください。
3. デスクトップ ブラウザーを実行しを参照*Page1.cshtml*します。
4. モバイル ブラウザー (または、モバイル デバイス エミュレーター) を実行しを参照*Page1.cshtml*します。 今回の Web ページを表示、ページのモバイル バージョンに注意してください。 

    > [!NOTE]
    > モバイル ページをテストするには、デスクトップ コンピューターで実行されているモバイル デバイス シミュレーターを使用できます。 このツールを使用して、モバイル デバイスでになりますが、web ページをテストできます (つまり、通常ははるかに小さいで領域を表示)。 シミュレーターの 1 つの例は、[ユーザー エージェント切り替えアドオン](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)、Mozilla firefox に対応することができます Firefox のデスクトップ バージョンからのさまざまなモバイル ブラウザーをエミュレートします。

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml*デスクトップ ブラウザーで表示されます。

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Firefox ブラウザーでは、Apple iPhone シミュレーター ビューに表示されます。 要求内容がにもかかわらず*Page1.cshtml*、アプリケーションのレンダリング*Page1.Mobile.cshtml*します。

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>その他のリソース

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web ページの 1 つのリソース

> [!NOTE]
> ほとんどの Web ページ 1 のプログラミングと API のリソースは、Web ページ 2 にも適用されます。

- [ASP.NET Web Pages のプログラミングの概要](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix のリソース

- [WebMatrix 2 はの新機能](http://webmatrix.com/next)
- [Microsoft WebMatrix サイト](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Microsoft WebMatrix を使用した Web 開発の開始](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(サンプルの使い方の Web ページ アプリケーションを含む)
