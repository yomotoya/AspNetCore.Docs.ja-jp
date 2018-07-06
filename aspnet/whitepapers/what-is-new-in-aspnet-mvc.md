---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: ASP.NET MVC 2 の新機能新機能 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。 このドキュメントもダウンロードできます。
ms.author: aspnetcontent
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 54f62b0f6bbde50f53d062eda422f5f58806cbe1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834165"
---
<a name="whats-new-in-aspnet-mvc-2"></a>ASP.NET MVC 2 の新機能新機能
====================
> このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。 このドキュメントが可能な[ダウンロード](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[概要](#_TOC1)   
[ASP.NET MVC 1.0 プロジェクトを ASP.NET MVC 2 にアップグレードします。](#_TOC2)   
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
[Visual Studio 2010 の新しいコード スニペット](#_TOC3_10)   
[新しい RequireHttpsAttribute アクション フィルター](#_TOC3_11)   
[HTTP メソッドの動詞のオーバーライド](#_TOC3_12)   
[テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス](#_TOC3_13)   
[Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。](#_TOC3_14)   
[T4 テンプレートでは、Visual Studio コードの生成固有では、.NET Framework のターゲット バージョンに](#_TOC3_15)[API の強化](#_TOC4)  
[重大な変更](#_TOC5)  
[免責事項](#_TOC6)  

## <a id="_TOC1"></a>  概要

ASP.NET MVC 2 は、ASP.NET MVC 1.0 をビルドし、多数の拡張機能と生産性の向上に重点を置いた機能を紹介します。 このリリースは、すべての知識、スキル、コードと ASP.NET MVC 1.0 用の拡張機能は引き続き適用に ASP.NET MVC 1.0 と互換性が。

ASP.NET MVC の詳細については、次のリソースを参照してください。

- [MSDN で ASP.NET MVC のドキュメント](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC Web サイト](https://asp.net/mvc/)
- [ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  ASP.NET MVC 1.0 プロジェクトを ASP.NET MVC 2 にアップグレードします。

ASP.NET MVC 2 は、ASP.NET MVC 2 に、ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択するには、アプリケーション開発者の柔軟性、同じサーバーで ASP.NET MVC 1.0 と共にインストールできます。 アップグレードする方法については、ドキュメントを参照してください。 [ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションのアップグレード](https://go.microsoft.com/fwlink/?LinkID=185459)します。

## <a id="_TOC3"></a>  新機能

このセクションが導入された機能について説明します、MVC 2 のリリースでします。

### <a id="_TOC3_1"></a>  テンプレート化されたヘルパー

テンプレート化されたヘルパーは、編集用に自動的に関連付ける HTML 要素を使用して、データ型の表示。 たとえば、System.DateTime 型のデータ ビューが表示されたら、日付の選択の UI 要素を自動的に表示できます。 ASP.NET Dynamic Data でのフィールド テンプレートの動作に似ています。 詳細については、次を参照してください。[データを表示するテンプレート化されたヘルパーを使用して](https://go.microsoft.com/fwlink/?LinkId=159062)MSDN Web サイト。

### <a id="_TOC3_2"></a>  領域

領域を使用して、大規模な Web アプリケーションの複雑さを管理するために複数の小さなセクションに、大規模なプロジェクトを整理できます。 通常、各セクション (「領域」) は、大規模な Web サイトの別のセクションを表し、コント ローラーとビューの関連する設定をグループ化するために使用します。 詳細については、次を参照してください。[チュートリアル: 領域で、ASP.NET MVC アプリケーションの整理](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN Web サイト。

ソリューション エクスプ ローラーで、新しい領域を作成するにプロジェクトを右クリックして、[追加] をクリックしておよび領域をクリックします。 これには、領域名の入力を求めるダイアログ ボックスが表示されます。 領域名を入力すると、Visual Studio によって、プロジェクトに新しい領域が追加されます。

次の図は、プロジェクト管理者とブログの 2 つの領域のレイアウト例を示します。

![](what-is-new-in-aspnet-mvc/_static/image1.png)

領域を作成するときに、Visual Studio は、各領域に対する AreaRegistration から派生したクラスを追加します。 このクラスは、次の例に示すように、領域と、そのルートを登録するために必要です。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 の既定のプロジェクト テンプレートには、Global.asax ファイルのコードで RegisterAllAreas メソッドの呼び出しが含まれています。 このメソッドは、AreaRegistration クラスから派生したすべての型を探して、型のインスタンスをインスタンス化する、インスタンスで RegisterArea メソッドを呼び出して、プロジェクト内の各領域を登録します。 次の例では、これを行う方法を示します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

RegisterArea メソッドでコンテキストを呼び出すことによって、名前空間を指定しないでください場合。Namespaces.Add メソッド、registration クラスの名前空間は、既定で使用されます。

### <a id="_TOC3_3"></a>  非同期コント ローラーのサポート

ASP.NET MVC 2 では、非同期的に要求を処理するコント ローラーができるようになりました。 これについては、サーバーを非ブロッキングの対応する代わりに (ネットワークの要求) などのブロック操作を頻繁に呼び出すようにすることでパフォーマンスの向上につながります。 詳細については、次を参照してください。、 [ASP.NET MVC で非同期コント ローラーを使用して](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)msdn トピック。

### <a id="_TOC3_4"></a>  DefaultValueAttribute アクション メソッド パラメーターのサポート

System.ComponentModel.DefaultValueAttribute クラスは、アクション メソッドに引数のパラメーターに指定する既定値を使用できます。 たとえば、次の既定のルートが定義されているとします。

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

また、次のコント ローラーとアクション メソッドが定義されていると仮定します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

次の要求の Url はメソッドを呼び出すビュー アクション前の例で定義されています。

- /記事/ビュー/123
- /記事/ビュー/123 ですか? ページ (実質的に同じ、前回の要求) 1 を =
- /記事/ビュー/123 でしょうかページ = 2。

DefaultValueAttribute 属性がない場合、上記の一覧から最初の URL はために機能しません、ページの引数が null 非許容値型の値が指定されていません。

Visual Basic 2010 または Visual c# 2010 でコードが書き込まれた場合は、次の例に示すように、DefaultValueAttribute 属性ではなく省略可能なパラメーターを使用できます。

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  モデル バインダーのバイナリ データをバインドするためのサポート

2 つの新しいオーバー ロード Html.Hidden ヘルパー base 64 エンコード文字列としてのバイナリ値をエンコードするにがあります。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

一般的な用途は、ビューでオブジェクトのタイムスタンプを埋め込むには。 たとえば、アプリケーションには、次の Product オブジェクトが含まれます。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

編集フォームには、次の例に示すようにフォームでタイムスタンプ プロパティを表示できます。

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

このマークアップは、次の例のような base 64 エンコードされた文字列としてのタイムスタンプ値で非表示の input 要素を表示します。

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

このフォームは、次の例に示すように、製品、型の引数のあるアクション メソッドにポスト可能性があります。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

アクション メソッドでタイムスタンプ プロパティが正しく入力されて、ポストされた base 64 エンコード文字列がバイト配列に変換されるためです。

### <a id="_TOC3_6"></a>  ModelMetadata と ModelMetadataProvider クラス

ModelMetadataProvider クラスは、ビュー内のモデルのメタデータを取得するための抽象型を提供します。 MVC 2 には、System.ComponentModel.DataAnnotations 名前空間の属性で公開されているメタデータを利用できるようにする既定のプロバイダーが含まれています。 データベースまたは XML ファイルなどの他のデータ ストアからのメタデータを指定するメタデータ プロバイダーを作成することになります。

ViewDataDictionary ModelMetadataProvider クラスによって、モデルから抽出されるメタデータを含む ModelMetadata オブジェクトが公開されます。 これにより、このメタデータを使用し、その出力を適宜調整するテンプレート化されたヘルパーができます。

詳細については、ドキュメントを参照して、 [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)と[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)クラス。

### <a id="_TOC3_7"></a>  DataAnnotations 属性のサポート

ASP.NET MVC 2 のサポート (System.ComponentModel.DataAnnotations 名前空間で定義されている) RangeAttribute、RequiredAttribute、StringLengthAttribute、および RegexAttribute 検証属性を使用して入力を提供するために、モデルへのバインド検証します。

詳細については、次を参照してください。[方法: 検証モデル データ DataAnnotations 属性を使用した](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN Web サイト。 これらの属性の使用を示すサンプル プロジェクトがダウンロードできる[ https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)します。

### <a id="_TOC3_8"></a>  モデルの検証コントロール プロバイダー

モデル検証プロバイダー クラスは、モデルの検証ロジックを提供する抽象化を表します。 ASP.NET MVC には、System.ComponentModel.DataAnnotations 名前空間に含まれている検証属性に基づく既定のプロバイダーが含まれています。 モデルにカスタム検証規則および検証規則のカスタム マッピングを定義する、独自の検証プロバイダーを作成することもできます。 詳細については、ドキュメントを参照して、 [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)クラス。

### <a id="_TOC3_9"></a>  クライアント側の検証

モデル検証コントロール プロバイダー クラスは、クライアント側の検証ライブラリで使用できる JSON でシリアル化されたデータの形式でブラウザーに検証メタデータを公開します。 ASP.NET MVC 2 には、クライアント検証ライブラリおよび前に説明した DataAnnotations 名前空間の検証属性をサポートするアダプターが含まれます。 プロバイダー クラスを使用して、JSON データと代替のライブラリへの呼び出しを処理するアダプターを記述することでその他のクライアント検証ライブラリを使用することもできます。

### <a id="_TOC3_10"></a>  Visual Studio 2010 の新しいコード スニペット

ASP.NET MVC 2 の HTML コード スニペットのセットは、Visual Studio 2010 と共にインストールされます。 [ツール] メニューで、これらのスニペットの一覧を表示するには、コード スニペット マネージャーを選択します。 言語、HTML を選択し、場所に、ASP.NET MVC 2 を選択します。 コード スニペットを使用する方法の詳細については、Visual Studio のドキュメントを参照してください。

### <a id="_TOC3_11"></a>  新しい RequireHttpsAttribute アクション フィルター

ASP.NET MVC 2 には、アクション メソッドとコント ローラーに適用できる新しい RequireHttpsAttribute クラスが含まれています。 既定では、フィルターは、SSL 対応 (HTTPS) に相当する、SSL 以外の HTTP 要求をリダイレクトします。

### <a id="_TOC3_12"></a>  HTTP メソッドの動詞のオーバーライド

REST のアーキテクチャ スタイルを使用して Web サイトをビルドするときは、HTTP 動詞を使用してリソースに対して実行するアクションを決定します。 残りの部分では、アプリケーションのサポートなど、一般的な HTTP 動詞の完全な範囲の GET、PUT、POST、および削除する必要があります。

ASP.NET MVC 2 には、アクション メソッドとその機能のコンパクトな構文に適用できる新しい属性が含まれています。 これらの属性は、HTTP 動詞に基づいて、アクション メソッドを選択する ASP.NET MVC を有効にします。 次の例では、POST 要求が最初のアクション メソッドを呼び出すし、PUT 要求は、2 つ目のアクション メソッドを呼び出します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

ASP.NET MVC の以前のバージョンでこれらのアクション メソッドでは、次の例に示すようより詳細な構文が必要です。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

ブラウザーが GET と HTTP の投稿の動詞のみをサポートするため、さまざまな動詞が必要なアクションに投稿することはできません。 したがって RESTful のすべての要求をネイティブにサポートすることはできません。

ただし、ASP.NET MVC 2、POST 操作中に要求は、RESTful をサポートするために、新しい HttpMethodOverride HTML ヘルパー メソッドを紹介します。 このメソッドは、非表示の input 要素は、任意の HTTP メソッドを効果的にエミュレートするためにフォームを表示します。 たとえば、HttpMethodOverride HTML ヘルパー メソッドを使用することが表示されるフォームの送信 PUT または DELETE 要求である場合。 HttpMethodOverride の動作では、次の属性に影響します。

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

非表示の input 要素は、X HTTP メソッド オーバーライドの名前とその値をエミュレートする HTTP 動詞に設定します。 名前/値ペアとして、HTTP ヘッダーまたはクエリ文字列の値は上書き値を指定もできます。

オーバーライドは、実際の要求が POST 要求である場合にのみ使用できます。 その他の HTTP 動詞を使用する要求は上書き値は無視されます。

### <a id="_TOC3_13"></a>  テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス

エディター テンプレートで、モデルを表示するときに、非表示の input 要素を表示する必要があるかどうかを指定するモデルのプロパティには、新しい HiddenInputAttribute 属性を適用できます。 (属性は HiddenInput の暗黙の UIHint 値を設定します。) 属性の値のプロパティでは、値がエディターに表示されるかどうかを指定し、モードを表示することができます。 何も表示される値が false に設定されている場合、HTML マークアップを通常のフィールドを囲む場合でも同様です。 値の既定値は true です。

次のシナリオで HiddenInputAttribute 属性を使用する場合があります。

- ビューにより、ユーザー オブジェクトの ID を編集して、コント ローラーに渡すことができるように、古い ID を含む非表示の input 要素を提供するためにも値を表示する必要があります。
- により、ビューとユーザーは、タイムスタンプ プロパティなど、表示することはありませんバイナリ プロパティを編集します。 その場合は、値とラベルと値の場合) などの周囲の HTML マークアップは表示されません。

次の例では、HiddenInputAttribute クラスを使用する方法を示します。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

属性が設定されている場合に true (またはパラメーターが指定されていない)、次のように。

- 表示のテンプレートでは、ラベルを表示し、値は、ユーザーに表示します。
- エディターのテンプレートでは、ラベルが表示され、値は、非表示の input 要素で表示されます。

属性が false に設定されている場合、以下の処理が行われます。

- 表示のテンプレートで何もはそのフィールドに表示されません。
- エディターのテンプレートでラベルは表示されませんし、値は、非表示の input 要素で表示されます。

### <a id="_TOC3_14"></a>  Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。

すべての検証エラーを常に表示するには、代わりには、モデル レベル エラーのみを表示する新しいオプションが Html.ValidationSummary ヘルパー メソッドにあります。 これにより、検証の概要に表示されるモデル レベルのエラーおよび各フィールドの横に表示されるフィールド固有のエラー。

### <a id="_TOC3_15"></a>  Visual Studio の生成コードに固有では T4 テンプレートを .NET Framework のターゲット バージョン

新しいプロパティを利用できる T4 ファイルから ASP.NET MVC の T4 ホスト アプリケーションによって使用される .NET Framework のバージョンを指定します。 これにより、コードと .NET Framework のバージョンに固有のマークアップを生成する T4 テンプレート。 Visual Studio 2008 では、値と、.NET 3.5 では常にします。 Visual Studio 2010 では、値は、.NET 3.5 または .NET 4 のいずれかが。

## <a id="_TOC4"></a>  API の強化

このセクションでは、既存の ASP.NET MVC の型とメンバーへの変更について説明します。

- コント ローラー クラスで保護された仮想 CreateActionInvoker メソッドを追加します。 このメソッドは、コント ローラーの ActionInvoker プロパティによってが呼び出され、呼び出し元が既に設定されていない場合、呼び出し元の遅延インスタンス化できます。
- AuthorizeAttribute クラスで保護された仮想 HandleUnauthorizedRequest メソッドを追加します。 これにより、承認に失敗したときの動作を制御する AuthorizeAttribute から派生するフィルター。
- ValueProviderDictionary クラスで、追加 (文字列キー、オブジェクトの値) のメソッドを追加します。 次の例のように、ValueProviderDictionary 辞書初期化子の構文を使用できます。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Get 追加\_Sys.Mvc.AjaxContext クラスのメソッドのオブジェクトします。 これは、get のような JavaScript メソッド\_場合、応答のコンテンツ タイプは application/json には、データ メソッドの取得\_オブジェクトが JSON オブジェクトを返します。
- AuthorizationContext クラスで、ActionDescriptor プロパティを追加します。
- プロパティが存在しない場合は、ID プロパティを含むモデルをバインドする際の問題を回避するために使用できる UrlParameter.Optional トークンを追加するフォームの post。 詳細については、エントリを参照してください。 [ASP.NET MVC 2 省略可能な URL パラメーター](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Phil Haack のブログ。

## <a id="_TOC5"></a>  重大な変更

次の変更では、既存の ASP.NET MVC 1.0 アプリケーション エラーが発生する可能性があります。

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>IDataErrorInfo を実装するクラスのプロパティの検証動作の変更します。

モデル オブジェクトの IDataErrorInfo を使用して検証を実行する場合は、新しい値が設定されているかどうかに関係なく、すべてのプロパティを検証します。 ASP.NET MVC 1.0 では、新しい値を設定したプロパティのみが検証されました。 ASP.NET MVC 2 では、すべてのプロパティの検証コントロールが成功した場合にのみ IDataErrorInfo のエラーのプロパティが呼び出されます。

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS スクリプト マップのスクリプトは、インストーラーで利用できなく

IIS スクリプト マップのスクリプトは、クラシック モードでスクリプト マップの IIS 6 と IIS 7 の構成に使用されるコマンド ライン スクリプトです。 Visual Studio 開発サーバーを使用する場合、または統合モードで IIS 7 を使用する場合は、スクリプト マップのスクリプトは必要ありません。 スクリプトでサポートされていない個別のダウンロードとして利用可能な[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)します。

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>MVC Futures で Html.Substitute ヘルパー メソッドは現在利用できません。

MVC ビュー エンジンのレンダリング動作の変更により、Html.Substitute ヘルパー メソッドは機能しません、削除されました。

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IValueProvider インターフェイス置き換えます IDictionary のすべての使用

今すぐ IDictionary MVC 1.0 で使用できるすべてのプロパティまたはメソッドの引数は、IValueProvider を受け取ります。 この変更では、カスタム値プロバイダーまたはカスタム モデル バインダーを含むアプリケーションのみに影響します。 プロパティと、この変更によって影響を受けたメソッドの例については、次のとおりです。

- ControllerBase と ModelBindingContext クラスの ValueProvider プロパティ。
- コント ローラー クラスの tryupdatemodel に渡しますメソッド。

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Site.css ファイルで追加された新しい CSS クラス

テンプレート化されたヘルパーと検証機能で使用される新しいスタイルに ASP.NET MVC プロジェクト テンプレートでは Site.css ファイルが更新されました。

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>ヘルパーが MvcHtmlString オブジェクトを返すようになりました

ASP.NET 4 の新しい HTML エンコード式の構文を利用するには HTML ヘルパーの戻り値の型が、MvcHtmlString、文字列の代わりにします。 HTML エンコードの構文を利用できません ASP.NET 3.5 に ASP.NET MVC 2 と新しいヘルパーを使用する場合新しい構文は、ASP.NET 4 で ASP.NET MVC 2 を実行する場合にのみ使用できます。

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult は、HTTP POST 要求にのみ応答します。

JSON ハイジャック既定では、情報漏えいの可能性のある攻撃を軽減するために JsonResult クラスは、ここで HTTP POST 要求にのみ応答します。 代わりに POST を使用するには、Ajax を取得する JsonResult オブジェクトを返すアクション メソッドの呼び出しを変更しなければなりません。 必要に応じて、JsonResult の新しい JsonRequestBehavior プロパティを設定してこの動作をオーバーライドできます。 潜在的な脆弱性の詳細については、ブログの投稿を参照してください。 [JSON ハイジャック](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack のブログ。

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>モデルと ModelBindingContext の ModelType プロパティ set アクセス操作子が廃止されています

新しい設定可能な ModelMetadata プロパティが ModelBindingContext クラスに追加されました。 新しいプロパティには、モデルと ModelType プロパティの両方がカプセル化します。 モデルと ModelType プロパティは廃止されていますが、旧バージョンとの互換性のためプロパティ get アクセス操作子も動作します。ModelMetadata プロパティに値を取得するデリゲートします。

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>DefaultControllerFactory クラスへの変更は、そこから派生するカスタム コント ローラー ファクトリを中断します。

DefaultControllerFactory クラスは、RequestContext プロパティを削除することで修正されました。 このプロパティは、代わりに要求コンテキストのインスタンスは、保護された仮想 GetControllerInstance と GetControllerType メソッドに渡されます。 この変更では、DefaultControllerFactory から派生するカスタム コント ローラー ファクトリに影響します。

カスタム コント ローラー ファクトリは、アプリケーションを提供するには、ASP.NET MVC の依存関係の挿入によく使用されます。 ASP.NET MVC 2 をサポートするためにカスタム コント ローラー ファクトリを更新するには、メソッドのシグネチャまたは新規のシグネチャと一致するシグネチャを変更し、プロパティの代わりに、要求のコンテキスト パラメーターを使用します。

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>「領域」予約済みのルート値のキーをここでは、

特別な意味は、ASP.NET mvc で"controller"と「アクション」を実行するのと同じ方法で、ルート値の「領域」という文字列によってようになりましたが。 1 つの意味では、HTML ヘルパーは、「領域」を含むルート値ディクショナリで提供されている場合、ヘルパーは、クエリ文字列で「領域」を不要になったに追加するがします。

区分機能を使用している場合は、ルート URL の一部として {領域} を使用しないことを確認してください。


## <a id="_TOC6"></a>  免責事項

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。

© 2010 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
