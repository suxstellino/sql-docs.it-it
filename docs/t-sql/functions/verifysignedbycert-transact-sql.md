---
description: VERIFYSIGNEDBYCERT (Transact-SQL)
title: VERIFYSIGNEDBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYSIGNEDBYCERT
- VERIFYSIGNEDBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- digitally signed data for changes [SQL Server]
- verifying digitally signed data for changes
- testing digitally signed data for changes
- checking digitally signed data for changes
- VERIFYSIGNEDBYCERT
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 4e041f33-60c4-4190-91c7-220d51dd6c8f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a0d08e5d4c85e34617685614b4305e3cd7b3529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210617"
---
# <a name="verifysignedbycert-transact-sql"></a>VERIFYSIGNEDBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Verifica se i dati con firma digitale sono stati modificati dopo la firma.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
VerifySignedByCert( Cert_ID , signed_data , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Cert_ID*  
 ID di un certificato nel database. *Cert_ID* è di tipo **int**.  
  
 *signed_data*  
 Variabile di tipo **nvarchar**, **char**, **varchar** o **nchar** contenente i dati firmati con un certificato.  
  
 *URL REST*  
 Firma allegata ai dati firmati. *signature* è di tipo **varbinary**.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
 Restituisce 1 se i dati firmati risultano invariati; in caso contrario, restituisce 0.  
  
## <a name="remarks"></a>Commenti  
 **VerifySignedBycert** decrittografa la firma dei dati usando la chiave pubblica del certificato specificato e confronta il valore decrittografato con un nuovo hash MD5 dei dati calcolato. Se i valori corrispondono, viene confermata la validità della firma.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DEFINITION per il certificato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-verifying-that-signed-data-has-not-been-tampered-with"></a>R. Verifica che i dati firmati non siano stati alterati  
 Nell'esempio seguente viene verificato se le informazioni incluse in `Signed_Data` sono state modificate dopo la firma tramite il certificato denominato `Shipping04`. La firma viene archiviata in `DataSignature`. Il certificato `Shipping04` viene passato a `Cert_ID`, che restituisce l'ID del certificato nel database. Se `VerifySignedByCert` restituisce 1, la firma è corretta. Se invece `VerifySignedByCert` restituisce 0, i dati in `Signed_Data` non corrispondono ai dati utilizzati per generare `DataSignature`. In questo caso, i dati in `Signed_Data` sono stati modificati dopo la firma oppure la firma di `Signed_Data` è stata eseguita con un certificato diverso.  
  
```sql
SELECT Data, VerifySignedByCert( Cert_Id( 'Shipping04' ),  
    Signed_Data, DataSignature ) AS IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
### <a name="b-returning-only-records-that-have-a-valid-signature"></a>B. Restituzione solo dei record che dispongono di una firma valida  
 La query restituisce solo i record che non hanno subito modifiche dopo essere stati firmati utilizzando il certificato `Shipping04`.  
  
```sql
SELECT Data FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByCert( Cert_Id( 'Shipping04' ), Data,   
    DataSignature ) = 1   
AND Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)   
 [SIGNBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/signbycert-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
