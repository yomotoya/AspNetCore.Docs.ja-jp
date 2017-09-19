---
title: "ASP.NET Core のタグ ヘルパーの作成"
author: rick-anderson
description: "ASP.NET Core のタグ ヘルパーの作成方法を説明します。"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a5222da1380c2fe768b287bfa1a49b300c02f2b
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>ASP.NET Core、サンプルとチュートリアルのタグ ヘルパーの作成

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a>タグ ヘルパーの使用を開始します。

このチュートリアルでは、タグ ヘルパーのプログラミングの概要を提供します。 [タグ ヘルパーの概要](intro.md)タグ ヘルパーを提供する利点について説明します。

タグ ヘルパーを実装するクラス、`ITagHelper`インターフェイスです。 ただし、タグ ヘルパーを作成する場合は、一般にから派生した`TagHelper`、これにアクセスするにより、`Process`メソッドです。

1. いう新しい ASP.NET Core プロジェクトを作成する**AuthoringTagHelpers**です。 認証は、このプロジェクトの必要はありません。

2. 呼ばれるタグ ヘルパーを保持するフォルダーを作成する*TagHelpers*です。 *TagHelpers*フォルダーは*いない*必要に応じてが、妥当な規則です。 今すぐ始めましょういくつかの単純なタグ ヘルパーを記述します。

## <a name="a-minimal-tag-helper"></a>最小タグ ヘルパー

このセクションでは、電子メール タグを更新するタグ ヘルパーを記述します。 例:

```html
<email>Support</email>
   ```

サーバーは次のように、そのマークアップを変換するのに、電子メール タグ ヘルパーを使用します。

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

つまり、アンカー タグにより、この電子メールのリンク。 これを行う場合は、ブログ エンジンを作成して、同じドメインにすべてマーケティング、サポート、およびその他の連絡先の電子メールを送信する際に必要する可能性があります。

1.  次の追加`EmailTagHelper`クラスを*TagHelpers*フォルダーです。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **注:**
    
    * タグ ヘルパーの使用をルート クラスの名前を持つ要素を対象とする名前付け規則 (マイナス、 *TagHelper*クラス名の部分)。 この例では、ルート名で**電子メール**TagHelper は*電子メール*ので、`<email>`タグの対象となります。 後でこのメソッドをオーバーライドする方法を示しますが、この名前付け規則がほとんどタグ ヘルパーの動作する必要があります。
    
    * `EmailTagHelper` クラスは `TagHelper` から派生したものです。 `TagHelper`クラス タグ ヘルパーを記述するためのメソッドとプロパティを提供します。
    
    * オーバーライドされた`Process`タグ ヘルパーが実行時にメソッドを制御します。 `TagHelper`クラスには、非同期バージョンも用意されています (`ProcessAsync`) 同じパラメーターを使用します。
    
    * コンテキスト パラメーターを`Process`(および`ProcessAsync`) 現在の HTML タグの実行に関連付けられている情報が含まれています。
    
    * 出力パラメーターを`Process`(および`ProcessAsync`)、HTML タグとコンテンツの生成に使用される元のソースの代表的なステートフルな HTML 要素が含まれています。
    
    * クラス名のサフィックスを持つ**TagHelper**、これは*いない*、必要なことがベスト プラクティス規則と考えられますが、します。 クラスを宣言します。
    
    ```csharp
    public class Email : TagHelper
    ```

2.  させる、 `EmailTagHelper` 、すべての Razor ビューに使用可能なクラスを追加、`addTagHelper`ディレクティブを*Views/_ViewImports.cshtml*ファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    上記のコードは、使用可能なアセンブリ内のすべてのタグ ヘルパーを指定するのにワイルドカードの構文を使用します。 後の最初の文字列`@addTagHelper`を読み込むタグ ヘルパーを指定します (使用する"*"すべてのタグ ヘルパーの)、2 番目の文字列"AuthoringTagHelpers"アセンブリを指定するのには、タグ ヘルパーとします。 また、ワイルドカードの構文を使用して ASP.NET Core MVC タグ ヘルパーの 2 番目の行がもたらすことに注意してください (これらのヘルパーは、後ほど[タグ ヘルパーの概要](intro.md))。`@addTagHelper`タグ ヘルパーの Razor ビューを使用できるようにするディレクティブ。 また、次に示すように、タグ ヘルパーの完全修飾名 (FQN) を指定できます。
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    タグ ヘルパーに追加する、FQN を使用するビューを最初に追加する、FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、およびアセンブリ名では、(*AuthoringTagHelpers*)。 ほとんどの開発者は、ワイルドカードの構文を使用するを選びます。 [タグ ヘルパーの概要](intro.md)タグ ヘルパーの追加、削除、階層、およびワイルドカードの構文の詳細を見ていきます。
    
3.  内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  アプリを実行して、アンカー マークアップを含む電子メール タグが置換されることを確認できるように HTML ソースの表示をお気に入りのブラウザーを使用して (たとえば、 `<a>Support</a>`)。 *サポート*と*マーケティング*は、リンクとして表示はありません、`href`ように機能する属性。 解決する次のセクションでします。

注: HTML タグと属性、タグ、クラス名および属性で Razor、および c# のように小文字は区別されません。

## <a name="setattribute-and-setcontent"></a>SetAttribute と SetContent

このセクションで更新されます、`EmailTagHelper`電子メール用の有効なアンカー タグが作成できるようにします。 Razor ビューから情報に更新されます (の形式で、`mail-to`属性) と、アンカーの生成に使用します。

更新プログラム、`EmailTagHelper`を次のクラス。

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**注:**

* タグ ヘルパーのクラスおよびプロパティを pascal で名前に変換されます、 [kebab ケースを下げる](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)です。 そのため、使用する、`MailTo`属性を使用する`<email mail-to="value"/>`と同等です。

* 最後の行は、最低限の機能のタグ ヘルパーの完了したコンテンツを設定します。

* 強調表示された行は、属性を追加するための構文を示しています。

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

属性コレクションに現在存在しない限り、その方法は、属性"href"できます。 使用することも、`output.Attributes.Add`タグ ヘルパー属性をタグの属性のコレクションの末尾に追加します。

1.  内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  アプリケーションを実行し、適切なリンクが生成されることを確認します。
    
    > [!NOTE]
    >電子メール タグ自己終了を記述する場合は (`<email mail-to="Rick" />`)、最終的な出力は自己終了になります。 開始タグのみを持つタグを記述する機能を有効にする (`<email mail-to="Rick">`) に次のクラスを装飾する必要があります。
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    自己終了電子メール タグ ヘルパーでは、出力ようになります`<a href="mailto:Rick@contoso.com" />`です。 自己終了アンカー タグは、有効な HTML を作成するのにはしたいはありませんが、自己終了タグのヘルパーを作成する場合があります。 タグ ヘルパーの種類の設定、`TagMode`タグを読み取った後のプロパティです。
    
### <a name="processasync"></a>ProcessAsync

このセクションでは非同期電子メール ヘルパーを記述します。

1.  置換、`EmailTagHelper`クラスを次のコードで。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **注:**

    * このバージョンでは、非同期`ProcessAsync`メソッドです。 非同期の`GetChildContentAsync`を返します、`Task`を含む、`TagHelperContent`です。

    * 使用して、`output`パラメーターを HTML 要素の内容を取得します。

2.  次の変更を行う、 *Views/Home/Contact.cshtml*ファイルのタグ ヘルパーは、ターゲット電子メールを取得できるようにします。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  アプリを実行して、有効な電子メールのリンクが生成されることを確認します。

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll、PreContent.SetHtmlContent および PostContent.SetHtmlContent

1.  次の追加`BoldTagHelper`クラスを*TagHelpers*フォルダーです。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **注:**
    
    * `[HtmlTargetElement]`属性のパスを示す HTML 属性を含む任意の HTML 要素が"bold"という名前の属性パラメーターは一致と`Process`クラスでオーバーライド メソッドが実行されます。 この例で、`Process`メソッドは、"bold"属性を削除しとを含むマークアップを囲む`<strong></strong>`です。
    
    * 既存のタグはコンテンツを交換しないため、開始を書き込む必要があります`<strong>`とタグ付け、`PreContent.SetHtmlContent`メソッドと終了`</strong>`とタグ付け、`PostContent.SetHtmlContent`メソッドです。
    
2.  変更、 *About.cshtml*を含むビュー、`bold`属性の値。 完成したコードは、以下に示します。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  アプリを実行します。 好みのブラウザーを使用するには、ソースを検査して、マークアップを確認してください。

    `[HtmlTargetElement]`上の属性のみが対象"bold"の属性名を提供する HTML マークアップ。 `<bold>`タグ ヘルパーによって要素が変更されていません。

4. コメント アウト、`[HtmlTargetElement]`属性行とそれは、対象とする既定`<bold>`タグ、フォームの HTML マークアップは、`<bold>`です。 ただし、既定の名前付け規則は、クラス名に一致**太字**に TagHelper`<bold>`タグ。

5. アプリを実行していることを確認、`<bold>`タグがタグ ヘルパーによって処理されます。

複数のクラスを装飾`[HtmlTargetElement]`ターゲットの論理 OR で結果の属性です。 たとえば、次のコードを使用するには、太字のタグまたは太字属性は一致します。

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

同じステートメントに複数の属性を追加するときに、ランタイムとして扱います論理 and。 たとえば、次のコードで HTML 要素必要がありますという名前が"bold"という名前の属性を"bold"(`<bold bold />`) 一致するようにします。

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

使用することも、`[HtmlTargetElement]`を対象となる要素の名前を変更します。 場合の例については、`BoldTagHelper`ターゲットに`<MyBold>`タグ、次の属性を使用すると。

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a>タグ ヘルパーにモデルを渡す

1.  追加、*モデル*フォルダーです。

2.  次の `WebsiteContext` クラスを *Models* フォルダーに追加します。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  次の追加`WebsiteInformationTagHelper`クラスを*TagHelpers*フォルダーです。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **注:**
    
    * 前述のように、タグ ヘルパーにより、c# を pascal クラス名とプロパティのでにタグ ヘルパー [kebab ケースを下げる](http://wiki.c2.com/?KebabCase)です。 そのため、使用する、 `WebsiteInformationTagHelper` Razor での記述`<website-information />`です。
    
    * ターゲット要素に明示的に識別しない、`[HtmlTargetElement]`属性、そのため、既定の`website-information`の対象となります。 場合は、次の属性 (kebab ケースではありませんが、クラス名と一致することに注意) が適用されます。
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    低い kebab ケース タグ`<website-information />`は一致しません。 使用する場合、`[HtmlTargetElement]`属性、kebab ケース次に示すようを使用するとします。
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * 自己終了要素のコンテンツがあるありません。 この例では、Razor マークアップでは、自己終了タグですがタグ ヘルパーの作成は、[セクション](http://www.w3.org/TR/html5/sections.html#the-section-element)要素 (自己終了ではないと、対応するには、内のコンテンツを記述する、`section`要素)。 そのため、設定する必要があります`TagMode`に`StartTagAndEndTag`出力に書き込みます。 行の設定をコメントする代わりに、`TagMode`と終了タグのマークアップを記述します。 (例マークアップがこのチュートリアルの後半で提供されています。)
    
    * `$` (ドル記号) で、次の行を使用して、[補間文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  次のマークアップを追加、 *About.cshtml*ビュー。 強調表示されたマークアップでは、web サイトの情報を表示します。
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > 次に示す Razor マークアップ。
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor、`info`属性は、文字列ではなく、クラスと c# コードを記述します。 しなくても、任意の文字列ではないタグ ヘルパー属性を記述する必要があります、`@`文字です。
    
5.  アプリを実行し、バージョン情報を表示する web サイトの情報に移動します。

    >[!NOTE]
    >終了タグで、次のマークアップを使用してでは、行を削除する`TagMode.StartTagAndEndTag`タグ ヘルパーで。
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>タグ ヘルパーを条件します。

条件タグ ヘルパーは、true 値を渡されるときに出力を表示します。

1.  次の追加`ConditionTagHelper`クラスを*TagHelpers*フォルダーです。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  内容を置き換える、 *Views/Home/Index.cshtml*次のマークアップ ファイル。

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  置換、`Index`メソッドで、`Home`コント ローラーを次のコード。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  アプリを実行して、ホーム ページに移動します。 条件付きでマークアップ`div`はレンダリングされません。 クエリ文字列の追加`?approved=true`URL に (たとえば、 `http://localhost:1235/Home/Index?approved=true`)。 `approved`true に設定され、条件付きマークアップが表示されます。

>[!NOTE]
>使用して、 [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子を太字タグ ヘルパーで行ったように文字列を指定するのではなく、対象の属性を指定します。
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子は、コードを保護する必要があります、これまでリファクタリングを行うこと (名前を変更する可能性があります`RedCondition`)。

### <a name="avoiding-tag-helper-conflicts"></a>タグ ヘルパーの競合を避ける

このセクションでは、自動リンク タグ ヘルパーのペアを作成します。 最初は、マークアップを含む、HTML アンカー タグ同じ URL を含む (および、URL へのリンクを生成するため) に HTTP で始まる URL で置き換えられます。 2 つ目は同じ操作を行いますは URL の www を開始します。

これら 2 つのヘルパーが密接に関連し、それらを今後リファクターする場合があります、ためにではこのままに同じファイルにします。

1.  次の追加`AutoLinkerHttpTagHelper`クラスを*TagHelpers*フォルダーです。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper`クラス ターゲット`p`要素および使用[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)アンカーを作成します。

2.  末尾に次のマークアップを追加、 *Views/Home/Contact.cshtml*ファイル。

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  アプリケーションを実行し、タグ ヘルパー アンカーが正しく表示されることを確認します。

4.  更新プログラム、`AutoLinker`クラスに含める、 `AutoLinkerWwwTagHelper` www テキスト www の元のテキストが含まれているアンカー タグに変換します。 更新されたコードは、以下が強調表示されます。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  アプリを実行します。 通知 www テキストがリンクとしてレンダリングされますが、HTTP テキストではありません。 両方のクラスでブレークポイントを配置した場合は、HTTP タグ ヘルパー クラスが最初に実行されることが確認できます。 問題には、タグ ヘルパーの出力がキャッシュされると、HTTP タグ ヘルパーからキャッシュされた出力 WWW タグ ヘルパーの実行時に上書きされます。 このチュートリアルで後ほどおでタグ ヘルパーが実行される順序を制御する方法が表示されます。 コードは、次のように修正されます。

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >Edition では、まず自動リンク タグ ヘルパーの次のコードで、ターゲットのコンテンツを取得しました。
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >つまり、呼び出す`GetChildContentAsync`を使用して、`TagHelperOutput`に渡される、`ProcessAsync`メソッドです。 説明したように以前は、出力がキャッシュされるため、最後にタグ付け wins を実行するためのヘルパー。 次のコードに問題が修正されました。
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >上記のコードは、コンテンツが変更されており場合は、出力バッファーからコンテンツを取得するかどうかをチェックします。

6.  アプリを実行して、2 つのリンクが期待どおりに動作することを確認します。 この自動リンカー タグ ヘルパーが適切な表示される、中に微妙な問題があります。 WWW タグ ヘルパーが実行される場合、最初 www へのリンクは不正確になります。 追加することで、コードを更新、`Order`オーバー ロードで、タグが実行される順序を制御します。 `Order`プロパティが同じ要素を対象とするその他のタグ ヘルパーの基準とした実行順序を決定します。 既定の順序の値は 0 と低い値を持つインスタンスが最初に実行されます。

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    上記のコードは、WWW タグ ヘルパーの前に、HTTP タグ ヘルパーが実行される保証されます。 変更`Order`に`MaxValue`WWW タグの生成されたマークアップが正しいことを確認してください。

## <a name="inspecting-and-retrieving-child-content"></a>検査および子コンテンツを取得します。

タグ ヘルパーは、コンテンツを取得するいくつかのプロパティを提供します。

-  結果`GetChildContentAsync`に追加できます`output.Content`です。
-  結果を調査して`GetChildContentAsync`で`GetContent`です。
-  変更した場合`output.Content`、TagHelper 本体は実行またはない限り、表示されません`GetChildContentAsync`自動リンカー サンプルのようにします。

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  複数回呼び出す`GetChildContentAsync`同じ値を返すし、再実行されません、`TagHelper`本文のキャッシュされた結果を使用しないことを示す場合は false にパラメーターを渡す場合を除き、します。
