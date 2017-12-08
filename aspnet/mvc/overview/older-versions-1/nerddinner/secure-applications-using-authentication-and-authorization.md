---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "認証と承認を使用してアプリケーションをセキュリティで保護された |Microsoft ドキュメント"
author: microsoft
description: "手順 9 ができるように、ユーザーが登録する必要があります、認証と承認を NerdDinner アプリケーションでは、セキュリティで保護を追加する方法を示しますおよび作成するには、サイトにログインしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>認証と承認を使用してアプリケーションをセキュリティ保護します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 9 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 9 ができるように、ユーザーが登録する必要があります、認証と承認を NerdDinner アプリケーションでは、セキュリティで保護を追加する方法を示します新しいディナー、および、夕食をホストしているユーザーのみを作成するサイトにログインが後で編集できます。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 手順 9: 認証と承認

今すぐサイトを作成し、夕食の詳細を編集する機能にアクセスする、NerdDinner がアプリケーションでは、すべてのユーザーを許可します。 ユーザーが登録する必要があるようにこれを変更してみましょうと新しいディナーを作成し、夕食をホストしているユーザーのみが後で編集できるように制限を追加するサイトにログインします。

これを有効にするには、アプリケーションをセキュリティで保護するのに認証と承認を使用します。

### <a name="understanding-authentication-and-authorization"></a>Understanding 認証と承認

*認証*を特定し、アプリケーションにアクセスするクライアントの id を検証するプロセスです。 さらに簡単に言うと、「ユーザー」、エンドユーザーは、web サイトを訪問するときに識別することです。 ASP.NET には、ブラウザーのユーザーを認証するいくつかの方法がサポートされています。 インターネットの web アプリケーションでは、使用される最も一般的な認証方法を [フォーム認証] と呼びます。 フォーム認証を使用すると、開発者は、アプリケーション内での HTML ログイン フォームを作成し、データベースやその他のパスワードの資格情報ストアに対して、エンドユーザーが送信したユーザー名とパスワードを検証します。 ユーザー名/パスワードの組み合わせが正しい場合は、開発者は、今後の要求間でユーザーを識別する暗号化された HTTP cookie を発行する ASP.NET を依頼できます。 NerdDinner するアプリケーションでフォーム認証を使用して、します。

*承認*は、認証されたユーザーが特定の URL/リソースへのアクセスまたはいくつかの操作を実行するアクセス許可を持つかどうかを決定するプロセスです。 たとえば、NerdDinner アプリケーション内でおしますでログオンしているユーザーのみがアクセスできることを承認するために、*ディナー/作成*URL し、新しいディナーを作成します。 承認ロジックを追加、夕食をホストしているユーザーだけは – を編集したり、他のすべてのユーザーの編集アクセス権を拒否するようにすることもあります。

### <a name="forms-authentication-and-the-accountcontroller"></a>フォーム認証と、AccountController

ASP.NET MVC の既定の Visual Studio プロジェクト テンプレートは、新しい ASP.NET MVC アプリケーションを作成すると、フォーム認証を自動的に有効にします。 自動的に、プロジェクト: を使用して、サイト内のセキュリティを統合する非常に簡単に構築済みのアカウントのログイン ページの実装を追加します。

既定の Site.master マスター ページにアクセスするユーザーが認証されていないときに、サイトの上部右にある [ログオン] のリンクが表示されます。

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

[ログオン] リンクをクリックすると、ユーザーを*/アカウント/ログオン*URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

訪問者を登録してこれを行う – しに「登録」リンクをクリックして、 */アカウント/レジスタ*URL しできるようにすると、アカウントの詳細を入力してください。

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

「登録」ボタンをクリックし、ASP.NET メンバーシップ システム内で新しいユーザーの作成は、フォーム認証を使用して、サイト上にユーザーを認証します。

Site.master 変更、「ようこそ [username]!」を出力するページの右上でユーザーがログインしている場合、 メッセージおよびレンダリング、"ログ Off"、"ログ オン"のいずれかの代わりにリンクします。 ユーザーをログアウト「ログオフ」リンクをクリックします。

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

上記のログイン、ログアウト ページ、および登録の機能は、プロジェクト、作成時に Visual Studio によってプロジェクトに追加された AccountController クラス内で実装されます。 \Views\Account ディレクトリ内でテンプレートの表示を使用して、AccountController の UI が実装されます。

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController クラスでは、ASP.NET フォーム認証システムを使用して、暗号化された認証 cookie、およびを保存し、ユーザー名とパスワードを検証する ASP.NET メンバーシップ API を発行します。 ASP.NET メンバーシップと、API は拡張可能使用するパスワード、資格情報ストアを有効にします。 ASP.NET は、SQL データベース内、または Active Directory 内のユーザー名とパスワードを格納する組み込みのメンバーシップ プロバイダーの実装に付属します。

NerdDinner アプリケーションは、プロジェクトのルートにある"web.config"ファイルを開き、検索で使用するメンバーシップ プロバイダーを構成して、&lt;メンバーシップ&gt;セクション内にします。 プロジェクトの作成時に追加された既定の web.config は SQL メンバーシップ プロバイダーを登録し、"ApplicationServices"を名前付き接続文字列を使用するように構成データベースの場所を指定します。

既定の"ApplicationServices"接続文字列 (内に指定されている、 &lt;connectionStrings&gt; web.config ファイルのセクション) SQL Express を使用するように構成します。 "ASPNETDB をという名前の SQL Express データベースを指しています。アプリケーションの下にある"MDF"アプリ\_データ"のディレクトリ。 このデータベースは、最初に、アプリケーション内で、メンバーシップ API を使用するときに存在しない、ASP.NET のデータベースの作成は自動的に作成し、適切なメンバーシップ データベース スキーマ内のプロビジョニングします。

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

はなく SQL Express (またはリモート データベースへの接続)、完全な SQL Server インスタンスを使用する場合、すべてが必要になりますタスクは、web.config ファイル内の"ApplicationServices"接続文字列を更新し、ことを確認して、適切なメンバシップ スキーマポイントのデータベースに追加されました。 実行することができます、"aspnet\_regsql.exe"メンバーシップと、その他の ASP.NET アプリケーション サービスの適切なスキーマをデータベースに追加する \Windows\Microsoft.NET\Framework\v2.0.50727\ ディレクトリ内でユーティリティです。

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>[Authorize] フィルターを使用して、ディナー/作成 URL の承認

セキュリティで保護された認証および NerdDinner アプリケーションのアカウント管理の実装を有効にするコードを記述するがありませんでした。 ユーザーは、このアプリケーション、およびサイトのログインとログアウトを新しいアカウントを登録できます。

今すぐ、アプリケーションに承認ロジックを追加し、認証の状態と訪問者のユーザー名を使用できます、確認と、サイト内で実行できない操作を制御おできます。 DinnersController クラスの「作成」のアクション メソッドに承認ロジックを追加してみましょう。 具体的には、私たちが必要にアクセスするユーザー、*ディナー/作成*に URL を記録する必要があります。 ログオンしていない場合リダイレクトにログイン ページにすることがサインインできるようにします。

このロジックを実装することは非常に簡単です。 必要なタスクは、アクション メソッドを作成するに [Authorize] フィルター属性を追加する次のようにします。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC には、アクション メソッドを宣言して適用できる再利用可能なロジックを実装するために使用する、「アクション フィルター」を作成する機能がサポートされています。 [Authorize] フィルターは、ASP.NET MVC によって提供される組み込みのアクション フィルターのいずれかと、開発者が宣言によってアクション メソッドとコント ローラー クラスに承認規則を適用することができます。

パラメーター (上記のような) を使用せずに適用されるときに [Authorize] フィルター、強制的にアクション メソッド要求を出しているユーザーは、– でログに記録する必要がありますが自動的にリダイレクトされますブラウザー ログイン URL にそうでない場合。 最初に要求された URL は、クエリ文字列引数として渡されるこのリダイレクトを実行する場合 (例:/アカウント/ログオンですか?ReturnUrl = % 2fDinners %2fcreate)。 ログインすると、AccountController は最初に要求された URL に、ユーザーをリダイレクトします。

必要に応じて、[Authorize] フィルターには、必要とするユーザーはいずれのログ記録とで許可されたユーザーや許可されているセキュリティ ロールのメンバーの一覧の中に使用できる、"Users"または「ロール」のプロパティを指定する機能がサポートしています。 たとえば、次のコードのみが許可ディナー/作成 URL にアクセスするには、次の 2 つの特定のユーザー、"scottgu"および"billg"。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

コード内で特定のユーザー名を埋め込むことがかなりされていない intainable ある傾向があります。 適切な方法は、上位レベルの"roles"を定義する、に対してコードをチェックして、そのユーザーをデータベースまたは (コードから外部に格納される実際のユーザー マッピングのリストを有効にする) active directory システムのいずれかを使用して、役割にマップするためにします。 ASP.NET には、組み込みのロールの管理 API だけでなく (SQL と Active Directory のものを含む) のロール プロバイダーのうち、このユーザー/ロール マッピングを実行できる一連の組み込みが含まれています。 ディナー/作成 URL にアクセスする特定の"admin"ロール内のユーザーのみを許可するコードを更新すること、します。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>作成するときに、User.Identity.Name プロパティを使用してディナー

コント ローラーの基本クラスで公開されている User.Identity.Name プロパティを使用して、要求の現在のログイン ユーザーのユーザー名を取得できます。

以前は、HTTP POST メソッドのバージョンの Create() アクションが実装された場合が発生しました。 ハードコードされた静的な文字列を Dinner の"HostedBy"プロパティです。 お User.Identity.Name プロパティを使用するには、このコードを更新したりできるよう Dinner を作成するホストの出欠に自動的に追加します。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Create() メソッドに [Authorize] 属性を追加したため ASP.NET MVC はアクション メソッドは、サイト上ディナー/作成 URL へのアクセス、ユーザーがログオンしている場合のみが実行されるようにします。 そのため、User.Identity.Name プロパティの値には、有効なユーザー名は常に含まれます。

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>編集するときに、User.Identity.Name プロパティを使用してディナー

今すぐディナー ホスト自体のプロパティ編集のみできるようにユーザーを制限するいくつかの承認ロジックを追加してみましょう。

このため、まず"IsHostedBy(username)"ヘルパー メソッド、夕食オブジェクト (前に作成した Dinner.cs 部分クラス) 内に追加します。 このヘルパー メソッドには、true または指定されたユーザー名が Dinner HostedBy プロパティと一致して、それらの大文字と小文字の文字列比較を実行する必要なロジックをカプセル化するかどうかに応じて false が返されます。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

追加 [Authorize] 属性 Edit() のアクション メソッドに、DinnersController クラス内で。 これにより、確実要求にユーザーにログインする必要があります、 */Dinners/編集/[id]* URL。

おできますし、コード メソッドを追加、編集を Dinner.IsHostedBy(username) ヘルパー メソッドを使用してログインしたユーザーが Dinner ホストと一致していることを確認します。 場合は、ユーザーは、ホストではありません、おを"InvalidOwner"ビューを表示し、要求を終了します。 これを行うコードは、次のようになります。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

お \Views\Dinners ディレクトリを右クリックし、このオプションを選択すると追加-&gt;新しい"InvalidOwner"ビューを作成するメニュー コマンドを表示します。 そこにありますは、次のエラー メッセージ。

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

ユーザーが所有していない dinner を編集しようとしたとき、みましょうエラー メッセージ。

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Delete() アクション メソッドのと同じ手順を繰り返しますおをロックダウン アクセス許可を同様に、ディナーを削除し、夕食のホストだけと削除することを確認し、コント ローラー内で。

### <a name="showinghiding-edit-and-delete-links"></a>表示/非表示の編集と削除のリンク

この詳細 URL から DinnersController クラスの編集および削除アクション メソッドにリンクします。

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

現在の詳細の URL に訪問者が dinner のホストであるかに関係なく、編集および削除アクション リンクを示します。 説明されるように変更、訪問したユーザーが dinner の所有者である場合にのみ、リンクが表示されます。

DinnersController 内 Details() アクション メソッドは、Dinner オブジェクトを取得し、モデル オブジェクトとして、ビュー テンプレートに渡します。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

条件付きで表示/非表示、編集および削除のリンク、Dinner.IsHostedBy() 以下のようなヘルパー メソッドを使用して、ビュー テンプレートを更新することをがします。

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>次の手順

これで、認証されたユーザーに AJAX を使用してディナーの RSVP を達成する方法を見てみましょう。

>[!div class="step-by-step"]
[前へ](implement-efficient-data-paging.md)
[次へ](use-ajax-to-deliver-dynamic-updates.md)
