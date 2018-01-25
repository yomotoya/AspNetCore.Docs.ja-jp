---
uid: mvc/overview/advanced/custom-mvc-templates
title: "カスタムの MVC テンプレート |Microsoft ドキュメント"
author: joeloff
description: "VSIX 拡張機能としてテンプレートを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>カスタムの MVC テンプレート
====================
によって[Jacques Eloff](https://github.com/joeloff)

MVC 3 Tools Update の Visual Studio 2010 のリリースでは、MVC プロジェクト用の個別のプロジェクト ウィザードが導入されました。 変更された 2 つの要因によって促進されます。 最初に、MVC 3 と Razor などの他のビュー エンジンのサポートで新しいテンプレートの概要は、Visual Studio で新しいプロジェクト ダイアログを overcrowding になります。 次に、機能拡張ポイントには、顧客を求められていたし、新しい MVC プロジェクト ウィザードに余裕がある us これらの要求に応答する機会をします。

カスタム テンプレートを追加することと、レジストリを使用して、新しいテンプレートに表示されるように、MVC プロジェクト ウィザードに依存した骨の折れる作業がでした。 新しいテンプレートの作成者は、内部で、MSI をインストール時に必要なレジストリ エントリが発生することを確認する必要があります。 代替手段を使用可能なテンプレートを含む ZIP ファイルを作成し、エンドユーザーが必要なレジストリ エントリを手動で作成しました。

によって提供される既存のインフラストラクチャの一部を利用することにしましたので、前述の方法のどちらもは理想的な[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)作成者を容易にできるように拡張機能を配布して MVC 4 以降では、カスタムの MVC テンプレートのインストールVisual Studio 2012。 この方法が提供する利点を次に示します。

- VSIX 拡張機能には、別の言語 (c# および Visual Basic) をサポートする複数のテンプレートと複数のビュー エンジン (ASPX および Razor) を含めることができます。
- VSIX 拡張機能の複数の Sku の Visual Studio Express の Sku を含む対象できます。
- [Visual Studio ギャラリー](https://visualstudiogallery.msdn.microsoft.com/)幅広いユーザー向けに拡張機能を配布するが容易になります。
- VSIX 拡張機能をアップグレードするには、簡単に修正と更新プログラムをカスタム テンプレートを作成します。

## <a name="prerequisites"></a>必須コンポーネント

- ユーザーは、vstemplate ファイルなどの必要なマークアップを含む、プロジェクト テンプレートの作成に慣れておく必要があります。
- ユーザーは、Visual Studio Professional である必要があり、以降がインストールされています。 Express の Sku は、VSIX プロジェクトの作成をサポートしていません。
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)をインストールします。

## <a name="example"></a>例

最初の手順では、c# または Visual Basic を使用して新しい VSIX プロジェクトを作成します。 選択**ファイル > 新しいプロジェクト**、順にクリックして**機能拡張**クリックし、左側のウィンドウで、 **VSIX プロジェクト**です。

![新しいプロジェクト](custom-mvc-templates/_static/image1.jpg)

プロジェクトを作成した後、VSIX デザイナーが開きます。

![プロジェクト デザイナーのメタデータ](custom-mvc-templates/_static/image2.jpg)

デザイナーを使用して、拡張機能をインストールまたは Visual Studio でインストールされた拡張機能を参照したときにユーザーに表示される拡張機能の全般プロパティの一部を編集することができます (**ツール > 拡張機能と更新プログラム**)。 一般情報 をクリックした後、**インストールの対象 タブ**です。

![プロジェクト デザイナーのインストールの対象](custom-mvc-templates/_static/image3.jpg)

このタブを使用して、Sku と拡張機能でサポートされている Visual Studio のバージョンを指定します。 チェック ボックスをオン**この VSIX はすべてのユーザーに対してインストール**VSIX のマシンごとにインストールを有効にします。 をクリックして、**新規**追加 Sku Web Developer Express (VWD) などを追加するには、右にボタンをクリックします。

![新しいインストールの対象を追加します。](custom-mvc-templates/_static/image4.jpg)

ファミリでは、最小の SKU を選択する必要をすべて、Professional および上位の Sku (Professional、Premium および Ultimate) をサポートする場合のみ**Microsoft.VisualStudio.Pro**です。 インストールの対象を完了すると、すべての変更を保存してください。

![プロジェクト デザイナーのインストールの対象](custom-mvc-templates/_static/image5.jpg)

**資産** タブを使用して、すべてのコンテンツ ファイルを VSIX に追加します。 使用する代わりに、VSIX マニフェスト ファイルの生の XML を編集して MVC カスタム メタデータを必要とするため、**資産** タブのコンテンツを追加します。 まず、VSIX プロジェクトにテンプレートの内容を追加します。 フォルダーとその内容の構造が、プロジェクトのレイアウトを反映することが重要です。 次の例には、基本的な MVC プロジェクト テンプレートから生成された 4 つのプロジェクト テンプレートが含まれています。 プロジェクト テンプレート (ProjectTemplates フォルダーの下にあるすべてのプロパティ) を構成するすべてのファイルが追加されたことを確認してください、**コンテンツ**itemgroup、vsix プロジェクト ファイルと各項目が含まれている、 **CopyToOutputDirectory**と**IncludeInVsix**メタデータが次の例に示すように設定します。

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

それ以外の場合は、IDE は、VSIX を作成する際にエラーが表示可能性がありますが、テンプレートの内容をコンパイルしようとしています。 テンプレート内のコード ファイルは多くの場合、特別な含める[テンプレート パラメーター](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)プロジェクト テンプレートがインスタンス化され、IDE でコンパイルすることはできません、Visual Studio で使用します。

![ソリューション エクスプローラー](custom-mvc-templates/_static/image6.jpg)

VSIX デザイナーを終了し、右クリックして、 **source.extension.manifest**ファイルを**ソリューション エクスプ ローラー**選択**ファイルを開く**を選択し、 **XML (テキスト) エディター**オプション。

![[ファイル] ダイアログを開く](custom-mvc-templates/_static/image7.jpg)

作成、 **&lt;資産&gt;**要素を追加し、 **&lt;資産&gt;** VSIX に含める必要がある各ファイルの要素。 **型**の各属性**&lt;資産&gt;**に要素を設定する必要があります**Microsoft.VisualStudio.Mvc.Template**です。 これは、MVC プロジェクトのウィザードのみを認識するカスタムの名前空間です。 マニフェスト ファイルのレイアウトと構造体の詳細については、VSIX 2.0 スキーマ ドキュメントを参照してください。

ファイルを VSIX に追加するだけでは、MVC ウィザードを使用して、テンプレートを登録する満たされません。 MVC ウィザードにテンプレートの名前、説明、サポートされているビュー エンジンおよびプログラミング言語などの情報を提供する必要があります。 関連付けられているカスタム属性でこの情報が送られる、 **&lt;資産&gt;**要素ごと**vstemplate**ファイル。

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

タイトル =&quot;カスタムの基本的な Web アプリケーション&quot;

説明 =&quot;基本的な MVC web アプリケーション (Razor) から派生したカスタム テンプレート&quot;

Version=&quot;4.0&quot;/&gt;

以下には、存在する必要があるカスタム属性の説明を示します。

- **ProjectType** MVC に設定する必要があります。
- **言語**テンプレートでサポートされる開発言語を指定します。 有効な値は、c# または vb です。
- **ViewEngine** Aspx など Razor テンプレートでサポートされているビュー エンジンを指定します。 このフィールドのカスタム値を指定することができます。
- **TemplateId**テンプレートをグループ化するために使用します。 値がなります、既存のテンプレート ID が一致する場合は、MVC ウィザードに既に登録済みのテンプレートをオーバーライドします。
- **タイトル**各プロジェクト テンプレートの下に、MVC ウィザードに表示される短い説明を指定します。
- **説明**テンプレートの詳細な説明を指定します。

マニフェストにすべてのファイルを追加すると、保存、表示になります、**資産**すべてのファイルをデザイナーのタブが表示されますが、カスタムではない属性に追加する、 **&lt;資産&gt;**用の要素、 **vstemplate**ファイル。

![プロジェクト デザイナーの資産](custom-mvc-templates/_static/image8.jpg)

ここでは、VSIX プロジェクトをコンパイルし、インストールします。

コンピューターに Visual Studio のすべてのインスタンスを閉じていることを確認して VSIX 拡張機能をテストするを作成します。 Visual Studio をスタートアップ中に、新しい拡張機能のスキャン、VSIX のインストール中に、IDE が開いている場合は、Visual Studio を再起動する必要があります。 ようにします。 起動する VSIX ファイルをエクスプ ローラーで、ダブルクリックして、 **VSIX インストーラー**をクリックして**インストール**し、Visual Studio を起動します。

![VSIX インストーラー](custom-mvc-templates/_static/image9.jpg)

メニューから、次のように選択します。**ツール > 拡張機能と更新プログラム**拡張機能がインストールされていることを確認します。 VSIX インストーラーには、拡張機能のインストール中にエラーが報告された場合は、VSIX インストーラー ログで詳細を表示できます。 ログが作成された通常の**%temp%**など、拡張機能をインストールしたユーザーのフォルダー **C:\Users\Bob\AppData\Local\Temp**です。

![拡張機能と更新プログラム](custom-mvc-templates/_static/image10.jpg)

ウィンドウを閉じてから、MVC ウィザードでは、新しいテンプレートが表示されているかどうかを MVC 4 プロジェクトを作成することができます。

![新しい ASP.NET MVC 4 プロジェクト](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>制限事項

1. MVC ウィザードは、ローカライズされたカスタム テンプレートをサポートしていません。
2. カスタム テンプレートを見つけることが失敗した場合、ウィザードはすべてのエラーを報告しません。 存在しない場合、必要なカスタム属性のいずれかの場合は、テンプレートだけで、ウィザードから除外されます。
