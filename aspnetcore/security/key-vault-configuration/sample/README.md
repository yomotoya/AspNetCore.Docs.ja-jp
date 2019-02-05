# <a name="key-vault-configuration-provider-sample-app"></a>Key Vault 構成プロバイダーのサンプル アプリ

このサンプルでは、Azure Key Vault 構成プロバイダーの使用を示します。

サンプルの実行によって 2 つのモードのいずれかで、`#define`の上部にあるステートメント、 *Program.cs*ファイル。 手順については、次を参照してください[サンプル コードでプリプロセッサ ディレクティブ](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):。

* `Basic` &ndash; Azure Key Vault に格納されているアクセス シークレットを Azure Key Vault のクライアント ID とシークレットの使用を示します。 このバージョンのサンプルは、Azure App Service または ASP.NET Core アプリのサービスを提供できる任意のホストに展開されている任意の場所から実行できます。
* `Managed` &ndash; Azure の使用方法を示します[管理対象サービス Id](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)アプリのコードまたは構成で資格情報なしの Azure AD 認証を使用した Azure Key Vault にアプリを認証します。 Azure AD のクライアント ID とシークレットは、Azure Key Vault に対する認証をアプリの必要ありません。 このサンプルは、管理対象 Id scearnio を探索する Azure App Service にデプロイする必要があります。

詳細については、次を参照してください。 [Azure Key Vault 構成プロバイダー](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)します。
