---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 認証と承認を使用してアプリケーションをセキュリティ保護 |Microsoft Docs
author: microsoft
description: 手順 9 は、ユーザーが登録する必要が、認証と承認 NerdDinner アプリケーションでは、セキュリティで保護するを追加する方法を説明し、作成するには、サイトにログインしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d28102c8b80433b58a42cadc70b26c9fb5bc4404
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369885"
---
<a name="secure-applications-using-authentication-and-authorization"></a>認証と承認を使用してアプリケーションをセキュリティで保護します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 9 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 9 は、ユーザーが登録する必要があるように、認証と承認 NerdDinner アプリケーションでは、セキュリティで保護するを追加する方法を示します新しい dinners と、dinner をホストしているユーザーのみを作成するには、サイトへのログインが後で編集できます。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 手順 9: 認証と承認

今すぐ、NerdDinner をアプリケーションに許可されたすべてのユーザーは、サイトを作成し、任意の dinner の詳細を編集する機能にアクセスします。 ユーザーが登録する必要がありますようにこれを変更してみましょうと新しい dinners を作成し、夕食をホストしているユーザーのみが後で編集できるように、制限を追加するサイトにログインします。

これを有効にするには、認証と承認をアプリケーションをセキュリティで保護するのにに使用します。

### <a name="understanding-authentication-and-authorization"></a>認証と承認

*認証*は識別およびアプリケーションにアクセスするクライアントの id を検証するプロセスです。 さらに簡単に言うと、「ユーザー」、エンドユーザーが web サイトを訪問するときに識別することです。 ASP.NET では、複数のブラウザーのユーザーを認証する方法をサポートします。 インターネットの web アプリケーション、使用される最も一般的な認証方法には「フォーム認証」が呼び出されます。 フォーム認証を使用すると、開発者は自社のアプリケーションの HTML ログイン フォームを作成し、データベースやその他のパスワード資格情報ストアに対して、エンドユーザーが送信するユーザー名とパスワードを検証します。 ユーザー名/パスワードの組み合わせが正しい場合は、開発者は、今後の要求間でユーザーを識別するために暗号化された HTTP クッキーを発行するための ASP.NET を依頼できます。 NerdDinner アプリケーションでフォーム認証を使用して作成します。

*承認*は、認証されたユーザーが特定の URL/リソースへのアクセスまたはアクションを実行するアクセス許可を持つかどうかを決定するプロセスです。 など、NerdDinner アプリケーション内で必要ありますでログオンしているユーザーのみがアクセスできることを承認するために、 */Dinners/作成*URL と新しい Dinners を作成します。 私たちは、夕食をホストしているユーザーのみが編集および他のすべてのユーザーに対して編集のアクセスが拒否されるように、承認ロジックを追加する必要もあります。

### <a name="forms-authentication-and-the-accountcontroller"></a>フォーム認証と、AccountController

ASP.NET MVC の既定の Visual Studio プロジェクト テンプレートは、新しい ASP.NET MVC アプリケーションを作成すると、フォーム認証を自動的に有効にします。 – プロジェクトを使用して、サイト内のセキュリティを統合する非常に簡単に構築済みのアカウントのログイン ページの実装も自動的に追加します。

既定の Site.master マスター ページでは、それにアクセスするユーザーが認証されていない場合、サイトの右上にある [ログオン] のリンクが表示されます。

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

ユーザーには、[ログオン] リンクをクリックすると、 */アカウント/ログオン*URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

訪問者に登録していないようにすることができる –「登録」リンクをクリックして、 */アカウント/登録*URL とアカウントの詳細を入力するようにします。

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

「登録」 ボタンをクリックすると、ASP.NET メンバーシップ システム内で新しいユーザーの作成し、フォーム認証を使用して、サイトにユーザーを認証します。

Site.master 変更、「ようこそ [username]!」を出力するページの右上でユーザーがログインしている場合、 メッセージと表示を「オン」、「ログの」いずれかの代わりにリンクします。 「ログオフ」リンクをクリックすると、ユーザーをログアウトします。

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

上記のログイン、ログアウト、および登録機能は、プロジェクトの作成時に Visual Studio によってプロジェクトに追加された AccountController クラス内で実装されます。 AccountController の UI は \Views\Account ディレクトリ内のビュー テンプレートを使用して実装されます。

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController クラスでは、ASP.NET フォーム認証システムを使用して、暗号化された認証 cookie、および格納し、ユーザー名/パスワードの検証に ASP.NET メンバーシップ API を発行します。 ASP.NET メンバーシップ API は、拡張により、任意のパスワード資格情報ストアの使用します。 ASP.NET は、SQL database、または Active Directory 内のユーザー名とパスワードを格納する組み込みのメンバーシップ プロバイダーの実装では出荷されます。

NerdDinner アプリケーションは、プロジェクトのルートに"web.config"ファイルを開き、検索で使用するメンバーシップ プロバイダーを構成することができます、&lt;メンバーシップ&gt;セクション内にします。 追加プロジェクトの作成時に既定の web.config が SQL メンバーシップ プロバイダーを登録し、"ApplicationServices"という名前の接続文字列を使用するように構成データベースの場所を指定します。

既定の"ApplicationServices"接続文字列 (内で指定される、 &lt;connectionStrings&gt; web.config ファイルのセクション) SQL Express を使用するように構成します。 "ASPNETDB をという名前の SQL Express データベースを指します。アプリケーションの下で"MDF"アプリ\_データ"のディレクトリ。 このデータベースが最初に、アプリケーション内で、メンバーシップ API を使用するときに存在しない場合 ASP.NET は自動的にデータベースを作成し、内に適切なメンバーシップ データベース スキーマをプロビジョニングします。

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

SQL Express を使用して、完全な SQL Server インスタンス (またはリモート データベースへの接続) を使用せずにすべてが必要になります to do 場合は、web.config ファイル内で"ApplicationServices"接続文字列を更新し、ことを確認しますが、適切なメンバーシップ スキーマポイントのデータベースに追加されました。 実行することができます、"aspnet\_regsql.exe"、データベースにメンバーシップと、その他の ASP.NET アプリケーション サービスの適切なスキーマを追加する \Windows\Microsoft.NET\Framework\v2.0.50727\ ディレクトリ内でユーティリティ。

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>[Authorize] フィルターを使用して、Dinners/作成 URL の承認

NerdDinner アプリケーションのアカウント管理の実装をセキュリティで保護された認証を有効にするコードを記述することがありませんでした。 ユーザーは、当社のアプリケーション、およびサイトのログイン/ログアウトを新しいアカウントを登録できます。

今すぐ、アプリケーションに承認ロジックを追加して認証の状態と訪問者のユーザー名を使用できる、確認と、サイト内で実行できないことを制御します。 まず、DinnersController クラスの「作成」のアクション メソッドに承認ロジックを追加することから始めます。 具体的には、私たちがする必要がありますにアクセスするユーザー、 */Dinners/作成*に URL を記録する必要があります。 ログオンしていない場合リダイレクトにログイン ページにことがサインインできるようにします。

このロジックを実装することは非常に簡単です。 To-do は、Create アクション メソッドに [Authorize] フィルター属性を追加するようになります。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC には、宣言でアクション メソッドに適用できる再利用可能なロジックを実装するために使用できる「アクション フィルター」を作成する機能がサポートしています。 [Authorize] フィルターは、ASP.NET MVC、によって提供される組み込みのアクション フィルターの 1 つでき、開発者は宣言でアクション メソッドとコント ローラー クラスに承認規則を適用できます。

(上記のような) パラメーターを指定せずに適用されるときに自動的にリダイレクトする必要がブラウザーのログイン URL にこれらができない場合、アクション メソッド要求を行っているユーザーは、– でログに記録する必要があります [Authorize] フィルターを適用します。 最初に要求された URL は、クエリ文字列引数として渡されるこのリダイレクトを実行する場合 (例:/アカウント/ログオンでしょうか。ReturnUrl = % 2fDinners %2fcreate)。 ログインしたら、AccountController は最初に要求された URL にし、ユーザーをリダイレクトします。

[Authorize] フィルターには、必要に応じて、ユーザーが両方に記録する許可されたユーザーまたは許可されているセキュリティ ロールのメンバーの一覧内で要求するように使用できる「ユーザー」または「ロール」のプロパティを指定する機能がサポートしています。 たとえば、次のコードは、Dinners/作成 URL にアクセスするには、2 つの特定のユーザー、"scottgu"および「billg のことです」をのみできます。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

埋め込みコード内で特定のユーザー名は、ただし非常されていない intainable ある傾向があります。 優れたアプローチより高度な"roles"を定義する、コードをチェックし、データベースまたは active directory システムの (コードから外部に格納される実際のユーザー マッピングの一覧を有効にする) を使用してロールにユーザーをマップします。 ASP.NET には、組み込みのロール管理 API と組み込みの (SQL と Active Directory のものを含む)、このユーザー/ロール マッピングを実行できるロール プロバイダーのセットが含まれています。 Dinners/作成 URL にアクセスする特定の「管理者」ロール内のユーザーのみを許可するコードを更新し、でした。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>作成するときに、User.Identity.Name プロパティを使用して Dinners

コント ローラーの基本クラスで公開されている User.Identity.Name プロパティを使用して、要求の現在のログイン ユーザーのユーザー名を取得できます。

以前、Create() アクション メソッドの HTTP POST のバージョンを実装したときにありましたハードコードされた静的な文字列を Dinner の"HostedBy"プロパティです。 私たちできます User.Identity.Name プロパティを使用するには、このコードを更新するようになりましただけでなく、夕食を作成するホストに、予約を自動的に追加。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Create() メソッドに [Authorize] 属性を追加したため、ASP.NET MVC は、アクション メソッドは、Dinners/作成の URL にアクセスするユーザーがサイトにログインしている場合のみが実行されるを確認します。 そのため、User.Identity.Name プロパティの値には、有効なユーザー名は常に含まれます。

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>編集するときに、User.Identity.Name プロパティを使用して Dinners

今すぐ dinners ホスト自体のプロパティしか編集するためにユーザーを制限するいくつかの承認ロジックを追加してみましょう。

このため、最初"IsHostedBy(username)"のヘルパー メソッド (前に作成した Dinner.cs 部分クラス) 内、Dinner オブジェクトに追加します。 True または false かどうか指定されたユーザー名は、Dinner HostedBy プロパティと一致して、それらの大文字の文字列比較を実行するために必要なロジックをカプセル化によって、このヘルパー メソッドが返されます。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

DinnersController クラス内で Edit() のアクション メソッドに [Authorize] 属性から追加します。 要求にユーザーに記録する必要があることにより、これを */Dinners/編集/[id]* URL。

コード Dinner.IsHostedBy(username) ヘルパー メソッドを使用してログインしたユーザーが Dinner ホストと一致していることを確認する、Edit メソッドを追加できます。 ユーザーが、ホストでない場合が"InvalidOwner"ビューを表示され、要求の終了します。 これを行うコードは、次のようになります。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

私たちし \Views\Dinners ディレクトリを右クリックし、選択追加-&gt;新しい"InvalidOwner"ビューを作成するメニュー コマンドを表示します。 それを設定しますが、次のエラー メッセージ。

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

ユーザーが所有していない夕食を編集しようとした場合、みましょうエラー メッセージ。

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Delete() アクション メソッドに対して同じ手順を繰り返し Dinners を同様に、削除して、Dinner のホストのみと削除することを確認するためのアクセス許可をロックダウンするコント ローラー内で。

### <a name="showinghiding-edit-and-delete-links"></a>表示/非表示の編集と削除のリンク

目的の詳細 URL から、DinnersController クラスの編集、削除のアクション メソッドにリンクします。

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

現在の詳細 URL への訪問者が、dinner のホストであるかに関係なく、編集、削除アクション リンクを示しています。 みましょうされるように変更、リンクは、訪問したユーザーが、dinner の所有者である場合にのみ表示されます。

DinnersController 内 Details() アクション メソッドでは、Dinner オブジェクトを取得し、モデル オブジェクトとしてビュー テンプレートに渡します。

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

条件付きで表示/非表示、編集、削除のリンク Dinner.IsHostedBy() の次のようなヘルパー メソッドを使用して、ビュー テンプレートを更新することができます。

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>次の手順

これで、認証されたユーザーの dinners AJAX を使用して予約を達成する方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](implement-efficient-data-paging.md)
> [次へ](use-ajax-to-deliver-dynamic-updates.md)
