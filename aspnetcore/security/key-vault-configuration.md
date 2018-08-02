---
title: ASP.NET Core での azure Key Vault 構成プロバイダー
author: guardrex
description: Azure Key Vault 構成プロバイダーを使用して、実行時に読み込まれる名前と値のペアを使用してアプリを構成する方法について説明します。
ms.author: riande
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 6474b9f5cb9e441854565a7891c4aac7f781c810
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410131"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core での azure Key Vault 構成プロバイダー

作成者 [Luke Latham](https://github.com/guardrex)および [Andrew Stanton-Nurse](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

2.x のサンプル コードを表示またはダウンロードします。

* [基本的なサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))- 秘密の値をアプリに読み込みます。
* [キー名のプレフィックス サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) - アプリのバージョン毎に異なるシークレット値のセットを読み込むことができるように、アプリのバージョンを表すキー名のプレフィックスを使用して、シークレット値を読み取ります。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

1.x のサンプル コードを表示またはダウンロードします。

* [基本的なサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))- 秘密の値をアプリに読み込みます。
* [キー名のプレフィックス サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) - アプリのバージョン毎に異なるシークレット値のセットを読み込むことができるように、アプリのバージョンを表すキー名のプレフィックスを使用して、シークレット値を読み取ります。

---

このドキュメントは、使用する方法を説明します、 [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)構成プロバイダーを Azure Key Vault シークレットからアプリの構成値を読み込めません。 Azure Key Vault とは、暗号化キーとアプリとサービスで使用されるシークレットを保護するのに役立つクラウド ベース サービスです。 一般的なシナリオは、機密性の高い構成データにアクセスを制御して、FIPS 140-2 の要件を満たすレベル 2 検証済みハードウェア セキュリティ モジュール (HSM) の構成データを格納する場合。 この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。

## <a name="package"></a>Package

プロバイダーを使用するへの参照を追加、 [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)パッケージ。

## <a name="app-configuration"></a>アプリの構成

使用してプロバイダーを調べることができます、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)します。 Key vault を確立し、資格情報コンテナーにシークレットを作成した後サンプル アプリは安全にシークレットの値をその構成に読み込むし、それらを web ページに表示します。

アプリの構成に、プロバイダーを追加、`AddAzureKeyVault`拡張機能。 サンプル アプリで、拡張機能はから読み込まれた 3 つの構成値を使用して、 *appsettings.json*ファイル。

| アプリ設定    | 説明                    | 例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure Key Vault 名           | contosovault                                 |
| `ClientId`     | Azure Active Directory アプリ Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory アプリ キー | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Key vault のシークレットを作成して構成値 (basic サンプル) を読み込む

1. Key vault を作成しにあるガイダンスに従って、アプリの Azure Active Directory (Azure AD) を設定[Azure Key Vault の概要](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)します。
   * 使用して、キー コンテナーにシークレットを追加、[キー コンテナーの AzureRM PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure Portal](https://portal.azure.com/)します。 シークレットは、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってはサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアのシークレットを作成するオプション。
     * 単純なシークレットは、名前と値のペアとして作成されます。 Azure Key Vault シークレットの名前は、英数字とダッシュに制限されます。
     * 階層型の値 (構成セクション) を使用して、 `--` (ダッシュを 2 つ)、サンプルでは、区切り記号として。 内のサブキーのセクションを区切るために通常使用される、コロン[ASP.NET Core 構成](xref:fundamentals/configuration/index)、シークレットの名前では許可されません。 そのため、2 つのダッシュが使用され、シークレットは、アプリの構成に読み込まれるときに、コロンのスワップします。
     * 2 つ作成*手動*のシークレットを次の名前と値のペアでします。 最初のシークレットは、単純な名前と値、し、2 つ目のシークレットは、セクションとシークレットの名前のサブキーを使用してシークレット値を作成します。
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Azure Active Directory でサンプル アプリを登録します。
   * キー コンテナーにアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認する PowerShell コマンドレットを提供`List`と`Get`でシークレットへのアクセス`-PermissionsToSecrets list,get`します。

2. アプリの更新*appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`します。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`シークレットの名前と同じ名前でします。
   * 非階層型の値: の値は、`SecretName`では、行わ`config["SecretName"]`します。
   * 階層型の値 (セクション): 使用`:`(コロン) 表記または`GetSection`拡張メソッド。 構成値を取得するのにには、これらの方法のいずれかを使用します。
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

アプリを実行すると、web ページには、読み込まれた、シークレットの値が表示されます。

![Azure Key Vault 構成プロバイダー経由で読み込まれるシークレットの値を表示するブラウザー ウィンドウ](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>プレフィックス付きの key vault のシークレットを作成して構成値 (キーの名前のプレフィックス-サンプル) を読み込む

`AddAzureKeyVault` 実装を受け取るオーバー ロードを提供も`IKeyVaultSecretManager`、構成キーに変換されます主要 vault のシークレットを制御できます。 たとえば、アプリの起動時に指定したプレフィックス値に基づくシークレットの値を読み込むインターフェイスを実装できます。 これにより、たとえば、アプリのバージョンに基づくシークレットを読み込めませんできます。

> [!WARNING]
> 同じ key vault にシークレットを複数のアプリを配置する環境のシークレットを配置するか、key vault のシークレットでプレフィックスを使用しないでください (たとえば、*開発*と*運用*シークレット) を同じ資格情報コンテナー。 別のアプリと開発/運用環境で最高レベルのセキュリティのアプリの環境を分離する個別の key vault を使用することをお勧めします。

Key vault にシークレットを作成する 2 つ目のサンプル アプリを使用して、 `5000-AppSecret` (key vault のシークレット名で許可されていない期間)、アプリのバージョン 5.0.0.0 用アプリのシークレットを表します。 シークレットを作成する別のバージョン、5.1.0.0、`5100-AppSecret`します。 各アプリのバージョンでは、としては、その構成に独自のシークレット値を読み込みます`AppSecret`シークレットが読み込まれるバージョンを削除します。 サンプルの実装は、以下に示します。

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`メソッドが付いているバージョンのプレフィックスを検索する資格情報コンテナーのシークレットを反復処理するプロバイダーのアルゴリズムによって呼び出されます。 バージョンのプレフィックスがで検出されたときに`Load`、アルゴリズムを使用して、`GetKey`シークレット名の構成名を返すメソッド。 シークレットの名前からのバージョンのプレフィックスを削除し、アプリの構成に名前/値ペアの読み込みのシークレット名の残りの部分を返します。

このアプローチを実装する場合。

1. Key vault のシークレットが読み込まれます。
2. 文字列のシークレットを`5000-AppSecret`が一致します。
3. バージョン、 `5000` (dash)、使用が終了、キー名から取り除かれます`AppSecret`シークレットの値を持つアプリの構成を読み込めません。

> [!NOTE]
> 独自に提供することも`KeyVaultClient`実装`AddAzureKeyVault`します。 カスタムのクライアントを指定してクライアントの構成プロバイダーとアプリの他の部分の 1 つのインスタンスを共有することができます。

1. Key vault を作成しにあるガイダンスに従って、アプリの Azure Active Directory (Azure AD) を設定[Azure Key Vault の概要](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)します。
   * 使用して、キー コンテナーにシークレットを追加、[キー コンテナーの AzureRM PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure Portal](https://portal.azure.com/)します。 シークレットは、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってはサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアのシークレットを作成するオプション。
     * 階層型の値 (構成セクション) を使用して、 `--` (ダッシュを 2 つ) を区切り記号として。
     * 2 つ作成*手動*のシークレットを次の名前と値のペアで。
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Azure Active Directory でサンプル アプリを登録します。
   * キー コンテナーにアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認する PowerShell コマンドレットを提供`List`と`Get`でシークレットへのアクセス`-PermissionsToSecrets list,get`します。

2. アプリの更新*appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`します。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`プレフィックス付きのシークレットの名前と同じ名前でします。 このサンプルでは、プレフィックスは、アプリのバージョンは、指定した、 `PrefixKeyVaultSecretManager` Azure Key Vault 構成プロバイダーが追加されたとき。 値は、`AppSecret`では、行わ`config["AppSecret"]`します。 アプリによって生成される web ページには、読み込まれた値が表示されます。

   ![アプリのバージョンは 5.0.0.0 場合に、Azure Key Vault 構成プロバイダーを使用してシークレット値を表示するブラウザー ウィンドウが読み込まれます](key-vault-configuration/_static/sample2-1.png)

4. プロジェクト ファイルからのアプリのアセンブリのバージョンを変更`5.0.0.0`に`5.1.0.0`し、再度アプリを実行します。 今回は、返されるシークレットの値は`5.1.0.0_secret_value`します。 アプリによって生成される web ページには、読み込まれた値が表示されます。

   ![アプリのバージョンは 5.1.0.0 場合に、Azure Key Vault 構成プロバイダーを使用してシークレット値を表示するブラウザー ウィンドウが読み込まれます](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>ClientSecret へのアクセスを制御します。

使用して、 [Secret Manager ツール](xref:security/app-secrets)維持するために、`ClientSecret`プロジェクト ソース ツリーの外部でします。 シークレット マネージャーを使用したアプリ シークレットを特定のプロジェクトに関連付けるし、複数のプロジェクトで共有できます。

証明書をサポートする環境での .NET Framework アプリを開発する場合は、X.509 証明書で、Azure Key Vault に認証できます。 X.509 証明書の秘密キーは、OS によって管理されます。 詳細については、次を参照してください。[クライアント シークレットの代わりに証明書による認証](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)します。 使用して、`AddAzureKeyVault`を受け入れるオーバー ロードを`X509Certificate2`(`_env`次の例。

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reloading-secrets"></a>シークレットを再読み込み

シークレットはまでキャッシュ`IConfigurationRoot.Reload()`が呼び出されます。 期限切れ、無効にし、まで、アプリで、key vault にシークレットが更新されたが守られていない`Reload`を実行します。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>無効になっており、期限切れのシークレット

無効になっており、期限切れのシークレットのスロー、`KeyVaultClientException`します。 アプリがスローされることを防ぐために、アプリの代わりにまたは、無効/有効期限切れのシークレットを更新します。

## <a name="troubleshooting"></a>トラブルシューティング

アプリは、プロバイダーを使用して構成の読み込みに失敗した場合、エラー メッセージが書き込む、 [ASP.NET のログ記録インフラストラクチャ](xref:fundamentals/logging/index)します。 次の条件は、読み込みを構成できないようにします。

* アプリは、Azure Active Directory で正しく構成されていません。
* Key vault は、Azure Key Vault に存在しません。
* アプリは、キー コンテナーにアクセスする権限はありません。
* アクセス ポリシーが含まれていない`Get`と`List`アクセス許可。
* Key vault で構成データ (名前と値のペア) が正しくという名前のないがない、有効期限が切れたか、無効にします。
* アプリが正しくない key vault の名前 (`Vault`)、Azure AD アプリ Id (`ClientId`)、または Azure AD のキー (`ClientSecret`)。
* Azure AD のキー (`ClientSecret`) 期限が切れています。
* 構成キー (名) は、ロードしようとしている値用のアプリで正しくないです。

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Key Vault のドキュメント](/azure/key-vault/)
* [Azure Key Vault のキーを生成し、HSM で保護された転送する方法](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient クラス](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
