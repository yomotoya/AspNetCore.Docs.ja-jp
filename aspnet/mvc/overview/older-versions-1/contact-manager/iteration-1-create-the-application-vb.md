---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: '繰り返し #1 – アプリケーションを作成する (VB) |Microsoft Docs'
author: microsoft
description: '最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および D.'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: aee177164293c178fa7d2d4acfb60f85dc98bb05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803007"
---
<a name="iteration-1--create-the-application-vb"></a>繰り返し #1 – アプリケーションを作成する (VB)
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> 最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>ASP.NET MVC 連絡先管理アプリケーション (VB) の構築

このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。 Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。

複数のイテレーションにおける、アプリケーションを構築します。 反復処理ごとに、アプリケーション徐々 に向上します。 この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。

- 繰り返し #1 - は、アプリケーションを作成します。 最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- 繰り返し #2 - は、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- 繰り返し #3 - フォーム検証を追加します。 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 また、電子メール アドレスと電話番号を検証しました。

- 繰り返し #4 - は、アプリケーションを疎結合を作成します。 この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。

- 繰り返し #5 - 単体テストを作成します。 5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。 データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。

- 繰り返し #6 - は、テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- 繰り返し #7 - Ajax 機能を追加します。 7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。

## <a name="this-iteration"></a>このイテレーション

この最初のイテレーションでは、基本的なアプリケーションを構築します。 目標は、最も高速かつ最も簡単な方法で、連絡先マネージャーを作成します。 後期のイテレーションでは、アプリケーションのデザインを改善します。

Contact Manager アプリケーションとは、基本的なデータベース駆動型アプリケーションです。 アプリケーションを使用して、新しい連絡先を作成、既存の連絡先を編集および連絡先を削除することができます。

このイテレーションでは、次の手順を完了しました。

1. ASP.NET MVC アプリケーション
2. 当社の連絡先を格納するデータベースを作成します。
3. Microsoft Entity Framework で、データベースのモデル クラスを生成します。
4. コント ローラー アクションとのすべてのデータベースに連絡先の一覧を表示できるビューを作成します。
5. コント ローラー アクションと、データベースに新しい連絡先を作成することができるビューを作成します。
6. コント ローラー アクションと、データベース内の既存の連絡先の編集可能なビューを作成します。
7. コント ローラー アクションと、データベース内の既存の連絡先を削除することができるビューを作成します。

## <a name="software-prerequisites"></a>ソフトウェアの前提条件

ASP.NET MVC アプリケーションでは、Visual Studio 2008 または Visual Web Developer 2008 が (Visual Web Developer は、すべての Visual Studio の高度な機能を含まない Visual Studio の無料版が)、コンピューターにインストールされているいずれかが必要です。 Visual Studio 2008 の評価版または Visual Web Developer のいずれかは、次のアドレスからダウンロードできます。

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Visual Web Developer と ASP.NET MVC アプリケーションの場合は、Visual Web Developer Service Pack 1 インストールが必要です。 Service Pack 1 では、Web アプリケーション プロジェクトを作成することはできません。


ASP.NET MVC フレームワーク。 ASP.NET MVC フレームワークは、次のアドレスからダウンロードできます。

[https://www.asp.net/mvc](../../../index.md)

このチュートリアルでは、データベースにアクセスするのに Microsoft Entity Framework を使用します。 Entity Framework は .NET Framework 3.5 Service Pack 1 に含まれています。 このサービス パックは、次の場所からダウンロードできます。

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&ampdisplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

これらのダウンロードを 1 つずつのそれぞれを実行する代わりに、Web Platform Installer (Web PI) の利点を実行できます。 Web PI は、次のアドレスからダウンロードできます。

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC プロジェクト

ASP.NET MVC Web アプリケーション プロジェクト。 Visual Studio を起動し、メニュー オプションを選択**ファイル]、[新しいプロジェクト**します。 **新しいプロジェクト**ダイアログでは、(図 1 参照) が表示されます。 選択、 **Web**プロジェクトの種類と**ASP.NET MVC Web アプリケーション**テンプレート。 新しいプロジェクトに名前を*ContactManager* [ok] ボタンをクリックします。


上部にあるドロップダウン リストから選択されている .NET Framework 3.5 があることを確認の右、**新しいプロジェクト**ダイアログ。 それ以外の場合、t が勝利した ASP.NET MVC Web アプリケーション テンプレートが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image2.png))。


ASP.NET MVC アプリケーション、**単体テスト プロジェクトの作成**ダイアログが表示されます。 このダイアログ ボックスを使用すると、作成し、ASP.NET MVC アプリケーションを作成するときに、単体テスト プロジェクトをソリューションに追加することを指定します。 獲得した t はこのイテレーションの単体テストを構築するには、オプションを選択する必要があります**はい、単体テスト プロジェクトを作成**後のイテレーションで単体テストを追加する予定なので。 最初に新しい ASP.NET MVC プロジェクトを作成するときは、テスト プロジェクトを追加するは、ASP.NET MVC プロジェクトが作成された後にテスト プロジェクトを追加するよりもはるかに簡単です。

> [!NOTE] 
> 
> Visual Web Developer では、テスト プロジェクトはサポートされていません、Visual Web Developer を使用する場合、単体テスト プロジェクトの作成ダイアログを取得しないできません。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image4.png))。


ASP.NET MVC アプリケーションは、Visual Studio ソリューション エクスプ ローラー ウィンドウに表示されます (図 3 を参照してください)。 表示されないが、ソリューション エクスプ ローラー ウィンドウを表示 メニュー オプションを選択して、このウィンドウを開くことができますし、場合**ビュー、ソリューション エクスプ ローラー**します。 2 つのプロジェクトがソリューションに含まれることに注意してください。 ASP.NET MVC プロジェクトとテストのプロジェクト。 ContactManager は ASP.NET MVC プロジェクトの名前し、ContactManager.Tests はテスト プロジェクトの名前します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**図 03**: ソリューション エクスプ ローラー ウィンドウ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image6.png))。


## <a name="deleting-the-project-sample-files"></a>プロジェクトのサンプル ファイルを削除します。

ASP.NET MVC プロジェクト テンプレートには、コント ローラーとビューのサンプル ファイルが含まれています。 新しい ASP.NET MVC アプリケーションを作成する前に、これらのファイルを削除する必要があります。 ソリューション エクスプ ローラー ウィンドウでファイルとフォルダーを削除するには、ファイルまたはフォルダーを右クリックし、メニュー オプションを選択して**削除**します。

ASP.NET MVC プロジェクトから、次のファイルを削除する必要があります。

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

また、テスト プロジェクトから次のファイルを削除する必要があります。

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>データベースの作成

Contact Manager アプリケーションとは、データベース駆動型 web アプリケーションです。 連絡先情報を格納するのにデータベースを使用します。

Microsoft SQL Server、Oracle、MySQL、および IBM DB2 データベースを含む、最新のデータベースでの ASP.NET MVC フレームワーク。 このチュートリアルでは、Microsoft SQL Server データベースを使用します。 Visual Studio をインストールするときに、Microsoft SQL Server データベースの無料版である Microsoft SQL Server Express をインストールするオプションが提供されます。

アプリを右クリックして新しいデータベースを作成\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、**データ**カテゴリと**SQL Server データベース**テンプレート (図 4 参照)。 新しいデータベース ContactManagerDB.mdf 名し、[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**図 04**: 新しい Microsoft SQL Server Express データベースを作成する ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image8.png))。


新しいデータベースを作成した後、データベースがアプリに表示\_ソリューション エクスプ ローラー ウィンドウで、データ フォルダー。 サーバー エクスプ ローラー ウィンドウを開き、データベースに接続する ContactManager.mdf ファイルをダブルクリックします。

> [!NOTE] 
> 
> サーバー エクスプ ローラー ウィンドウには、Microsoft Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウが呼び出されます。


サーバー エクスプ ローラー ウィンドウを使用すると、データベース テーブル、ビュー、トリガー、ストアド プロシージャなどの新しいデータベース オブジェクトを作成します。 [テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。 (図 5 を参照してください)、データベースのテーブル デザイナーが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**図 05**: データベースのテーブル デザイナー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image10.png))。


次の列を含むテーブルを作成する必要があります。

<a id="0.2_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | int | False |
| FirstName | nvarchar (50) | False |
| LastName | nvarchar (50) | False |
| 電話番号 | nvarchar (50) | False |
| 電子メール | nvarchar(255) | False |


最初の列、Id 列は特殊です。 Id 列と主キー列として Id 列をマークする必要があります。 列にある列のプロパティ (図 6 下部にあるを参照) を展開し、Identity の指定のプロパティの下にスクロールして、Id 列を指定します。 設定、 **(Id)** プロパティ値を**はい**します。

主キー列として列を選択し、キーのアイコンのボタンをクリックして列をマークするとします。 列が主キー列としてマークされている、キーのアイコンが、列の横に表示されます (図 6 参照)。

テーブルの作成が完了したら、新しいテーブルを保存する (フロッピー ディスクのアイコンのボタン) の保存 ボタンをクリックします。 新しいテーブルの名前を付けて*連絡先*します。

連絡先データベースのテーブルの作成を完了するには後、は、テーブルにレコードの一部を追加する必要があります。 サーバー エクスプ ローラー ウィンドウで、Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。 表示されたグリッドで 1 つまたは複数の連絡先を入力します。

## <a name="creating-the-data-model"></a>データ モデルを作成します。

ASP.NET MVC アプリケーションは、モデル、ビュー、およびコント ローラーで構成されます。 まず、前のセクションで作成した Contacts テーブルを表すモデル クラスを作成します。

このチュートリアルでは、データベースからモデル クラスを自動的に生成するのに Microsoft Entity Framework を使用します。

> [!NOTE] 
> 
> ASP.NET MVC フレームワークは、任意の方法で Microsoft Entity Framework には関連付けられません。 ASP.NET MVC は、NHibernate、LINQ to SQL、または ADO.NET を含む別のデータベース アクセス テクノロジで使用できます。


データ モデル クラスを作成する次の手順に従います。

1. ソリューション エクスプ ローラー ウィンドウで、Models フォルダーを右クリックして**追加、新しい項目の**します。 **新しい項目の追加**ダイアログでは、(図 6 参照) が表示されます。
2. 選択、**データ**カテゴリと**ADO.NET Entity Data Model**テンプレート。 データ モデルの名前を付けます*ContactManagerModel.edmx*  をクリックし、**追加**ボタンをクリックします。 (図 7 を参照してください)、Entity Data Model ウィザードが表示されます。
3. **モデルのコンテンツの選択**手順で、**データベースから生成**(図 7 を参照してください)。
4. **データ接続の選択**ステップ、ContactManagerDB.mdf データベースを選択し、名前を入力*ContactManagerDBEntities*エンティティ接続設定が (図 8 参照)。
5. **データベース オブジェクトの選択**ステップで、テーブル (図 9 参照) というラベルの付いたチェック ボックスをオンします。 データ モデル (だけ 1 つである、Contacts テーブル)、データベースに含まれるすべてのテーブルが含まれます。 名前空間を入力*モデル*します。 ウィザードを完了するには、[完了] をクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**図 06**: [新しい項目の追加] ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image12.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**図 07**: [モデルのコンテンツ ([フルサイズの画像を表示する] をクリックします](iteration-1-create-the-application-vb/_static/image14.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**図 08**: データ接続の選択 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image16.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**図 09**: データベース オブジェクトの選択 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image18.png))。


Entity Data Model ウィザードを完了すると、Entity Data Model のデザイナーが表示されます。 デザイナーでは、モデル化される各テーブルに対応するクラスが表示されます。 連絡先をという名前の 1 つのクラスが表示されます。

Entity Data Model ウィザードでは、データベース テーブル名に基づくクラス名を生成します。 ほとんどの場合、ウィザードによって生成されたクラスの名前を変更する必要があります。 デザイナーで、Contacts クラスを右クリックし、メニュー オプションを選択**の名前を変更**します。 連絡先 (複数) から、クラスの名前を変更するには (単数) にお問い合わせください。 クラス名を変更した後は、図 10 のようなクラスが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**図 10**: Contact クラス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image20.png))。


この時点で、データベース モデルを作成しました。 データベースに特定の連絡先レコードを表す、Contact クラスを使用できます。

## <a name="creating-the-home-controller"></a>Home コント ローラーを作成します。

次の手順では、ホーム コント ローラーを作成します。 ホーム コント ローラーは、ASP.NET MVC アプリケーションで呼び出される既定のコント ローラーです。

ホーム コント ローラーのクラスを作成するには、ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択する**追加、コント ローラー** (図 11 を参照してください)。 チェック ボックスに注意してください**アクション メソッドの作成、更新、および詳細のシナリオを追加**します。 クリックする前にこのチェック ボックスがオンになっていることを確認、**追加**ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**図 11**: Home コント ローラーの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image22.png))。


Home コント ローラーを作成するときに、リスト 1 で、クラスを取得します。

**1 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>連絡先の一覧を表示します。

連絡先のデータベース テーブル内のレコードを表示するためには、Index() アクションと、インデックス ビューを作成する必要があります。

Home コント ローラーには、既に Index() アクションが含まれています。 リスト 2 のように見えるように、このメソッドを変更する必要があります。

**2 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

リスト 2 で、ホーム コント ローラー クラスにというプライベート フィールドが含まれている通知\_エンティティ。 \_エンティティ フィールドは、データ モデルからエンティティを表します。 使用して、\_エンティティのフィールドに、データベースと通信します。

Index() メソッドは、ビューを連絡先データベースのテーブルからすべての連絡先を表すを返します。 式\_エンティティ。ContactSet.ToList() は、ジェネリック リストとして、連絡先の一覧を返します。

これまで作成インデックス コント ローラーで、次に、インデックス ビューを作成する必要があります。 インデックス ビューを作成する前に、メニュー オプションを選択してアプリケーションをコンパイルします**ソリューションのビルドをビルドします。** します。 常に表示されるモデル クラスの一覧の順序でビューを追加する前にプロジェクトをコンパイルする必要があります、**ビューの追加**ダイアログ。

Index() メソッドを右クリックし、メニュー オプションを選択して、インデックス ビューを作成する**ビューの追加**(図 12 を参照してください)。 このメニュー オプションを選択すると表示、**ビューの追加**ダイアログ (図 13 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**図 12**: インデックス ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image24.png))。


**ビューの追加**ダイアログ ボックスで、チェック ボックスをラベル付け**厳密に型指定されたビューを作成**。 ビューのデータ クラス ContactManager.Contact とコンテンツ リストの表示を選択します。 これらのオプションを選択するには、連絡先のレコードの一覧を表示するビューが生成されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**図 13**: ビューの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image26.png))。


クリックすると、**追加** ボタン、リスト 3 のインデックス ビューを生成します。 通知、&lt;ページ % @ %&gt;ディレクティブ ファイルの先頭に表示されます。 インデックス ビューが継承、ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;クラス。 つまり、ビュー モデル クラスは、Contact エンティティの一覧を表します。

Index ビューの本文には、各モデルのクラスによって表されるメンバーを反復処理する foreach ループが含まれています。 連絡先クラスの各プロパティの値は、HTML テーブル内で表示されます。

**3 - Views\Home\Index.aspx (未変更の状態) を一覧表示します。**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

インデックス ビューに 1 つの変更を加える必要があります。 詳細ビューを作成することはありません、ため、[詳細] リンクを削除できます。 検索し、インデックス ビューから、次のコードを削除します。

{.id = item.Id})%&gt;

インデックス ビューを変更した後は、Contact Manager アプリケーションを実行できます。 [デバッグ開始] メニュー オプション、デバッグを選択するか、f5 キーを押すだけです。 初めてアプリケーションを実行するダイアログ ボックスを図 14 に取得します。 オプションを選択**デバッグを有効にする Web.config ファイルを変更**[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**図 14**: デバッグの有効化 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image28.png))。


既定では、インデックス ビューが返されます。 このビューでは、連絡先データベースのテーブルからデータをすべて一覧表示されます (図 15 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**図 15**: The Index ビュー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image30.png))。


インデックス ビューに、ビューの下部にある場合は、新規作成というラベルの付いたリンクが含まれることに注意してください。 次のセクションでは、新しい連絡先を作成する方法について説明します。

## <a name="creating-new-contacts"></a>新しい連絡先を作成します。

新しい連絡先を作成するユーザーを有効にするのには、Home コント ローラーに 2 つの Create() アクションを追加する必要があります。 新しい連絡先を作成するための HTML フォームを返す 1 つの Create() アクションを作成する必要があります。 新しい連絡先の実際のデータベースの挿入を実行する 2 番目の Create() アクションを作成する必要があります。

Home コント ローラーに追加する必要がある新しい Create() メソッドは、リスト 4 に含まれます。

**4 - Controllers\HomeController.vb (でメソッドを作成する) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

2 番目の Create() メソッドは、HTTP POST によってのみ呼び出すことがときに、HTTP GET を使用 Create() の最初のメソッドを実行することができます。 つまり、2 番目の Create() メソッドは、HTML フォームを投稿する場合にのみ起動できます。 最初の Create() メソッドは、単に、新しい連絡先を作成するための HTML フォームを含むビューを返します。 2 番目の Create() メソッドにずっと興味深いものです。 データベースに新しい連絡先を追加します。

連絡先クラスのインスタンスをそのまま使用する 2 番目の Create() メソッドが変更されたことに注意してください。 HTML フォームからポストされたフォーム値にバインドされますこの連絡先クラス、ASP.NET MVC フレームワークによって自動的に。 作成する HTML フォームから各フォーム フィールドは、連絡先のパラメーターのプロパティに割り当てられます。

連絡先のパラメーターが [Bind] 属性で修飾されたことに注意してください。 [バインド] 属性は、連絡先 Id プロパティをバインドから除外に使用されます。 Id プロパティは、Identity プロパティを表すため、Id プロパティを設定することはありません。

Create() メソッドの本文には、Entity Framework がデータベースに新しい連絡先の挿入に使用されます。 新しい連絡先が連絡先の既存のセットに追加され、SaveChanges() メソッドは、基になるデータベースにこれらの変更をプッシュ バックします。

2 つの Create() メソッドのいずれかを右クリックし、メニュー オプションを選択して新しい連絡先を作成するための HTML フォームを生成する**ビューの追加**(図 16 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**図 16**: 作成ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image32.png))。


**ビューの追加**ダイアログ ボックスで、 **ContactManager.Contact**クラスおよび**作成**コンテンツの表示のオプション (図 17 を参照してください)。 クリックすると、**追加**ボタン、作成ビューが自動的に生成されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**図 17**: 展開のページが表示 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image34.png))。


ビュー作成するにはには、各連絡先クラスのプロパティのフォームのフィールドが含まれています。 ビューを作成するためのコードは、リスト 5 に含まれます。

**Listing 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Create() メソッドを変更し、作成ビューを追加した後は、連絡先マネージャー アプリケーションを実行し、新しい連絡先を作成することができます。 をクリックして、**新規作成**作成ビューに移動するインデックス ビューに表示されるリンク。 図 18 に表示する必要があります。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**図 18**: ビューの作成 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image36.png))。


## <a name="editing-contacts"></a>連絡先の編集

連絡先のレコードを編集するための機能の追加は、新しい連絡先のレコードを作成するための機能を追加するとよく似ています。 最初に、ホーム コント ローラーのクラスに新しい 2 つの Edit メソッドを追加する必要があります。 これらの新しい Edit() メソッドは、6 の一覧に含まれます。

**6 - Controllers\HomeController.vb (と Edit メソッド) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

最初の Edit() メソッドは、HTTP GET 操作で呼び出されます。 Id パラメーターは編集されている連絡先のレコードの Id を表します。 このメソッドに渡されます。 Entity Framework は、id と一致する連絡先を取得するために使用します。レコードを編集するための HTML フォームを含むビューが返されます。

2 番目の Edit() メソッドは、データベースに実際の更新プログラムを実行します。 このメソッドは、Contact クラスのインスタンスをパラメーターとして受け取ります。 ASP.NET MVC フレームワーク フォーム フィールド編集フォームからこのクラスに自動的にバインドします。 T がないことに注意してくださいには、(必要があります、Id プロパティの値) の連絡先を編集するときに、[バインド] 属性が含まれます。

Entity Framework は、データベースに変更された連絡先を保存に使用されます。 元の連絡先は、まずデータベースから取得する必要があります。 次に、Entity Framework ApplyPropertyChanges() メソッドが呼び出され、連絡先に変更を記録します。 最後に、基になるデータベースの変更を永続化、Entity Framework SaveChanges() メソッドが呼び出されます。

Edit() メソッドを右クリックし、追加のビュー メニュー オプションを選択して編集フォームを含むビューを生成することができます。 ビューの追加] ダイアログ ボックスで、[、 **ContactManager.Models.Contact**クラスおよび**編集**コンテンツを表示 (図 19 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**図 19**: ビューの編集の追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image38.png))。


[追加] ボタンをクリックすると、新しい編集ビューが自動的に生成されます。 生成される HTML フォームには、各連絡先クラス (7 のリストを参照) のプロパティに対応するフィールドが含まれています。

**Listing 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>連絡先を削除します。

連絡先を削除する場合は、ホーム コント ローラー クラスに 2 つの Delete() アクションを追加する必要があります。 Delete() の最初のアクションは、削除の確認フォームを表示します。 2 番目の Delete() アクションは、実際の削除を実行します。

> [!NOTE] 
> 
> 後で、7 では、イテレーションでよう変更 Contact Manager Ajax の削除、1 つの手順をサポートするようします。


2 つの新しい Delete() メソッドは、8 の一覧に含まれます。

**8 - Controllers\HomeController.vb (削除メソッド) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

最初の Delete() メソッドは、連絡先のレコードをデータベースから削除するための確認フォームを返します (Figure20 を参照してください)。 2 番目の Delete() メソッドは、データベースに対して実際の削除操作を実行します。 データベースから元の連絡先の取得が完了した後は、データベースの削除を実行する、Entity Framework DeleteObject() と SaveChanges() メソッドが呼び出されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**図 20**: 削除確定ビュー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image40.png))。


(図 21 を参照してください) に連絡先レコードを削除するためのリンクが含まれているように、インデックス ビューを変更する必要があります。 [編集] リンクを含む同じテーブル セルに次のコードを追加する必要があります。

{.id = item.Id})%&gt;


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**図 21**: 編集リンクを使用してビューにインデックス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image42.png))。


次に、削除確定ビューを作成する必要があります。 ホーム コント ローラー クラスの Delete() メソッドを右クリックし、追加のビュー メニュー オプションを選択します。 (図 22 を参照してください) ビューの追加 ダイアログが表示されます。

異なりは、リスト、作成、編集ビューの場合、ビューの追加ダイアログには、削除ビューを作成するオプションがありません。 代わりに、選択、 **ContactManager.Models.Contact**データ クラスと**空**コンテンツを表示します。 コンテンツのオプションでは、自分たちが、ビューを作成することが必要、空のビューを選択します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**図 22**: 削除確認ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image44.png))。


一覧表示する 9 Delete ビューのコンテンツが含まれています。 このビューには、確認のフォームが含まれます (図 21 を参照) を削除するかどうか、特定の連絡先があります。

**Listing 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>既定のコント ローラーの名前を変更します。

連絡先を操作するため、コント ローラー クラスの名前が HomeController クラスということをわざわざ可能性があります。 べきで t、コント ローラーは、ContactController を指定しますか。

この問題は、簡単に修正します。 最初に、Home コント ローラーの名前をリファクターする必要があります。 HomeController クラスにでは、Visual Studio コード エディターを開き、クラスの名前を右クリックしてメニュー オプションを選択**の名前を変更**します。 このメニュー オプションを選択すると、名前の変更 ダイアログが開きます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**図 23**: コント ローラーの名前のリファクタリング ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image46.png))。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**図 24**: 名前の変更 ダイアログを使用して ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image48.png))。


コント ローラー クラスの名前を変更すると、Visual Studio にも、Views フォルダー内のフォルダーの名前が更新されます。 \Views\Contact フォルダーには、visual Studio は \Views\Home フォルダーを変更します。

この変更を行った後、アプリケーションは、ホーム コント ローラーを不要になったがあります。 アプリケーションを実行すると、図 25 のエラー ページが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**図 25**: 既定のコント ローラーなし ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-vb/_static/image50.png))。


Home コント ローラーではなく、連絡先のコント ローラーを使用する Global.asax ファイル内の既定のルートを更新する必要があります。 Global.asax ファイルを開き、既定のルート (10 のリストを参照) で使用される既定のコント ローラーを変更します。

**10 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

これらの変更を行った後、連絡先マネージャーが正しく実行されます。 ここで、既定のコント ローラーとして、連絡先のコント ローラー クラスが使用されます。

## <a name="summary"></a>まとめ

この最初のイテレーションで作成した基本的な Contact Manager アプリケーションをすばやく可能です。 このコント ローラーとビューの最初のコードを自動的に生成する Visual Studio を活用しました。 データベース モデル クラスを自動的に生成する Entity Framework の利点をしました。

現時点では、一覧表示、作成、編集、および Contact Manager アプリケーションに連絡先レコードを削除をことができますします。 つまり、すべてのデータベース駆動型 web アプリケーションで必要な基本的なデータベース操作を実行できます。

残念ながら、このアプリケーションでは、いくつかの問題があります。 最初はこれを認めるお、Contact Manager アプリケーションが最も魅力的なアプリケーションではありません。 何らかのデザイン作業が必要があります。 次のイテレーションでは、既定ビュー マスター ページと、アプリケーションの外観を向上させるためにカスケード スタイル シートを変更した方法を紹介します。

次に、すべてのフォーム検証を実装していません。 たとえば、何もない、フォームのフィールドのいずれかの値を入力せずにお問い合わせフォームを作成するを送信することを防止するためです。 さらに、無効な電話番号と電子メール アドレスを入力することができます。 繰り返し #3 で、フォーム検証の問題に取り組むまずします。

最後に、および最も重要なは、Contact Manager アプリケーションの現在のイテレーションを簡単に変更または維持することはできません。 たとえば、データベース アクセス ロジックには、コント ローラー アクションに右が組み込まれています。 つまり、これらのコント ローラーを変更することがなく、データ アクセス コードを修正することはできません。 後のイテレーションで実装できる、連絡先マネージャーに変更に柔軟に対応するソフトウェア設計のパターンについて説明します。

> [!div class="step-by-step"]
> [前へ](iteration-7-add-ajax-functionality-cs.md)
> [次へ](iteration-2-make-the-application-look-nice-vb.md)
