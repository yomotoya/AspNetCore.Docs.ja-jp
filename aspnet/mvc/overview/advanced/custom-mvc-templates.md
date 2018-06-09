---
uid: mvc/overview/advanced/custom-mvc-templates
title: カスタムの MVC テンプレート |Microsoft ドキュメント
author: joeloff
description: VSIX 拡張機能としてテンプレートを作成します。
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
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034608"
---
<a name="custom-mvc-template"></a><span data-ttu-id="aa41a-103">カスタムの MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="aa41a-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="aa41a-104">によって[Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="aa41a-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="aa41a-105">MVC 3 Tools Update の Visual Studio 2010 のリリースでは、MVC プロジェクト用の個別のプロジェクト ウィザードが導入されました。</span><span class="sxs-lookup"><span data-stu-id="aa41a-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="aa41a-106">変更された 2 つの要因によって促進されます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-106">The change was driven by two factors.</span></span> <span data-ttu-id="aa41a-107">最初に、MVC 3 と Razor などの他のビュー エンジンのサポートで新しいテンプレートの概要は、Visual Studio で新しいプロジェクト ダイアログを overcrowding になります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="aa41a-108">次に、機能拡張ポイントには、顧客を求められていたし、新しい MVC プロジェクト ウィザードに余裕がある us これらの要求に応答する機会をします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="aa41a-109">カスタム テンプレートを追加することと、レジストリを使用して、新しいテンプレートに表示されるように、MVC プロジェクト ウィザードに依存した骨の折れる作業がでした。</span><span class="sxs-lookup"><span data-stu-id="aa41a-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="aa41a-110">新しいテンプレートの作成者は、内部で、MSI をインストール時に必要なレジストリ エントリが発生することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="aa41a-111">代替手段を使用可能なテンプレートを含む ZIP ファイルを作成し、エンドユーザーが必要なレジストリ エントリを手動で作成しました。</span><span class="sxs-lookup"><span data-stu-id="aa41a-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="aa41a-112">によって提供される既存のインフラストラクチャの一部を利用することにしましたので、前述の方法のどちらもは理想的な[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)作成者を容易にできるように拡張機能を配布して MVC 4 以降では、カスタムの MVC テンプレートのインストールVisual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="aa41a-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="aa41a-113">この方法が提供する利点を次に示します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="aa41a-114">VSIX 拡張機能には、別の言語 (c# および Visual Basic) をサポートする複数のテンプレートと複数のビュー エンジン (ASPX および Razor) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="aa41a-115">VSIX 拡張機能の複数の Sku の Visual Studio Express の Sku を含む対象できます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="aa41a-116">[Visual Studio ギャラリー](https://visualstudiogallery.msdn.microsoft.com/)幅広いユーザー向けに拡張機能を配布するが容易になります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="aa41a-117">VSIX 拡張機能をアップグレードするには、簡単に修正と更新プログラムをカスタム テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa41a-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="aa41a-118">Prerequisites</span></span>

- <span data-ttu-id="aa41a-119">ユーザーは、vstemplate ファイルなどの必要なマークアップを含む、プロジェクト テンプレートの作成に慣れておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="aa41a-120">ユーザーは、Visual Studio Professional である必要があり、以降がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="aa41a-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="aa41a-121">Express の Sku は、VSIX プロジェクトの作成をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="aa41a-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="aa41a-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="aa41a-123">例</span><span class="sxs-lookup"><span data-stu-id="aa41a-123">Example</span></span>

<span data-ttu-id="aa41a-124">最初の手順では、c# または Visual Basic を使用して新しい VSIX プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="aa41a-125">選択**ファイル > 新しいプロジェクト**、順にクリックして**機能拡張**クリックし、左側のウィンドウで、 **VSIX プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![新しいプロジェクト](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="aa41a-127">プロジェクトを作成した後、VSIX デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-127">After the project is created, the VSIX designer will be opened.</span></span>

![プロジェクト デザイナーのメタデータ](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="aa41a-129">デザイナーを使用して、拡張機能をインストールまたは Visual Studio でインストールされた拡張機能を参照したときにユーザーに表示される拡張機能の全般プロパティの一部を編集することができます (**ツール > 拡張機能と更新プログラム**)。</span><span class="sxs-lookup"><span data-stu-id="aa41a-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="aa41a-130">一般情報 をクリックした後、**インストールの対象 タブ**です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![プロジェクト デザイナーのインストールの対象](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="aa41a-132">このタブを使用して、Sku と拡張機能でサポートされている Visual Studio のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="aa41a-133">チェック ボックスをオン**この VSIX はすべてのユーザーに対してインストール**VSIX のマシンごとにインストールを有効にします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="aa41a-134">をクリックして、**新規**追加 Sku Web Developer Express (VWD) などを追加するには、右にボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![新しいインストールの対象を追加します。](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="aa41a-136">ファミリでは、最小の SKU を選択する必要をすべて、Professional および上位の Sku (Professional、Premium および Ultimate) をサポートする場合のみ**Microsoft.VisualStudio.Pro**です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="aa41a-137">インストールの対象を完了すると、すべての変更を保存してください。</span><span class="sxs-lookup"><span data-stu-id="aa41a-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![プロジェクト デザイナーのインストールの対象](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="aa41a-139">**資産** タブを使用して、すべてのコンテンツ ファイルを VSIX に追加します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="aa41a-140">使用する代わりに、VSIX マニフェスト ファイルの生の XML を編集して MVC カスタム メタデータを必要とするため、**資産** タブのコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="aa41a-141">まず、VSIX プロジェクトにテンプレートの内容を追加します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="aa41a-142">フォルダーとその内容の構造が、プロジェクトのレイアウトを反映することが重要です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="aa41a-143">次の例には、基本的な MVC プロジェクト テンプレートから生成された 4 つのプロジェクト テンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="aa41a-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="aa41a-144">プロジェクト テンプレート (ProjectTemplates フォルダーの下にあるすべてのプロパティ) を構成するすべてのファイルが追加されたことを確認してください、**コンテンツ**itemgroup、vsix プロジェクト ファイルと各項目が含まれている、 **CopyToOutputDirectory**と**IncludeInVsix**メタデータが次の例に示すように設定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="aa41a-145">&lt;コンテンツを含める =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="aa41a-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="aa41a-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="aa41a-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="aa41a-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="aa41a-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="aa41a-148">&lt;/Content&gt;</span><span class="sxs-lookup"><span data-stu-id="aa41a-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="aa41a-149">それ以外の場合は、IDE は、VSIX を作成する際にエラーが表示可能性がありますが、テンプレートの内容をコンパイルしようとしています。</span><span class="sxs-lookup"><span data-stu-id="aa41a-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="aa41a-150">テンプレート内のコード ファイルは多くの場合、特別な含める[テンプレート パラメーター](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)プロジェクト テンプレートがインスタンス化され、IDE でコンパイルすることはできません、Visual Studio で使用します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![ソリューション エクスプローラー](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="aa41a-152">VSIX デザイナーを終了し、右クリックして、 **source.extension.manifest**ファイルを**ソリューション エクスプ ローラー**選択**ファイルを開く**を選択し、 **XML (テキスト) エディター**オプション。</span><span class="sxs-lookup"><span data-stu-id="aa41a-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![[ファイル] ダイアログを開く](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="aa41a-154">作成、 **&lt;資産&gt;** 要素を追加し、 **&lt;資産&gt;** VSIX に含める必要がある各ファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="aa41a-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="aa41a-155">**型**の各属性**&lt;資産&gt;** に要素を設定する必要があります**Microsoft.VisualStudio.Mvc.Template**です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="aa41a-156">これは、MVC プロジェクトのウィザードのみを認識するカスタムの名前空間です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="aa41a-157">マニフェスト ファイルのレイアウトと構造体の詳細については、VSIX 2.0 スキーマ ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa41a-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="aa41a-158">ファイルを VSIX に追加するだけでは、MVC ウィザードを使用して、テンプレートを登録する満たされません。</span><span class="sxs-lookup"><span data-stu-id="aa41a-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="aa41a-159">MVC ウィザードにテンプレートの名前、説明、サポートされているビュー エンジンおよびプログラミング言語などの情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="aa41a-160">関連付けられているカスタム属性でこの情報が送られる、 **&lt;資産&gt;** 要素ごと**vstemplate**ファイル。</span><span class="sxs-lookup"><span data-stu-id="aa41a-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="aa41a-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="aa41a-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="aa41a-163">d:Source=&quot;File&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="aa41a-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="aa41a-165">ProjectType=&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="aa41a-166">Language =&quot;(C#)&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="aa41a-167">ViewEngine=&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="aa41a-168">TemplateId=&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="aa41a-169">タイトル =&quot;カスタムの基本的な Web アプリケーション&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="aa41a-170">説明 =&quot;基本的な MVC web アプリケーション (Razor) から派生したカスタム テンプレート&quot;</span><span class="sxs-lookup"><span data-stu-id="aa41a-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="aa41a-171">バージョン =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="aa41a-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="aa41a-172">以下には、存在する必要があるカスタム属性の説明を示します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="aa41a-173">**ProjectType** MVC に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa41a-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="aa41a-174">**言語**テンプレートでサポートされる開発言語を指定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="aa41a-175">有効な値は、c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="aa41a-176">**ViewEngine** Aspx など Razor テンプレートでサポートされているビュー エンジンを指定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="aa41a-177">このフィールドのカスタム値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="aa41a-178">**TemplateId**テンプレートをグループ化するために使用します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="aa41a-179">値がなります、既存のテンプレート ID が一致する場合は、MVC ウィザードに既に登録済みのテンプレートをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="aa41a-180">**タイトル**各プロジェクト テンプレートの下に、MVC ウィザードに表示される短い説明を指定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="aa41a-181">**説明**テンプレートの詳細な説明を指定します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="aa41a-182">マニフェストにすべてのファイルを追加すると、保存、表示になります、**資産**すべてのファイルをデザイナーのタブが表示されますが、カスタムではない属性に追加する、 **&lt;資産&gt;** 用の要素、 **vstemplate**ファイル。</span><span class="sxs-lookup"><span data-stu-id="aa41a-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![プロジェクト デザイナーの資産](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="aa41a-184">ここでは、VSIX プロジェクトをコンパイルし、インストールします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="aa41a-185">コンピューターに Visual Studio のすべてのインスタンスを閉じていることを確認して VSIX 拡張機能をテストするを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="aa41a-186">Visual Studio をスタートアップ中に、新しい拡張機能のスキャン、VSIX のインストール中に、IDE が開いている場合は、Visual Studio を再起動する必要があります。 ようにします。</span><span class="sxs-lookup"><span data-stu-id="aa41a-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="aa41a-187">起動する VSIX ファイルをエクスプ ローラーで、ダブルクリックして、 **VSIX インストーラー**をクリックして**インストール**し、Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![VSIX インストーラー](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="aa41a-189">メニューから、次のように選択します。**ツール > 拡張機能と更新プログラム**拡張機能がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="aa41a-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="aa41a-190">VSIX インストーラーには、拡張機能のインストール中にエラーが報告された場合は、VSIX インストーラー ログで詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="aa41a-191">ログが作成された通常の **%temp%** など、拡張機能をインストールしたユーザーのフォルダー **C:\Users\Bob\AppData\Local\Temp**です。</span><span class="sxs-lookup"><span data-stu-id="aa41a-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![拡張機能と更新プログラム](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="aa41a-193">ウィンドウを閉じてから、MVC ウィザードでは、新しいテンプレートが表示されているかどうかを MVC 4 プロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![新しい ASP.NET MVC 4 プロジェクト](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="aa41a-195">制限事項</span><span class="sxs-lookup"><span data-stu-id="aa41a-195">Limitations</span></span>

1. <span data-ttu-id="aa41a-196">MVC ウィザードは、ローカライズされたカスタム テンプレートをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="aa41a-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="aa41a-197">カスタム テンプレートを見つけることが失敗した場合、ウィザードはすべてのエラーを報告しません。</span><span class="sxs-lookup"><span data-stu-id="aa41a-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="aa41a-198">存在しない場合、必要なカスタム属性のいずれかの場合は、テンプレートだけで、ウィザードから除外されます。</span><span class="sxs-lookup"><span data-stu-id="aa41a-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
