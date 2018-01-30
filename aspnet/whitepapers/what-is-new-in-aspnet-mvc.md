---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: "ASP.NET MVC 2 の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。 このドキュメントはダウンロードもできます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-mvc-2"></a>ASP.NET MVC 2 の新機能
====================
> このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。 このドキュメントも[ダウンロード](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[概要](#_TOC1)   
[ASP.NET MVC 2 に ASP.NET MVC 1.0 プロジェクトをアップグレードします。](#_TOC2)   
[新機能](#_TOC3)   
[テンプレート化されたヘルパー](#_TOC3_1)   
[領域](#_TOC3_2)   
[非同期コント ローラーのサポート](#_TOC3_3)   
[DefaultValueAttribute アクション メソッド パラメーターのサポート](#_TOC3_4)   
[モデル バインダーのバイナリ データをバインドするためのサポート](#_TOC3_5)   
[ModelMetadata と ModelMetadataProvider クラス](#_TOC3_6)   
[DataAnnotations 属性のサポート](#_TOC3_7)   
[モデルの検証コントロール プロバイダー](#_TOC3_8)   
[クライアント側の検証](#_TOC3_9)   
[Visual Studio 2010 用の新しいコード スニペット](#_TOC3_10)   
[新しい RequireHttpsAttribute アクション フィルター](#_TOC3_11)   
[HTTP メソッドの動詞のオーバーライド](#_TOC3_12)   
[テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス](#_TOC3_13)   
[Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。](#_TOC3_14)   
[Visual Studio の生成コードに固有では T4 テンプレートを .NET Framework のターゲット バージョンに](#_TOC3_15)[API の機能強化](#_TOC4)  
[重大な変更](#_TOC5)  
[免責事項](#_TOC6)  

## <a id="_TOC1"></a>概要

ASP.NET MVC 2 は、ASP.NET MVC 1.0 を上に構築し、豊富な機能強化と生産性を高めることに注目している機能を紹介します。 このリリースは ASP.NET MVC 1.0 と互換性のあるすべてのナレッジ、スキル、コード、および ASP.NET MVC 1.0 用の拡張機能の適用を続行します。

ASP.NET MVC の詳細については、次のリソースを参照してください。

- [ASP.NET MVC の MSDN ドキュメントへ](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC Web サイト](https://asp.net/mvc/)
- [ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>ASP.NET MVC 2 に ASP.NET MVC 1.0 プロジェクトをアップグレードします。

ASP.NET MVC 2 は、ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択するには、アプリケーション開発者の柔軟性、同じサーバー上の ASP.NET MVC 1.0 サイド バイ サイドでインストールできます。 アップグレードする方法については、ドキュメントを参照してください。 [ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする](https://go.microsoft.com/fwlink/?LinkID=185459)です。

## <a id="_TOC3"></a>新機能

このセクションの内容が導入された機能について説明します、MVC 2 リリースでします。

### <a id="_TOC3_1"></a>テンプレート化されたヘルパー

テンプレート化されたヘルパーは、編集用に自動的に関連付ける HTML 要素を使用してのデータ型を表示します。 たとえば、System.DateTime 型のデータが表示されたら、ビューで、日付選択カレンダー UI 要素に自動的に表示できます。 ASP.NET 動的データでのフィールド テンプレートの作業に似ています。 詳細については、次を参照してください。[データを表示するテンプレート化されたヘルパーを使用して](https://go.microsoft.com/fwlink/?LinkId=159062)MSDN Web サイトです。

### <a id="_TOC3_2"></a>領域

領域では、大規模な Web アプリケーションの複雑さを管理するために複数の小さなセクションに、大規模なプロジェクトを整理できます。 (「領域」) の各セクションは通常、大規模な Web サイトの別のセクションを表すし、コント ローラーとビューの関連する設定をグループ化するために使用します。 詳細については、次を参照してください。[チュートリアル: ASP.NET MVC アプリケーションによって、アプリケーションの領域を整理する](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN Web サイトです。

ソリューション エクスプ ローラーで、新しい領域を作成するにプロジェクトを右クリックして、[追加] をクリックしておよび領域をクリックします。 これには、領域名の入力を求めるダイアログ ボックスが表示されます。 領域の名前を入力した後、Visual Studio はプロジェクトに新しい領域を追加します。

次の図は、管理者、ブログの次の 2 つの領域を持つプロジェクトのレイアウトの例を示します。

![](what-is-new-in-aspnet-mvc/_static/image1.png)

領域を作成するときに、Visual Studio は、各領域に AreaRegistration から派生するクラスを追加します。 次の例で示すようにこのクラスは、領域とそのルートを登録するために必要です。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 の既定のプロジェクト テンプレートには、Global.asax ファイルのコードで RegisterAllAreas メソッドへの呼び出しが含まれています。 このメソッドは、AreaRegistration クラスから派生したすべての型を探して、インスタンス化して、型のインスタンスをインスタンスで RegisterArea メソッドを呼び出して、プロジェクト内の各領域を登録します。 次の例では、これを行う方法を示します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

RegisterArea メソッドでコンテキストを呼び出すことによって、名前空間を指定しないでください場合。Namespaces.Add メソッド、登録クラスの名前空間は、既定で使用されます。

### <a id="_TOC3_3"></a>非同期コント ローラーのサポート

ASP.NET MVC 2 では、要求を非同期的に処理するコント ローラーができるようになりました。 これについては、非ブロッキング古くてを呼び出してくださいを頻繁に (ネットワーク要求) などのブロッキング操作を呼び出すことがサーバーを許可することでパフォーマンスの向上につながります。 詳細については、次を参照してください。、 [ASP.NET MVC での非同期コント ローラーを使用して](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN のトピックです。

### <a id="_TOC3_4"></a>DefaultValueAttribute アクション メソッド パラメーターのサポート

System.ComponentModel.DefaultValueAttribute クラスは、アクション メソッドの引数パラメーターに対して指定する既定値を使用できます。 たとえば、次の既定のルートが定義されていると仮定します。

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

次のコント ローラーとアクション メソッドが定義されていると想定されます。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

次のような要求の Url はメソッドを呼び出すビュー アクション前の例で定義されています。

- /Article/View/123
- /記事/ビュー/123 ですか? ページ (実質的に同じ、前回の要求) 1 を =
- /記事/ビュー/123 ですか? ページ 2 を =

DefaultValueAttribute 属性がない、上記の一覧から最初の URL は機能しません、ページ引数は値が指定されていない null 非許容値型であるためです。

Visual Basic 2010 または Visual c# 2010 で、コードが書き込まれると場合、は、次の例のように、DefaultValueAttribute 属性ではなく省略可能なパラメーターを使用できます。

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>モデル バインダーのバイナリ データをバインドするためのサポート

これには 2 つの新しいオーバー ロードされた Html.Hidden ヘルパー base 64 エンコード文字列としてのバイナリ値をエンコードするがあります。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

一般的な使用は、ビューでオブジェクトのタイムスタンプを埋め込むにです。 たとえば、アプリケーションには、次の製品オブジェクトが含まれます。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

編集フォームは、次の例で示すように、フォームでタイムスタンプ プロパティを表示します。

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

このマークアップは、次の例のような base 64 エンコードされた文字列としてのタイムスタンプ値で、非表示の input 要素を表示します。

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

このフォームは、次の例で示すように、型の引数を持つアクション メソッドへポストされた可能性があります。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

アクション メソッドに、タイムスタンプ プロパティが設定されます正しくポストの base 64 エンコード文字列がバイト配列に変換されるためです。

### <a id="_TOC3_6"></a>ModelMetadata と ModelMetadataProvider クラス

ModelMetadataProvider クラスでは、ビュー内のモデルのメタデータを取得するための抽象化を提供します。 MVC 2 には System.ComponentModel.DataAnnotations 名前空間内の属性によって公開されるメタデータを利用できるようにする既定のプロバイダーが含まれています。 データベースまたは XML ファイルなど、他のデータ ストアからメタデータを提供するメタデータ プロバイダーを作成することができます。

ViewDataDictionary クラスでは、ModelMetadataProvider クラスによって、モデルから抽出されたメタデータを含む ModelMetadata オブジェクトを公開します。 これにより、テンプレート化されたヘルパーをこのメタデータを使用し、その出力は必要に応じて調整できます。

詳細については、ドキュメントを参照して、 [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)と[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)クラスです。

### <a id="_TOC3_7"></a>DataAnnotations 属性のサポート

ASP.NET MVC 2 (System.ComponentModel.DataAnnotations 名前空間で定義されている) RangeAttribute、RequiredAttribute、StringLengthAttribute、および RegexAttribute 検証属性の使用をサポートしている入力を提供するために、モデルへのバインド検証します。

詳細については、次を参照してください。[する方法: 検証モデル データ DataAnnotations 属性を使用した](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN Web サイトです。 これらの属性の使用方法について説明するサンプル プロジェクトはダウンロード[https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)です。

### <a id="_TOC3_8"></a>モデルの検証コントロール プロバイダー

モデル検証プロバイダー クラスは、モデルの検証ロジックを提供する抽象型を表します。 ASP.NET MVC には、System.ComponentModel.DataAnnotations 名前空間に含まれている検証の属性に基づいて既定のプロバイダーが含まれています。 モデルへのカスタム検証規則と検証規則のカスタム マッピングを定義する、独自の検証プロバイダーを作成することもできます。 詳細については、ドキュメントを参照して、 [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)クラスです。

### <a id="_TOC3_9"></a>クライアント側の検証

モデルの検証コントロール プロバイダー クラスは、クライアント側検証ライブラリで利用できる JSON でシリアル化されたデータの形でブラウザーに検証メタデータを公開します。 ASP.NET MVC 2 には、クライアント検証ライブラリおよび前に述べた DataAnnotations 名前空間の検証属性をサポートするアダプターが含まれます。 プロバイダー クラスを使用して、JSON データと代替のライブラリへの呼び出しを処理するアダプターを記述して他のクライアント検証ライブラリを使用することもできます。

### <a id="_TOC3_10"></a>Visual Studio 2010 用の新しいコード スニペット

ASP.NET MVC 2 の HTML コード スニペットのセットは、Visual Studio 2010 と共にインストールされます。 [ツール] メニューで、これらのスニペットの一覧を表示するには、コード スニペット マネージャーを選択します。 言語については、HTML を選択し、場所、ASP.NET MVC 2 を選択します。 コード スニペットを使用する方法の詳細については、Visual Studio のマニュアルを参照してください。

### <a id="_TOC3_11"></a>新しい RequireHttpsAttribute アクション フィルター

ASP.NET MVC 2 には、アクション メソッドとコント ローラーに適用できる新しい RequireHttpsAttribute クラスが含まれています。 既定では、フィルターは、SSL 対応 (HTTPS) に相当する、SSL 以外の HTTP 要求をリダイレクトします。

### <a id="_TOC3_12"></a>HTTP メソッドの動詞のオーバーライド

REST アーキテクチャ スタイルを使用して Web サイトを構築するときは、HTTP 動詞を使用してリソースに対して実行するアクションを決定します。 残りの部分では、アプリケーションのサポートを含む、共通の HTTP 動詞のすべての範囲の GET、PUT、POST、および削除する必要があります。

ASP.NET MVC 2 には、アクション メソッドとその機能のコンパクトな構文に適用できる新しい属性が含まれています。 これらの属性には、ASP.NET MVC を HTTP 動詞に基づいてアクション メソッドの選択が有効にします。 次の例で POST 要求が最初のアクション メソッドを呼び出すし、PUT 要求では、2 つ目のアクション メソッドを呼び出します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

以前のバージョンの ASP.NET MVC では、これらのアクション メソッドでは、次の例で示すようにより詳細な構文が必要です。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

ブラウザー GET と POST の HTTP 動詞のみサポートされているために、別の動詞が必要なアクションに投稿することはできません。 したがって rest ベースのすべての要求をネイティブにサポートすることはできません。

ただし、POST 操作、ASP.NET MVC 2 進行中の要求は、RESTful をサポートするために、新しい HttpMethodOverride HTML ヘルパー メソッドを紹介します。 このメソッドは、フォームが任意の HTTP メソッドを効果的にエミュレートするために非表示の input 要素を表示します。 たとえば、HttpMethodOverride HTML ヘルパー メソッドを使用すると、することが表示されるフォームの送信 PUT、または DELETE 要求であります。 HttpMethodOverride の動作では、次の属性に影響します。

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

非表示の input 要素は、X HTTP メソッド オーバーライドの名前とその値をエミュレートするために、HTTP 動詞に設定します。 名前/値のペアとして、HTTP ヘッダー内、またはクエリ文字列の値で上書き値を指定もできます。

オーバーライドは、実際の要求が POST 要求である場合にのみ使用できます。 その他の HTTP 動詞を使用する要求のオーバーライド値が無視されます。

### <a id="_TOC3_13"></a>テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス

エディター テンプレートで、モデルを表示するときに、非表示の input 要素を表示するかどうかを示すためにモデル プロパティには、新しい HiddenInputAttribute 属性を適用できます。 (属性は、HiddenInput の暗黙の UIHint 値を設定します。) 属性の値のプロパティでは、エディターで、値が表示されるかどうかを指定し、モードを表示できます。 何も表示値が false に設定されている場合、HTML マークアップを通常のフィールドを囲むであってもです。 値の既定値は true です。

次のシナリオで HiddenInputAttribute 属性を使用する場合があります。

- ユーザー オブジェクトの ID を編集できるように、ビューとコント ローラーに渡すことができるように、古い ID を格納する非表示の input 要素を提供するためにも値を表示する必要があります。
- ビューにより、ユーザーはタイムスタンプ プロパティなど、表示されることのあるバイナリ プロパティを編集します。 その場合は、値とラベルと値の場合) などの周囲の HTML マークアップは表示されません。

次の例では、HiddenInputAttribute クラスを使用する方法を示します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

属性が設定されている場合に true (またはパラメーターが指定されていない)、次のように。

- 表示テンプレートでラベルを表示し、値は、ユーザーに表示します。
- エディター テンプレートで、ラベルが表示され、非表示の input 要素で、値が表示されます。

属性が false に設定されている場合、以下の処理が行われます。

- そのフィールドに対しては何も行われませんが、画面テンプレートでレンダリングされます。
- エディター テンプレートでラベルは表示されませんし、値は、非表示の input 要素で表示されます。

### <a id="_TOC3_14"></a>Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。

Html.ValidationSummary ヘルパー メソッドではすべての検証エラーを常に表示するには、代わりに、モデル レベル エラーのみを表示する新しいオプションがあります。 これにより、検証概要に表示されるモデル レベル エラーおよび各フィールドの横に表示されるフィールド固有のエラーです。

### <a id="_TOC3_15"></a>Visual Studio の生成コードに固有では T4 テンプレートを .NET Framework のターゲット バージョン

新しいプロパティが使用可能な T4 ファイルを ASP.NET MVC の T4 ホスト アプリケーションによって使用される .NET Framework のバージョンを指定するからです。 これにより、T4 テンプレート コードとは、.NET Framework のバージョンに固有のマークアップを生成できます。 Visual Studio 2008 では、値と、.NET 3.5 では常にします。 Visual Studio 2010 では、値は、.NET 3.5 または .NET 4 のいずれかがします。

## <a id="_TOC4"></a>API の機能強化

このセクションでは、既存の ASP.NET MVC 型とメンバーへの変更について説明します。

- コント ローラー クラスにプロテクト仮想 CreateActionInvoker メソッドを追加します。 このメソッドは、コント ローラーの ActionInvoker プロパティによって呼び出され、呼び出し元が既に設定されていない場合、呼び出し元の限定的なインスタンス化できます。
- AuthorizeAttribute クラスにプロテクト仮想 HandleUnauthorizedRequest メソッドを追加します。 これにより、承認に失敗したときに、動作を制御する AuthorizeAttribute から派生するフィルターが有効です。
- ValueProviderDictionary クラスで、追加 (文字列キー、オブジェクトの値) のメソッドを追加します。 これにより、次の例のように ValueProviderDictionary に辞書初期化子構文を使用します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- 追加の get\_Sys.Mvc.AjaxContext クラスのメソッドのオブジェクトします。 これは JavaScript メソッド get に類似した\_場合、応答のコンテンツ タイプは application/json には、データのメソッドを取得\_JSON オブジェクトを返します。
- AuthorizationContext クラスで ActionDescriptor プロパティを追加します。
- 問題を回避するプロパティがない場合に ID プロパティが含まれるモデルをバインドするときに使用できる UrlParameter.Optional トークンを追加、フォーム ポストにします。 詳細については、エントリを参照してください。 [ASP.NET MVC 2 省略可能な URL パラメーター](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Phil Haack のブログです。

## <a id="_TOC5"></a>重大な変更

次の変更は、既存の ASP.NET MVC 1.0 アプリケーションでエラーを発生させる可能性があります。

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>IDataErrorInfo を実装するクラスのプロパティの検証の動作の変更します。

IDataErrorInfo を使用して検証を実行するモデル オブジェクトの場合は、新しい値が設定されているかどうかに関係なく、すべてのプロパティを検証します。 ASP.NET MVC 1.0 では、設定した新しい値のあるプロパティのみが検証されました。 ASP.NET MVC 2 では、すべてのプロパティの検証コントロールが成功した場合にのみ IDataErrorInfo のエラーのプロパティが呼び出されます。

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS スクリプト マッピングのスクリプトは、インストーラーで使用できなく

IIS スクリプト マッピング スクリプトは、クラシック モードで IIS 6 と IIS 7 のスクリプト マップを構成に使用されるコマンド ライン スクリプトです。 スクリプト マッピング スクリプトには、Visual Studio 開発サーバーを使用する場合、または統合モードで IIS 7 を使用する場合は不要です。 スクリプトは、個別にサポートされていないダウンロードで使用可能な[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)です。

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>MVC フューチャで Html.Substitute ヘルパー メソッドは現在利用できません。

MVC ビュー エンジンのレンダリング動作の変更により、Html.Substitute ヘルパー メソッドは機能しません、削除されました。

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IValueProvider インターフェイスは、IDictionary のすべての使用を置き換えます

MVC 1.0 では IDictionary を今すぐ受け入れすべてプロパティまたはメソッドの引数は、IValueProvider を受け入れます。 この変更では、カスタム値プロバイダーまたはカスタム モデル バインダーを含むアプリケーションのみに影響します。 プロパティと、この変更によって影響を受ける方法の例については、次のとおりです。

- ControllerBase および ModelBindingContext クラスの ValueProvider プロパティです。
- コント ローラー クラスの TryUpdateModel メソッドです。

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Site.css ファイルで追加された新しい CSS クラス

テンプレート化されたヘルパーと、検証機能で使用される新しいスタイルを含めます、ASP.NET MVC プロジェクト テンプレートで Site.css ファイルが更新されました。

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>ヘルパーが MvcHtmlString オブジェクトを返すようになりました

ASP.NET 4 で、新しい HTML エンコーディング式の構文を利用するために HTML ヘルパーの戻り値の型は今すぐ MvcHtmlString ではなく文字列です。 HTML エンコードの構文を活用できませんする ASP.NET 3.5 に ASP.NET MVC 2 と新しいヘルパーを使用する場合新しい構文は、ASP.NET 4 で ASP.NET MVC 2 を実行する場合にのみ使用できます。

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult は、HTTP POST 要求にのみ応答します。

既定では、情報漏えいの可能性のある攻撃をハイジャックしている JSON を軽減するために JsonResult クラスは、今すぐ HTTP POST 要求にのみ応答します。 代わりに POST を使用するには、Ajax GET JsonResult オブジェクトを返すアクション メソッドの呼び出しを変更してください。 必要に応じて、JsonResult の新しい JsonRequestBehavior プロパティを設定して動作をオーバーライドすることができます。 潜在的な脆弱性の詳細については、ブログの投稿を参照してください。 [JSON をハイジャックしている](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack のブログです。

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>モデルおよび ModelBindingContext の ModelType プロパティ set アクセス操作子が今後使用しません

新しい設定可能な ModelMetadata プロパティは ModelBindingContext クラスに追加されました。 新しいプロパティには、モデルと ModelType プロパティの両方がカプセル化します。 モデルと ModelType プロパティは今後使用しませんが、旧バージョンとの互換性のためプロパティ get アクセス操作子も機能です。ModelMetadata プロパティに委任するには値を取得します。

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>DefaultControllerFactory クラスへの変更は、そこから派生するカスタム コント ローラー ファクトリを中断します。

DefaultControllerFactory クラスは、RequestContext プロパティを削除することによって修正されました。 このプロパティは、代わりに、要求コンテキストのインスタンスは、保護された仮想 GetControllerInstance と GetControllerType メソッドに渡されます。 この変更では、DefaultControllerFactory から派生したカスタム コント ローラー ファクトリに影響します。

ASP.NET MVC の依存関係の挿入をアプリケーションに提供するカスタム コント ローラー ファクトリが使用されます。 ASP.NET MVC 2 をサポートするためにカスタム コント ローラー ファクトリを更新するには、メソッドのシグネチャまたは新規のシグネチャと一致する署名を変更し、プロパティではなく、要求コンテキスト パラメーターを使用します。

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>「区分」予約済みのルート値のキーを今すぐには

ルートの値の文字列「領域」ようになりました特別な意味 ASP.NET mvc で"controller"と"action"を実行するのと同じようにします。 1 つは、HTML ヘルパーは、「区分」を含むルート値ディクショナリで提供されている、している場合、ヘルパーはクエリ文字列に「領域」を不要になったに追加するされます。

領域の機能を使用している場合は、ルートの URL の一部として {領域} を使用することを確認してください。


## <a id="_TOC6"></a>免責事項

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

特に明記されない限り、使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、およびイベント実在は、架空のもの、および関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所またはイベントはまたは意図して推論する必要があります。

© 2010 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
