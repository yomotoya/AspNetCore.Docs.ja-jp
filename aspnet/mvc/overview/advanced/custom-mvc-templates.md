---
uid: mvc/overview/advanced/custom-mvc-templates
title: カスタム MVC テンプレート |Microsoft Docs
author: joeloff
description: VSIX 拡張機能としてテンプレートを作成します。
ms.author: aspnetcontent
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 67cb01e0fae036f01b1851695ae5a4358136d28e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809778"
---
<a name="custom-mvc-template"></a>カスタム MVC テンプレート
====================
によって[Jacques Eloff](https://github.com/joeloff)

MVC 3 Tools Update の Visual Studio 2010 のリリースでは、MVC プロジェクトの別のプロジェクト ウィザードが導入されました。 変更が、2 つの要因によって決まります。 最初に、MVC 3 と剃刀などの追加のビュー エンジンのサポートで新しいテンプレートの概要は、Visual Studio で新しいプロジェクト ダイアログを overcrowding 可能性があります。 第 2 に、お客様が要望があった機能拡張ポイントと、新しい MVC プロジェクト ウィザードは余裕がある米国これらの要求に応答する機会をします。

カスタム テンプレートを追加すると、レジストリを使用して、新しいテンプレートを MVC プロジェクト ウィザードに表示されるようにすることに依存する困難な作業がでした。 新しいテンプレートの作成者は、内部で、MSI インストール時に必要なレジストリ エントリが作成することを確認する必要があります。 代わりを使用可能なテンプレートを含む ZIP ファイルを作成し、エンドユーザーが必要なレジストリ エントリを手動で作成されました。

によって提供される既存のインフラストラクチャの一部を活用することにしましたので、前述の方法のどちらもは理想的な[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)の作成者を簡単にする拡張機能の配布し、MVC 4 以降では、カスタムの MVC テンプレートをインストールします。Visual Studio 2012。 このアプローチによって提供される利点を次に示します。

- VSIX 拡張機能は、さまざまな言語 (c# および Visual Basic) をサポートする複数のテンプレートと複数のビュー エンジン (ASPX と剃刀) に含めることができます。
- 複数の Sku の Visual Studio Express Sku を含む VSIX 拡張機能を対象にできます。
- [Visual Studio ギャラリー](https://visualstudiogallery.msdn.microsoft.com/)多様な対象ユーザーに拡張機能の配布を容易にします。
- VSIX 拡張機能をアップグレードするには、容易に修正し、カスタム テンプレートの更新を作成します。

## <a name="prerequisites"></a>必須コンポーネント

- ユーザーは、vstemplate ファイルなどの必要なマークアップを含め、プロジェクト テンプレートの作成に慣れておく必要があります。
- ユーザーは、Visual Studio Professional が必要し、以上をインストールします。 Express の Sku は、VSIX プロジェクトの作成をサポートしていません。
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)をインストールします。

## <a name="example"></a>例

最初の手順では、c# または Visual Basic を使用して、新しい VSIX プロジェクトを作成します。 選択**ファイル > 新しいプロジェクト**、順にクリックします**拡張**クリックし、左側のウィンドウで、 **VSIX プロジェクト**。

![新しいプロジェクト](custom-mvc-templates/_static/image1.jpg)

プロジェクトの作成後に、VSIX デザイナーが開きます。

![プロジェクト デザイナーのメタデータ](custom-mvc-templates/_static/image2.jpg)

デザイナーを使用して、拡張機能をインストールまたは Visual Studio でインストールされている拡張機能の参照したときに、ユーザーに表示する拡張機能の全般的なプロパティの一部を編集することができます (**ツール > 拡張機能と更新**)。 一般的な情報 をクリックした後、**インストール ターゲット タブ**します。

![プロジェクト デザイナーのインストールのターゲット](custom-mvc-templates/_static/image3.jpg)

このタブを使用して、Sku と、拡張機能によってサポートされている Visual Studio のバージョンを指定します。 チェック ボックスをオン**すべてのユーザーに対してこの VSIX がインストールされている**VSIX のマシンごとにインストールを有効にします。 をクリックして、**新規**Web Developer Express (VWD) などの他の Sku を追加するには、右にボタンをクリックします。

![新しいインストール先を追加します。](custom-mvc-templates/_static/image4.jpg)

のみファミリでは、最小の SKU を選択する必要がある場合は、すべての Professional 以上の Sku (Professional、Premium および Ultimate) をサポートするために**です**します。 インストールの対象を完了すると、すべての変更を保存してください。

![プロジェクト デザイナーのインストールのターゲット](custom-mvc-templates/_static/image5.jpg)

**資産** タブは、すべてのコンテンツ ファイルを VSIX に追加するために使用します。 使用する代わりに、VSIX のマニフェスト ファイルの生の XML を編集して MVC には、カスタムのメタデータが必要なため、**資産**コンテンツを追加するには、タブ。 VSIX プロジェクト テンプレートの内容を追加することで開始します。 フォルダーとその内容の構造が、プロジェクトのレイアウトをミラーリングすることが重要です。 次の例には、基本的な MVC プロジェクト テンプレートから取得した 4 つのプロジェクト テンプレートが含まれています。 プロジェクト テンプレート (ProjectTemplates フォルダーの下にあるすべてのプロパティ) を構成するすべてのファイルが追加されたことを確認、**コンテンツ**itemgroup vsix プロジェクトのファイルおよび各項目が含まれている、 **CopyToOutputDirectory**と**IncludeInVsix**メタデータが次の例に示すように設定します。

&lt;コンテンツを含める =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

有効でない場合は、IDE は、VSIX をビルドすると、エラー表示される可能性が、テンプレートの内容をコンパイルしようとしています。 テンプレート内のコード ファイルは多くの場合、特別な含む[テンプレート パラメーター](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)プロジェクト テンプレートがインスタンス化され、IDE でコンパイルすることはできません、Visual Studio で使用します。

![ソリューション エクスプローラー](custom-mvc-templates/_static/image6.jpg)

VSIX デザイナーを閉じてから、右クリック、 **source.extension.manifest**ファイル**ソリューション エクスプ ローラー**選択**プログラムから開く**を選択し、 **XML (テキスト) エディター**オプション。

![ダイアログを開く](custom-mvc-templates/_static/image7.jpg)

作成、 **&lt;資産&gt;** 要素を追加し、 **&lt;資産&gt;** VSIX に含める必要がある各ファイルの要素。 **型**の各属性**&lt;資産&gt;** に要素を設定する必要があります**Microsoft.VisualStudio.Mvc.Template**します。 これは、MVC プロジェクトのウィザードのみを認識するカスタムの名前空間です。 構造とマニフェスト ファイルのレイアウトの詳細については、VSIX 2.0 スキーマ ドキュメントを参照してください。

ファイルを VSIX に追加するだけは MVC のウィザードを使用して、テンプレートを登録するだけで十分ではありません。 MVC のウィザードをテンプレートの名前、説明、サポートされているビュー エンジンとプログラミング言語などの情報を提供する必要があります。 関連付けられているカスタム属性にこの情報が含まれる、 **&lt;資産&gt;** 要素ごと**vstemplate**ファイル。

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

言語 =&quot;(C#)&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

タイトル =&quot;カスタムの基本的な Web アプリケーション&quot;

説明 =&quot;基本的な MVC web アプリケーション (Razor) から派生したカスタム テンプレート&quot;

バージョン =&quot;4.0&quot;/&gt;

次に必要なカスタム属性の説明に示します。

- **ProjectType** MVC に設定する必要があります。
- **言語**テンプレートでサポートされている開発言語を指定します。 有効な値は、c# または VB. です。
- **ViewEngine** Aspx など Razor テンプレートでサポートされているビュー エンジンを指定します。 このフィールドのカスタム値を指定することができます。
- **TemplateId**テンプレートをグループ化するために使用します。 値は、既存のテンプレート ID が一致する場合は、MVC のウィザードを使用して以前に登録されているテンプレートをオーバーライドします。
- **タイトル**各プロジェクト テンプレートの下に MVC ウィザードに表示される簡単な説明を指定します。
- **説明**テンプレートのより詳細な説明を指定します。

マニフェストにすべてのファイルを追加すると、保存、表示になります、**資産**すべてのファイルをデザイナーのタブが表示されますが、カスタムではない属性に追加した、 **&lt;資産&gt;** の要素、 **vstemplate**ファイル。

![プロジェクト デザイナーの資産](custom-mvc-templates/_static/image8.jpg)

ここでは、VSIX プロジェクトをコンパイルしてインストールします。

コンピューターの Visual Studio のすべてのインスタンスが閉じられていることを確認して VSIX 拡張機能をテストするを作成します。 Visual Studio は起動時に、新しい拡張機能のスキャンため、VSIX のインストール中に IDE が開いている場合は、Visual Studio を再起動する必要があります。 します。 エクスプ ローラーで、起動する VSIX ファイルをダブルクリックします。、 **VSIX インストーラー**、 をクリック**インストール**し、Visual Studio を起動します。

![VSIX インストーラー](custom-mvc-templates/_static/image9.jpg)

メニューから、次のように選択します。**ツール > 拡張機能と更新**を、拡張機能がインストールされていることを確認します。 VSIX インストーラーには、拡張機能のインストール中にエラーが報告された場合は、VSIX インストーラーのログで詳細を表示できます。 ログを作成して、通常、 **%temp%** など、拡張機能をインストールしたユーザーのフォルダー **C:\Users\Bob\AppData\Local\Temp**します。

![拡張機能と更新プログラム](custom-mvc-templates/_static/image10.jpg)

ウィンドウを閉じた後に、MVC ウィザードで、新しいテンプレートが表示されているかどうかを MVC 4 プロジェクトを作成することができます。

![新しい ASP.NET MVC 4 プロジェクト](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>制限事項

1. MVC のウィザードは、ローカライズされたカスタム テンプレートをサポートしていません。
2. カスタム テンプレートを見つけることが失敗した場合、ウィザードはすべてのエラーを報告しません。 存在しない場合、必要なカスタム属性のいずれかの場合は、テンプレート、単に、ウィザードから除外されます。
