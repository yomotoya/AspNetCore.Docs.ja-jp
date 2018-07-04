---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 ページ モデル |Microsoft Docs
author: microsoft
description: Asp.net 1.x では、開発者は、インライン コード モデルとコード分離コード モデルの選択を必要があります。 分離コードは、いずれかの Src 属性を使用して実装できます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379888"
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 ページ モデル
====================
によって[Microsoft](https://github.com/microsoft)

> Asp.net 1.x では、開発者は、インライン コード モデルとコード分離コード モデルの選択を必要があります。 Src 属性または分離コード属性を使用して、分離コードを実装することも、@Pageディレクティブ。 ASP.NET 2.0 では、開発者はインライン コードと分離コードの間の選択肢があるが分離コード モデルを大幅に強化されました。


Asp.net 1.x では、開発者は、インライン コード モデルとコード分離コード モデルの選択を必要があります。 Src 属性または分離コード属性を使用して、分離コードを実装することも、@Pageディレクティブ。 ASP.NET 2.0 では、開発者はインライン コードと分離コードの間の選択肢があるが分離コード モデルを大幅に強化されました。

## <a name="improvements-in-the-code-behind-model"></a>分離コード モデルの機能強化

ASP.NET 2.0 で分離コード モデルの変更を完全に理解するためにモデルをそのままをすばやく確認することをお勧めは ASP.NET に存在していた 1.x します。

## <a name="the-code-behind-model-in-aspnet-1x"></a>Asp.net 分離コード モデル 1.x

Asp.net 1.x では、ASPX ファイル (web フォーム) およびプログラミング コードを含む分離コード ファイルの分離コード モデルを行いました。 2 つのファイルを使用して接続されていた、@Pageを ASPX ファイルにディレクティブ。 ASPX ページ上の各コントロールには、インスタンス変数として、分離コード ファイルに対応する宣言が必要があります。 分離コード ファイルもイベント バインドのコードに含まれているし、Visual Studio のデザイナーに必要なコードを生成します。 このモデルはかなりよく動作しましたが、コードとコンテンツの完全な分離がないため、ASPX ページのすべての ASP.NET 要素には、分離コード ファイルに対応するコードが必要に応じて、します。 たとえば、デザイナーでは、Visual Studio IDE の外部で ASPX ファイルを新しいサーバー コントロールが追加された場合、アプリケーション使用分離コード ファイルでそのコントロールの宣言がないのためにできなくなります。

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 では、分離コード モデル

ASP.NET 2.0 は、このモデルが大幅に向上します。 新しいを使用して ASP.NET 2.0 で分離コードが実装されます*部分クラス*ASP.NET 2.0 で提供します。 ASP.NET 2.0 では、分離コード クラスは、つまりクラス定義の一部のみが含まれている部分クラスとして定義します。 クラス定義の残りの部分は、実行時に、または Web サイトをプリコンパイル済みの ASPX ページを使用して ASP.NET 2.0 を動的に生成します。 まだ @ Page ディレクティブを使用して、分離コード ファイルと ASPX ページ間のリンクが確立されます。 ただし、分離コード ファイルまたは Src 属性ではなく ASP.NET 2.0 では、CodeFile 属性。 Inherits 属性は、ページのクラス名を指定するも使用されます。

一般的な @ Page ディレクティブは、次のようになります。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

ASP.NET 2.0 の分離コード ファイルでの一般的なクラス定義は、次のようになります。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# および Visual Basic は、部分クラスがサポートされているマネージ言語のみです。 そのため、j# を使用する開発者は ASP.NET 2.0 で分離コード モデルを使用できません。


新しいモデルは、開発者は自分が作成したコードのみを含むコード ファイルがあるようになりましたので、分離コード モデルを強化します。 分離コード ファイル内の変数宣言のインスタンスが存在しないためもコードとコンテンツの完全な分離を提供します。

> [!NOTE]
> ASPX ページの部分クラスは、イベントのバインドが行われる場所であるために、Visual Basic 開発者は、分離コードでイベントをバインドする Handles キーワードを使用して、わずかなパフォーマンスが向上を実現できます。 C# には、同等のキーワードがありません。


## <a name="new--page-directive-attributes"></a>新しい @ Page ディレクティブの属性

ASP.NET 2.0 では、@ Page ディレクティブに多くの新しい属性を追加します。 次の属性は、ASP.NET 2.0 の新機能です。

## <a name="async"></a>Async

非同期の属性を使用すると、非同期的に実行するページを構成できます。 このモジュールの後で非同期ページについても説明します。

## <a name="asynctimeout"></a>AsyncTimeout

非同期ページのタイムアウトを指定します。 既定値には 45 秒です。

## <a name="codefile"></a>CodeFile

CodeFile 属性は、Visual Studio 2002/2003 での分離コード属性の代わりです。

### <a name="codefilebaseclass"></a>CodeFileBaseClass

CodeFileBaseClass 属性は、複数のページ 1 つの基本クラスから派生する必要がある場合に使用されます。 この属性がない場合、ASP.NET では、部分クラスの実装のため、ASPX ページで宣言されたコントロールを参照する共有の共通フィールドを使用する基本クラスが正しく動作しないため、ASP します。ネット コンパイル エンジンでは、ページ内のコントロールに基づく新しいメンバーが自動的に作成します。 そのため、ASP.NET の 2 つまたは複数のページに共通の基本クラスを必要な場合に必要定義 CodeFileBaseClass 属性の基底クラスを指定し、その基底クラスから各ページのクラスを派生します。 CodeFile 属性がこの属性を使用する場合にも必要です。

## <a name="compilationmode"></a>CompilationMode

この属性の ASPX ページ CompilationMode プロパティを設定することができます。 CompilationMode プロパティは、値を含む列挙**常に**、**自動**、および**Never**します。 既定値は**常に**します。 **自動**設定可能であれば、ページのコンパイルを動的に ASP.NET を有効になります。 動的コンパイルからページを除外すると、パフォーマンスが向上します。 ただし、除外されているページにそのコードをコンパイルする必要がありますが含まれている場合、エラーがスローされます、ページが参照されます。

## <a name="enableeventvalidation"></a>EnableEventValidation

この属性は、ポストバックおよびコールバック イベントを検証するかどうかを指定します。 これを有効にすると、ポストバックの引数またはコールバック イベントはそれらを最初に表示されるサーバー コントロールから発生したことを確認するチェックされます。

## <a name="enabletheming"></a>EnableTheming

この属性は、ページで、ASP.NET のテーマを使用するかどうかを指定します。 既定値は **false** です。 ASP.NET のテーマは、「[モジュール 10](profiles-themes-and-web-parts.md)します。

## <a name="linepragmas"></a>LinePragmas

この属性は、コンパイル時に、行プラグマを追加するかどうかを指定します。 行プラグマは、コードの特定のセクションをマークするデバッガーによって使用されるオプションです。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

この属性は、ポストバック間でのスクロール位置を維持するために、ページに JavaScript が挿入されたかどうかを指定します。 この属性は**false**既定。

この属性の場合は**true**、ASP.NET は追加、&lt;スクリプト&gt;次のようなポストバック時にブロック。

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

このスクリプト ブロックの src が WebResource.axd であることに注意してください。 このリソースは、物理パスではありません。 このスクリプトが要求されると、ASP.NET では、スクリプトが動的に作成します。

### <a name="masterpagefile"></a>MasterPageFile

この属性は、現在のページのマスター ページファイルを指定します。 相対パスまたは絶対パスができます。 マスター ページに掲載されて[モジュール 4](master-pages.md)します。

## <a name="stylesheettheme"></a>StyleSheetTheme

この属性では、ASP.NET 2.0 テーマによって定義されたユーザー インターフェイスの外観プロパティをオーバーライドできます。 テーマは、「[モジュール 10](profiles-themes-and-web-parts.md)します。

## <a name="theme"></a>テーマ

ページのテーマを指定します。 StyleSheetTheme 属性の値が指定されていない場合、Theme 属性は、ページ上のコントロールに適用されるすべてのスタイルをオーバーライドします。

## <a name="title"></a>Title

ページのタイトルを設定します。 ここで指定した値が表示されます、&lt;タイトル&gt;レンダリングされたページの要素。

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

ViewStateEncryptionMode 列挙の値を設定します。 使用できる値は**常に**、**自動**、および**Never**します。 既定値は**自動**します。値にこの属性を設定すると**自動**、viewstate が暗号化されて、コントロールが呼び出すことによって要求が、 **RegisterRequiresViewStateEncryption**メソッド。

## <a name="setting-public-property-values-via-the--page-directive"></a>パブリック プロパティの値を使用して、@ Page ディレクティブの設定

ASP.NET 2.0 の @ Page ディレクティブのもう 1 つの新しい機能は、基底クラスのパブリック プロパティの初期値を設定する機能です。 たとえば、というパブリック プロパティがあるとします**しれません**基底クラスとそのせることに初期化する**こんにちは**ページが読み込まれるときにします。 @ Page ディレクティブの値を設定するだけでこれを実現できますようになります。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**しれません**@ Page ディレクティブの属性を基底クラスでしれませんプロパティの初期値を設定する*こんにちは!* します。 次のビデオは、@ Page ディレクティブを使用して、基底クラスのパブリック プロパティの初期値の設定のチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image1.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>ページ クラスの新しいパブリック プロパティ

次のパブリック プロパティは、ASP.NET 2.0 の新機能。

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

ページまたはコントロールをアプリケーション相対パスを返します。 たとえば、あるページhttp://app/folder/page.aspxプロパティを返します。 ~/フォルダー/。

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

ページまたはコントロールには、仮想ディレクトリの相対パスを返します。 あるページなどのhttp://app/folder/page.aspxプロパティを返します。 ~/folder/page.aspx します。

## <a name="asynctimeout"></a>AsyncTimeout

取得または非同期ページ処理のために使用されるタイムアウトを設定します。 (非同期ページはこのモジュールの後で説明されます)。

## <a name="clientquerystring"></a>ClientQueryString

要求された URL のクエリ文字列の部分を返す読み取り専用のプロパティ。 この値は、URL エンコードします。 HttpServerUtility クラスの UrlDecode メソッドを使用して、デコードすることができます。

## <a name="clientscript"></a>ClientScript

このプロパティは、クライアント側スクリプトの出力を ASP.NETs の管理に使用できる、ClientScriptManager オブジェクトを返します。 (ClientScriptManager クラスはこのモジュールの後半で説明します。)

## <a name="enableeventvalidation"></a>EnableEventValidation

このプロパティは、ポストバックおよびコールバック イベントのイベントの検証が有効になっているかどうかを制御します。 有効な場合、ポストバックの引数またはコールバック イベントが最初に表示されるサーバー コントロールから発生したことを確認する検証されます。

## <a name="enabletheming"></a>EnableTheming

このプロパティを取得または設定ページに、ASP.NET 2.0 テーマが適用されるかどうかを指定するブール値。

## <a name="form"></a>フォーム

このプロパティは、ASPX ページの HTML フォームを HtmlForm オブジェクトとして返します。

## <a name="header"></a>Header

このプロパティは、ページ ヘッダーを含む HtmlHead オブジェクトへの参照を返します。 スタイル シート、メタ タグなどを取得/設定は、返された HtmlHead オブジェクトを使用できます。

## <a name="idseparator"></a>IdSeparator

この読み取り専用プロパティは、ASP.NET がページ上のコントロールの一意の ID を作成するときにコントロール id を区別するために使用する文字を取得します。 コードから直接使用するためのものではありません。

## <a name="isasync"></a>IsAsync

このプロパティは、非同期ページを使用します。 非同期ページは、このモジュールの後で説明します。

## <a name="iscallback"></a>IsCallback

この読み取り専用プロパティを返します**true**ページがコールバックの結果である場合。 このモジュールでは、コールバックがについて説明します。

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

この読み取り専用プロパティを返します**true**ページがページ間ポストバックの一部である場合。 このモジュールでは、ページ間ポストバックがについて説明します。

## <a name="items"></a>項目

ページ コンテキストに格納されているすべてのオブジェクトを含む IDictionary インスタンスへの参照を返します。 この IDictionary オブジェクトに項目を追加することができ、コンテキストの有効期間全体で使用可能になります。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

このプロパティは、ASP.NET がページのスクロール、ポストバックが発生した後、ブラウザー内の位置を保持する JavaScript を生成するかどうかを制御します。 (このプロパティの詳細については、この章で説明した)。

## <a name="master"></a>マスター

この読み取り専用プロパティは、マスター ページが適用されているページのマスター インスタンスへの参照を返します。

## <a name="masterpagefile"></a>MasterPageFile

取得またはページのマスター ページのファイル名を設定します。 このプロパティは、PreInit メソッドでのみ設定できます。

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

このプロパティを取得またはページの状態の最大長をバイト単位で設定します。 プロパティが正の数に設定されている場合、ページのビュー ステートに分割する複数の非表示フィールド指定されたバイト数を超えないようにします。 プロパティが負の数値の場合は、ビュー ステートはチャンクに分割されません。

## <a name="pageadapter"></a>PageAdapter

要求元ブラウザーのページを変更する PageAdapter オブジェクトへの参照を返します。

## <a name="previouspage"></a>PreviousPage

Server.Transfer、または、ページ間ポストバックの場合、前のページへの参照を返します。

## <a name="skinid"></a>SkinID

ページに適用する ASP.NET 2.0 のスキンを指定します。

## <a name="stylesheettheme"></a>StyleSheetTheme

このプロパティを取得または設定ページに適用されるスタイル シート。

## <a name="templatecontrol"></a>TemplateControl

ページの親コントロールへの参照を返します。

## <a name="theme"></a>テーマ

取得または設定 ページに適用されている ASP.NET 2.0 テーマの名前。 この値は、PreInit メソッドの前に設定する必要があります。

## <a name="title"></a>Title

このプロパティを取得またはページのヘッダーから取得した、ページのタイトルを設定します。

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

取得またはページの ViewStateEncryptionMode を設定します。 このモジュールは、このプロパティの詳細についてを参照してください。

## <a name="new-protected-properties-of-the-page-class"></a>ページ クラスの新しい保護されているプロパティ

ASP.NET 2.0 ページ クラスの新しい保護されているプロパティを次に示します。

## <a name="adapter"></a>アダプター

デバイス上のページをレンダリングする ControlAdapter への参照が要求を返します。

## <a name="asyncmode"></a>AsyncMode

このプロパティは、ページが非同期的に処理されるかどうかを示します。 ランタイムによって、コードで直接使用するものでは。

## <a name="clientidseparator"></a>ClientIDSeparator

このプロパティは、コントロールの一意のクライアント Id を作成するときに、区切り記号として使用される文字を返します。 ランタイムによって、コードで直接使用するものでは。

## <a name="pagestatepersister"></a>PageStatePersister

このプロパティは、ページの PageStatePersister オブジェクトを返します。 このプロパティは、主に ASP.NET コントロールの開発者によって使用します。

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

このプロパティは、ブラウザーのキャッシュのファイル パスに追加される一意 suffic を返します。 既定値は\_ \__ufps = と 6 桁の数字です。

## <a name="new-public-methods-for-the-page-class"></a>ページ クラスの新しいパブリック メソッド

次のパブリック メソッドは、ASP.NET 2.0 ページ クラスにします。

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

このメソッドは、非同期ページの実行のイベント ハンドラー デリゲートを登録します。 非同期ページは、このモジュールの後で説明します。

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

ページのスタイル シート内のプロパティをページに適用されます。

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

このメソッドを相手の非同期タスク。

### <a name="getvalidators"></a>GetValidators

指定されていない場合は、指定した検証グループまたは既定の検証グループの検証コントロールのコレクションを返します。

## <a name="registerasynctask"></a>RegisterAsyncTask

このメソッドは、新しい非同期タスクを登録します。 非同期ページは、このモジュールの後半で説明します。

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

このメソッドは、ページ コントロールの状態を永続化する必要を ASP.NET に指示します。

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

このメソッドは、ページの viewstate が暗号化を要求することを ASP.NET に指示します。

## <a name="resolveclienturl"></a>ResolveClientUrl

画像などのクライアント要求のために使用する相対 URL を返します。

## <a name="setfocus"></a>SetFocus

このメソッドは、ページが最初に読み込まれるときに指定されているコントロールにフォーカスが設定されます。

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

このメソッドは不要になったコントロールの状態の永続化を要求するように渡されるコントロールに登録を解除します。

## <a name="changes-to-the-page-lifecycle"></a>ページのライフ サイクルへの変更

ASP.NET 2.0 では、ページ ライフ サイクルが大幅に変更されていないが意識する必要があるいくつかの新しいメソッドがあります。 ASP.NET 2.0 ページのライフ サイクルを以下に示します。

## <a name="preinit-new-in-aspnet-20"></a>PreInit (ASP.NET 2.0) の新機能

PreInit イベントは、開発者がアクセスできるライフ サイクルの早い段階です。 このイベントの追加により、プログラムでマスター ページ、ASP.NET 2.0 テーマの変更、ASP.NET 2.0 プロファイルなどのプロパティにアクセスすることです。場合は、Viewstate がまだ適用されていない、ライフ サイクルの時点でコントロールを実現することが重要なポストバックの状態にしています。 そのため、開発者は、この段階で、コントロールのプロパティを変更する場合可能性上書きされますページ ライフ サイクルの後半。

## <a name="init"></a>Init

ASP.NET から Init イベントが変更されていない 1.x します。 これは、読み取り、または、ページ上のコントロールのプロパティを初期化するとします。 このステージ、マスター ページ、テーマなどは、ページに既に適用します。

## <a name="initcomplete-new-in-20"></a>InitComplete (2.0) の新機能

InitComplete イベントは、ページの初期化ステージの終了時に呼び出されます。 この時点で、ライフ サイクルにアクセスできます ページで、コントロールがその状態が設定されていません。

## <a name="preload-new-in-20"></a>(2.0) の新機能の事前読み込み

このイベントは、すべてのポストバック データが適用された後、ページの直前に呼び出されますが\_ロードします。

## <a name="load"></a>Load

ASP.NET から読み込みイベントが変更されていない 1.x します。

## <a name="loadcomplete-new-in-20"></a>LoadComplete (2.0) の新機能

LoadComplete イベントは、ページの読み込み段階の最後のイベントです。 この段階では、ポストバックと viewstate のすべてのデータがページに適用されました。

## <a name="prerender"></a>PreRender

ページに動的に追加されるコントロールを正しく保持する viewstate の場合は、PreRender イベントはそれらを追加する最後の機会にします。

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (2.0) の新機能

PreRenderComplete 段階では、ページに追加されたすべてのコントロールと、ページを表示する準備ができます。 PreRenderComplete イベントは、ページの viewstate を保存する前に発生した最後のイベントです。

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (2.0) の新機能

SaveStateComplete イベントは、すべてのページの viewstate およびコントロールの状態が保存された直後後に呼び出されます。 これは、ページがブラウザーに実際にレンダリングされる前に、最後のイベントです。

## <a name="render"></a>レンダリング

Render メソッドが ASP.NET 以降変更されていない 1.x します。 これは、場所、HtmlTextWriter は初期化されており、ページがブラウザーに表示されます。

## <a name="cross-page-postback-in-aspnet-20"></a>ASP.NET 2.0 では、ページ間ポストバック

Asp.net 1.x では、ポストバックが同じページに投稿するために必要です。 ページ間ポストバックが許可されていません。 ASP.NET 2.0 では、IButtonControl インターフェイス経由で別のページにポストバックする機能を追加します。 (ボタン、LinkButton、およびサードパーティ製のカスタム コントロールだけでなく ImageButton) は、新しい IButtonControl インターフェイスを実装する任意のコントロールでは、PostBackUrl 属性を使用してこの新しい機能を利用できます。 次のコードでは、2 番目のページにポストバックするボタン コントロールを示します。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

ページがポストバック時に、ポストバックを開始するページが 2 番目のページの PreviousPage プロパティ経由でアクセスできます。 新しい web フォームを使用してこの機能は実装\_DoPostBackWithOptions クライアント側の関数は別のページへのコントロールのポストバック時に、ページに ASP.NET 2.0 を表示します。 この JavaScript 関数は、クライアントにスクリプトを生成する新しい WebResource.axd ハンドラーによって提供されます。

次のビデオは、ページ間ポストバックのチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image2.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>詳細については、ページ間ポストバック

### <a name="viewstate"></a>Viewstate

可能性がありますが求め自分で既にページ間ポストバック シナリオの最初のページから、viewstate に起こったことについてです。 結局のところ、IPostBackDataHandler を実装していない任意のコントロールは、ページ間ポストバックの 2 ページ目では、そのコントロールのプロパティにアクセスする、viewstate を使用してその状態のため保持されます、ページの viewstate にアクセスする必要があります。 ASP.NET 2.0 と呼ばれる 2 番目のページで新しい非表示フィールドを使用してこのシナリオに任せ\_ \_PREVIOUSPAGE します。 \_ \_PREVIOUSPAGE フォーム フィールドは、2 番目のページですべてのコントロールのプロパティにアクセスができるように、最初のページの viewstate を含まれています。

### <a name="circumventing-findcontrol"></a>FindControl の回避

ページ間ポストバックのビデオ チュートリアル、FindControl メソッドを使用して最初のページで、TextBox コントロールへの参照を取得します。 メソッドは、その目的に適していて、FindControl は高価な追加のコードを記述する必要があります。 さいわい、ASP.NET 2.0 では、多くのシナリオでは動作をこの目的の FindControl の代替を提供します。 PreviousPageType ディレクティブでは、TypeName または VirtualPath 属性を使用して、前のページへの参照を厳密に型指定を行うことができます。 TypeName 属性 VirtualPath 属性を使用すると、仮想パスを使用して、前のページを参照中に、前のページの種類を指定することができます。 PreviousPageType ディレクティブを設定すると後、は、コントロール、パブリック プロパティを使用してアクセスを許可するなど、公開する必要があります。

## <a name="lab-1-cross-page-postback"></a>ラボ 1 - クロスページ ポストバック

このラボでは、ASP.NET 2.0 の新しいページ間ポストバック機能を使用するアプリケーションを作成します。

1. Visual Studio 2005 を開き、新しい ASP.NET Web サイトを作成します。
2. Page2.aspx と呼ばれる新しい web フォームを追加します。
3. デザイン ビューで、Default.aspx を開き、ボタン コントロールと TextBox コントロールを追加します。 

    1. ボタン コントロールの ID を与える**損害賠償**、テキスト ボックス コントロールの ID と**UserName**します。
    2. Page2.aspx をボタンの PostBackUrl プロパティを設定します。
4. ソース ビューでは、page2.aspx を開きます。
5. 次に示すように、@ PreviousPageType ディレクティブを追加します。
6. 次のコード ページを追加\_page2.aspx の分離コードの読み込み。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. ビルドのビルド メニューをクリックしてプロジェクトをビルドします。
8. Default.aspx の分離コードには、次のコードを追加します。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. ページを変更する\_page2.aspx、次の負荷。 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. プロジェクトをビルドします。
11. プロジェクトを実行します。
12. テキスト ボックスに名前を入力し、ボタンをクリックします。
13. 結果とは何ですか。

## <a name="asynchronous-pages-in-aspnet-20"></a>ASP.NET 2.0 の非同期ページ

ASP.NET で多くの競合の問題は、(Web サービスまたはデータベース呼び出し) などの外部の呼び出し、ファイル IO の待機時間などの待機時間が原因です。ASP.NET アプリケーションに対して要求が行われると、ASP.NET はその要求にサービスのワーカー スレッドのいずれかを使用します。 その要求では、要求が完了し、応答が送信されるまで、そのスレッドが所有しています。 ASP.NET 2.0 では、ページを非同期的に実行する機能を追加することでこれらの種類の問題の待ち時間の問題を解決するのにはシークします。 つまり、ワーカー スレッドが、要求を開始し、すばやく使用可能なスレッド プールに返すため、別のスレッドに追加の実行を渡すことができます。 ファイル IO、データベースの呼び出しなどが完了したら、新しいスレッドは、要求の完了をスレッド プールから取得されます。

ページが非同期的に実行する最初の手順が設定するのには、 **Async**よう page ディレクティブの属性。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

この属性では、ASP.NET ページに IHttpAsyncHandler を実装するように指示します。

次の手順では、PreRender する前に、ページのライフ サイクルの時点で、AddOnPreRenderCompleteAsync メソッドを呼び出します。 (ページでこのメソッドが呼び出されます通常\_ロードします)。AddOnPreRenderCompleteAsync メソッドは 2 つのパラメーターを受け取りますBeginEventHandler し、EndEventHandler します。 BeginEventHandler、EndEventHandler にパラメーターとして渡されます IAsyncResult を返します。

次のビデオは、非同期のページ要求のチュートリアルです。


![](the-asp-net-2-0-page-model/_static/image3.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> EndEventHandler が完了するまで、非同期ページは、ブラウザーに表示されません。 非同期要求非同期コールバックに似ている一部の開発者はことは間違いありませんと考えてください。 いるものを実現する重要です。 非同期要求のメリットは、最初のワーカー スレッドなど、IO バインドであるため競合を減らす、サービスの新しい要求をスレッド プールに返されることです。


## <a name="script-callbacks-in-aspnet-20"></a>ASP.NET 2.0 におけるスクリプト コールバック

Web 開発者は、コールバックに関連付けられているちらつきを防ぐための方法については常にしました。 Asp.net 1.x では、SmartNavigation がちらつき、回避するための最も一般的な方法が SmartNavigation は複雑で、クライアントでは、その実装のため一部の開発者の問題の原因とします。 ASP.NET 2.0 では、スクリプト コールバックでは、この問題を解決します。 スクリプト コールバックは、JavaScript を使用して Web サーバーに対する要求に XMLHttp を利用します。 XMLHttp 要求は、ブラウザーの DOM を経由して操作できる、XML データを返します 新しい WebResource.axd ハンドラーによって、ユーザーが XMLHttp コードは表示されません。

ASP.NET 2.0 のスクリプト コールバックを構成するために必要ないくつかの手順があります。

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>手順 1: ICallbackEventHandler インターフェイスを実装します。

ASP.NET スクリプト コールバックに参加するいると、ページを認識するためには、ICallbackEventHandler インターフェイスを実装する必要があります。 分離コード ファイルでこれを行うようになります。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

これは、@ Implements ディレクティブなどがこれを使用しても実行することができます。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

通常、インライン ASP.NET コードを使用する場合は、@ Implements ディレクティブを使用します。

## <a name="step-2--call-getcallbackeventreference"></a>手順 2: 呼び出し GetCallbackEventReference

前述のように、XMLHttp 呼び出しは WebResource.axd ハンドラーにカプセル化します。 ASP.NET が web フォームへの呼び出しを追加、ページが表示されると、\_DoCallback、WebResource.axd によって提供されるクライアント スクリプト。 Web フォーム\_DoCallback 関数は、 \_\_のコールバックを doPostBack 関数。 注意して\_ \_doPostBack プログラムでページ上のフォームを送信します。 コールバックのシナリオで、ポストバックのためようにしたい\_ \_doPostBack だけでは不十分です。

> [!NOTE]
> \_\_doPostBack は、クライアント スクリプト コールバック シナリオでのページにレンダリングされます。 ただし、コールバックは使用されません。


Web フォームの引数\_DoCallback クライアント側の関数は、サーバー側関数に通常のページで呼び出されます GetCallbackEventReference を通じて提供\_ロードします。 このよう GetCallbackEventReference を通常の呼び出しになります。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> この場合、cm、ClientScriptManager のインスタンスです。 ClientScriptManager クラスについては、このモジュールで後で説明します。


GetCallbackEventReference のいくつかのオーバー ロードされたバージョンがあります。 この場合、引数は次のとおりです。

`this`

GetCallbackEventReference が呼び出されているコントロールへの参照。 この場合、ページ自体を勧めします。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

クライアント側コードからサーバー側のイベントに渡される文字列引数を指定します。 この場合、Im、ドロップダウンの値を渡すには、ddlCompany が呼び出されます。

`ShowCompanyName`

(文字列) として、サーバー側のコールバック イベントから戻り値を受け入れるクライアント側の関数の名前。 この関数は、サーバー側のコールバックが成功した場合にのみ呼び出すことは。 そのため、堅牢性、説明を一般にお勧めの GetCallbackEventReference エラーが発生した場合に実行するクライアント側の関数の名前を指定する追加の文字列引数を受け取るオーバー ロードされたバージョンを使用します。

`null`

サーバーにコールバックの前に開始したクライアント側の関数を表す文字列。 ここではありません、このようなスクリプトのため、引数は null です。

`true`

コールバックを非同期的に実行するかどうかを指定するブール値。

Web フォームへの呼び出し\_DoCallback クライアントではこれらの引数を渡します。 そのため、クライアントでこのページが表示されると、そのコードになりますようになります。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

クライアントの関数のシグネチャが少し異なることに注意してください。 クライアント側の関数は、5 つの文字列とブール値を渡します。 (これは上記の例では null) 追加の文字列には、サーバー側のコールバックで発生したエラーを処理するクライアント側の関数が含まれています。

## <a name="step-3--hook-the-client-side-control-event"></a>手順 3: クライアント側コントロールのイベントをフックします。

GetCallbackEventReference 上記の戻り値が文字列変数に割り当てられたことに注意してください。 その文字列を使用して、コールバックを開始するコントロールのクライアント側のイベントがフックされます。 この例で、コールバックが ページで、ドロップダウン リストで開始フックするため、 *OnChange*イベント。

次のように、クライアント側のマークアップにハンドラーを単に追加、クライアント側のイベントをフック。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

いることを思い出してください*cbRef* GetCallbackEventReference への呼び出しから戻り値です。 Web フォームへの呼び出しが含まれている\_DoCallback 上に表示されました。

## <a name="step-4--register-the-client-side-script"></a>手順 4: クライアント側スクリプトを登録します。

GetCallbackEventReference への呼び出しが、クライアント側スクリプトと呼ばれることを指定したことを思い出してください**ShowCompanyName**はサーバー側のコールバックが成功したときに実行されます。 このスクリプトは、ClientScriptManager インスタンスを使用して、ページに追加する必要があります。 (ClientScriptManager クラスは、このモジュールの後半で示すになります)そのためそのなどを行います。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>手順 5: ICallbackEventHandler インターフェイスのメソッドを呼び出す

ICallbackEventHandler には、コードで実装する必要がある 2 つのメソッドが含まれています。 **RaiseCallbackEvent**と**GetCallbackEvent**します。

**RaiseCallbackEvent**を引数として文字列を受け取り、nothing を返します。 Web フォームへのクライアント側の呼び出しから文字列引数が渡された\_DoCallback します。 この場合、その値は、*値*ddlCompany と呼ばれるドロップダウンの属性です。 RaiseCallbackEvent メソッドで、サーバー側コードを配置する必要があります。 たとえば、コールバックは、外部リソースに対する WebRequest を行う場合は、そのコードを RaiseCallbackEvent に配置する必要があります。

**GetCallbackEvent**をクライアントにコールバックの戻り値を処理します。 引数を受け取らないし、文字列を返します。 返される文字列は、引数として関数に渡す、クライアント側、ここで*ShowCompanyName*します。

上記の手順を完了すると後、は、ASP.NET 2.0 のスクリプト コールバックを実行する準備が完了したら。


![](the-asp-net-2-0-page-model/_static/image4.png)


[開いているビデオを全画面](the-asp-net-2-0-page-model/_static/callback1.wmv)


ASP.NET におけるスクリプト コールバックは、XMLHttp 呼び出すをサポートするブラウザーでサポートされます。 すべての最新のブラウザーを含む使用今日。 Internet Explorer は、その他の最新のブラウザー (今後の IE 7 を含む) が、組み込みの XMLHttp オブジェクトを使用して、XMLHttp ActiveX オブジェクトを使用します。 プログラムで使用することができます、ブラウザーがコールバックをサポートする場合を決定する、 **Request.Browser.SupportCallback**プロパティ。 このプロパティは返します**true**要求元のクライアント スクリプト コールバックをサポートしている場合。

## <a name="working-with-client-script-in-aspnet-20"></a>ASP.NET 2.0 のクライアント スクリプトの操作

ASP.NET 2.0 でのクライアント スクリプトが ClientScriptManager クラスを使用して管理されます。 ClientScriptManager クラスは、型と名前を使用してクライアント スクリプトの追跡を保持します。 これは、同じスクリプトが複数回挿入ページにプログラムを使用することを防ぎます。

> [!NOTE]
> スクリプトがページに正常に登録された後後続しようとすると、同じスクリプトを登録するは、2 回目が登録されていないスクリプトの結果となるだけ。 重複するスクリプトは追加されず、例外が発生しません。 不要な計算を回避するためには、複数回登録しないでように、スクリプトが既に登録されてかどうかを判断するために使用できる方法があります。


現在のすべての ASP.NET 開発者に馴染み深い ClientScriptManager のメソッドがあります。

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

このメソッドは、スクリプトを表示するページの上部に追加します。 これは、クライアントで明示的に呼び出される関数を追加するために役立ちます。

このメソッドの 2 つのオーバー ロードされたバージョンがあります。 3 つの 4 つの引数は、それらの間で共通です。 それらは次のとおりです。

`type (string)`

***型***引数は、スクリプトの種類を識別します。 一般に、(このページの種類を使用することをお勧め。型の GetType()) します。

`key (string)`

***キー***引数は、スクリプトのユーザー定義のキー。 各スクリプトに対して一意でがあります。 同じキーと追加済みのスクリプトの種類のスクリプトを追加しようとした場合に追加されません。

`script (string)`

***スクリプト***引数は、追加する実際のスクリプトを含む文字列。 スクリプトを作成し、StringBuilder で ToString() メソッドを使用して割り当てるには、StringBuilder を使用することをお勧め、***スクリプト***引数。

3 つの引数のみを取るオーバー ロードされた RegisterClientScriptBlock を使用する場合は、スクリプト要素を含める必要があります (&lt;スクリプト&gt;と&lt;/script&gt;) スクリプト内で。

4 番目の引数を受け取る RegisterClientScriptBlock のオーバー ロードを使用することができます。 4 番目の引数は、ASP.NET でのスクリプト要素を追加する必要があるかどうかを指定するブール値です。 この引数は場合**true**スクリプトはスクリプト要素は明示的に含まれません。

IsClientScriptBlockRegistered メソッドを使用して、スクリプトは既に登録されているかを判断します。 これにより、既に登録されているスクリプトを再登録しようとしないようにすることができます。

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (2.0) の新機能

RegisterClientScriptInclude タグは、外部スクリプト ファイルにリンクしているスクリプト ブロックを作成します。 2 つのオーバー ロードがあります。 1 つは、キーと URL を受け取ります。 2 つ目は、種類を指定する 3 番目の引数を追加します。

たとえば、次のコードはスクリプト ブロックをアプリケーションの scripts フォルダーのルートで jsfunctions.js にリンクするには。

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

このコードは、レンダリングされたページで次のコードを生成します。

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> スクリプト ブロックは、ページの下部に表示されます。


IsClientScriptIncludeRegistered メソッドを使用して、スクリプトは既に登録されているかを判断します。 これにより、スクリプトを再登録を試行しないようにすることができます。

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript メソッドは、RegisterClientScriptBlock メソッドと同じ引数を受け取ります。 ページの読み込み後、OnLoad クライアント側のイベントの前に RegisterStartupScript に登録されているスクリプトを実行します。 終了直前に配置されていた RegisterStartupScript に登録されたスクリプトでは、1.X では、 &lt;/form&gt; RegisterClientScriptBlock に登録されたスクリプトが開始した直後に配置されていたときにタグを付ける&lt;フォーム&gt;タグ。 ASP.NET 2.0 で両方が終了する直前に配置されます。 &lt;/form&gt;タグ。

> [!NOTE]
> RegisterStartupScript を関数を登録した場合、クライアント側のコードで明示的に呼び出すまでその関数が実行されません。


IsStartupScriptRegistered メソッドを使用して、スクリプトは既に登録されているかを判断を回避しようとするスクリプトを再登録します。

## <a name="other-clientscriptmanager-methods"></a>その他の ClientScriptManager メソッド

ここでは、ClientScriptManager クラスの他の便利なメソッドの一部です。


|  <strong>GetCallbackEventReference</strong>   |                                                 このモジュールで前述したスクリプト コールバックを参照してください。                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                JavaScript の参照を取得します (javascript:&lt;呼び出す&gt;) を使用してクライアント側のイベントから投稿をことができます。                 |
|  <strong>GetPostBackEventReference</strong>   |                                   クライアントからの投稿を開始するために使用する文字列を取得します。                                    |
|      <strong>GetWebResourceUrl</strong>       | アセンブリに埋め込まれているリソースに URL を返します。 組み合わせて使用する必要があります<strong>RegisterClientScriptResource</strong>します。 |
| <strong>RegisterClientScriptResource</strong> |     Web リソースをページに登録します。 これらは、リソース アセンブリに埋め込まれていると、新しい WebResource.axd ハンドラーによって処理されます。      |
|     <strong>RegisterHiddenField</strong>      |                                                 ページに隠しフォーム フィールドを登録します。                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  HTML フォームが送信されるときに実行されるクライアント側のコードを登録します。                                   |

