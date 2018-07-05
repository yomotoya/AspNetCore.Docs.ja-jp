---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: '繰り返し #4 – ようにアプリケーションを疎結合 (c#) |Microsoft Docs'
author: microsoft
description: この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 .
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 8eff8088398d0f7afc020b2bbdf41526ae51591a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829500"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>繰り返し #4 – アプリケーションを疎結合 (c#) を作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築

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

この 4 番目のイテレーションの Contact Manager アプリケーションは、アプリケーションをより弱い結合にアプリケーションをリファクタリングします。 アプリケーションが疎結合、ときに、アプリケーションの他の部分のコードを変更することがなくアプリケーションの 1 つの部分のコードを変更できます。 疎結合アプリケーションは、変更に柔軟に対応します。

現時点では、すべての Contact Manager アプリケーションで使用されるデータ アクセスと検証ロジックは、コント ローラー クラスに含まれます。 これは、不適切な考え方です。 アプリケーションの 1 つの部分を変更する必要がある場合、アプリケーションの別の部分にバグが生じる可能性があります。 たとえば、検証ロジックを変更する恐れが、データ アクセスまたはコント ローラー ロジックに新しいバグを導入あります。

> [!NOTE] 
> 
> (SRP) クラスを変更する 1 つ以上の理由がないことはありません必要があります。 大規模な単一責任の原則違反は、コント ローラー、検証、およびデータベース ロジックを混在させます。


アプリケーションを変更する必要があるいくつかの理由があります。 アプリケーションに新しい機能を追加する必要があります、アプリケーションでは、バグを修正する必要があります。 またはアプリケーションの機能を実装する方法を変更する必要があります。 アプリケーションは、静的なことはほとんどありません。 拡張し、時間の経過と共に変化する傾向があります。

たとえば、変更、データ アクセス層を実装する方法を決定することに想像してください。 右、Contact Manager アプリケーションを使用して Microsoft Entity Framework がデータベースにアクセスします。 ただし、ADO.NET Data Services や NHibernate などの新規または別のデータ アクセス テクノロジに移行することがあります。 ただし、データ アクセス コードを検証し、コント ローラーのコードから分離されてないため、データ アクセスに直接関連付けられていないその他のコードを変更することがなく、アプリケーションでデータ アクセス コードを変更する方法はありません。

アプリケーションが疎結合、ときにその一方で、ことができますに変更を加える 1 つのアプリケーションの一部をアプリケーションの他の部分に触れることがなく。 たとえば、検証またはコント ローラーのロジックを変更することがなく、データ アクセス テクノロジを切り替えることができます。

このイテレーションより疎結合アプリケーションに Contact Manager アプリケーションのリファクタリングを有効にするいくつかのソフトウェア設計パターンの利点を実行します。 私たちが完了すると、t が勝利した Contact Manager 何も実行するには一致しませんでした t の前に操作を行います。 ただし、今後より簡単にアプリケーションを変更することを予定です。

> [!NOTE] 
> 
> リファクタリングは、既存の機能が失われないようにアプリケーションを書き直しのプロセスです。


## <a name="using-the-repository-software-design-pattern"></a>リポジトリのソフトウェア設計パターンを使用

最初の変更は、リポジトリ パターンと呼ばれるソフトウェアの設計パターンの活用することです。 アプリケーションの残りの部分から、データ アクセス コードを分離するのにリポジトリ パターンを使用します。

リポジトリ パターンを実装するには、次の 2 つの手順を完了する必要があります。

1. インターフェイスを作成します。
2. インターフェイスを実装する具象クラスを作成します。

最初に、すべての実行に必要なデータ アクセス メソッドを記述するインターフェイスを作成する必要があります。 リスト 1 で IContactManagerRepository インターフェイスが含まれています。 このインターフェイスは、5 つの方法をについて説明します。 CreateContact()、DeleteContact()、EditContact()、GetContact、および ListContacts() します。

**1 - Models\IContactManagerRepositiory.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

次に、IContactManagerRepository インターフェイスを実装する具象クラスを作成する必要があります。 Microsoft Entity Framework を使用して、データベースへのアクセスには、ため EntityContactManagerRepository をという名前の新しいクラスを作成します。 このクラスは、リスト 2 に含まれます。

**2 - Models\EntityContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

EntityContactManagerRepository クラスが IContactManagerRepository インターフェイスを実装することに注意してください。 クラスは、そのインターフェイスによって記述されたメソッドの 5 つすべてを実装します。

なぜインターフェイスで頭を悩ませる必要があるでしょうか。 なぜ、インターフェイスとそれを実装するクラスの両方を作成する必要があるのでしょうか。

1 つの例外を除き、アプリケーションの残りの部分は操作インターフェイスと具象クラスではありません。 EntityContactManagerRepository クラスによって公開されるメソッドを呼び出す代わりに、IContactManagerRepository インターフェイスによって公開されるメソッドを呼び出します。

これにより、アプリケーションの残りの部分を変更することがなく新しいクラスを使用してインターフェイスを実装しましたできます。 たとえば、いくつかの将来の日付で DataServicesContactManagerRepository IContactManagerRepository インターフェイスを実装するクラスを実装する可能性があります。 DataServicesContactManagerRepository クラスは、ADO.NET Data Services を使用して、Microsoft Entity Framework ではなく、データベースにアクセスする可能性があります。

EntityContactManagerRepository の具象クラスではなく IContactManagerRepository インターフェイスに対して、アプリケーション コードがプログラミングされた場合は、コードの残りの部分のいずれかを変更することがなく具象クラスを切り替えることできます。 たとえば、切り替えることができます、EntityContactManagerRepository クラスから DataServicesContactManagerRepository クラスに、データ アクセスまたは検証ロジックを変更することがなく。

具象クラスではなくインターフェイス (抽象化) に対するプログラミングによって、アプリケーションの変更に柔軟に対応します。

> [!NOTE] 
> 
> メニュー オプションのリファクタリング、インターフェイスの抽出を選択して、Visual Studio 内の具象クラスからインターフェイスをすばやく作成できます。 たとえば、まず EntityContactManagerRepository クラスを作成でき、IContactManagerRepository インターフェイスを自動的に生成するインターフェイスの抽出を使用できます。


## <a name="using-the-dependency-injection-software-design-pattern"></a>依存関係の挿入ソフトウェア設計パターンを使用

別のリポジトリ クラスには、データ アクセス コードを移行しましたが、このクラスを使用して、連絡先のコント ローラーを変更する必要があります。 依存関係の挿入、コント ローラーで、リポジトリ クラスを使用すると呼ばれるソフトウェアの設計パターンの利用されます。

変更後にお問い合わせくださいコント ローラーは、リスト 3 に含まれています。

**3 - Controllers\ContactController.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

リスト 3 の連絡先のコント ローラーが 2 つのコンス トラクターをあることに注意してください。 IContactManagerRepository インターフェイスの具象インスタンスは、最初のコンス トラクターは、2 番目のコンス トラクターに渡します。 連絡先のコント ローラー クラスは*コンス トラクターの依存関係挿入*します。

1 つのみで場所と EntityContactManagerRepository クラスを使用することは最初のコンス トラクターです。 クラスの残りの部分では、具体的な EntityContactManagerRepository クラスではなく IContactManagerRepository インターフェイスを使用します。

これにより、今後 IContactManagerRepository クラスの実装を切り替えるには簡単です。 EntityContactManagerRepository クラスではなく DataServicesContactRepository クラスを使用する場合は、最初のコンス トラクターを変更するだけです。

コンス トラクターの依存関係の挿入によっても、連絡先のコント ローラー クラス非常にテスト可能です。 単体テストでは、IContactManagerRepository クラスのモック実装を渡すことによって、連絡先のコント ローラーをインスタンス化できます。 この機能の依存関係の挿入は、Contact Manager アプリケーションの単体テストを構築するときに、次の反復処理することで非常に重要なになります。

> [!NOTE] 
> 
> IContactManagerRepository インターフェイスの特定の実装から連絡先のコント ローラー クラスを完全に分離する場合を行う StructureMap や Microsoft などの依存関係の挿入をサポートするフレームワークを活用Entity Framework (MEF)。 依存関係の挿入のフレームワークを利用してしないコードで具象クラスを参照する必要があります。


## <a name="creating-a-service-layer"></a>サービス層の作成

お気付きかもしれません、検証ロジックがリスト 3 の変更後のコント ローラー クラスで、コント ローラー ロジックの混在まだします。 データ アクセス ロジックを分離することをお勧めする同様の理由で、検証ロジックを分離することをお勧めを勧めします。

この問題を解決するには、個別を作成できます[*サービス層*](http://martinfowler.com/eaaCatalog/serviceLayer.html)します。 サービス層は、コント ローラーおよびリポジトリ クラスの間に挿入できる別のレイヤーです。 サービス層には、検証ロジックのすべてを含む、ビジネス ロジックが含まれています。

リスト 4、ContactManagerService が含まれています。 連絡先のコント ローラー クラスからの検証ロジックが含まれています。

**4 - Models\ContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

ContactManagerService のコンス トラクターが、ValidationDictionary が必要なことに注意してください。 サービス層は、この ValidationDictionary によってコント ローラーのレイヤーと通信します。 デコレーター パターンについて説明しますと、次のセクションで詳しく ValidationDictionary について説明します。

さらに、ContactManagerService が IContactManagerService インターフェイスを実装することに注意してください。 常に具象クラスではなくインターフェイスに対するプログラミングを行うよう努力する必要があります。 Contact Manager アプリケーションの他のクラスは ContactManagerService クラスと直接対話しません。 代わりに、1 つの例外を除き、Contact Manager アプリケーションの残りの部分は IContactManagerService インターフェイスに対してプログラミングします。

IContactManagerService インターフェイスは、リスト 5 に含まれています。

**5 - Models\IContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

変更後にお問い合わせくださいコント ローラー クラスは、リスト 6 に含まれます。 連絡先のコント ローラーが不要になった ContactManager リポジトリと対話することに注意してください。 代わりに、連絡先のコント ローラーは、ContactManager サービスと対話します。 各レイヤーは、他のレイヤーから可能な限り分離します。

**6 - Controllers\ContactController.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

アプリケーションで不要になった単一責任原則 (SRP) の実行します。 リスト 6 で連絡先コント ローラーがアプリケーションの実行フローを制御する以外のすべての責任の切り離されました。 すべての検証ロジックは、連絡先のコント ローラーから削除され、サービス層にプッシュされます。 すべてのデータベース ロジックはリポジトリ層にプッシュされました。

## <a name="using-the-decorator-pattern"></a>デコレーター パターンを使用します。

完全に、コント ローラーの層から、サービス層を分離するためにできるようにします。 原則として、MVC アプリケーションへの参照を追加することがなく、コント ローラーの層から別のアセンブリに、サービス層をコンパイルすることができなければなりません。

ただし、このサービス層は、コント ローラーのレイヤーに検証エラー メッセージを渡すことができる必要があります。 コント ローラーとサービス層を結合せずに検証エラー メッセージを通信するために、サービス層を有効にいますか。 という名前のソフトウェア デザイン パターンの活用する、[デコレーター パターン](http://en.wikipedia.org/wiki/Decorator_pattern)します。

コント ローラーでは、ModelState をという名前の ModelStateDictionary を使用して、検証エラーを表します。 そのため、ModelState をコント ローラーのレイヤーからサービス レイヤーに渡すしたくなるかもしれません。 ただし、ModelState を使用して、サービス層では、サービス層に依存しないように、ASP.NET MVC フレームワークの機能です。 ASP.NET MVC アプリケーションではなく、WPF アプリケーションで、サービス層を使用する、将来は不良になります。 その場合は、払い戻し t は ModelStateDictionary クラスを使用する ASP.NET MVC フレームワークを参照するとします。

デコレーター パターンでは、新しいクラスにインターフェイスを実装するために、既存のクラスをラップすることができます。 連絡先マネージャー プロジェクトには、リスト 7 に含まれる ModelStateWrapper クラスが含まれています。 ModelStateWrapper クラスは、8 の一覧で、インターフェイスを実装します。

**7 - Models\Validation\ModelStateWrapper.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**8 - Models\Validation\IValidationDictionary.cs を一覧表示します。**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

見てリスト 5 を実行する場合は、ContactManager サービス層が排他的 IValidationDictionary インターフェイスを使用することが表示されます。 ContactManager サービスでは、ModelStateDictionary クラスに依存しません。 連絡先のコント ローラーは、ContactManager サービスを作成するとき、コント ローラーは次のように、ModelState をラップします。

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>まとめ

このイテレーションで Contact Manager アプリケーションに新しい機能を追加したことしませんでした。 このイテレーションの目標は、リファクタリング、Contact Manager アプリケーションを維持し、変更ができるので簡単でした。

まず、リポジトリのソフトウェア設計パターンを実装します。 すべてのデータ アクセス コードを ContactManager リポジトリ クラスを個別に移行したとします。

私たちも、検証ロジックをコント ローラー ロジックから分離されます。 すべての検証コードを含む別のサービス層を作成しました。 コント ローラー レイヤー、サービス層と対話し、サービス層がリポジトリ層と対話します。

サービス層を作成したときに、ModelState をサービス層から分離するデコレーター パターンの利点をしました。 サービス層には、ModelState のではなく IValidationDictionary インターフェイスに対してプログラミングされました。

最後に、依存関係の注入パターンをという名前のソフトウェア デザイン パターンの利用をしました。 このパターンでは、具象クラスではなくインターフェイス (抽象化) に対してプログラミングできます。 依存関係の挿入のデザイン パターンを実装することも、によりコードがしやすい。 次のイテレーションでは、このプロジェクトに単体テストを追加します。

> [!div class="step-by-step"]
> [前へ](iteration-3-add-form-validation-cs.md)
> [次へ](iteration-5-create-unit-tests-cs.md)
