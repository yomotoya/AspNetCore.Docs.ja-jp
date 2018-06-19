---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages (Razor) API クイック リファレンス |Microsoft ドキュメント
author: tfitzmac
description: このページには、最もよく使用されるオブジェクト、プロパティ、および ASP.NET Web ページ Razor 構文を使用したプログラミング方法の簡単な例を含む一覧が含まれています。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897585"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages (Razor) API クイック リファレンス
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このページには、最もよく使用されるオブジェクト、プロパティ、および ASP.NET Web ページ Razor 構文を使用したプログラミング方法の簡単な例を含む一覧が含まれています。
> 
> 説明"(v2)"とマークされている ASP.NET Web Pages 2 のバージョンで導入されました。
> 
> API リファレンス ドキュメントを参照してください、 [ASP.NET Web Pages のリファレンス ドキュメント](https://go.microsoft.com/fwlink/?LinkId=208659)msdn です。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、(v2 マークされている機能) を除く ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。


このページには、次の参照情報が含まれています。

- [クラス](#Classes)
- [データ](#Data)
- [ヘルパー](#Helpers)
- [検証](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>クラス

### `AppState[key], AppState[index],App`

アプリケーション内のすべてのページで共有できるデータが含まれています。 動的を使用する`App`プロパティを次の例のように、同じデータにアクセスします。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

文字列値をブール値 (true または false) に変換します。 False を返します。 または、文字列が true または false を表していない場合は、指定された値。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

日付/時刻を文字列値に変換します。 返します`DateTime.MinValue`または文字列が日付/時刻を表していない場合は、指定された値。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

文字列値を 10 進値に変換します。 指定した値文字列は 10 進数値を表していない場合または 0.0 を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

文字列値を float に変換します。 指定した値文字列は 10 進数値を表していない場合または 0.0 を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

文字列値を整数に変換します。 文字列は整数値を表していない場合は、指定した値または 0 を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

省略可能な追加のパス部分とのローカル ファイル パスから、ブラウザーと互換性のある URL を作成します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

レンダリング*値*に HTML エンコードされた出力をレンダリングするのではなく HTML マークアップとして。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

値は、文字列から指定された型に変換できる場合は true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

オブジェクトまたは変数に値があるない場合は true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

要求が POST である場合は true を返します。 (最初の要求には通常、GET) です。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

このページに適用するレイアウト ページのパスを指定します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

現在の要求のページ、レイアウト ページ、および部分ページ間で共有データが含まれています。 動的を使用する`Page`プロパティを次の例のように、同じデータにアクセスします。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(レイアウト ページ)任意の名前付きセクションに含まれていないコンテンツ ページのコンテンツをレンダリングします。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

指定したパスと省略可能な追加データを使用し、コンテンツ ページを表示します。 余分なパラメーターの値を取得できる`PageData`位置 (例: 1) またはキー (例: 2) 使用しています。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(レイアウト ページ)名前を持つコンテンツ セクションを表示します。 設定*必要*が false にセクションを省略可能です。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

取得または HTTP クッキーの値を設定します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

現在の要求でアップロードされたファイルを取得します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

(文字列) として、フォームがポストされたデータを取得します。 `Request[key]` 両方のチェック、`Request.Form`と`Request.QueryString`コレクション。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

URL クエリ文字列で指定されているデータを取得します。 `Request[key]` 両方のチェック、`Request.Form`と`Request.QueryString`コレクション。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

選択的に無効には、form 要素は、クエリ文字列値、cookie、またはヘッダーの値の検証を要求します。 要求の検証は、既定で有効にし、ユーザーがマークアップやその他の危険性のあるコンテンツを送信するを防ぎます。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

HTTP サーバーのヘッダーを応答に追加します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

指定した時間、ページの出力をキャッシュします。 必要に応じて設定*スライディング*を各ページにアクセスでのタイムアウトをリセットして*varyByParams*ページ要求内の各別のクエリ文字列のページの異なるバージョンをキャッシュします。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

ブラウザー要求を新しい場所にリダイレクトします。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

ブラウザーに送信する HTTP ステータス コードを設定します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

内容を書き込みます*データ*省略可能な MIME の種類の応答にします。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

ファイルの内容を応答に書き込みます。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(レイアウト ページ)名前を持つコンテンツ セクションを定義します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Html エンコードする文字列をデコードします。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

HTML マークアップで表示するための文字列をエンコードします。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

指定した仮想パスのサーバーの物理パスを返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

URL からテキストをデコードします。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

URL に格納するテキストをエンコードします。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

取得またはユーザーがブラウザーを閉じるまでに存在する値を設定します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

オブジェクトの値の文字列表現を表示します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

URL から取得された追加データ (たとえば、 *MyPage/ExtraData*)。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

指定したユーザーのパスワードを変更します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

アカウント確認トークンを使用して、アカウントを確認します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

指定されたユーザー名とパスワードを持つ新しいユーザー アカウントを作成します。 確認トークンを要求するに true を渡します*requireConfirmationToken です。*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

現在のログイン ユーザーの整数識別子を取得します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

現在のログイン ユーザーの名前を取得します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

送信できる電子メールでユーザーに、ユーザーがパスワードをリセットできるようにパスワード リセット トークンを生成します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

ユーザー名とユーザー ID を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

現在のユーザーがログオンしている場合に true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

(たとえば、確認の電子メール) を通じて、ユーザーが確認されている場合は true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

現在のユーザーの名前が指定されたユーザー名と一致する場合は true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

内のユーザーは、cookie に認証トークンを設定してログに記録します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

認証トークンのクッキーを削除することで、ユーザーをサインアウト ログオンします。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

ユーザーが認証されていない場合、HTTP ステータスを 401 (Unauthorized) に設定します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

現在のユーザーが指定されたロールのいずれかのメンバーでない場合、HTTP ステータスを 401 (Unauthorized) に設定します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

現在のユーザーがで指定されたユーザーではないかどうか*username*、HTTP ステータス 401 (Unauthorized) を設定します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

パスワード リセット トークンが有効な場合は、新しいパスワードにユーザーのパスワードを変更します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>データ

### `Database.Execute(SQLstatement [,parameters]`

実行*SQLstatement* (省略可能なパラメーター) の INSERT、DELETE、または UPDATE などの影響を受けるレコードの数を返します。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

最近挿入された行から id 列を返します。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

指定されたデータベース ファイルまたは名前付き接続文字列を使用して指定されたデータベースのいずれかを開き、 *Web.config*ファイル。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

接続文字列を使用してデータベースを開きます。 (これとは対照的`Database.Open`、接続文字列名を使用します)。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

使用してデータベースを照会*SQLstatement* (必要に応じてパラメーターを渡す) し、結果をコレクションとして返します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

実行*SQLstatement* (省略可能なパラメーターは) を使用し、1 つのレコードを返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

実行*SQLstatement* (省略可能なパラメーターは) を使用し、1 つの値を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>ヘルパー

### `Analytics.GetGoogleHtml(webPropertyId)`

指定した ID の Google Analytics JavaScript コードを表示します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

指定されたプロジェクトの StatCounter Analytics JavaScript コードを表示します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

指定されたアカウント用の Yahoo Analytics JavaScript コードを表示します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

検索を Bing に渡します。 設定することができます、サイトを検索し、検索ボックスのタイトルを指定する、`Bing.SiteUrl`と`Bing.SiteTitle`プロパティです。 これらのプロパティを設定する通常、  *\_AppStart*ページ。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

グラフを初期化します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

グラフに凡例を追加します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

グラフには、一連の値を追加します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

指定されたデータのハッシュを返します。 既定のアルゴリズムは`sha256`します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Facebook ユーザー ページへの接続を作成することができます。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

ファイルをアップロードするには、UI をレンダリングします。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

指定した Xbox ゲーマー タグをレンダリングします。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

指定された電子メール アドレスの Gravatar イメージをレンダリングします。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

データ オブジェクトを JavaScript Object Notation (JSON) 形式の文字列に変換します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

JSON でエンコードされた入力文字列を反復処理したり、データベースに挿入できるデータ オブジェクトに変換します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

指定されたタイトルと省略可能な URL を使用して、ソーシャル ネットワーク リンクを表示します。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

エラー メッセージをフォーム フィールドに関連付けます。 使用して、`ModelState`このメンバーにアクセスするためのヘルパー。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

エラー メッセージをフォームに関連付けます。 使用して、`ModelState`このメンバーにアクセスするためのヘルパー。

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

検証エラーがない場合は true を返します。 使用して、`ModelState`このメンバーにアクセスするためのヘルパー。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

オブジェクト プロパティと、オブジェクトとその子の値を表示します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

ReCAPTCHA 検証テストを表示します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

ReCAPTCHA サービスのパブリックおよびプライベートのキーを設定します。 これらのプロパティを設定する通常、  *\_AppStart*ページ。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

ReCAPTCHA テストの結果を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

状態情報については、ASP.NET Web ページを表示します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

指定したユーザーの Twitter のストリームを表示します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

指定された検索テキストの Twitter のストリームを表示します。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

省略可能な幅と高さで指定したファイルのフラッシュ ビデオ プレーヤーをレンダリングします。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

省略可能な幅と高さで指定したファイルの Windows Media player をレンダリングします。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

指定された Silverlight プレーヤーをレンダリング *.xap*必要な幅と高さを持つファイルです。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

指定されたオブジェクトを返します*キー*、またはオブジェクトが見つからない場合は null です。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

指定されたオブジェクトを削除*キー*キャッシュからです。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

配置*値*で指定された名前でキャッシュに*キー*です。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

新たに作成`WebGrid`オブジェクト クエリからのデータを使用します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

HTML テーブルのデータを表示するマークアップを表示します。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

レンダリングのポケットベル、`WebGrid`オブジェクト。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

指定されたパスからイメージを読み込みます。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

ウォーターマークとして、指定されたイメージを追加します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

イメージを指定したテキストを追加します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

水平方向または垂直方向には、イメージを反転します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

ファイルのアップロード中にイメージがページにポストされたときに、イメージを読み込みます。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

サイズを変更する画像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

左または右に画像を回転します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

指定されたパスにイメージを保存します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

SMTP サーバーのパスワードを設定します。 このプロパティを設定する通常、  *\_AppStart*ページ。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

電子メールを送信します。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

SMTP サーバー名を設定します。 このプロパティを設定する通常、<em>\_AppStart</em>ページ。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

SMTP サーバーのユーザー名を設定します。 通常このプロパティを設定する必要があります、  *\_AppStart*ページ。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>検証

### `Html.ValidationMessage(field)`

(v2)指定したフィールドの検証エラー メッセージを表示します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2)すべての検証エラーの一覧を表示します。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2)検証の指定した型のユーザー入力要素を登録します。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2)検証エラー メッセージをフォーマットすることができるように、クライアント側の検証の CSS クラス属性を動的にレンダリングします。 (適切なクライアント スクリプト ライブラリを参照していると、CSS クラスを定義することが必要)。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2)ユーザーの入力フィールドのクライアント側の検証を有効にします。 (適切なクライアント スクリプト ライブラリを参照している必要があります)。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2)検証のための登録は、すべてのユーザー入力要素には、有効な値が含まれている場合に true を返します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2)ユーザーが、ユーザー入力要素の値を指定する必要がありますを指定します。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2)ユーザーがユーザー入力要素の各値を指定する必要がありますを指定します。 このメソッドでは、カスタム エラー メッセージを指定することはできません。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2)使用すると、検証テストを指定、`Validation.Add`メソッドです。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
