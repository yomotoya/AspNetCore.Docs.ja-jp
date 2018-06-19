---
title: ASP.NET Core のキー管理の拡張機能
author: rick-anderson
description: ASP.NET Core データ保護キーの管理の拡張性について説明します。
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e3042b371cf7be8fa0218c1906042d2810b180e3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
ms.locfileid: "30074165"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core のキー管理の拡張機能

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> 読み取り、[キー管理](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)ようにこれらの Api の背後にある基本的な概念について説明します、このセクションを読む前にセクションです。

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。

## <a name="key"></a>キー

`IKey`インターフェイスは、キー暗号方式での基本的な形式です。 用語キーは、抽象の意味での「暗号キー マテリアル」文字どおりの意味ではなくここで使用されます。 キーには、次のプロパティがあります。

* アクティブ化、作成、および有効期限の日付

* 失効状態

* キー識別子 (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

さらに、`IKey`公開、`CreateEncryptor`を作成するために使用するメソッド、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

さらに、`IKey`公開、`CreateEncryptorInstance`を作成するために使用するメソッド、 [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

---

> [!NOTE]
> 未加工の暗号化マテリアルを取得する API はありません、`IKey`インスタンス。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager`インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。 3 つの高度な操作を公開します。

* 新しいキーを作成し、記憶域に保存します。

* 記憶域からすべてのキーを取得します。

* 1 つまたは複数のキーを取り消すし、記憶域に失効情報の保持されます。

>[!WARNING]
> 書き込み、`IKeyManager`は非常に高度なタスクの場合であり、開発者の大部分が試行しないでください。 代わりに、ほとんどの開発者を活用してによって提供される機能、 [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)クラスです。

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager`型は、インボックスの具象`IKeyManager`です。 キー エスクロー、残りの部分でのキーの暗号化など、いくつかに役立つ機能を提供します。 このシステム内のキーは、XML 要素として表現されます (具体的には、 [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。

`XmlKeyManager` そのタスクを満たす操作の過程でその他のいくつかのコンポーネントに依存します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`、新しいキーで使用されるアルゴリズムを決定します。

* `IXmlRepository`、どのコントロールのキーがストレージに保存されます。

* `IXmlEncryptor` [オプション]、静止したキーを暗号化できるようにします。

* `IKeyEscrowSink` [オプション] をキー エスクロー サービスを提供します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`、どのコントロールのキーがストレージに保存されます。

* `IXmlEncryptor` [オプション]、静止したキーを暗号化できるようにします。

* `IKeyEscrowSink` [オプション] をキー エスクロー サービスを提供します。

---

内でこれらのコンポーネントを一緒にワイヤードは方法を示す高度な図を次に示します`XmlKeyManager`です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![キーの作成](key-management/_static/keycreation2.png)

   *Key Creation / CreateNewKey*

実装で`CreateNewKey`、`AlgorithmConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`、し、XML としてシリアル化されます。 キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。 暗号化されていない XML を実行し、 `IXmlEncryptor` (必要な場合) を暗号化された XML ドキュメントを生成します。 この暗号化されたドキュメントが経由での長期的なストレージに保存され、`IXmlRepository`です。 (ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントで永続化されて、 `IXmlRepository`)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![キーの作成](key-management/_static/keycreation1.png)

   *Key Creation / CreateNewKey*

実装で`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`コンポーネントの作成に使用する一意`IAuthenticatedEncryptorDescriptor`、し、XML としてシリアル化されます。 キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。 暗号化されていない XML を実行し、 `IXmlEncryptor` (必要な場合) を暗号化された XML ドキュメントを生成します。 この暗号化されたドキュメントが経由での長期的なストレージに保存され、`IXmlRepository`です。 (ない場合は`IXmlEncryptor`が構成されている場合、暗号化されていないドキュメントで永続化されて、 `IXmlRepository`)。

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![キーの取得](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![キーの取得](key-management/_static/keyretrieval1.png)

---

   *キーの取得/GetAllKeys*

実装で`GetAllKeys`、XML ドキュメントを表すキーおよび取り消した場合は、基になるから読み取ら`IXmlRepository`です。 これらのドキュメントが暗号化されている場合、システムは自動的に復号化にします。 `XmlKeyManager` 適切な作成`IAuthenticatedEncryptorDescriptorDeserializer`にドキュメントを逆シリアル化するインスタンスを戻します`IAuthenticatedEncryptorDescriptor`インスタンスで、個々 にラップし、`IKey`インスタンス。 このコレクションの`IKey`インスタンスが、呼び出し元に返されます。

特定の XML 要素の詳細についてで参照できる、[キーの格納形式のドキュメント](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)です。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository`インターフェイスは、XML を永続化したり、バッキング ストアから XML を取得できる型を表します。 2 つの Api を公開します。

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (XElement 要素、文字列の friendlyName)

実装`IXmlRepository`それらに渡される XML を解析する必要はありません。 上位のレイヤーを生成して、ドキュメントの解析について心配を非透過的に XML ドキュメントを扱うさせている必要があります。

実装する 2 つの組み込み具象型がある`IXmlRepository`:`FileSystemXmlRepository`と`RegistryXmlRepository`です。 参照してください、[キー記憶域プロバイダーのドキュメント](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers)詳細についてはします。 カスタムの登録`IXmlRepository`Azure Blob ストレージなど、各種のバッキング ストアを使用する適切な方法になります。

アプリケーション全体の既定のリポジトリを変更するにはカスタム コントロールの登録`IXmlRepository`インスタンス。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor`インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。 単一の API を公開します。

* (XElement plaintextElement) の暗号化: EncryptedXmlInfo

場合は、シリアル化された`IAuthenticatedEncryptorDescriptor`し、「暗号化を必要とする」としてマークされているすべての要素を含む`XmlKeyManager`を通じて、構成されたこれらの要素を実行`IXmlEncryptor`の`Encrypt`メソッド、およびそれに enciphered 要素を保持ではなく、プレーン テキスト要素を`IXmlRepository`です。 出力、`Encrypt`メソッドは、`EncryptedXmlInfo`オブジェクト。 このオブジェクトは enciphered、両方の結果を含むラッパー`XElement`と型を表す、`IXmlDecryptor`の対応する要素を復号化に使用できます。

実装している 4 つの組み込み具象型がある`IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

参照してください、 [rest ドキュメント キーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)詳細についてはします。

既定のキーの暗号化に rest メカニズム アプリケーション全体を変更するにはカスタム コントロールの登録`IXmlEncryptor`インスタンス。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor`インターフェイスが暗号化を解除する方法を認識する型を表す、`XElement`を介して enciphered ですが、`IXmlEncryptor`です。 単一の API を公開します。

* (XElement encryptedElement) を暗号化解除: XElement

`Decrypt`メソッドによって実行された暗号化を元に戻します`IXmlEncryptor.Encrypt`です。 各具象型では一般に、`IXmlEncryptor`実装は、対応する具象型を持つは`IXmlDecryptor`実装します。

実装する型`IXmlDecryptor`次の 2 つのパブリック コンス トラクターのいずれかである必要があります。

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`に渡されるコンス トラクターを null にすることがあります。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink`インターフェイスは、機密情報のエスクローを実行できる型を表します。 導入にこれは、シリアル化された記述子は、(暗号化マテリアル) などの機密情報を含む可能性がありますに注意してください、 [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)最初の場所を入力します。 ただし、事故が発生する、キーのリングが削除できるか、または壊れています。

エスクロー インターフェイスには、構成されているいずれかが変換される前に、生のシリアル化された XML へのアクセスを許可、緊急脱出口が用意されています[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)です。 インターフェイスは、単一の API を公開します。

* ストア (Guid keyId、XElement 要素)

`IKeyEscrowSink`ビジネス ポリシーと一致するセキュリティで保護された方法では、指定した要素の処理を実装します。 ここでは、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの可能な実装がある可能性があります。`CertificateXmlEncryptor`型がこれを支援できます。 `IKeyEscrowSink`実装は、指定した要素を適切に保持する役割もします。

既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](xref:security/data-protection/configuration/machine-wide-policy)です。 構成することもプログラムを介して、`IDataProtectionBuilder.AddKeyEscrowSink`メソッド以下のサンプルで示すようにします。 `AddKeyEscrowSink`メソッドのオーバー ロードのミラー、`IServiceCollection.AddSingleton`と`IServiceCollection.AddInstance`、オーバー ロードとして`IKeyEscrowSink`インスタンスはシングルトンとして意図されています。 複数`IKeyEscrowSink`インスタンスを登録すると、キーを同時に複数のメカニズムをエスクローできるようにそのそれぞれがキーの生成中に呼び出されます。

API から情報を読み取るにはありません、`IKeyEscrowSink`インスタンス。 これはエスクロー機構の設計理論と一致: キー マテリアルを信頼された機関にアクセスできるようにするためのものが、および独自 escrowed マテリアルへのアクセスを含めることはできませんので、アプリケーション自体がない信頼された証明機関でします。

次のサンプル コードは、作成、および登録を示しています、 `IKeyEscrowSink` "CONTOSODomain Admins"のメンバーだけが回復できるようにキーはエスクロー場所。

> [!NOTE]
> このサンプルを実行するドメインに参加している Windows 8 で Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 以降にする必要があります。

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
