---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: 作成した MVC 3 Razor および控えめな JavaScript を持つアプリケーション |Microsoft ドキュメント
author: microsoft
description: ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。 サンプル アプリケーション s.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30874695"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>作成した MVC 3 Razor および控えめな JavaScript を持つアプリケーション
====================
によって[Microsoft](https://github.com/microsoft)

> ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。 サンプル アプリケーションでは、ASP.NET MVC version 3 での新しい Razor ビュー エンジンおよび Visual Studio 2010 を使用して、作成、表示、編集、削除するユーザーなどの機能が含まれています、架空ユーザー一覧 web サイトを作成する方法を示します。
> 
> このチュートリアルでは、ユーザーの一覧のサンプルの ASP.NET MVC 3 アプリケーションをビルドするために実行された手順について説明します。 このトピックは、Visual Studio プロジェクトと c# および VB のソース コード:[ダウンロード](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)です。 このチュートリアルに関して質問がある場合は、投稿してくださいに、 [MVC フォーラム](https://forums.asp.net/1146.aspx)です。


## <a name="overview"></a>概要

構築するアプリケーションは、単純なユーザー リストの web サイトです。 ユーザーは入力、表示、およびユーザー情報を更新します。

![サンプル サイト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

VB と c# の完成したプロジェクトをダウンロードする[ここ](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)です。

## <a name="creating-the-web-application"></a>Web アプリケーションを作成します。

チュートリアルを開始する Visual Studio 2010 を開き、使用して新しいプロジェクトを作成、 *ASP.NET MVC 3 Web アプリケーション*テンプレート。 アプリケーションの名前&quot;Mvc3Razor&quot;です。

[![新しい MVC 3 プロジェクト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

**新しい ASP.NET MVC 3 プロジェクト**ダイアログで、**インターネット アプリケーション**Razor ビュー エンジンを選択して、をクリックして**OK**です。

![新しい ASP.NET MVC 3 プロジェクト] ダイアログ ボックス](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

このチュートリアルではない使用する ASP.NET メンバーシップ プロバイダー ログオンおよびメンバーシップに関連付けられているすべてのファイルを削除できるようにします。 **ソリューション エクスプ ローラー**、次のファイルとディレクトリを削除します。

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (およびこのディレクトリ内のすべてのファイル)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

編集、 <em> \_Layout.cshtml</em>ファイルし、内部のマークアップを置換、`<div>`という名前の要素`logindisplay`メッセージ<em> &quot;</em>ログイン無効&quot;. 次の例では、新しいマークアップを示します。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>モデルを追加します。

**ソリューション エクスプ ローラー**を右クリックし、*モデル*フォルダーを選択**追加**、クリックして**クラス**です。

![新しいユーザー Mdl クラス](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

クラスに `UserModel` という名前を付けます。 内容を置き換える、 *UserModel*を次のコード ファイル。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel`クラスは、ユーザーを表します。 クラスの各メンバーの注釈が付いて、[必要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性から、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。 内の属性、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、web アプリケーションの自動クライアントとサーバー側の検証を提供します。

開く、`HomeController`クラスを追加、`using`アクセスできるようにするディレクティブ、`UserModel`と`Users`クラス。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

直後、`HomeController`宣言では、次のコメントおよびへの参照を追加、`Users`クラス。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users`クラスはこのチュートリアルで使用するシンプルなメモリ内データ ストアです。 実際のアプリケーションでは、ユーザー情報を格納するのにデータベースを使用します。 最初の数行、`HomeController`ファイルは、次の例に示します。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

ユーザー モデルを次の手順でスキャフォールディング ウィザードを使用できるようにアプリケーションをビルドします。

## <a name="creating-the-default-view"></a>既定のビューを作成します。

次の手順では、アクション メソッドと、ユーザーを表示するビューを追加します。

既存の削除*Views\Home\Index*ファイル。 新規に作成する、*インデックス*ファイルが、ユーザーを表示します。

`HomeController`クラスの内容を交換してください、`Index`メソッドを次のコード。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

内部を右クリックし、`Index`メソッドをクリックして**ビューの追加**です。

![ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

選択、**厳密に型指定されたビューを作成する**オプション。 **データ クラスを表示**[ **Mvc3Razor.Models.UserModel**です。 (表示されない場合**Mvc3Razor.Models.UserModel**で、**データ クラスを表示**ボックスで、プロジェクトをビルドする必要があります)。ビュー エンジンに設定されていることを確認してください**Razor**です。 設定**コンテンツを表示**に**リスト**] をクリックし、**追加**です。

![インデックス ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

新しいビューが自動的に渡されるユーザー データを scaffolds、`Index`ビュー。 新しく生成された確認*Views\Home\Index*ファイル。 **新規作成**、**編集**、**詳細**、および**削除**リンクが機能しませんが、ページの残りの部分は機能します。 ページを実行します。 ユーザーの一覧を参照してください。

![ページをインデックスします。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

開く、 *Index.cshtml*ファイルし、置換、`ActionLink`のマークアップ**編集**、**詳細**、および**削除**を次のコード:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

選択したレコードを検索するユーザー名が ID として使用が、**編集**、**詳細**、および**削除**リンクします。

## <a name="creating-the-details-view"></a>詳細ビューの作成

次の手順が追加するには、`Details`アクション メソッドとユーザーの詳細を表示するために表示します。

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

次の追加`Details`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

内部を右クリックし、`Details`メソッドと、選択<strong>ビューの追加</strong>です。 いることを確認、<strong>データ クラスを表示</strong>ボックスには<strong>Mvc3Razor.Models.UserModel</strong><em>です。</em> 設定<strong>コンテンツを表示</strong>に<strong>詳細</strong>] をクリックし、<strong>追加</strong>です。

![詳細ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

アプリケーションを実行し、詳細情報のリンクを選択します。 自動のスキャフォールディングでは、モデルの各プロパティを示します。

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>編集ビューを作成します。

次の追加`Edit`メソッド、home コント ローラーにします。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

前の手順と同様にビューを追加するが、設定**コンテンツを表示**に**編集**です。

![編集ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

アプリケーションを実行し、ユーザーの 1 つの最初と最後の名前を編集します。 いずれかに違反した場合`DataAnnotation`に適用されている制約、`UserModel`クラス、フォームを送信するときに検証エラーが発生するサーバー コードによって生成されます。 たとえば、最初の名前を変更する&quot;Ann&quot;に&quot;A&quot;フォームを送信するときに、フォームに次のエラーが表示されます。

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

このチュートリアルでは、主キーとしてユーザー名を扱うしています。 そのため、ユーザー名プロパティを変更できません。 *Edit.cshtml*ファイル直後、`Html.BeginForm`ステートメントでは、隠しフィールドであるユーザー名を設定します。 これにより、モデルに渡されるプロパティです。 次のコード フラグメントの位置を示しています、`Hidden`ステートメント。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

置換、`TextBoxFor`と`ValidationMessageFor`マークアップをユーザー名と、`DisplayFor`呼び出します。 `DisplayFor`メソッドは、読み取り専用の要素として、プロパティを表示します。 次の例は、完全なマークアップを示しています。 元の`TextBoxFor`と`ValidationMessageFor`Razor begin コメント終了コメント文字を使って呼び出しをコメント アウト (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>クライアント側の検証を有効にします。

ASP.NET MVC 3 でのクライアント側の検証を有効にするには、2 つのフラグを設定する必要があり、次の 3 つの JavaScript ファイルをインクルードする必要があります。

アプリケーションの開く*Web.config*ファイル。 確認`that ClientValidationEnabled`と`UnobtrusiveJavaScriptEnabled`に設定されているアプリケーションの設定では true です。 次のフラグメント ルートから*Web.config*ファイルが、正しい設定を示しています。

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

設定`UnobtrusiveJavaScriptEnabled`有効控えめな Ajax および控えめなクライアント検証は、true にします。 控えめな検証を使用する場合、検証規則が HTML5 属性に変換します。 HTML5 属性名は、小文字、数字、およびダッシュのみで構成できます。

設定`ClientValidationEnabled`クライアント側検証の場合は true を有効にします。 アプリケーションでこれらのキーを設定して*Web.config*ファイル、クライアント検証と控えめな JavaScript アプリケーション全体に対して有効にします。 有効にするにまたは、個々 のビューまたは次のコードを使用してコント ローラーのメソッドでこれらの設定を無効にすることができますも。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

また、レンダリングされるビューに複数の JavaScript ファイルを含める必要があります。 JavaScript は、すべてのビューを簡単に追加するのには、 *\shared\\_Layout.cshtml*ファイル。 置換、`<head>`の要素、 * \_Layout.cshtml*を次のコード ファイル。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

最初の 2 つの jQuery スクリプトは、Microsoft Ajax コンテンツ配信ネットワーク (CDN) でホストされています。 Microsoft Ajax CDN を利用して、アプリケーションの初回のパフォーマンスを大幅に向上させることができます。

アプリケーションを実行し、[編集] リンクをクリックします。 ブラウザーでページのソースを表示します。 ブラウザー ソースは、フォームの多くの属性を示しています。 `data-val` (のデータ検証)。 クライアント検証と控えめな JavaScript が有効にすると、クライアント検証規則とは、入力フィールドが含まれて、`data-val="true"`控えめなクライアント検証をトリガーする属性。 たとえば、 `City` 、モデル内のフィールドがで修飾された、[必要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性は、次の例に示すように html 形式で結果を。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

それぞれのクライアント検証規則では、属性が追加、フォームを含む`data-val-rulename="message"`です。 使用して、`City`フィールド前の例で必要なクライアント検証規則を生成、`data-val-required`属性と、メッセージ&quot;、市区町村] フィールドは必須&quot;です。 アプリケーションを実行はユーザーの 1 つを編集およびオフ、`City`フィールドです。 フィールドから移動するとき、クライアント側の検証エラー メッセージを参照してください。

![必要な市区町村](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

同様に、クライアント検証規則内の各パラメーターの属性を追加、フォームを含む`data-val-rulename-paramname=paramvalue`です。 たとえば、`FirstName`プロパティの注釈が付いて、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性および 3 の最小長と 8 の最大長を指定します。 という名前のデータ検証ルール`length`パラメーターの名前を持つ`max`と 8 のパラメーターの値。 次に示しますに対して生成される HTML、`FirstName`ユーザーを編集するときにフィールドします。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

控えめなクライアント検証の詳細については、エントリを参照してください。 [ASP.NET MVC 3 の控えめなクライアント検証](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad Wilson のブログでします。

> [!NOTE]
> ASP.NET MVC 3 のベータ版である場合があります、クライアント側の検証を開始するために、フォームを送信する必要があります。 これは、最終リリースの変更があります。


## <a name="creating-the-create-view"></a>作成ビューを作成します。

次の手順が追加するには、`Create`アクション メソッドと、新しいユーザーを作成するユーザーを有効にするために表示します。 次の追加`Create`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

前の手順と同様にビューを追加するが、設定**コンテンツを表示**に**作成**です。

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

アプリケーションの実行で、選択、**作成**リンク、および新しいユーザーを追加します。 `Create`メソッドが自動的にクライアント側およびサーバー側の検証を活用を受け取ります。 空白文字を含むユーザー名を入力しようとしています。 &quot;Ben X&quot;です。 ユーザー名フィールドで、クライアント側の検証エラー外] タブと (`White space is not allowed`) が表示されます。

## <a name="add-the-delete-method"></a>Delete メソッドを追加します。

このチュートリアルを完了するには、次のコードを追加`Delete`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

追加、`Delete`設定、前の手順と同様にビュー**コンテンツを表示**に**削除**です。

![ビューを削除します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

検証に単純ですが、完全に機能 ASP.NET MVC 3 アプリケーションがあるようになりました。
