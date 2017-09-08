# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>サンプル アプリケーションのキー コンテナーの構成プロバイダーのプレフィックス (ASP.NET Core 1.x)

このサンプルは、ASP.NET Core の Azure キー資格情報コンテナーの構成プロバイダーの使用を示しています。 1.x キー名のプレフィックスを使用します。 ASP.NET Core 2.x サンプルでは、次を参照してください。[キー資格情報コンテナーの構成プロバイダーのプレフィックス サンプル アプリケーション (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)です。

> [!NOTE]
> 構成プロバイダーを ASP.NET Core 1.0 を使用できません。 構成プロバイダーを実装するし、アプリは、ASP.NET Core 1.0 アプリ、1.1 以降の最初にアプリをアップグレードします。

サンプルの動作の詳細については、次を参照してください。、 [Azure Key Vault の構成プロバイダー](xref:security/key-vault-configuration)トピックです。

## <a name="using-the-sample"></a>サンプルの使用
1. Key vault の作成し、ガイダンスに従って、アプリケーションの Azure Active Directory (Azure AD) を設定[Azure Key Vault の使用開始](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)です。
  * Azure PowerShell モジュール、Azure 管理 API または Azure ポータルを使用して key vault にシークレットを追加します。 機密情報は、いずれかとして作成*手動*または*証明書*シークレット。 *証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってサポートされていません。 使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアの機密情報を作成するオプションです。
    * 階層型の値 (構成セクション) を使用して`--`(2 つのハイフン)、区切り記号として。
    * サンプル アプリの 2 つ作成*手動*次の名前/値ペアのシークレット。
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Azure Active Directory とサンプル アプリを登録します。
  * Key vault にアクセスするアプリを承認します。 使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認するために PowerShell コマンドレットを提供`List`と`Get`で機密データへのアクセス`-PermissionsToKeys list,get`です。
2. アプリの更新*される appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`です。
3. その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`プレフィックス付きのシークレットの名前と同じ名前にします。 このサンプルではプレフィックスは、アプリのバージョンは、指定した、 `PrefixKeyVaultSecretManager` Azure Key Vault の構成プロバイダーを追加するとき。 値は、`AppSecret`で取得した`config["AppSecret"]`です。
4. プロジェクト ファイルでのアプリ アセンブリのバージョンを変更`5.0.0.0`に`5.1.0.0`アプリをもう一度実行します。 このとき、返される秘密の値は`5.1.0.0_secret_value`します。
