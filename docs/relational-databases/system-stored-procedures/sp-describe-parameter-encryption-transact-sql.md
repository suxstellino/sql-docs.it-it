---
description: sp_describe_parameter_encryption (Transact-SQL)
title: sp_describe_parameter_encryption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_parameter_encryption
- sp_describe_parameter_encryption_TSQL
- sys.sp_describe_parameter_encryption
- sys.sp_describe_parameter_encryption_TSQL
helpviewer_keywords:
- sp_describe_parameter_encryption
ms.assetid: 706ed441-2881-4934-8d5e-fb357ee067ce
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41821edc34835f60cef201ae78b3448f3d3f4699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200777"
---
# <a name="sp_describe_parameter_encryption-transact-sql"></a>sp_describe_parameter_encryption (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Analizza l'istruzione specificata [!INCLUDE[tsql](../../includes/tsql-md.md)] e i relativi parametri, per determinare quali parametri corrispondono alle colonne del database protette tramite la funzionalità Always Encrypted. Restituisce i metadati di crittografia per i parametri che corrispondono alle colonne crittografate.  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_describe_parameter_encryption   
    [ @tsql = ] N'Transact-SQL_batch' ,   
    [ @params = ] N'parameters'   
[ ;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ \@ TSQL =]' Transact-SQL_batch '  
 Una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Transact-SQL_batch può essere di tipo nvarchar (n) o nvarchar (max).  
  
 [ \@ params =] N'parameters '  
 *\@ params* fornisce una stringa di dichiarazione per i parametri del batch Transact-SQL, che è simile a sp_executesql. I parametri possono essere di tipo nvarchar (n) o nvarchar (max).  
  
 È una stringa che contiene le definizioni di tutti i parametri incorporati nella [!INCLUDE[tsql](../../includes/tsql-md.md)] _batch. La stringa deve essere una costante o una variabile Unicode. Ogni definizione di parametro è costituita da un nome del parametro e da un tipo di dati. *n* è un segnaposto che indica definizioni di parametro aggiuntive. Ogni parametro specificato nell'istruzione deve essere definito in *\@ params*. Se l' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione o il batch nell'istruzione non contiene parametri, *\@ params* non è necessario. Il valore predefinito per questo parametro è NULL.  
  
## <a name="return-value"></a>Valore restituito  
 0 indica esito positivo. Qualsiasi altra indicazione di errore.  
  
## <a name="result-sets"></a>Set di risultati  
 **sp_describe_parameter_encryption** restituisce due set di risultati:  
  
-   Il set di risultati che descrive le chiavi crittografiche configurate per le colonne del database, i parametri dell' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione specificata corrispondono a.  
  
-   Il set di risultati che descrive il modo in cui i parametri specifici devono essere crittografati. Questo set di risultati fa riferimento alle chiavi descritte nel primo set di risultati.  
  
 Ogni riga del primo set di risultati descrive una coppia di chiavi; una chiave di crittografia della colonna crittografata e la chiave master della colonna corrispondente.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**column_encryption_key_ordinal**|**int**|ID della riga nel set di risultati.|  
|**database_id**|**int**|ID database.|  
|**column_encryption_key_id**|**int**|ID della chiave di crittografia della colonna. Nota: questo ID indica una riga nel sys.column_encryption_keys &#40;vista del catalogo [&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) .|  
|**column_encryption_key_version**|**int**|Riservato per utilizzi futuri. Attualmente, contiene sempre 1.|  
|**column_encryption_key_metadata_version**|**binario (8)**|Timestamp che rappresenta l'ora di creazione della chiave di crittografia della colonna.|  
|**column_encryption_key_encrypted_value**|**varbinary(4000)**|Valore crittografato della chiave di crittografia della colonna.|  
|**column_master_key_store_provider_name**|**sysname**|Nome del provider per l'archivio delle chiavi che contiene la chiave master della colonna usata per produrre il valore crittografato della chiave di crittografia della colonna.|  
|**column_master_key_path**|**nvarchar(4000)**|Percorso della chiave master della colonna usato per produrre il valore crittografato della chiave di crittografia della colonna.|  
|**column_encryption_key_encryption_algorithm_name**|**sysname**|Nome dell'algoritmo di crittografia utilizzato per generare il valore di crittografia della chiave di crittografia della colonna.|  
  
 Ogni riga del secondo set di risultati contiene i metadati di crittografia per un parametro.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int**|ID della riga nel set di risultati.|  
|**parameter_name**|**sysname**|Nome di uno dei parametri specificati nell'argomento *\@ params* .|  
|**column_encryption_algorithm**|**tinyint**|Codice che indica l'algoritmo di crittografia configurato per la colonna, il parametro corrisponde a. I valori attualmente supportati sono: 2 per **AEAD_AES_256_CBC_HMAC_SHA_256**.|  
|**column_encryption_type**|**tinyint**|Codice che indica il tipo di crittografia configurato per la colonna, il parametro corrisponde a. I valori supportati sono:<br /><br /> 0-testo non crittografato (la colonna non è crittografata)<br /><br /> 1-crittografia casuale<br /><br /> 2-crittografia deterministica.|  
|**column_encryption_key_ordinal**|**int**|Codice della riga nel primo set di risultati. La riga a cui viene fatto riferimento descrive la chiave di crittografia della colonna configurata per la colonna, il parametro corrisponde a.|  
|**column_encryption_normalization_rule_version**|**tinyint**|Numero di versione dell'algoritmo di normalizzazione dei tipi.|  
  
## <a name="remarks"></a>Commenti  
 Un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver client, che supporta always Encrypted, chiama automaticamente **sp_describe_parameter_encryption** per recuperare i metadati di crittografia per le query con parametri rilasciate dall'applicazione. Successivamente, il driver utilizza i metadati di crittografia per crittografare i valori dei parametri che corrispondono alle colonne di database protette con Always Encrypted e sostituisce i valori di parametro del testo non crittografato, inviati dall'applicazione, con i valori del parametro crittografati, prima di inviare la query al motore di database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiedere le autorizzazioni **Visualizza definizione chiave di crittografia della colonna** e **Visualizza tutte le definizioni della chiave master della colonna** nel database.  
  
## <a name="examples"></a>Esempio  
  
```sql  
CREATE COLUMN MASTER KEY [CMK1]  
WITH  
(  
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',  
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'  
);  
GO  
  
CREATE COLUMN ENCRYPTION KEY [CEK1]  
WITH VALUES  
(  
       COLUMN_MASTER_KEY = [CMK1],  
    ALGORITHM = 'RSA_OAEP',  
    ENCRYPTED_VALUE =   
0x016E000001630075007200720065006E00740075007300650072002F006D007  
9002F00610036003600620062003000660036006400640037003000620064006  
6006600300032006200360032006400300066003800370065003300340030003  
200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF991  
37B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA51  
7A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6  
686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015B  
DB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8  
C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE3  
74DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A3472  
3276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550E  
C5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E0  
35175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D8  
01ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B  
4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF8  
1A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202F  
C24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B0195883360  
4707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E  
9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C  
3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085  
);  
GO  
  
CREATE TABLE t1 (  
c1 INT ENCRYPTED WITH (  
    COLUMN_ENCRYPTION_KEY = [CEK_Auto1],   
    ENCRYPTION_TYPE = Randomized,   
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL,  
);  
  
EXEC sp_describe_parameter_encryption N'INSERT INTO t1 VALUES(@c1)',  N'@c1 INT';  
```  
  
 Ecco il primo set di risultati:  
  
|column_encryption_key_ordinal|database_id|column_encryption_key_id|column_encryption_key_version|column_encryption_key_metadata_version|column_encryption_key_encrypted_value|  
|--------------------------------------|------------------|---------------------------------|--------------------------------------|------------------------------------------------|-----------------------------------------------|  
|1|5|1|1|0x99EDA60083A50000|0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98 AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085|  
  
 (I risultati continuano.)  
  
|column_master_key_store_provider_name|column_master_key_path|column_encryption_key_encryption_algorithm_name|  
|------------------------------------------------|-------------------------------|----------------------------------------------------------|  
|MSSQL_CERTIFICATE_STORE|CurrentUser/My/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305|RSA_OAEP|  
  
 Ecco il secondo set di risultati:  
  
|parameter_ordinal|parameter_name|column_encryption_algorithm|column_encryption_type|  
|------------------------|---------------------|-----------------------------------|------------------------------|  
|1|\@C1|1|1|  
  
 (I risultati continuano.)  
  
|column_encryption_key_ordinal|column_encryption_normalization_rule_version|  
|--------------------------------------|------------------------------------------------------|  
|1|1|  
  
## <a name="see-also"></a>Vedere anche  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Sviluppare un'applicazione con Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
  
