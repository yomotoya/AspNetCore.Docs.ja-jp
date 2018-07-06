---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: MVC 3 の作成と Razor および控えめな JavaScript アプリケーション |Microsoft Docs
author: microsoft
description: ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。 サンプル アプリケーション s.
ms.author: aspnetcontent
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 136c87cba70525da53c1f74576c50c12f8759539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840469"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>MVC 3 の作成と Razor および控えめな JavaScript アプリケーション
====================
によって[Microsoft](https://github.com/microsoft)

> ユーザーの一覧のサンプル web アプリケーションでは、Razor ビュー エンジンを使用して ASP.NET MVC 3 アプリケーションを作成するは簡単な方法を示します。 サンプル アプリケーションでは、ASP.NET MVC version 3 での新しい Razor ビュー エンジンと Visual Studio 2010 を使用して、作成、表示、編集、削除するユーザーなどの機能が含まれています、架空ユーザー リスト web サイトを作成する方法を示します。
> 
> このチュートリアルでは、ユーザーの一覧のサンプル ASP.NET MVC 3 アプリケーションをビルドするために実行された手順について説明します。 C# および VB のソース コードを Visual Studio プロジェクトはこのトピックと共に使用できます:[ダウンロード](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)します。 このチュートリアルについて質問等がございましたら、投稿してくださいに、 [MVC フォーラム](https://forums.asp.net/1146.aspx)します。


## <a name="overview"></a>概要

作成するアプリケーションは、シンプルなユーザーのリストの web サイトです。 ユーザーは、入力、表示、およびユーザー情報を更新できます。

![サンプル サイト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

VB と c# の完成したプロジェクトをダウンロードする[ここ](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)します。

## <a name="creating-the-web-application"></a>Web アプリケーションを作成します。

チュートリアルを開始する Visual Studio 2010 を開き、使用して新しいプロジェクトを作成、 *ASP.NET MVC 3 Web アプリケーション*テンプレート。 アプリケーションの名前を&quot;Mvc3Razor&quot;します。

[![新しい MVC 3 プロジェクト](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**、Razor ビュー エンジンを選択し、クリックして**OK**します。

![新しい ASP.NET MVC 3 プロジェクト ダイアログ](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

このチュートリアルではない使用する、ASP.NET メンバーシップ プロバイダー ログオンおよびメンバーシップに関連付けられているすべてのファイルを削除できるようにします。 **ソリューション エクスプ ローラー**、次のファイルとディレクトリを削除します。

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (およびこのディレクトリ内のすべてのファイル)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

編集、  <em>\_Layout.cshtml</em>ファイルを開き、マークアップ内で、`<div>`という名前の要素`logindisplay`メッセージ <em>&quot;</em>ログイン無効&quot;. 次の例では、新しいマークアップを示します。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>モデルを追加します。

**ソリューション エクスプ ローラー**を右クリックし、*モデル*フォルダーで、**追加**、 をクリックし、**クラス**。

![新しいユーザー Mdl クラス](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

クラスに `UserModel` という名前を付けます。 内容を置き換える、 *UserModel*を次のコード ファイル。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel`クラスは、ユーザーを表します。 クラスの各メンバーは、注釈が付けられた、[必要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性を[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。 内の属性、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、web アプリケーションの自動クライアントとサーバー側の検証を提供します。

開く、`HomeController`クラスを追加、`using`がアクセスできるように、ディレクティブ、`UserModel`と`Users`クラス。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

直後、`HomeController`宣言では、次のコメントとへの参照を追加、`Users`クラス。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users`クラスはこのチュートリアルで使用する簡略化された、メモリ内データ ストア。 実際のアプリケーションでユーザー情報を格納するのにデータベースを使用するとします。 最初の数行、`HomeController`ファイルは、次の例に示します。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

ユーザー モデルを次の手順でスキャフォールディング ウィザードで使用できるようにアプリケーションをビルドします。

## <a name="creating-the-default-view"></a>既定のビューを作成します。

次の手順では、アクション メソッドと、ユーザーを表示するビューを追加します。

既存の削除*Views\Home\Index*ファイル。 新規に作成する、*インデックス*ユーザーを表示するファイル。

`HomeController`クラスの内容を交換してください、`Index`メソッドを次のコード。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

内側を右クリックし、`Index`メソッドとクリック**ビューの追加**。

![ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

選択、**厳密に型指定されたビューを作成する**オプション。 **データ クラスを表示**、 **Mvc3Razor.Models.UserModel**します。 (表示されない場合**Mvc3Razor.Models.UserModel**で、**データ クラスを表示**ボックスで、プロジェクトをビルドする必要があります)。ビュー エンジンに設定されていることを確認**Razor**します。 設定**コンテンツを表示**に**一覧** をクリックし、**追加**します。

![インデックス ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

新しいビューに渡されるユーザー データを自動的にスキャフォールディング、`Index`ビュー。 新しく生成された確認*Views\Home\Index*ファイル。 **新規作成**、**編集**、**詳細**、および**削除**リンクは機能しませんが、ページの残りの部分は機能します。 ページを実行します。 ユーザーの一覧を表示します。

![インデックス ページ](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

開く、 *Index.cshtml*ファイルを開き、`ActionLink`のマークアップ**編集**、**詳細**、および**削除**を次のコード:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

選択したレコードを検索する ID としてユーザー名が使用される、**編集**、**詳細**、および**削除**リンク。

## <a name="creating-the-details-view"></a>詳細ビューの作成

次の手順が追加するには、`Details`アクション メソッドとユーザーの詳細を表示するために表示します。

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

次の追加`Details`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

内側を右クリックし、`Details`メソッドと、選択<strong>ビューの追加</strong>します。 いることを確認、<strong>データ クラスを表示</strong>ボックスには<strong>Mvc3Razor.Models.UserModel</strong><em>します。</em> 設定<strong>コンテンツを表示</strong>に<strong>詳細</strong> をクリックし、<strong>追加</strong>します。

![詳細ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

アプリケーションを実行し、[詳細] リンクを選択します。 自動スキャフォールディングでは、モデルの各プロパティを示します。

![説明](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>編集ビューを作成します。

次の追加`Edit`home コント ローラーへのメソッド。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

前の手順のようにビューを追加するが、設定**コンテンツを表示**に**編集**します。

![編集ビューを追加します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

アプリケーションを実行し、ユーザーのいずれかの最初と最後の名前を編集します。 いずれかに違反した場合`DataAnnotation`に適用されている制約、`UserModel`クラス、フォームを送信するときにエラーが発生する検証サーバー コードによって生成されます。 たとえば、最初の名前を変更する&quot;Ann&quot;に&quot;A&quot;フォームを送信するときに、フォームに次のエラーが表示されます。

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

このチュートリアルでは、主キーとしてユーザー名を処理しています。 そのため、ユーザー名プロパティを変更できません。 *Edit.cshtml*直後、ファイル、`Html.BeginForm`ステートメントでは、非表示フィールドであるユーザー名を設定します。 これにより、モデルに渡されるプロパティです。 次のコード フラグメントの位置を示しています、`Hidden`ステートメント。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

置換、`TextBoxFor`と`ValidationMessageFor`マークアップをユーザー名と、`DisplayFor`呼び出します。 `DisplayFor`メソッドは、読み取り専用要素としてプロパティを表示します。 次の例では、完全なマークアップを示します。 元の`TextBoxFor`と`ValidationMessageFor`Razor コメントの開始と終了コメント文字を含む呼び出しがコメント アウト (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>クライアント側の検証を有効にします。

ASP.NET MVC 3 でのクライアント側検証を有効にするには、2 つのフラグを設定する必要があり、3 つの JavaScript ファイルを含める必要があります。

アプリケーションの開く*Web.config*ファイル。 確認`that ClientValidationEnabled`と`UnobtrusiveJavaScriptEnabled`に設定されているアプリケーションの設定では true。 次のフラグメントのルートから*Web.config*ファイルは、正しい設定を示しています。

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

設定`UnobtrusiveJavaScriptEnabled`控えめな Ajax を有効にし、控えめなクライアント検証を true にします。 控え目な検証を使用すると検証規則は、HTML5 属性に変換されます。 HTML5 属性名は、小文字、数字、およびダッシュのみで構成できます。

設定`ClientValidationEnabled`クライアント側の検証を true にします。 アプリケーションでこれらのキーを設定して*Web.config*ファイル、クライアントの検証と控えめな JavaScript アプリケーション全体に対して有効にします。 有効にするか、個々 のビューまたはコント ローラーのメソッドに次のコードを使用してこれらの設定を無効にします。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

描画されたビューにいくつかの JavaScript ファイルを含める必要があります。 すべてのビューで、JavaScript を含める簡単な方法に追加する、 *views \shared\\_Layout.cshtml*ファイル。 置換、`<head>`の要素、  *\_Layout.cshtml*を次のコード ファイル。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

最初の 2 つの jQuery スクリプトは、Microsoft Ajax Content Delivery Network (CDN) によってホストされています。 Microsoft Ajax CDN を利用して、アプリケーションの初回のパフォーマンスを大幅に向上させることができます。

アプリケーションを実行し、編集リンクをクリックします。 ブラウザーでページのソースを表示します。 ブラウザー ソースは、フォームの多くの属性を示しています。 `data-val` (用データの検証)。 クライアント検証と控えめな JavaScript が有効になっているときに、クライアント検証規則の入力フィールドが含まれて、`data-val="true"`控えめなクライアント検証をトリガーする属性。 たとえば、`City`で、モデル内のフィールドが修飾されて、[ために必要な](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性には、次の例に示すように html 形式で結果します。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

フォームを持つ各クライアント検証規則の属性が追加されます`data-val-rulename="message"`します。 使用して、`City`フィールド前述の例に、必要なクライアント検証規則を生成、`data-val-required`属性と、メッセージ&quot;、市区町村 フィールドが必要です。&quot;します。 アプリケーションを実行し、ユーザーの 1 つを編集、オフ、`City`フィールド。 フィールドから移動するときは、クライアント側検証エラー メッセージを参照してください。

![必要な市区町村](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

同様に、クライアント検証規則では、各パラメーターの属性を追加、フォームを持つ`data-val-rulename-paramname=paramvalue`します。 たとえば、`FirstName`プロパティ注釈が、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性し、3 の最小の長さと 8 の最大長を指定します。 という名前のデータの検証規則`length`パラメーターの名前を持つ`max`と 8 パラメーターの値。 に対して生成される HTML を次に示します、`FirstName`ユーザーのいずれかを編集するときにフィールドします。

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

控えめなクライアント検証の詳細については、エントリを参照してください。 [ASP.NET MVC 3 の控えめなクライアント検証](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad Wilson のブログにします。

> [!NOTE]
> ASP.NET MVC 3 のベータ版である場合があります、クライアント側の検証を開始するには、フォームを送信する必要があります。 これは、最終リリースの変更可能性があります。


## <a name="creating-the-create-view"></a>作成ビューを作成します。

次の手順が追加するには、`Create`アクション メソッドと新しいユーザーを作成するユーザーを有効にするために表示します。 次の追加`Create`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

前の手順のようにビューを追加するが、設定**コンテンツを表示**に**作成**です。

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

アプリケーションの実行、選択、**作成**リンク、および新しいユーザーを追加します。 `Create`メソッドは自動的にはクライアント側とサーバー側検証利用します。 など、空白を含むユーザー名を入力しようとしています。 &quot;Ben X&quot;します。 ユーザー名フィールドで、クライアント側の検証エラー外 タブと (`White space is not allowed`) が表示されます。

## <a name="add-the-delete-method"></a>Delete メソッドを追加します。

このチュートリアルを完了するには、次のコードを追加`Delete`メソッドを home コント ローラー。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

追加、`Delete`設定、前の手順のようにビュー**コンテンツを表示**に**削除**します。

![ビューを削除します。](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

検証にすべての機能が単純な ASP.NET MVC 3 アプリケーションがあるようになりました。
