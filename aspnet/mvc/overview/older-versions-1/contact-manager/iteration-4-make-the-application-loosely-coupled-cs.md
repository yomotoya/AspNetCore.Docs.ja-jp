---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: "4 番目の反復処理する疎結合アプリケーション (c#) |Microsoft ドキュメント"
author: microsoft
description: "この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b8df72f51b4730a1fa9178b51a3770ce9edf181
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>4 番目の反復処理する疎結合アプリケーション (c#)
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築

この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。 お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。

私たちは、複数のイテレーションにおける、アプリケーションを構築します。 各イテレーションで、アプリケーション、徐々 に向上します。 この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。

- イテレーション 1 には、アプリケーションを作成します。 最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- イテレーション 2 では、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- イテレーション 3 - フォーム検証を追加します。 3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 私たちも電子メール アドレスと電話番号を検証します。

- 4: イテレーションは、疎結合アプリケーションを作成します。 この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。

- イテレーション #5 - 単体テストを作成します。 5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。 データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。

- イテレーション 6 - テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- イテレーション #7 - Ajax 機能を追加します。 7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。

## <a name="this-iteration"></a>このイテレーション

連絡先のマネージャー アプリケーションの 4 番目このイテレーションより疎結合アプリケーションを作成するアプリケーションをリファクタリングします。 アプリケーションが疎結合されたときに、アプリケーションの他の部分でコードを変更することがなく、アプリケーションの 1 つの部分でコードを変更できます。 疎結合アプリケーションは、回復力を変更します。

現時点では、すべての連絡先のマネージャー アプリケーションによって使用されるデータのアクセスと検証ロジックはコント ローラー クラスに含まれます。 これは、良い考えです。 アプリケーションの 1 つの部分を変更する必要がある場合、アプリケーションの別の部分にバグが生じる可能性があります。 たとえばの検証ロジックを変更する場合とする可能性が、データ アクセスまたはコント ローラー ロジックに新しいバグを導入します。

> [!NOTE] 
> 
> (SRP) クラスを変更する 1 つ以上の理由が付与されないようにします。 単一の責任の原則の大規模な違反は、コント ローラー、検証、およびデータベースのロジックが混在します。


アプリケーションを変更する必要があるいくつかの理由があります。 アプリケーションに新しい機能を追加する必要があります、アプリケーションでバグを修正する必要があります。 または、アプリケーションの機能を実装する方法を変更する必要があります。 アプリケーションは、静的なことはほとんどありません。 拡張し、時間の経過と共に変化する傾向があります。

たとえば、変更、データ アクセス層を実装する方法を決定することに想像してください。 右、連絡先のマネージャー アプリケーションを使用して、Microsoft の Entity Framework データベースにアクセスします。 ただし、ADO.NET Data Services や NHibernate など、新規または別のデータ アクセス テクノロジに移行することができます。 ただし、データ アクセス コードを検証し、コント ローラーのコードから分離されてないため、データ アクセスに直接関連しない他のコードを変更することがなく、アプリケーションでデータ アクセス コードを変更する方法はありません。

アプリケーションが疎結合されたときにその一方で、ことができますを変更するアプリケーションの 1 つの部分、アプリケーションの他の部分に触れることがなくです。 たとえば、検証またはコント ローラーのロジックを変更することがなく、データ アクセス テクノロジを切り替えることができます。

このイテレーションおをより疎結合アプリケーションに連絡先のマネージャー アプリケーションのリファクタリングを有効にするいくつかのソフトウェアの設計パターンの利用です。 おが完了すると、t が勝利した連絡先のマネージャー何も実行することでした t は前に行います。 ただし、後でより簡単にアプリケーションを変更するようおになります。

> [!NOTE] 
> 
> リファクタリングとは、既存の機能が失われないようにアプリケーションを書き直しのプロセスです。


## <a name="using-the-repository-software-design-pattern"></a>リポジトリのソフトウェアのデザイン パターンを使用します。

最初の変更は、リポジトリ パターンと呼ばれるソフトウェアの設計パターンの利用を開始します。 アプリケーションの残りの部分から、データ アクセス コードを分離するのに、リポジトリ パターンを使用します。

リポジトリ パターンを実装するには、次の 2 つの手順を完了することが必要です。

1. インターフェイスを作成します。
2. インターフェイスを実装する具象クラスを作成します。

最初に、すべての実行に必要なデータ アクセス方法を説明するインターフェイスを作成する必要があります。 IContactManagerRepository インターフェイスが 1 のリストに含まれています。 このインターフェイスは、5 つの方法を説明します。 CreateContact()、DeleteContact()、EditContact()、GetContact、および ListContacts() です。

**1 - Models\IContactManagerRepositiory.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

次に、IContactManagerRepository インターフェイスを実装する具象クラスを作成する必要があります。 使用しているため、Microsoft の Entity Framework データベースへのアクセス、EntityContactManagerRepository をという名前の新しいクラスを作成します。 このクラスは、2 のリストに含まれます。

**2 - Models\EntityContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

EntityContactManagerRepository クラスが IContactManagerRepository インターフェイスを実装することを確認します。 クラスでは、そのインターフェイスによって記述されたメソッドの 5 つすべてを実装します。

なぜお気にインターフェイスを使用する必要があるでしょうか。 なぜインターフェイスとそれを実装するクラスの両方を作成する必要があるのですか。

1 つの例外を除き、アプリケーションの残りの部分は、インターフェイスと具象クラスではなく対話します。 EntityContactManagerRepository クラスによって公開されるメソッドを呼び出す代わりに、IContactManagerRepository インターフェイスによって公開されるメソッドとします。

ようにして、アプリケーションの残りの部分を変更しなくても新しいクラスとインターフェイスを実装おできます。 たとえば、いくつかの将来の日付では、IContactManagerRepository インターフェイスを実装する DataServicesContactManagerRepository クラス実装はします可能性があります。 DataServicesContactManagerRepository クラスは、ADO.NET Data Services を使用して、Microsoft の Entity Framework ではなくデータベースにアクセスする可能性があります。

EntityContactManagerRepository の具象クラスではなく IContactManagerRepository インターフェイスに対して、アプリケーションのコードをプログラミングする場合、コードの残りの部分のいずれかを変更することがなく具象クラスを切り替えるおことができます。 たとえば、お EntityContactManagerRepository クラスからに切り替える DataServicesContactManagerRepository クラス、データ アクセスまたは検証ロジックを変更しなくてもします。

具象クラスではなくインターフェイス (抽象) に対するプログラミングでは、アプリケーションを変更する回復力のあるになります。

> [!NOTE] 
> 
> メニュー オプションのリファクタリング、インターフェイスの抽出 を選択して、Visual Studio 内での具象クラスからインターフェイスをすばやく作成できます。 たとえば、まず EntityContactManagerRepository クラスを作成でき、IContactManagerRepository インターフェイスを自動的に生成するインターフェイスの抽出を使用できます。


## <a name="using-the-dependency-injection-software-design-pattern"></a>依存関係挿入ソフトウェアのデザイン パターンを使用します。

データ アクセス コード リポジトリ クラスを個別に移行が、このクラスを使用して、連絡先のコント ローラーの変更は必要があります。 クラスを使用する、リポジトリ、コント ローラーに依存関係の挿入と呼ばれるソフトウェアの設計パターンの利用されます。

変更した連絡先コント ローラーは、3 の一覧に含まれます。

**3 - Controllers\ContactController.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

リスト 3 の連絡先コント ローラーが 2 つのコンス トラクターをあることに注意してください。 最初のコンス トラクターは、IContactManagerRepository インターフェイスの具象インスタンスを 2 番目のコンス トラクターに渡します。 連絡先のコント ローラー クラス使用*コンス トラクターの依存性の注入*です。

最初のコンス トラクターと EntityContactManagerRepository クラスを使用する配置のみの一覧です。 クラスの残りの部分は、具象 EntityContactManagerRepository クラスではなく IContactManagerRepository インターフェイスを使用します。

これにより、今後 IContactManagerRepository クラスの実装を切り替えるには簡単です。 EntityContactManagerRepository クラスではなく DataServicesContactRepository クラスを使用する場合は、最初のコンス トラクターを変更するだけです。

コンス トラクターの依存関係挿入も検証を実現にお問い合わせくださいコント ローラー クラス非常にします。 単体テストでは、IContactManagerRepository クラスのモック実装を渡すことによって、連絡先のコント ローラーをインスタンス化できます。 依存関係の挿入には、この機能は、連絡先のマネージャー アプリケーションの単体テストを構築するときに、次のイテレーションで弊社にとって非常に重要になります。

> [!NOTE] 
> 
> IContactManagerRepository インターフェイスの特定の実装から連絡先コント ローラー クラスを完全に分離する場合、行える StructureMap または Microsoft などの依存関係の挿入をサポートするフレームワークを活用Entity Framework (MEF)。 依存関係の挿入のフレームワークを利用してしないコード内の具象クラスを参照する必要があります。


## <a name="creating-a-service-layer"></a>サービス レイヤーを作成します。

お気付きリスト 3 に変更されたコント ローラー クラスで、コント ローラー ロジックで、検証ロジックがまだ混在させることです。 同じ理由で、データ アクセス ロジックを分離することをお勧めするは、この検証ロジックを分離することをお勧めを勧めします。

この問題を修正するのには、個別を作成できるよう[*サービス層*](http://martinfowler.com/eaaCatalog/serviceLayer.html)です。 サービス層は、このコント ローラーおよびリポジトリ クラス間を挿入したり別のレイヤーです。 サービス層には、検証ロジックのすべてを含めて、ビジネス ロジックが含まれています。

ContactManagerService が 4 の一覧に含まれています。 連絡先のコント ローラー クラスからの検証ロジックが含まれています。

**4 - Models\ContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

ContactManagerService のコンス トラクターは、必要な ValidationDictionary であることに注意してください。 サービス レイヤーは、この ValidationDictionary によってコント ローラー レイヤーと通信します。 デコレータ パターンについて説明している場合は、次のセクションで詳しく ValidationDictionary について説明します。

さらに、ContactManagerService が IContactManagerService インターフェイスを実装することに注意してください。 常に努めること具象クラスではなくインターフェイスをプログラミングします。 連絡先のマネージャー アプリケーションの他のクラスはいない ContactManagerService クラス直接やり取りします。 代わりに、1 つの例外を除き、連絡先のマネージャー アプリケーションの残りの部分が IContactManagerService インターフェイスに対するプログラミングされます。

IContactManagerService インターフェイスは、5 の一覧に含まれます。

**5 - Models\IContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

変更された連絡先のコント ローラー クラスは、一覧表示する 6 に含まれます。 連絡先のコント ローラーが不要になった ContactManager リポジトリと対話することに注意してください。 代わりに、連絡先のコント ローラーは、ContactManager サービスと対話します。 各レイヤーで他のレイヤーから可能な限り分離します。

**6 - Controllers\ContactController.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

不要になったアプリケーションの 1 つの責任の原則 (SRP)。 リスト 6 で連絡先コント ローラーがアプリケーションの実行のフローを制御する以外のすべての責任の切り離されました。 すべての検証ロジックは連絡先コント ローラーから削除され、サービス層にプッシュされます。 リポジトリのレイヤーにすべてのデータベースのロジックが押されました。

## <a name="using-the-decorator-pattern"></a>デコレーターのパターンを使用します。

完全に、コント ローラーの層から、サービス層を分離することができるようにすることができます。 原則として、MVC アプリケーションへの参照を追加することがなく、コント ローラーの層から独立したアセンブリに、サービス層をコンパイルすることもあります。

ただし、このサービス層は、コント ローラーのレイヤーに検証エラー メッセージを渡すことができる必要があります。 コント ローラーとサービス層を結合することがなく検証エラー メッセージを通信するためにサービス層を有効する方法をおしますか。 という名前のソフトウェアの設計パターンの利用できる、[デコレータ パターン](http://en.wikipedia.org/wiki/Decorator_pattern)です。

コント ローラーでは、名前付き ModelState ModelStateDictionary を使用して、検証エラーを表します。 このため、コント ローラーのレイヤーからサービス層に渡される ModelState をしたくなるする必要があります。 ただし、ModelState を使用して、サービス層で実行するように、サービス層、ASP.NET MVC フレームワークの機能に依存します。 ASP.NET MVC アプリケーションではなく、WPF アプリケーションで、サービス層を使用する、将来は不良になります。 その場合と思いません t は ModelStateDictionary クラスを使用して、ASP.NET MVC フレームワークを参照するとします。

デコレータ パターンでは、新しいクラスでインターフェイスを実装するために、既存のクラスをラップすることができます。 連絡先のマネージャーのプロジェクトには、7 の一覧に含まれる ModelStateWrapper クラスが含まれています。 ModelStateWrapper クラスでは、8 の一覧で、インターフェイスを実装します。

**7 - Models\Validation\ModelStateWrapper.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**8 - Models\Validation\IValidationDictionary.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

5 の一覧を詳しく見てを実行する場合は IValidationDictionary インターフェイスを排他的に使用 ContactManager サービス レイヤーが表示されます。 ContactManager サービスは、ModelStateDictionary クラスに依存しません。 連絡先コント ローラー ContactManager サービスを作成すると、コント ローラーは、次のようにその ModelState をラップします。

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>概要

このイテレーションで、連絡先のマネージャー アプリケーションに新しい機能を追加おできません。 このイテレーションの目標を保守して変更するのにように簡単に、連絡先のマネージャー アプリケーションをリファクターするでした。

まず、リポジトリのソフトウェアの設計パターンを実装します。 ContactManager リポジトリ クラスを個別に移行したすべてのデータ アクセス コード。

また、検証ロジックを分離、コント ローラー ロジックからです。 すべての検証コードを含む別のサービス層を作成しました。 コント ローラー レイヤーは、サービス層と対話し、サービス層がリポジトリ レイヤーとやり取りします。

サービス層を作成したときは、サービス層から ModelState を分離するデコレータ パターンの利点がかかりました。 サービス層には、ModelState ではなく IValidationDictionary インターフェイスに対してプログラミングされました。

最後に、依存関係の挿入のパターンをという名前のソフトウェアの設計パターンの利点をかかりました。 このパターンでは、具象クラスではなくインターフェイス (抽象) でプログラミングできます。 依存関係の挿入のデザイン パターンを実装することも検証を実現しているコードよりです。 次のイテレーションで、プロジェクトに単体テストを追加します。

>[!div class="step-by-step"]
[前へ](iteration-3-add-form-validation-cs.md)
[次へ](iteration-5-create-unit-tests-cs.md)
