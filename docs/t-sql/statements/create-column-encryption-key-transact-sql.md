---
description: CREATE COLUMN ENCRYPTION KEY (Transact-SQL)
title: CREATE COLUMN ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_COLUMN_ENCRYPTION_KEY_TSQL
- SQL13.SWB.NEWCOLUMNENCRYPTIONKEY.GENERAL.F1
- ENCRYPTION_KEY_TSQL
- CREATE COLUMN ENCRYPTION KEY
- ENCRYPTION KEY
- COLUMN ENCRYPTION KEY
- CREATE_COLUMN_ENCRYPTION_TSQL
- SQL13.SWB.COLUMNENCRYPTIONKEY.GENERAL.F1
- COLUMN_ENCRYPTION_KEY_TSQL
- CREATE COLUMN ENCRYPTION
dev_langs:
- TSQL
helpviewer_keywords:
- Always Encrypted, create column encryption key
- column encryption key, create
- column encryption key
- CREATE COLUMN ENCRYPTION KEY statement
ms.assetid: 517fe745-d79b-4aae-99a7-72be45ea6acb
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: ad44213480ea1a48e49c685952474b436491c65b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204929"
---
# <a name="create-column-encryption-key-transact-sql"></a>CREATE COLUMN ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Crea un oggetto metadati della chiave di crittografia della colonna per [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con enclavi sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Un oggetto metadati della chiave di crittografia della colonna contiene uno o due valori crittografati di una chiave di crittografia della colonna usata per crittografare i dati in una colonna. Ogni valore viene crittografato con una chiave master della colonna. 
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE COLUMN ENCRYPTION KEY key_name   
WITH VALUES  
  (  
    COLUMN_MASTER_KEY = column_master_key_name,   
    ALGORITHM = 'algorithm_name',   
    ENCRYPTED_VALUE = varbinary_literal  
  )   
[, (  
    COLUMN_MASTER_KEY = column_master_key_name,   
    ALGORITHM = 'algorithm_name',   
    ENCRYPTED_VALUE = varbinary_literal  
  ) ]   
[;]  
```  

## <a name="arguments"></a>Argomenti
_key\_name_  
Nome con il quale sarà nota la chiave di crittografia della colonna all'interno del database.  
  
_column\_master\_key\_name_ Specifica il nome della chiave master della colonna personalizzata usata per crittografare la chiave di crittografia della colonna.  
  
_algorithm\_name_  
Nome dell'algoritmo di crittografia usato per crittografare il valore della chiave di crittografia della colonna. L'algoritmo per i provider di sistema deve essere **RSA_OAEP**.  
  
_varbinary\_literal_  
BLOB del valore della chiave di crittografia della colonna crittografato.  
  
> [!WARNING]  
>  Non passare mai i valori della chiave di crittografia della colonna in testo non crittografato in questa istruzione, altrimenti sarà compromesso il vantaggio di questa funzionalità.  

## <a name="remarks"></a>Osservazioni
L'istruzione `CREATE COLUMN ENCRYPTION KEY` deve includere almeno uno o due valori. È possibile usare l'istruzione [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](alter-column-encryption-key-transact-sql.md) per aggiungere un secondo valore in un secondo momento. È inoltre possibile usare l'istruzione `ALTER COLUMN ENCRYPTION KEY` per rimuovere un valore.  
  
In genere, una chiave di crittografia della colonna viene creata con un solo valore crittografato. In alcuni casi, tuttavia, è necessario ruotare una chiave master della colonna per sostituire la chiave corrente con la nuova chiave master della colonna. In questi casi, aggiungere un nuovo valore della chiave di crittografia della colonna, crittografato con la nuova chiave master della colonna. Questa rotazione consente di garantire alle applicazioni client l'accesso ai dati crittografati con la chiave di crittografia della colonna, mentre la nuova chiave master della colonna sarà resa disponibile alle applicazioni client. Un driver abilitato per Always Encrypted in un'applicazione client che non ha accesso alla nuova chiave master potrà usare il valore della chiave di crittografia della colonna crittografato con la vecchia chiave master della colonna per accedere ai dati sensibili.  

  
Per gli algoritmi di crittografia supportati da Always Encrypted è necessario che il valore del testo non crittografato sia di 256 bit.  
  
Per gestire le chiavi di crittografia della colonna, si consiglia di usare strumenti quali SQL Server Management Studio (SSMS) o PowerShell. Questi strumenti generano valori crittografati ed emettono automaticamente istruzioni `CREATE COLUMN ENCRYPTION KEY` per creare un oggetto metadati della chiave di crittografia della colonna. Vedere [Effettuare il provisioning di chiavi Always Encrypted usando SQL Server Management Studio](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md) ed [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md). 

È possibile anche generare un valore chiave di crittografia della colonna a livello di codice usando un provider dell'archivio chiavi in grado di incapsulare l'archivio chiavi contenente la chiave master della colonna. Per altre informazioni, vedere [Sviluppare applicazioni usando Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md).
  
Usare [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md), [sys.column_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) e [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) per visualizzare le informazioni sulle chiavi di crittografia della colonna.  
  
## <a name="permissions"></a>Autorizzazioni  
È necessaria l'autorizzazione **ALTER ANY COLUMN ENCRYPTION KEY**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-column-encryption-key"></a>R. Creazione di una chiave di crittografia della colonna  
Nell'esempio seguente viene creata una chiave di crittografia della colonna denominata `MyCEK`.  
  
```sql  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
GO  
```  
  
### <a name="creating-a-column-encryption-key-with-two-values"></a>Creazione di una chiave di crittografia della colonna con due valori  
Nell'esempio seguente viene creata una chiave di crittografia della colonna denominata `TwoValueCEK` con due valori.  
  
```sql

CREATE COLUMN ENCRYPTION KEY TwoValueCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = CMK1,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E00740075007300650072002F006D0079002F0037006300380061003100310033003400320037003800620037003000630038003100390062003900630039003400360061006600340039006500610030003200650038006200650038003400340065006C33A82ECF04A7185824B4545457AC5244CD9C219E64067B9520C0081B8399B58C2863F7494ABE3694BD87D55FFD7576FFDC47C28F94ECC99577DF4FB8FA19AA95764FEF889CDE0F176DA5897B74382FBB22756CE2921050A09201A0EB6AF3D6091014C30146EA62635EE8CBF0A8074DEDFF125CEA80D1C0F5E8C58750A07D270E2A8BF824EE4C0C156366BF26D38CCE49EBDD5639A2DF029A7DBAE5A5D111F2F2FA3246DF8C2FA83C1E542C10570FADA98F6B29478DC58CE5CBDD407CCEFCDB97814525F6F32BECA266014AC346AC39C4F185C6C0F0A24FEC4DFA015649624692DE7865B9827BA22C3B574C9FD169F822B609F902288C5880EB25F14BD990D871B1BC4BA3A5B237AF76D26354773FA2A25CF4511AF58C911E601CFCB1905128C997844EED056C2AE7F0B48700AB41307E470FF9520997D0EB0D887DE11AFE574FFE845B7DC6C03FEEE8D467236368FC0CB2FDBD54DADC65B10B3DE6C80DF8B7B3F8F3CE5BE914713EE7B1FA5B7A578359592B8A5FDFDDE5FF9F392BC87C3CD02FBA94582AC063BBB9FFAC803FD489E16BEB28C4E3374A8478C737236A0B232F5A9DDE4D119573F1AEAE94B2192B81575AD6F57E670C1B2AB91045124DFDAEC2898F3F0112026DFC93BF9391D667D1AD7ED7D4E6BB119BBCEF1D1ADA589DD3E1082C3DAD13223BE438EB9574DA04E9D8A06320CAC6D3EC21D5D1C2A0AA484C7C  
),  
(  
    COLUMN_MASTER_KEY = CMK2,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E00740075007300650072002F006D0079002F0064006500650063006200660034006100340031003000380034006200350033003200360066003200630062006200350030003600380065003900620061003000320030003600610037003800310066001DDA6134C3B73A90D349C8905782DD819B428162CF5B051639BA46EC69A7C8C8F81591A92C395711493B25DCBCCC57836E5B9F17A0713E840721D098F3F8E023ABCDFE2F6D8CC4339FC8F88630ED9EBADA5CA8EEAFA84164C1095B12AE161EABC1DF778C07F07D413AF1ED900F578FC00894BEE705EAC60F4A5090BBE09885D2EFE1C915F7B4C581D9CE3FDAB78ACF4829F85752E9FC985DEB8773889EE4A1945BD554724803A6F5DC0A2CD5EFE001ABED8D61E8449E4FAA9E4DD392DA8D292ECC6EB149E843E395CDE0F98D04940A28C4B05F747149B34A0BAEC04FFF3E304C84AF1FF81225E615B5F94E334378A0A888EF88F4E79F66CB377E3C21964AACB5049C08435FE84EEEF39D20A665C17E04898914A85B3DE23D56575EBC682D154F4F15C37723E04974DB370180A9A579BC84F6BC9B5E7C223E5CBEE721E57EE07EFDCC0A3257BBEBF9ADFFB00DBF7EF682EC1C4C47451438F90B4CF8DA709940F72CFDC91C6EB4E37B4ED7E2385B1FF71B28A1D2669FBEB18EA89F9D391D2FDDEA0ED362E6A591AC64EF4AE31CA8766C259ECB77D01A7F5C36B8418F91C1BEADDD4491C80F0016B66421B4B788C55127135DA2FA625FB7FD195FB40D90A6C67328602ECAF3EC4F5894BFD84A99EB4753BE0D22E0D4DE6A0ADFEDC80EB1B556749B4A8AD00E73B329C95827AB91C0256347E85E3C5FD6726D0E1FE82C925D3DF4A9  
);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[ALTER COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-column-encryption-key-transact-sql.md)   
[DROP COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
[CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
[sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)   
[sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
[sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
[Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
[Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
[Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
[Gestire le chiavi per Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
  
  
