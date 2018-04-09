---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 ページ モデル |Microsoft ドキュメント
author: microsoft
description: ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。 分離コードは、いずれかの Src 属性を使用して実装可能性があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 ページ モデル
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。 Src 属性または分離コード属性を使用して分離コードを実装することも、@Pageディレクティブです。 ASP.NET 2.0 では、開発者がまだインライン コードと分離コードのいずれかを含むことが大幅に強化されて、分離コード モデルがあった。


ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。 Src 属性または分離コード属性を使用して分離コードを実装することも、@Pageディレクティブです。 ASP.NET 2.0 では、開発者がまだインライン コードと分離コードのいずれかを含むことが大幅に強化されて、分離コード モデルがあった。

## <a name="improvements-in-the-code-behind-model"></a>分離コード モデルの機能強化

ASP.NET で ASP.NET 2.0 の分離コード モデルの変更を完全に理解するために、最適なモデルをそのままを簡単に確認が存在していた 1.x です。

## <a name="the-code-behind-model-in-aspnet-1x"></a>Asp.net 分離コード モデル 1.x

ASP.NET で 1.x では、ASPX ファイル (web フォーム) とプログラミング コードを含む分離コード ファイルの分離コード モデルを行いました。 2 つのファイルを使用して接続されていた、 @Page ASPX ファイルのディレクティブ。 ASPX ページ上の各コントロールには、インスタンス変数として、分離コード ファイルに対応する宣言が必要があります。 また、分離コード ファイルはイベント バインド用のコードが含まれるし、Visual Studio デザイナーに必要なコードを生成します。 コードおよびコンテンツの完全な分離がなかった ASPX ページのすべての ASP.NET 要素には、分離コード ファイルに対応するコードが必要なためではこのモデルがよく、作業します。 たとえば場合は、デザイナーでは、Visual Studio IDE の外部で ASPX ファイルを新しいサーバー コントロールを追加、アプリケーション使用の分離コード ファイルにそのコントロールの宣言がないのためにできなくなります。

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 で分離コード モデル

ASP.NET 2.0 は、このモデルを大幅に向上します。 ASP.NET 2.0 で分離コードを実装する新しい*部分クラス*ASP.NET 2.0 で提供します。 ASP.NET 2.0 では、分離コード クラスは、クラス定義の一部のみが含まれているので、部分クラスとして定義します。 クラス定義の残りの部分は、実行時に、または Web サイトをプリコンパイル ASPX ページを使用して ASP.NET 2.0 によって動的に生成されます。 まだ @ Page ディレクティブを使用して、分離コード ファイルと ASPX ページ間のリンクが確立されます。 ただしではなく、分離コードまたは Src 属性 ASP.NET 2.0 今すぐ使用 CodeFile 属性。 Inherits 属性は、ページのクラス名を指定するも使用されます。

一般的な @ Page ディレクティブは、次のようになります。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

ASP.NET 2.0 の分離コード ファイル内の一般的なクラス定義は、次のようになります。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# および Visual Basic は、現在部分クラスをサポートしているマネージ言語のみです。 したがって、j# を使用する開発者は ASP.NET 2.0 で分離コード モデルを使用できません。


新しいモデルでは、開発者は自分が作成したコードのみを含むコード ファイルを持つようになりましたので、分離コード モデルが強化されます。 分離コード ファイル内の変数宣言のインスタンスが存在しないためもコードとコンテンツの完全な分離を提供します。

> [!NOTE]
> ASPX ページの部分クラスが、イベントのバインドが行われるために、Visual Basic 開発者は、分離コードでイベントをバインドする Handles キーワードを使用して、わずかなパフォーマンスが向上を実感できます。 C# の場合に、同等のキーワードがありません。


## <a name="new--page-directive-attributes"></a>新しい @ Page ディレクティブ属性

ASP.NET 2.0 では、@ Page ディレクティブを多数の新しい属性を追加します。 次の属性は、ASP.NET 2.0 の新機能です。

## <a name="async"></a>Async

Async 属性では、非同期的に実行する ページを構成することができます。 後でこのモジュールでの非同期のページについても説明します。

## <a name="asynctimeout"></a>AsyncTimeout

非同期のページのタイムアウトを指定します。 既定値は 45 秒です。

## <a name="codefile"></a>CodeFile

CodeFile 属性は、Visual Studio の 2002年/2003 での分離コード属性の代わりのです。

### <a name="codefilebaseclass"></a>CodeFileBaseClass

CodeFileBaseClass 属性は、複数のページを 1 つの基本クラスから派生させる必要がある場合に使用します。 この属性を持たない ASP.NET では、部分クラスの実装のため ASPX ページで宣言されているコントロールを参照する共有の共通のフィールドを使用する基本クラスが正しく機能しないため ASP です。ネット コンパイル エンジンでは、ページのコントロールに基づく新しいメンバーを自動的に作成されます。 したがって、する場合は、共通の基本クラスの 2 つまたは複数のページを ASP.NET で、必要がありますを定義する CodeFileBaseClass 属性で、基本クラスを指定し、その基底クラスから各ページのクラスを派生します。 CodeFile 属性がこの属性を使用する場合にも必要です。

## <a name="compilationmode"></a>CompilationMode

この属性では、ASPX ページの CompilationMode プロパティを設定することができます。 CompilationMode プロパティは、値を含む列挙**常に**、**自動**、および**Never**です。 既定値は**常に**です。 **自動**設定はコンパイルできないようにする ASP.NET 動的にページ可能な場合です。 動的コンパイルからページを除外すると、パフォーマンスが向上します。 ただし、除外されているページをコンパイルする必要があるコードが含まれている場合、エラーがスローされます、ページが参照されます。

## <a name="enableeventvalidation"></a>EnableEventValidation

この属性は、ポストバックとコールバックのイベントが検証されるかどうかを指定します。 これを有効にすると、引数をポストバックまたはコールバック イベントはそれらを最初に表示するサーバー コントロールから発生したことを確認するチェックされます。

## <a name="enabletheming"></a>EnableTheming

この属性は、ASP.NET のテーマがページで使用されるかどうかを指定します。 既定値は **false** です。 ASP.NET のテーマは、「[モジュール 10](profiles-themes-and-web-parts.md)です。

## <a name="linepragmas"></a>LinePragmas

この属性は、コンパイル時に行プラグマを追加するかどうかを指定します。 行プラグマは、コードの特定のセクションをマークする、デバッガーによって使用されるオプションです。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

この属性は、ポストバック間でのスクロール位置を維持するために、ページに JavaScript が挿入されるかどうかを指定します。 この属性は**false**既定です。

この属性の場合は**true**、ASP.NET は追加、&lt;スクリプト&gt;次のようなポストバック時にブロック。

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

このスクリプト ブロックの src は WebResource.axd であることに注意してください。 このリソースは、物理パスではありません。 このスクリプトが要求されると、ASP.NET は、スクリプトを動的に構築します。

### <a name="masterpagefile"></a>MasterPageFile

この属性は、現在のページのマスター ページ ファイルを指定します。 相対パスまたは絶対パスができます。 マスター ページは、「[第 4 章](master-pages.md)です。

## <a name="stylesheettheme"></a>StyleSheetTheme

この属性では、ASP.NET 2.0 テーマによって定義されたユーザー インターフェイスの外観プロパティをオーバーライドすることができます。 テーマは、「[モジュール 10](profiles-themes-and-web-parts.md)です。

## <a name="theme"></a>テーマ

ページのテーマを指定します。 StyleSheetTheme 属性の値が指定されていない場合、Theme 属性は、ページ上のコントロールに適用されるすべてのスタイルをオーバーライドします。

## <a name="title"></a>Title

ページのタイトルを設定します。 ここで指定した値に表示されます、&lt;タイトル&gt;レンダリングされるページの要素。

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

ViewStateEncryptionMode 列挙の値を設定します。 使用できる値は**常に**、**自動**、および**Never**です。 既定値は**自動**です。値にこの属性を設定すると**自動**、viewstate が暗号化を呼び出して、コントロールが要求したが、 **RegisterRequiresViewStateEncryption**メソッドです。

## <a name="setting-public-property-values-via-the--page-directive"></a>パブリック プロパティの値を使用して、@ Page ディレクティブの設定

ASP.NET 2.0 では、@ Page ディレクティブの別の新機能は、基本クラスのパブリック プロパティの初期値を設定する機能です。 すると、たとえば、パブリック プロパティがあることと呼ばれる**しれません**で基底クラスのせることに初期化された**こんにちは**ページが読み込まれるときにします。 これを行う @ Page ディレクティブで値を設定するだけで次のようにします。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**しれません**@ Page ディレクティブの属性では、しれませんプロパティの初期値を設定する基本クラスで*こんにちは!*です。 次のビデオは、@ Page ディレクティブを使用して、基底クラスのパブリック プロパティの初期値の設定のチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image1.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>ページのクラスの新しいのパブリック プロパティ

次のパブリック プロパティは、ASP.NET 2.0 の新機能です。

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

アプリケーションの相対パス、ページまたはコントロールを返します。 たとえば、あるページhttp://app/folder/page.aspx、プロパティから返される ~/フォルダー/です。

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

仮想ディレクトリの相対パス、ページまたはコントロールを返します。 あるページをたとえばhttp://app/folder/page.aspx、プロパティから返される ~/folder/page.aspx です。

## <a name="asynctimeout"></a>AsyncTimeout

取得または非同期ページを処理するために使用されるタイムアウトを設定します。 (非同期ページとり上げます後述します。)

## <a name="clientquerystring"></a>ClientQueryString

要求された URL のクエリ文字列の部分を返す読み取り専用のプロパティです。 この値は、URL エンコードです。 HttpServerUtility クラスの UrlDecode メソッドを使用すると、デコードします。

## <a name="clientscript"></a>ClientScript

このプロパティは、クライアント側スクリプトの ASP.NETs 出力を管理するために使用する ClientScriptManager オブジェクトを返します。 (ClientScriptManager クラスは、このモジュールの後半に記載されて)。

## <a name="enableeventvalidation"></a>EnableEventValidation

このプロパティは、ポストバックとコールバックのイベントのイベントの検証が有効かどうかを制御します。 有効な場合、引数をポストバックまたはコールバック イベントを確認してからそれらを最初に表示するサーバー コントロールから発生したことを確認してください。

## <a name="enabletheming"></a>EnableTheming

このプロパティを取得またはのページに ASP.NET 2.0 テーマが適用されるかどうかを指定するブール値を設定します。

## <a name="form"></a>フォーム

このプロパティは、HtmlForm オブジェクトとして、ASPX ページの HTML フォームを返します。

## <a name="header"></a>Header

このプロパティは、ページ ヘッダーを含む HtmlHead オブジェクトへの参照を返します。 返された HtmlHead オブジェクトを使用するにはスタイル シート、メタ タグなどを取得/設定します。

## <a name="idseparator"></a>IdSeparator

この読み取り専用プロパティは、ASP.NET がページ上のコントロールの一意の ID を構築するときに、コントロールの識別子を区切るために使用される文字を取得します。 コードから直接使用するためのものではありません。

## <a name="isasync"></a>IsAsync

このプロパティは、非同期ページを使用できます。 このモジュールでは、非同期ページがについて説明します。

## <a name="iscallback"></a>IsCallback

この読み取り専用プロパティを返します**true**ページがコールバックの結果である場合。 このモジュールでは、コールバックがについて説明します。

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

この読み取り専用プロパティを返します**true**ページがページ間のポストバックの一部である場合。 このモジュールでは、クロス ページのポストバックがについて説明します。

## <a name="items"></a>項目

ページ コンテキストに格納されているすべてのオブジェクトを含む IDictionary インスタンスへの参照を返します。 コンテキストの有効期間中に使用できるし、この IDictionary オブジェクトに項目を追加できます。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

このプロパティは、ASP.NET がページのスクロールのポストバックが発生した後、ブラウザー内の位置を保持する JavaScript を生成するかどうかを制御します。 (このプロパティの詳細については、この章で説明した)。

## <a name="master"></a>マスター

この読み取り専用プロパティでは、マスター ページが適用されているページの MasterPage インスタンスへの参照を返します。

## <a name="masterpagefile"></a>MasterPageFile

取得またはページのマスター ページのファイル名を設定します。 このプロパティは、PreInit メソッドでのみ設定できます。

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

このプロパティを取得またはページの状態の最大長をバイト単位で設定します。 プロパティが正の数値に設定されている場合ページ ビュー ステートに分割されます。 複数の隠しフィールド指定されたバイト数を超えないようにします。 プロパティが負の数値の場合は、ビュー ステートはチャンクに分割されません。

## <a name="pageadapter"></a>PageAdapter

要求元ブラウザーのページを変更する PageAdapter オブジェクトへの参照を返します。

## <a name="previouspage"></a>PreviousPage

Server.Transfer のページ間のポストバックの場合、前のページへの参照を返します。

## <a name="skinid"></a>SkinID

ASP.NET 2.0 に適用するスキン ページを指定します。

## <a name="stylesheettheme"></a>StyleSheetTheme

このプロパティを取得または設定ページに適用されるスタイル シートです。

## <a name="templatecontrol"></a>TemplateControl

ページの親コントロールへの参照を返します。

## <a name="theme"></a>テーマ

取得または設定 ページに適用されている ASP.NET 2.0 のテーマの名前。 PreInit メソッドの前に、この値を設定する必要があります。

## <a name="title"></a>Title

このプロパティを取得またはページのヘッダーから取得した、ページのタイトルを設定します。

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

取得またはページの ViewStateEncryptionMode を設定します。 このモジュールには、このプロパティの詳細についてを参照してください。

## <a name="new-protected-properties-of-the-page-class"></a>ページのクラスの新しいプロテクト プロパティ

ASP.NET 2.0 では、ページ クラスの新しい保護されているプロパティを次に示します。

## <a name="adapter"></a>アダプター

デバイス上のページを表示するある ControlAdapter への参照が要求を返します。

## <a name="asyncmode"></a>AsyncMode

このプロパティは、ページが非同期的に処理されるかどうかを示します。 これは、使用するため、ランタイムによってコードで直接します。

## <a name="clientidseparator"></a>ClientIDSeparator

このプロパティは、コントロールの一意のクライアント Id を作成するときに、区切り記号として使用される文字を返します。 これは、使用するため、ランタイムによってコードで直接します。

## <a name="pagestatepersister"></a>PageStatePersister

このプロパティは、ページの PageStatePersister オブジェクトを返します。 このプロパティは、主に、ASP.NET コントロールの開発者によって使用します。

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

このプロパティは、ブラウザーのキャッシュ ファイルのパスに追加される一意 suffic を返します。 既定値は\_ \__ufps = と 6 桁の数字です。

## <a name="new-public-methods-for-the-page-class"></a>ページ クラスの新しいパブリック メソッド

次のパブリック メソッドは、ASP.NET 2.0 内のページ クラスにします。

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

このメソッドは、非同期ページの実行のイベント ハンドラー デリゲートを登録します。 このモジュールでは、非同期ページがについて説明します。

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

ページには、ページのスタイル シートでプロパティを適用します。

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

このメソッドの開始、非同期タスク。

### <a name="getvalidators"></a>GetValidators

指定されていない場合は、指定された検証グループまたは既定の検証グループの検証コントロールのコレクションを返します。

## <a name="registerasynctask"></a>RegisterAsyncTask

このメソッドは、新しい非同期タスクを登録します。 このモジュールでは、非同期ページがについて説明します。

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

この方法では ASP.NET ページ コントロールの状態を永続化する必要があります。

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

このメソッドは、ページの viewstate が暗号化を要求することを ASP.NET に指示します。

## <a name="resolveclienturl"></a>ResolveClientUrl

画像などのクライアント要求のために使用する相対 URL を返します。

## <a name="setfocus"></a>SetFocus

このメソッドは、ページが最初に読み込まれるときに指定されているコントロールにフォーカスが設定されます。

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

このメソッドはコントロールを不要になったコントロールの状態の永続化を必須として渡すときに登録を解除します。

## <a name="changes-to-the-page-lifecycle"></a>ページのライフ サイクルへの変更

ASP.NET 2.0 のページのライフ サイクルが大幅に変更されていないが認識する必要があるいくつかの新しいメソッドがあります。 以下を ASP.NET 2.0 ページ ライフ サイクルについて説明します。

## <a name="preinit-new-in-aspnet-20"></a>PreInit (新規の ASP.NET 2.0)

PreInit イベントは、開発者がアクセスできるライフ サイクルの早い段階です。 このイベントを追加できるようになりますプログラムでマスター ページ、ASP.NET 2.0 のテーマの変更、ASP.NET 2.0 プロファイルなどのプロパティにアクセスします。Viewstate がまだ適用されていない、ライフ サイクルでこの時点でのコントロールを実現することは重要ポストバックの状態の場合は。 そのため、開発者は、この段階で、コントロールのプロパティを変更する場合、ページのライフ サイクルの後半可能性があります上書きされます。

## <a name="init"></a>Init

ASP.NET から Init イベントが変更されていない 1.x です。 これは、読み取りまたはページ上のコントロールのプロパティを初期化する場所です。 このステージ、マスター ページ、テーマでなどは、ページに既に適用します。

## <a name="initcomplete-new-in-20"></a>(2.0) で新しい InitComplete

InitComplete イベントは、ページの初期化段階の最後に呼び出されます。 この時点で、ライフ サイクルでアクセスできますが、ページ上のコントロールの状態が設定されていません。

## <a name="preload-new-in-20"></a>プリロード (2.0 の新機能)

このイベントは、すべてのポストバック データが適用された後、ページの直前に呼び出されますが\_ロードします。

## <a name="load"></a>Load

ASP.NET から Load イベントが変更されていない 1.x です。

## <a name="loadcomplete-new-in-20"></a>(2.0) で新しい LoadComplete

LoadComplete イベントは、ページ読み込みの段階での最後のイベントです。 この段階では、ポストバックおよび viewstate すべてのデータがページに適用されました。

## <a name="prerender"></a>PreRender

ページに動的に追加されるコントロールを保持する正しく viewstate の場合は、それらを追加する最後の機会は PreRender イベントです。

## <a name="prerendercomplete-new-in-20"></a>(2.0) で新しい PreRenderComplete

PreRenderComplete 段階では、ページに追加されたすべてのコントロールと、ページを描画する準備ができます。 PreRenderComplete イベントは、最後のページの viewstate を保存する前に発生するイベントです。

## <a name="savestatecomplete-new-in-20"></a>(2.0) で新しい SaveStateComplete

SaveStateComplete イベントには、すべてのページの viewstate とコントロールの状態を保存した後すぐには呼び出されます。 これは、ページがブラウザーに実際に表示される前に、最後のイベントです。

## <a name="render"></a>レンダリング

Render メソッドが ASP.NET 以降変更されていない 1.x です。 これは、ここでは、HtmlTextWriter に初期化され、ページがブラウザーにレンダリングされます。

## <a name="cross-page-postback-in-aspnet-20"></a>ASP.NET 2.0 のページ間のポストバック

ASP.NET で 1.x では、ポストバックが同じページにポストするために必要です。 複数ページのポストバックが許可されていません。 ASP.NET 2.0 では、IButtonControl インターフェイス経由で別のページへのポストバックをする機能を追加します。 (ボタン、LinkButton、およびサード パーティのカスタム コントロールのほかの ImageButton) は、新しい IButtonControl インターフェイスを実装する任意のコントロールでは、PostBackUrl 属性を使用してこの新しい機能を利用できます。 次のコードでは、2 番目のページへのポストバックを Button コントロールを示します。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

ページがポストバック時に、ポストバックを開始するページは、2 ページ目で PreviousPage プロパティからアクセス可能です。 新しい web フォームを使用してこの機能は実装されて\_DoPostBackWithOptions クライアント側の関数は別のページへのコントロールのポストバック時に、ASP.NET 2.0 がページを表示します。 この JavaScript 関数は、クライアントにスクリプトを生成する新しい WebResource.axd ハンドラーによって提供されます。

次のビデオは、ページ間のポストバックのチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image2.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>ポストバックの詳細について

### <a name="viewstate"></a>Viewstate

要求がありますが自分で既にページ間のポストバック シナリオでは最初のページから、viewstate をどのようになるか。 結局のところ、IPostBackDataHandler を実装しない任意のコントロールがページ間のポストバックの 2 ページ目では、そのコントロールのプロパティにアクセスする権限が viewstate、経由での状態をように保持されます、ページの viewstate にアクセスする必要があります。 ASP.NET 2.0 と呼ばれる 2 番目のページの別の非表示フィールドを使用してこのシナリオの処理\_ \_PREVIOUSPAGE です。 \_ \_PREVIOUSPAGE フォーム フィールドは、最初のページの viewstate を含むため、2 番目のページにすべてのコントロールのプロパティにアクセスすることがあるできます。

### <a name="circumventing-findcontrol"></a>FindControl を回避する方法

ページ間のポストバックのビデオ チュートリアルでは、FindControl メソッドを使用すれば、最初のページにテキスト ボックス コントロールへの参照を取得します。 そのメソッドは、そのような目的に適してが FindControl 高価なは、追加のコードを記述する必要があります。 幸いにも、ASP.NET 2.0 では、多くのシナリオでは動作をこの目的のため FindControl する代わりを提供します。 方法ディレクティブを使用すると、TypeName または VirtualPath 属性を使用して、前のページへの厳密に型指定された参照があることができます。 TypeName 属性では、VirtualPath 属性では、仮想パスを使用して、前のページを参照することができます、前のページの種類を指定することができます。 方法ディレクティブを設定すると、後に、コントロールなどのパブリック プロパティを使用してアクセスを許可するを公開する必要があります。

## <a name="lab-1-cross-page-postback"></a>ラボ 1 ページ間のポストバック

このラボでは、ASP.NET 2.0 の新しいページ間のポストバック機能を使用するアプリケーションを作成します。

1. Visual Studio 2005 を開き、新しい ASP.NET Web サイトを作成します。
2. Page2.aspx と呼ばれる新しい web フォームを追加します。
3. デザイン ビューで、Default.aspx を開き、ボタン コントロールとテキスト ボックス コントロールを追加します。 

    1. ボタン コントロールの ID を与える**損害賠償**、テキスト ボックス コントロールの ID と**UserName**です。
    2. Page2.aspx にボタンの PostBackUrl プロパティを設定します。
4. ソース ビューで page2.aspx を開きます。
5. 次に示すように、@ 方法ディレクティブを追加します。
6. 次のコード ページを追加\_page2.aspx の分離コードの読み込み。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. ビルド [ビルド] メニューをクリックして、プロジェクトをビルドします。
8. Default.aspx の分離コードに次のコードを追加します。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. ページを変更する\_次 page2.aspx への読み込み。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. プロジェクトをビルドします。
11. プロジェクトを実行します。
12. テキスト ボックスに名前を入力し、ボタンをクリックします。
13. 結果は何ですか。

## <a name="asynchronous-pages-in-aspnet-20"></a>ASP.NET 2.0 での非同期のページ

ASP.NET で多くの競合の問題は、(Web サービスまたはデータベースの呼び出し) などの外部の呼び出し、ファイル IO の待機時間などの待機時間が原因です。ASP.NET アプリケーションに対して要求が行われると、ASP.NET はその要求を処理するのに 1 つのワーカー スレッドを使用します。 その要求では、要求が完了し、応答が送信されたまで、そのスレッドが所有しています。 ASP.NET 2.0 がページを非同期的に実行する機能を追加することでこの種の問題と待機時間に関する問題を解決しようとするとします。 つまり、ワーカー スレッドが、要求の開始し、し、それによって、使用可能なスレッド プールにすばやくを返す別のスレッドに追加の実行を渡すことができます。 ファイル IO、データベースの呼び出しなどが完了したら、新しいスレッドは、要求を終了するスレッド プールから取得されます。

ページを非同期的に実行する最初の手順を設定、 **Async**ようページ ディレクティブの属性。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

この属性では、ASP.NET ページの IHttpAsyncHandler を実装するように指示します。

次の手順は、メソッドを呼び出す AddOnPreRenderCompleteAsync PreRender の前に、ページのライフ サイクルの時点でです。 (このメソッドと通常呼ばれるページの\_ロードします)。AddOnPreRenderCompleteAsync メソッドは 2 つのパラメーターです。BeginEventHandler し、EndEventHandler します。 BeginEventHandler、EndEventHandler にパラメーターとして渡される、IAsyncResult を返します。

下のビデオは、非同期のページ要求のチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image3.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> EndEventHandler が完了するまで、非同期ページは、ブラウザーに表示されません。 されることがない状態が不明な一部の開発者に非同期要求を非同期コールバックのようなものとして認識します。 ことが重要であることを実現します。 非同期要求のメリットは、最初のワーカー スレッドなど、IO がバインドされていることによって競合が減少サービス新しい要求をスレッド プールに返されることです。


## <a name="script-callbacks-in-aspnet-20"></a>ASP.NET 2.0 のスクリプトのコールバック

Web 開発者は、コールバックと関連付けられているちらつきを防ぐための方法については常に説明しました。 ASP.NET で 1.x では、SmartNavigation がちらつき、回避するための最も一般的な方法が SmartNavigation クライアントでその実装の複雑さのため一部の開発者の問題の原因とします。 ASP.NET 2.0 では、スクリプトのコールバックでこの問題を説明します。 スクリプトのコールバックでは、JavaScript を使用して Web サーバーに対する要求に XMLHttp を利用します。 XMLHttp 要求は、ブラウザーの DOM を経由して操作できる、XML データを返します XMLHttp のコードは、ユーザーから、新しい WebResource.axd ハンドラーによって隠ぺいされます。

ASP.NET 2.0 のスクリプトのコールバックを構成するために必要ないくつかの手順があります。

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>手順 1: ICallbackEventHandler インターフェイスを実装します。

ASP.NET スクリプト コールバックに参加するいると、ページを認識するためには、ICallbackEventHandler インターフェイスを実装する必要があります。 こうことは、分離コード ファイルで次のようにします。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

できますもこれを行うため、@ Implements ディレクティブ like を使用します。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

通常、ASP.NET のインライン コードを使用する場合は、@ Implements ディレクティブを使用します。

## <a name="step-2--call-getcallbackeventreference"></a>手順 2: 呼び出し GetCallbackEventReference

前述のように、XMLHttp の呼び出しは、WebResource.axd ハンドラーでカプセル化されます。 ASP.NET が web フォームへの呼び出しを追加、ページが表示されると\_DoCallback、WebResource.axd によって提供されるクライアント スクリプトです。 Web フォーム\_DoCallback 関数は、 \_\_のコールバックを doPostBack 関数。 注意して\_ \_doPostBack がプログラムで、ページのフォームを送信します。 そのため、ポストバックを防ぐためにするシナリオでは、コールバック、 \_ \_doPostBack だけでは不十分です。

> [!NOTE]
> \_\_doPostBack は引き続きクライアント スクリプト コールバック シナリオ内のページに表示されます。 ただし、コールバックは使用されません。


Web フォームの引数\_DoCallback クライアント側の関数は、サーバー側の関数に通常のページで呼び出されます GetCallbackEventReference を通じて提供\_ロードします。 GetCallbackEventReference に通常の呼び出しは、次のようになります。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> この場合、cm、ClientScriptManager のインスタンスです。 ClientScriptManager クラスは、後でこのモジュールで取り上げます。


GetCallbackEventReference のいくつかのオーバー ロードされたバージョンがあります。 この場合、引数は次のとおりです。

`this`

GetCallbackEventReference が呼び出されているコントロールへの参照。 この場合、ページ自体を勧めします。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

サーバー側のイベントをクライアント側のコードから渡される文字列引数。 この場合、Im、ドロップダウンの値を渡すには、ddlCompany が呼び出されます。

`ShowCompanyName`

サーバー側のコールバック イベントからの (文字列) としての戻り値が許容するクライアント側の関数の名前。 この関数が、サーバー側のコールバックが成功した場合にのみ呼び出されます。 したがって、堅牢性、ここでは、通常は推奨エラーが発生した場合に実行するクライアント側の関数の名前を指定する追加の文字列引数を受け取る GetCallbackEventReference のオーバー ロードされたバージョンを使用します。

`null`

サーバーにコールバックの前に開始したクライアント側の関数を表す文字列。 ここではありません、このようなスクリプトのため、引数は null です。

`true`

コールバックを非同期的に実行するかどうかを指定するブール値。

Web フォームへの呼び出し\_DoCallback クライアントではこれらの引数を渡します。 そのため、クライアントでこのページが表示されると、そのコードになります次のようにします。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

クライアントでの関数のシグネチャが少し異なることに注意してください。 クライアント側の関数は、5 つの文字列とブール値を渡します。 (これは上記の例では null) 追加の文字列には、サーバー側のコールバックで発生したエラーを処理するクライアント側の関数が含まれています。

## <a name="step-3--hook-the-client-side-control-event"></a>手順 3: クライアント側コントロール イベントをフックします。

GetCallbackEventReference 上記の戻り値が文字列変数に割り当てられたことに注意してください。 その文字列を使用すると、コールバックを開始するコントロールのクライアント側のイベントをフックします。 この例では、コールバックによって開始されるページで、ドロップダウン リストをフックするため、 *OnChange*イベント。

次のようにクライアント側のマークアップにハンドラーを追加するだけをクライアント側のイベントにフックします。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

注意してください*cbRef* GetCallbackEventReference への呼び出しからの戻り値です。 Web フォームへの呼び出しが含まれている\_DoCallback 上に表示されました。

## <a name="step-4--register-the-client-side-script"></a>手順 4: クライアント側スクリプトを登録します。

GetCallbackEventReference への呼び出しは、クライアント側スクリプトと呼ばれることを指定すること**ShowCompanyName**サーバー側のコールバックが成功したときに実行されます。 そのスクリプトは、ClientScriptManager インスタンスを使用して、ページに追加する必要があります。 (ClientScriptManager クラスになりますこのモジュールの後で示す)。そのようなのでを実行します。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>手順 5: ICallbackEventHandler インターフェイスのメソッドを呼び出す

ICallbackEventHandler には、コードで実装する必要のある 2 つのメソッドが含まれています。 **RaiseCallbackEvent**と**GetCallbackEvent**です。

**RaiseCallbackEvent**を引数として文字列を受け取り、nothing を返します。 Web フォームのクライアント側呼び出しから文字列引数が渡された\_DoCallback です。 この場合、その値は、*値*ddlCompany と呼ばれる、ドロップダウンの属性です。 RaiseCallbackEvent メソッドには、サーバー側コードを配置する必要があります。 など、コールバックが外部のリソースに対して WebRequest を行っている場合は、そのコードを RaiseCallbackEvent に配置する必要があります。

**GetCallbackEvent**をクライアントにコールバックの戻り値を処理します。 引数を使用しないと、文字列を返します。 返される文字列は、引数として関数に渡す、クライアント側、ここでは*ShowCompanyName*です。

上記の手順を完了すると、ASP.NET 2.0 では、スクリプトのコールバックを実行する準備ができたらです。


![](the-asp-net-2-0-page-model/_static/image4.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/callback1.wmv)


ASP.NET でのスクリプトのコールバックは、XMLHttp 呼び出すをサポートするブラウザーでサポートされます。 すべての最新ブラウザーを含むを使用して今日。 Internet Explorer は、(予定されている IE 7 を含む) の他の最新のブラウザーが組み込み XMLHttp オブジェクトを使用して、XMLHttp ActiveX オブジェクトを使用します。 プログラムで使用することができます、ブラウザーがコールバックをサポートする場合を決定する、 **Request.Browser.SupportCallback**プロパティです。 このプロパティは**true**要求元のクライアントは、スクリプトのコールバックをサポートしている場合。

## <a name="working-with-client-script-in-aspnet-20"></a>ASP.NET 2.0 内のクライアント スクリプトの操作

クライアント スクリプトを ASP.NET 2.0 では、ClientScriptManager クラスを使用して管理されます。 ClientScriptManager クラスの追跡クライアント スクリプトを型および名前を使用します。 これは、同じスクリプトがプログラムによって挿入されるページに複数回することを防ぎます。

> [!NOTE]
> スクリプトがページに正常に登録された後後続しようとすると、同じスクリプトを登録するだけになります、もう一度登録されていないスクリプト。 スクリプトが重複しては追加されず、例外が発生しません。 不要な計算を回避するのを決定できるように、複数回登録しないで、スクリプトは既に登録されてかどうかに使用できる方法があります。


ClientScriptManager のメソッドは、現在のすべての ASP.NET 開発者にについて理解する必要があります。

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

このメソッドは、スクリプトを表示するページの上部に追加します。 これは、クライアントで明示的に呼び出される関数を追加するために役立ちます。

このメソッドの 2 つのオーバー ロードされたバージョンがあります。 3 つの 4 つの引数は、それらの間で共通です。 それらは次のとおりです。

`type (string)`

***型***引数は、スクリプトの種類を識別します。 一般に、(このページの種類を使用することをお勧め。型の GetType()) します。

`key (string)`

***キー***引数は、スクリプトのユーザー定義のキー。 これは、スクリプトごとに一意にする必要があります。 同じキーと、既に追加されたスクリプトの種類のスクリプトを追加しようとする場合に追加されません。

`script (string)`

***スクリプト***引数は、追加する実際のスクリプトを含む文字列。 スクリプトを作成し、StringBuilder の ToString() メソッドを使用して割り当てるには、StringBuilder を使用することをお勧めしますが、***スクリプト***引数。

のみ次の 3 つの引数を受け取るオーバー ロードされた RegisterClientScriptBlock を使用する場合は、スクリプト要素を含める必要があります (&lt;スクリプト&gt;と&lt;/script&gt;)、スクリプトでします。

4 番目の引数を受け取る RegisterClientScriptBlock のオーバー ロードを使用することができます。 4 番目の引数は、ASP.NET でのスクリプト要素を追加する必要があるかどうかを指定するブール値です。 この引数は場合**true**スクリプトはスクリプト要素は明示的に含まれません。

IsClientScriptBlockRegistered メソッドを使用して、スクリプトは既に登録されているかどうかを判断します。 これによりは既に登録されているスクリプトを再登録の試行を回避できます。

### <a name="registerclientscriptinclude-new-in-20"></a>(2.0) で新しい RegisterClientScriptInclude

RegisterClientScriptInclude タグは、外部スクリプト ファイルにリンクしているスクリプト ブロックを作成します。 2 つのオーバー ロードがあります。 1 つは、キーと URL を受け取ります。 2 つ目は、型を指定する 3 番目の引数を追加します。

たとえば、次のコードが生成されます、スクリプト ブロック jsfunctions.js にリンクしているアプリケーションのスクリプト フォルダーのルートに。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

このコードは、レンダリングされるページで次のコードを生成します。

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> スクリプト ブロックは、ページの下部に表示されます。


IsClientScriptIncludeRegistered メソッドを使用して、スクリプトは既に登録されているかどうかを判断します。 これにより、スクリプトを再登録の試行を回避できます。

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript メソッドは、RegisterClientScriptBlock メソッドと同じ引数を受け取ります。 RegisterStartupScript に登録されているスクリプトは、ページが読み込まれた後、OnLoad クライアント側イベントの前に実行します。 終了の直前に配置されていた RegisterStartupScript に登録されたスクリプト 1.X で&lt;/form&gt; RegisterClientScriptBlock に登録されたスクリプトが開始した直後に配置されていたときにタグを付ける&lt;フォーム&gt;タグ。 ASP.NET 2.0 の両方が終了する直前に配置されます。 &lt;/form&gt;タグ。

> [!NOTE]
> RegisterStartupScript で関数を登録した場合、クライアント側のコードで明示的に呼び出されるまでその関数が実行されません。


スクリプトは既に登録されているかどうかを確認し、スクリプトを再登録の試行を回避するには、IsStartupScriptRegistered メソッドを使用します。

## <a name="other-clientscriptmanager-methods"></a>その他の ClientScriptManager メソッド

ClientScriptManager クラスの他の便利なメソッドのいくつか示します。


|  <strong>GetCallbackEventReference</strong>   |                                                 このモジュールで前述のスクリプトのコールバックを参照してください。                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                JavaScript の参照を取得 (javascript:&lt;呼び出す&gt;) を使用してクライアント側のイベントからの投稿をことができます。                 |
|  <strong>GetPostBackEventReference</strong>   |                                   クライアントからの投稿を開始するために使用する文字列を取得します。                                    |
|      <strong>GetWebResourceUrl</strong>       | アセンブリに埋め込まれているリソースへの URL を返します。 組み合わせて使用する必要があります<strong>RegisterClientScriptResource</strong>です。 |
| <strong>RegisterClientScriptResource</strong> |     ページに Web リソースを登録します。 これらは、リソース アセンブリに埋め込まれているし、新しい WebResource.axd ハンドラーによって処理されます。      |
|     <strong>RegisterHiddenField</strong>      |                                                 ページを非表示のフォーム フィールドを登録します。                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  HTML フォームが送信されるときに実行されるクライアント側のコードを登録します。                                   |

