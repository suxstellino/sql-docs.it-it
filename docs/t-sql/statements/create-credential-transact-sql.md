---
description: CREATE CREDENTIAL (Transact-SQL)
title: CREATE CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREDENTIAL_TSQL
- SQL13.SWB.CREDENTIAL.GENERAL.F1
- CREATE CREDENTIAL
- CREATE_CREDENTIAL_TSQL
- CREDENTIAL
dev_langs:
- TSQL
helpviewer_keywords:
- SECRET clause
- authentication [SQL Server], credentials
- CREATE CREDENTIAL statement
- credentials [SQL Server], CREATE CREDENTIAL statement
ms.assetid: d5e9ae69-41d9-4e46-b13d-404b88a32d9d
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 14230828089399d943eb4332e7c456c6ad68957f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192764"
---
# <a name="create-credential-transact-sql"></a>CREATE CREDENTIAL (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Crea una credenziale a livello di server. Una credenziale è un record contenente le informazioni di autenticazione necessarie per connettersi a una risorsa all'esterno di SQL Server. La maggior parte delle credenziali include un utente e una password di Windows. Quando si salva il backup di un database in un determinato percorso, ad esempio, può essere necessario specificare credenziali speciali per accedere a tale percorso. Per altre informazioni, vedere [Credenziali (motore di database)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

> [!NOTE]
> Per creare credenziali a livello di database, usare [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). Usare una credenziale a livello di server quando è necessario usare le stesse credenziali per più database nel server. Usare le credenziali con ambito database per rendere portabile il database. Quando un database viene spostato in un nuovo server, vengono spostate anche le credenziali con ambito database. Usare le credenziali con ambito database nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]
        [ FOR CRYPTOGRAPHIC PROVIDER cryptographic_provider_name ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*credential_name* specifica il nome delle credenziali create. *credential_name* non può iniziare con il simbolo del cancelletto (#). perché tale simbolo viene utilizzato per le credenziali di sistema. 

> [!IMPORTANT]
> Quando si usa una firma di accesso condiviso (SAS), questo nome deve corrispondere al percorso del contenitore, iniziare con https e non deve contenere una barra. Vedere l'[esempio D](#d-creating-a-credential-using-a-sas-token).

IDENTITY **='** _identity\_name_ **'** specifica il nome dell'account da usare per la connessione all'esterno del server. Quando la credenziale viene usata per accedere a Azure Key Vault, **IDENTITY** è il nome dell'insieme di credenziali delle chiavi. Vedere l'esempio C riportato di seguito. Quando le credenziali usano una firma di accesso condiviso (SAS), il valore di **IDENTITY** è *SHARED ACCESS SIGNATURE*. Vedere l'esempio D riportato di seguito.

> [!IMPORTANT]
> Database SQL di Azure supporta solo le identità Azure Key Vault e di firma di accesso condiviso. Le identità utente di Windows non sono supportate.

SECRET **='** _secret_ **'** specifica il segreto richiesto per l'autenticazione in uscita.

Quando vengono usate le credenziali per accedere ad Azure Key Vault, l'argomento **SECRET** di **CREATE CREDENTIAL** richiede che l' *\<Client ID>* (senza trattini) e il valore *\<Secret>* di un'**entità servizio** in Azure Active Directory vengano passati insieme senza essere separati da uno spazio. Vedere l'esempio C riportato di seguito. Quando le credenziali usano una firma di accesso condiviso, il valore di **SECRET** è il token della firma di accesso condiviso. Vedere l'esempio D riportato di seguito. Per informazioni sulla creazione di criteri di accesso archiviati e una firma di accesso condiviso in un contenitore di Azure, vedere [Lezione 1: Creare criteri di accesso archiviati e una firma di accesso condiviso in un contenitore di Azure](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#1---create-stored-access-policy-and-shared-access-storage).

FOR CRYPTOGRAPHIC PROVIDER *cryptographic_provider_name* specifica il nome di un *provider EKM (Enterprise Key Management provider)* . Per altre informazioni sulla gestione delle chiavi, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Osservazioni

Se IDENTITY è un utente di Windows, il segreto può essere la password. Il segreto viene crittografato con la chiave master del servizio. Se la chiave master del servizio viene rigenerata, il segreto viene ricrittografato con la nuova chiave master del servizio.

Dopo aver creato una credenziale, è possibile eseguirne il mapping a un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) o [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md). È possibile eseguire il mapping di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una sola credenziale, mentre è possibile eseguire il mapping di una credenziale a più account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md). È possibile eseguire il mapping di una credenziale solo a livello di server, non a un utente del database. 

Le informazioni sulle credenziali sono visibili nella vista del catalogo [sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md).

Se non sono presenti credenziali su cui viene eseguito il mapping a un account accesso per il provider, vengono utilizzate le credenziali sui cui viene eseguito il mapping all'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

A un account di accesso è possibile eseguire il mapping di più credenziali, a condizione che vengano utilizzate con provider distinti. È possibile eseguire il mapping di una sola credenziale per provider per ogni account di accesso. Sulla stessa credenziale è possibile eseguire il mapping ad altri account di accesso.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione **ALTER ANY CREDENTIAL**.

## <a name="examples"></a>Esempi

### <a name="a-creating-a-credential-for-windows-identity"></a>R. Creazione di una credenziale per l'identità Windows

Nell'esempio seguente viene creata la credenziale denominata `AlterEgo`. Tale credenziale contiene l'utente di Windows `Mary5` e una password.

```sql
CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-credential-for-ekm"></a>B. Creazione di una credenziale per EKM

Nell'esempio seguente viene usato un account creato in precedenza e denominato `User1OnEKM` in un modulo EKM tramite gli strumenti di gestione di EKM, con un tipo di account di base e una password. L'account **sysadmin** nel server crea una credenziale che viene usata per connettersi all'account EKM e la assegna all'account `User1` di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```sql
CREATE CREDENTIAL CredentialForEKM
    WITH IDENTITY='User1OnEKM', SECRET='<EnterStrongPasswordHere>'
    FOR CRYPTOGRAPHIC PROVIDER MyEKMProvider;
GO

/* Modify the login to assign the cryptographic provider credential */
ALTER LOGIN User1
ADD CREDENTIAL CredentialForEKM;
```

### <a name="c-creating-a-credential-for-ekm-using-the-azure-key-vault"></a>C. Creazione di una credenziale per EKM con l'insieme di credenziali chiave di Azure

Nell'esempio seguente viene creata una credenziale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzabile dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] per l'accesso ad Azure Key Vault mediante il **Connettore SQL Server per Microsoft Azure Key Vault**. Per un esempio completo dell'uso del connettore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Extensible Key Management con Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).

> [!IMPORTANT]
> L'argomento **IDENTITY** di **CREATE CREDENTIAL** richiede il nome dell'insieme di credenziali delle chiavi. L'argomento **SECRET** di **CREATE CREDENTIAL** richiede che i valori di *\<Client ID>* (senza trattini) e *\<Secret>* siano passati insieme senza spazi aggiuntivi.

 Nell'esempio seguente l' **ID client** (`EF5C8E09-4D2A-4A76-9998-D93440D8115D`) viene immesso con tutti i trattini rimossi come stringa `EF5C8E094D2A4A769998D93440D8115D` e il **Segreto** è rappresentato dalla stringa *SECRET_DBEngine*.

```sql
USE master;
CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault',
    SECRET = 'EF5C8E094D2A4A769998D93440D8115DSECRET_DBEngine'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;
```

Nell'esempio seguente viene creata la stessa credenziale usando le variabili per le stringhe **ID client** e **Secret**, che vengono quindi concatenate insieme per formare l'argomento **SECRET**. La funzione **REPLACE** viene usata per rimuovere i trattini dall'ID client.

```sql
DECLARE @AuthClientId uniqueidentifier = 'EF5C8E09-4D2A-4A76-9998-D93440D8115D';
DECLARE @AuthClientSecret varchar(200) = 'SECRET_DBEngine';
DECLARE @pwd varchar(max) = REPLACE(CONVERT(varchar(36), @AuthClientId) , '-', '') + @AuthClientSecret;

EXEC ('CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault', SECRET = ''' + @PWD + '''
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;');
```

### <a name="d-creating-a-credential-using-a-sas-token"></a>D. Creazione di una credenziale mediante un token di firma di accesso condiviso

**Si applica a**: da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level) e a Istanza gestita di SQL di Azure.

Nell'esempio seguente viene creata una credenziale di firma di accesso condiviso usando un token di firma di accesso condiviso. Per un'esercitazione sulla creazione di criteri di accesso archiviati e di una firma di accesso condiviso in un contenitore di Azure e sulla creazione, in un secondo momento, di una credenziale usando la firma di accesso condiviso, vedere [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).

> [!IMPORTANT]
> L'argomento **CREDENTIAL NAME** richiede che il nome corrisponda al percorso del contenitore, inizi con https e non contenga una barra finale. L'argomento **IDENTITY** richiede il nome, *SHARED ACCESS SIGNATURE*. L'argomento **SECRET** richiede il token della firma di accesso condiviso.
>
> L'argomento SECRET di **SHARED ACCESS SIGNATURE** non deve essere preceduto da **?** .

```sql
USE master
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] -- this name must match the container path, start with https and must not contain a trailing forward slash.
    WITH IDENTITY='SHARED ACCESS SIGNATURE' -- this is a mandatory string and do not change it.
    , SECRET = 'sharedaccesssignature' -- this is the shared access signature token
GO
```

### <a name="e-creating-a-credential-for-managed-identity"></a>E. Creazione di una credenziale per l'identità gestita

Nell'esempio seguente vengono create le credenziali che rappresentano l'identità gestita del servizio SQL di Azure o Azure Synapse. La password e il segreto non sono applicabili in questo caso.

```sql
CREATE CREDENTIAL ServiceIdentity WITH IDENTITY = 'Managed Identity';
GO
```

## <a name="see-also"></a>Vedere anche

- [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-credential-transact-sql.md)
- [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)
- [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
- [Lezione 2: Creare credenziali di SQL Server usando una firma di accesso condiviso](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature)
- [Uso delle firme di accesso condiviso](/azure/storage/common/storage-sas-overview)