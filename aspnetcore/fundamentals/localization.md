---
title: "グローバリゼーションとローカリゼーション ASP.NET Core"
author: rick-anderson
description: "コンテンツをさまざまな言語およびカルチャにローカライズするため、ASP.NET Core がミドルウェアとサービスを提供する方法について説明します。"
keywords: "ASP.NET Core、ローカリゼーション、カルチャ、言語、リソース ファイル、グローバリゼーション、国際化、ロケール"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: a3fdbf8a1ab4ca397824a46da445fa34ddd35204
ms.sourcegitcommit: 4be61844141d3cfb6f263636a36aebd26e90fb28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/22/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a>グローバリゼーションとローカリゼーション ASP.NET Core

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Damien Bowden](https://twitter.com/damien_bod)、 [Bart Calixto](https://twitter.com/bartmax)、 [Nadeem Afana](https://twitter.com/NadeemAfana)、および[Hisham Bin Ateya](https://twitter.com/hishambinateya)

ASP.NET Core 多言語 web サイトを作成すると、多数の対象者に到達するようにサイトが許可されます。 ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。

国際化では、[グローバリゼーション](https://docs.microsoft.com/dotnet/api/system.globalization)と[ローカリゼーション](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)です。 グローバリゼーションとは異なるカルチャをサポートするアプリを設計するプロセスです。 グローバリゼーションでは、入力、表示、および定義済みの一連の特定の地理的領域に関連する言語のスクリプトの出力のサポートを追加します。

ローカリゼーションとは、グローバライズされたアプリ、特定のカルチャとロケールに、ローカライズの可能性が既に処理に対応させるプロセス。  詳細については、次を参照してください。**グローバリゼーションおよびローカリゼーションの条項**このドキュメントの末尾近くです。

アプリのローカライズには、次の項目が含まれます。

1. アプリのコンテンツをローカライズできるようにします

2. 言語およびサポートするカルチャのローカライズされたリソースを提供します。

3. 要求ごとに言語/カルチャを選択するための戦略を実装します。

## <a name="make-the-apps-content-localizable"></a>アプリのコンテンツをローカライズできるようにします

ASP.NET Core で導入された`IStringLocalizer`と`IStringLocalizer<T>`ローカライズされたアプリの開発時に、生産性を向上させるために設計されています。 `IStringLocalizer`使用して、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)と[ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)を実行時にカルチャ固有のリソースを提供します。 シンプルなインターフェイスが、インデクサー、および`IEnumerable`ローカライズされた文字列を返すためです。 `IStringLocalizer`リソース ファイルに既定の言語識別文字列を格納するには不要です。 開発の早い段階でリソース ファイルを作成する必要はなく、ローカリゼーションの対象となるアプリを開発できます。 次のコードでは、ローカライズ文字列「タイトルは」をラップする方法を示します。

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

上記のコードで、`IStringLocalizer<T>`実装に由来[依存性の注入](dependency-injection.md)です。 「タイトルは」のローカライズされた値が見つからないかどうかは、インデクサーのキーが返されます、つまり、文字列「タイトルは」です。 アプリで、既定の言語のリテラル文字列をそのままし、ローカライザーにラップされるよう、アプリの開発に専念することができます。 既定の言語でアプリを開発し、既定のリソース ファイルを作成しなくても、ローカリゼーション手順用に準備します。 または、従来のアプローチを使用し、既定の言語文字列を取得するキーを指定できます。 多くの開発者にとって新しいワークフローの既定の言語がない*.resx*ファイルと、文字列リテラルをラップすることだけがアプリをローカライズするオーバーヘッドを削減できます。 他の開発者には、そのやすく長い文字列リテラルを操作およびローカライズされた文字列を更新しやすくように従来の作業の流れを選びます。

使用して、 `IHtmlLocalizer<T>` HTML を格納しているリソースの実装です。 `IHtmlLocalizer`HTML エンコードされたリソース文字列で書式設定される引数が、HTML ではありませんは、リソース文字列そのものをエンコードします。 サンプルでは、次の値のみ強調表示されます`name`パラメーターは、HTML エンコードします。

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**注:**のみテキストと HTML ではないをローカライズする場合、通常します。

最下位のレベルを取得できます`IStringLocalizerFactory`不在[依存性の注入](dependency-injection.md):

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

上記のコードを示します、2 つのファクトリの各メソッドを作成します。

領域で、コント ローラーによって、ローカライズされた文字列をパーティション分割または 1 つのコンテナーを持つことができます。 サンプル アプリで、ダミーという名前のクラス`SharedResource`共有リソースのために使用します。

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

一部の開発者が使用して、`Startup`グローバルまたは共有の文字列を格納するクラス。 以下のサンプルで、`InfoController`と`SharedResource`ローカライザーが使用されます。

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>ビューのローカライズ

`IViewLocalizer`サービス提供のローカライズされた文字列、[ビュー](https://docs.microsoft.com/aspnet/core)です。 `ViewLocalizer`クラスは、このインターフェイスを実装し、ビューのファイル パスからリソースの場所を検索します。 次のコードは、の既定の実装を使用する方法を示しています`IViewLocalizer`:。

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

既定の実装`IViewLocalizer`ビューのファイルの名前に基づいてリソース ファイルを検索します。 共有のグローバル リソース ファイルを使用するオプションはありません。 `ViewLocalizer`実装を使用してローカライザー `IHtmlLocalizer`Razor しない HTML のため、ローカライズされた文字列をエンコードします。 リソース文字列をパラメーター化できると`IViewLocalizer`HTML エンコードされますが、パラメーター、リソース文字列ではありません。 次の Razor マークアップを考慮してください。

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

フランス語のリソース ファイルに次が可能性があります。

| キー | [値] |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

描画のビューには、リソース ファイルから HTML マークアップが含まれます。

**注:**のみテキストと HTML ではないをローカライズする場合、通常します。

ビュー内の共有リソース ファイルを使用するのには、挿入`IHtmlLocalizer<T>`:

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>DataAnnotations ローカリゼーション

DataAnnotations エラー メッセージとローカライズ`IStringLocalizer<T>`です。 オプションを使用して`ResourcesPath = "Resources"`、エラー メッセージで`RegisterViewModel`は次のパスのいずれかで格納されていることができます。

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 および以上、非検証属性のローカライズされています。 ASP.NET Core MVC 1.0 は**いない**の非検証属性のローカライズされた文字列を検索します。

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>複数のクラスの 1 つのリソース文字列を使用します。

次のコードは、複数のクラスに検証属性の 1 つのリソース文字列を使用する方法を示しています。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

上記のコード、 `SharedResource` resx に対応するクラスには、検証メッセージを格納します。 この方法を DataAnnotations を使ってのみ`SharedResource`、各クラスのリソースではなくです。 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>言語およびサポートするカルチャのローカライズされたリソースを提供します。  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures と SupportedUICultures

ASP.NET Core では、2 つのカルチャ値を指定できます。`SupportedCultures`と`SupportedUICultures`です。 [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)オブジェクトに対する`SupportedCultures`日付、時刻、数、および通貨の書式設定など、カルチャに依存する関数の結果を決定します。 `SupportedCultures`テキスト、大文字と小文字の表記規則、および文字列比較の並べ替え順序を決定します。 参照してください[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)サーバーがカルチャを取得する方法の詳細。 `SupportedUICultures`を決定し、これは文字列 (から*.resx*ファイル) を検索、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)です。 `ResourceManager`によって決定されるカルチャ固有の文字列を単に検索`CurrentUICulture`です。 .NET のすべてのスレッドが`CurrentCulture`と`CurrentUICulture`オブジェクト。 ASP.NET Core は、カルチャに依存する関数を表示するときに、これらの値を検査します。 たとえば、現在のスレッドのカルチャが"EN-US"(英語、米国) に設定されている`DateTime.Now.ToLongDateString()`場合は、「木曜日、2016 年 2 月 18日」が表示されます`CurrentCulture`設定されている"ES-ES"(スペイン語、スペイン) に出力になります"jueves、18 de febrero de 2016" です。

## <a name="resource-files"></a>リソース ファイル

リソース ファイルは、ローカライズ可能な文字列をコードから分離するために役立ちますメカニズムです。 既定以外の言語の翻訳された文字列は分離*.resx*リソース ファイル。 という名前のスペイン語のリソース ファイルを作成するなど、 *Welcome.es.resx*を含む文字列を変換します。 "es"では、スペイン語の言語コードを示します。 Visual Studio でこのリソース ファイルを作成します。

1. **ソリューション エクスプ ローラー**、リソース ファイルを格納するフォルダーを右クリックして >**追加** > **新しい項目の**します。

    ![入れ子になったコンテキスト メニュー: ソリューション エクスプ ローラーでコンテキスト メニューは、リソース用に開かれています。 2 つ目のコンテキスト メニューが強調表示されている新しい項目のコマンドを示す追加用に開いています。](localization/_static/newi.png)

2. **検索には、テンプレートがインストールされている**ボックスで、「リソース」を入力し、ファイルの名前します。

    ![[新しい項目の追加] ダイアログ](localization/_static/res.png)

3. キーの値 (ネイティブの文字列) で入力、**名前**列と翻訳された文字列を**値**列です。

    ![Hello の 名前 列と 値 列内の単語 Hola (スペイン語でのこんにちは) Welcome.es.resx ファイル (へようこそ のリソース ファイルのスペイン語)](localization/_static/hola.png)

    Visual Studio の表示、 *Welcome.es.resx*ファイル。

    ![ソリューション エクスプ ローラーのようこそスペイン語 (es) のリソース ファイルを表示](localization/_static/se.png)

<a name="error"></a>

Visual Studio 2017 Preview バージョン 15.3 を使用している場合、リソース エディターでエラー インジケーターが表示されます。 削除、 *ResXFileCodeGenerator*値から、*カスタム ツール*このエラー メッセージを防ぐためにプロパティ グリッド。

![Resx エディター](localization/_static/err.png)

また、このエラーを無視することができます。 次のリリースでこの問題を解決する予定です。

## <a name="resource-file-naming"></a>リソース ファイルの名前付け

リソースは、アセンブリ名を除いて、クラスの完全な型名の名前です。 フランス語のリソースをメインのアセンブリがプロジェクトになど`LocalizationWebsite.Web.dll`クラスの`LocalizationWebsite.Web.Startup`という名前になります*Startup.fr.resx*です。 クラスのリソース`LocalizationWebsite.Web.Controllers.HomeController`という名前になります*Controllers.HomeController.fr.resx*です。 対象となるクラスの名前空間は、アセンブリ名と同じではない場合は、完全な型名を必要があります。 たとえば、サンプルには、プロジェクトの種類用のリソース`ExtraNamespace.Tools`という名前になります*ExtraNamespace.Tools.fr.resx*です。

サンプル プロジェクトで、`ConfigureServices`メソッドのセット、 `ResourcesPath` 「リソース」をそのため、home コント ローラーのフランス語りソース ファイルのプロジェクトの相対パスは*Resources/Controllers.HomeController.fr.resx*です。 また、リソース ファイルを整理するのにフォルダーを使用することができます。 Home コント ローラーで、パスはなります*Resources/Controllers/HomeController.fr.resx*です。 使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルは、プロジェクトの基本ディレクトリに移動します。 リソース ファイルの`HomeController`という名前になります*Controllers.HomeController.fr.resx*です。 ドットまたはパスの名前付け規則を選択した場合は、リソース ファイルを整理する方法によって異なります。

| リソース名 | ドットまたはパスの名前を付ける |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | ドット  |
| Resources/Controllers/HomeController.fr.resx  | パス |
|    |     |

リソース ファイルを使用して`@inject IViewLocalizer`Razor ビューで、同様のパターンに従います。 ビュー用のリソース ファイルは、ドット名前またはパスの名前付けを使用して名前付きことができます。 Razor ビューのリソース ファイルは、関連するビューのファイルのパスを模倣します。 設定と仮定した場合、`ResourcesPath`に「リソース」、フランス語のリソース ファイルに関連付けられて、 *Views/Home/About.cshtml*ビューでは、次のいずれかの可能性があります。

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルのビューは、ビューと同じフォルダーに配置するとします。

## <a name="culture-fallback-behavior"></a>カルチャ フォールバック動作

例として、既定のリソース ファイルは読み取り".fr"カルチャの指定子を削除し、カルチャをフランス語に設定を使用している場合と、文字列はローカライズします。 リソース マネージャーでは、既定値またはフォールバック リソースを何も行われませんが、要求されたカルチャを満たす場合に指定します。 要求されたカルチャのリソースが見つかりません必要があります、既定のリソース ファイルは持っていない場合にだけ、キーを返す場合は。

### <a name="generate-resource-files-with-visual-studio"></a>Visual Studio でのリソース ファイルを生成します。

Visual Studio で、ファイル名にカルチャせず、リソース ファイルを作成した場合 (たとえば、 *Welcome.resx*)、Visual Studio は各文字列のプロパティを使用して、c# クラスを作成します。 通常、たくないと ASP.NET Core です。通常、既定値がない*.resx*リソース ファイル (A *.resx*カルチャ名のないファイル)。 作成することをお勧めします。、 *.resx*カルチャ名を持つファイル (たとえば*Welcome.fr.resx*)。 作成するときに、 *.resx*カルチャ名、Visual Studio でのファイルは、クラス ファイルを生成しません。 多くの開発者に**いない**既定の言語リソース ファイルを作成します。

### <a name="add-other-cultures"></a>その他のカルチャを追加します。

(既定の言語) 以外の言語とカルチャの組み合わせごとに一意のリソース ファイルが必要です。 ISO 言語コードがファイル名の一部となるリソース ファイルの新規作成して異なるカルチャとロケールのリソース ファイルを作成する (たとえば、 **en-us**、 **fr ca**、および**en gb**)。 これらの ISO コードを配置しているファイル名の間、 *.resx*ファイル名拡張子として*Welcome.es MX.resx* (スペイン語/メキシコ)。 カルチャ ニュートラル言語を指定するには、国コードを削除 (`MX`前の例で)。 スペイン語のニュートラル カルチャのリソース ファイル名が*Welcome.es.resx*です。

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>要求ごとに言語/カルチャを選択するための戦略を実装します。  

### <a name="configure-localization"></a>ローカリゼーションを構成します。

ローカリゼーションがで構成されている、`ConfigureServices`メソッド。

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization`ローカリゼーション サービスをサービス コンテナーに追加します。 上記のコードは、「リソース」のリソース パスを設定します。

* `AddViewLocalization`ファイルのローカライズされた表示のサポートを追加します。 このサンプル ビューでのローカライズをビュー ファイルのサフィックスに基づきます。 たとえば"fr"で、 *Index.fr.cshtml*ファイル。

* `AddDataAnnotationsLocalization`ローカライズのサポートが追加されて`DataAnnotations`を介してメッセージを検証`IStringLocalizer`抽象化します。

### <a name="localization-middleware"></a>ローカリゼーション ミドルウェア

要求時に、現在のカルチャは、ローカリゼーションで設定[ミドルウェア](middleware.md)です。 ローカライズ ミドルウェアが有効になっている、`Configure`メソッドの*Program.cs*ファイル。 要求のカルチャをチェックするすべてのミドルウェアの前に、ローカリゼーション ミドルウェアを構成する必要があります、注意してください (たとえば、 `app.UseMvcWithDefaultRoute()`)。

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization`初期化、`RequestLocalizationOptions`オブジェクト。 すべての要求リストの`RequestCultureProvider`で、`RequestLocalizationOptions`が列挙され、要求のカルチャが正常に決定できる最初のプロバイダーが使用されます。 既定のプロバイダーに由来、`RequestLocalizationOptions`クラス。

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

既定の一覧は、最も限定的に最も固有から移動します。 記事の後半では、順序を変更し、であっても、カスタム プロバイダーを追加する方法が表示されます。 要求のカルチャを判断、プロバイダーの場合、`DefaultRequestCulture`を使用します。

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

いくつかのアプリでは、クエリ文字列を使用して設定は、[カルチャおよび UI カルチャ](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)です。 をクッキーまたは Accept-language ヘッダーのアプローチを使用するアプリの URL にクエリ文字列を追加することはデバッグおよびコードのテストに役立ちます。 既定では、`QueryStringRequestCultureProvider`の最初のローカリゼーション プロバイダーとして登録されて、 `RequestCultureProvider`  ボックスの一覧です。 クエリ文字列パラメーターを渡す`culture`と`ui-culture`です。 次の例では、特定のカルチャ (言語および地域) をスペイン語/メキシコに設定します。

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

2 つのいずれかのみを渡す場合 (`culture`または`ui-culture`)、クエリ文字列のプロバイダーに渡された 1 つを使用して両方の値を設定します。 たとえば、カルチャだけを設定は両方を設定、`Culture`と`UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

実稼働アプリケーションは、ASP.NET Core カルチャ cookie を使用してカルチャを設定するためのメカニズムを提供して多くの場合、します。 使用して、 `MakeCookieValue` cookie を作成します。

`CookieRequestCultureProvider` `DefaultCookieName`カルチャ情報を指定したユーザーを追跡するために使用する既定の cookie 名を返します。 既定の cookie 名は"です。AspNetCore.Culture"です。

Cookie の形式が`c=%LANGCODE%|uic=%LANGCODE%`ここで、`c`は`Culture`と`uic`は`UICulture`例。

    c=en-UK|uic=en-US

のみを指定するカルチャ情報と UI カルチャのいずれかの場合、指定されたカルチャはカルチャ情報と UI カルチャの両方に使用されます。

### <a name="the-accept-language-http-header"></a>ブラウザーの言語の HTTP ヘッダー

[Accept-language ヘッダー](https://www.w3.org/International/questions/qa-accept-lang-locales)ほとんどのブラウザーで設定可能であり、ユーザーの言語を指定するためのものが最初。 この設定は、ブラウザーが送信に設定されているかは、基になるオペレーティング システムから継承する新機能を示します。 ブラウザーの要求で Accept Language HTTP ヘッダーは、ユーザーの優先言語を検出するない方法ではありません (を参照してください[ブラウザーの言語設定を設定する](https://www.w3.org/International/questions/qa-lang-priorities.en.php))。 運用アプリには、ユーザーの任意のカルチャをカスタマイズする方法を含める必要があります。

### <a name="set-the-accept-language-http-header-in-ie"></a>IE で Accept Language HTTP ヘッダーを設定します。

1. 歯車アイコンをタップ**インターネット オプション**です。

2. タップ**言語**します。

    ![インターネット オプション](localization/_static/lang.png)

3. タップ**言語設定**です。

4. タップ**言語を追加**です。

5. 言語を追加します。

6. 言語をタップしてタップ**上へ移動**です。

### <a name="use-a-custom-provider"></a>カスタム プロバイダーを使用します。

お客様は、データベース内の言語とカルチャを格納できるようにするとします。 ユーザーの姓名を検索するプロバイダーを記述することもできます。 次のコードは、カスタム プロバイダーを追加する方法を示しています。

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

使用して`RequestLocalizationOptions`を追加またはローカライズのプロバイダーを削除します。

### <a name="set-the-culture-programmatically"></a>カルチャをプログラムで設定します。

このサンプル**Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)を設定するための UI が含まれています、`Culture`です。 *Views/Shared/_SelectLanguagePartial.cshtml*ファイルでは、サポートされているカルチャの一覧から、カルチャを選択することができます。

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml*にファイルが追加、`footer`できるすべてのビューに使用できるようにレイアウト ファイルのセクション。

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage`メソッドはカルチャ cookie を設定します。

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

接続することはできません、 *_SelectLanguagePartial.cshtml*このプロジェクトのサンプル コードにします。 **Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)フローするコードが含ま、`RequestLocalizationOptions`を通じて部分 Razor を[依存性の注入](dependency-injection.md)コンテナーです。

## <a name="globalization-and-localization-terms"></a>グローバリゼーションとローカリゼーションの用語

アプリをローカライズするプロセスは、最新のソフトウェアの開発でよく使用される、関連する文字セットの基本的な理解とそれらに関連した問題の理解にも必要です。 すべてのコンピューターは、数値 (コード) としてテキストを格納、さまざまなシステムは別の番号を使用して、同じテキストを格納します。 ローカリゼーション処理は、特定のカルチャとロケールのアプリのユーザー インターフェイス (UI) の変換を指します。

[ローカライズ化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)は、グローバライズされたアプリのローカライズ可能を確認するための中間プロセスです。

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)カルチャ名の書式を設定"<languagecode2>-< country/regioncode2 >"ここで、<languagecode2>言語コードは、< country/regioncode2 > となりです。 たとえば、`es-CL`スペイン語 (チリ) 用`en-US`英語 (米国) の場合と`en-AU`英語 (オーストラリア) 用です。 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) ISO 639 言語に関連付けられている 2 文字の小文字のカルチャ コードと国または地域に関連付けられている 2 文字の大文字となり、ISO 3166 の組み合わせです。  参照してください[言語カルチャ名](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)です。

国際化は、"I18N"に多くの場合、省略されています。 省略形は、最初と最後の文字を受け取り、それらの間でため 18 文字の数は、最後の"N"、"I"、最初の文字の数を意味します。 (G11N) のグローバリゼーションおよびローカリゼーション (L10N) にも当てはまります。

条件:

* グローバリゼーション (G11N): アプリ別の言語と地域をサポートするためのプロセスです。
* ローカライズ (L10N): のカスタマイズ プロセスの特定の言語および地域のアプリです。
* 国際化 (I18N): は、グローバリゼーションおよびローカリゼーションの両方について説明します。
* カルチャ: は、言語および、必要に応じて、領域です。
* ニュートラル カルチャ: 指定した言語が地域ではないカルチャ。 (たとえば"en"、"es")
* 特定のカルチャ: 指定された言語と地域を持つカルチャ。 (例"EN-US"、"EN-GB"、"es CL") の
* カルチャの親: を含む特定のカルチャ ニュートラル カルチャです。 (たとえば、"en"は"EN-US"と"EN-GB"の親カルチャ)
* ロケール: ロケールとは、カルチャと同じです。

## <a name="additional-resources"></a>その他の技術情報

* [Localization.StarterWeb プロジェクト](https://github.com/aspnet/entropy)アーティクルで使用します。
* [Visual Studio でのリソース ファイル](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [.Resx ファイル内のリソース](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
