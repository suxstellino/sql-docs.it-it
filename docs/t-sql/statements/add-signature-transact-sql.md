---
description: ADD SIGNATURE (Transact-SQL)
title: ADD SIGNATURE (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD SIGNATURE
- ADD_SIGNATURE_TSQL
helpviewer_keywords:
- ADD SIGNATURE statement
- adding digital signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 64d8b682-6ec1-4e5b-8aee-3ba11e72d21f
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.custom: ''
ms.date: 03/16/2021
ms.openlocfilehash: c33f34611db384dfcafabefeb2e9c1eee1b135f5
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611228"
---
# <a name="add-signature-transact-sql"></a>ADD SIGNATURE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Aggiunge una firma digitale a una stored procedure, una funzione, un assembly o un trigger DML. Aggiunge anche una controfirma a una stored procedure, una funzione, un assembly o un trigger DML.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintassi

```syntaxsql
ADD [ COUNTER ] SIGNATURE TO module_class::module_name
    BY <crypto_list> [ ,...n ]
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | CERTIFICATE cert_name [ WITH PASSWORD = 'password' ]
    | CERTIFICATE cert_name WITH SIGNATURE = signed_blob
    | ASYMMETRIC KEY Asym_Key_Name  
    | ASYMMETRIC KEY Asym_Key_Name [ WITH PASSWORD = 'password'.]
    | ASYMMETRIC KEY Asym_Key_Name WITH SIGNATURE = signed_blob
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*module_class*  
Classe del modulo a cui viene aggiunta la firma. L'impostazione predefinita per i moduli definiti a livello di ambito dello schema è OBJECT.  
  
 *module_name*  
 Nome di una stored procedure, una funzione, un assembly o un trigger da firmare o controfirmare.  
  
 CERTIFICATE *cert_name*  
 Nome di un certificato con cui firmare o controfirmare la stored procedure, la funzione, l'assembly o il trigger.  
  
 WITH PASSWORD ='*password*'  
 Password necessaria per decrittografare la chiave privata del certificato o della chiave asimmetrica. Questa clausola è necessaria solo se la chiave privata non è protetta tramite la chiave master del database.  
  
 SIGNATURE =*signed_blob*  
 Specifica l'oggetto BLOB (Binary Large Object) firmato del modulo. Questa clausola risulta utile se si desidera fornire un modulo senza fornire la chiave privata. Se si utilizza questa clausola, sono necessari solo il modulo, la firma e la chiave pubblica per aggiungere l'oggetto BLOB firmato a un database. *signed_blob* è l'oggetto BLOB in formato esadecimale.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Nome di una chiave asimmetrica con cui firmare o controfirmare la stored procedure, la funzione, l'assembly o il trigger.  
  
## <a name="remarks"></a>Osservazioni

Il modulo che viene firmato o controfirmato e il certificato o la chiave asimmetrica utilizzati per la firma devono essere già esistenti. Ogni carattere del modulo è utilizzato nel calcolo della firma, inclusi gli avanzamenti di riga e i ritorni a capo iniziali.  
  
 Un modulo può essere firmato e controfirmato da un numero qualsiasi di certificati e chiavi simmetriche.  
  
 Se un modulo viene modificato, la firma viene eliminata.  
  
 Se un modulo contiene una clausola EXECUTE AS, nel processo di firma viene incluso anche l'ID di sicurezza (SID) dell'entità.  
  
> [!CAUTION]
> È consigliabile utilizzare la firma del modulo solo per concedere le autorizzazioni, mai per negarle o revocarle.  
  
 I trigger DDL (Data Definition Language) e le funzioni inline con valori di tabella non possono essere firmati.  
  
 Le informazioni sulle firme sono visibili nella vista di catalogo sys.crypt_properties.  
  
> [!WARNING]
>  Quando si ricrea una procedura per la firma, tutte le istruzioni del batch originale devono corrispondere al batch di ricreazione. Se una parte del batch è diversa, anche in spazi o commenti, la firma risultante sarà diversa.  
  
## <a name="countersignatures"></a>Controfirme  
 In caso di esecuzione di un modulo con segno, le firme verranno aggiunte temporaneamente al token SQL, tuttavia verranno perse se il modulo esegue un altro modulo o termina l'esecuzione. Una controfirma è uno speciale tipo di firma che da sola non concede autorizzazioni, tuttavia consente il mantenimento delle firme create dallo stesso certificato o dalla stessa chiave asimmetrica per la durata della chiamata eseguita all'oggetto controfirmato.  
  
 Si supponga, ad esempio, che l'utente Alice chiami la routine ProcForAlice, che chiama la routine ProcSelectT1, che seleziona dalla tabella T1. Alice dispone dell'autorizzazione EXECUTE per ProcForAlice e ProcSelectT1, ma non dispone dell'autorizzazione SELECT per T1 e non è necessario alcun concatenamento della proprietà nell'intera catena. Alice non può accedere alla tabella T1, né direttamente né tramite l'uso di ProcForAlice e ProcSelectT1. Poiché si vuole che Alice usi sempre ProcForAlice per l'accesso, non si vuole concedere l'autorizzazione per l'esecuzione di ProcSelectT1. Ecco come ottenere questo risultato  
  
-   Se si firma ProcSelectT1, in modo che ProcSelectT1 possa accedere a T1, Alice può richiamare direttamente ProcSelectT1 e non deve chiamare ProcForAlice.  
  
-   È possibile negare ad Alice l'autorizzazione EXECUTE per ProcSelectT1, ma Alice non sarebbe in grado di chiamare ProcSelectT1 tramite ProcForAlice.
  
-   La firma di ProcForAlice non funzionerebbe da sola, perché la firma andrebbe persa nella chiamata a ProcSelectT1.  
  
Tuttavia, da controfirma ProcSelectT1 con lo stesso certificato usato per firmare ProcForAlice, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la firma viene mantenuta nella catena di chiamate e consentirà l'accesso a T1. Se Alice tenta di chiamare direttamente ProcSelectT1, non può accedere a T1, perché controfirma non concede alcun diritto. Nell'esempio C di seguito viene mostrato l'uso di [!INCLUDE[tsql](../../includes/tsql-md.md)] per l'esempio proposto.  

:::image type="content" source="media/signing-and-countersignature.png" alt-text="esempio di firma":::

## <a name="permissions"></a>Autorizzazioni  

Sono richieste l'autorizzazione ALTER per l'oggetto e l'autorizzazione CONTROL per il certificato o la chiave asimmetrica. Se una chiave privata associata è protetta tramite una password, è necessario che anche l'utente disponga della password.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-signing-a-stored-procedure-by-using-a-certificate"></a>R. Firma di una stored procedure tramite un certificato

 Nell'esempio seguente la stored procedure `HumanResources.uspUpdateEmployeeLogin` viene firmata tramite il certificato `HumanResourcesDP`.  
  
```sql  
USE AdventureWorks2012;  
ADD SIGNATURE TO HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
### <a name="b-signing-a-stored-procedure-by-using-a-signed-blob"></a>B. Firma di una stored procedure tramite un oggetto BLOB firmato

Nell'esempio seguente viene creato un nuovo database e un certificato da utilizzare nell'esempio. Nell'esempio viene creata e firmata una stored procedure semplice e viene recuperato l'oggetto BLOB della firma da `sys.crypt_properties`. La firma viene quindi eliminata e aggiunta nuovamente. Nell'esempio la stored procedure viene firmata tramite la sintassi WITH SIGNATURE.  
  
```sql  
CREATE DATABASE TestSignature ;  
GO  
USE TestSignature ;  
GO  
-- Create a CERTIFICATE to sign the procedure.  
CREATE CERTIFICATE cert_signature_demo   
    ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
    WITH SUBJECT = 'ADD SIGNATURE demo';  
GO  

-- Create a simple procedure.  
CREATE PROC [sp_signature_demo]  
AS  
    PRINT 'This is the content of the procedure.' ;  
GO  
-- Sign the procedure.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]   
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  

-- Get the signature binary BLOB for the sp_signature_demo procedure.  
SELECT cp.crypt_property  
    FROM sys.crypt_properties AS cp  
    JOIN sys.certificates AS cer  
        ON cp.thumbprint = cer.thumbprint  
    WHERE cer.name = 'cert_signature_demo' ;  
GO  
```  
  
 La firma `crypt_property` restituita da questa istruzione sarà diversa ogni volta che si crea una stored procedure. Annotare il risultato per utilizzarlo successivamente in questo esempio. Per questo esempio, il risultato dimostrato è: `0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373`.  
  
```sql  
-- Drop the signature so that it can be signed again.  
DROP SIGNATURE FROM [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo];  
GO  

-- Add the signature. Use the signature BLOB obtained earlier.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]  
    WITH SIGNATURE = 0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373 ;  
GO  
```  
  
### <a name="c-accessing-a-procedure-using-a-countersignature"></a>C. Accesso a una procedura mediante una controfirma

Nell'esempio seguente viene mostrato come la controfirma può aiutare a controllare l'accesso a un oggetto.  
  
```sql  
-- Create tesT1 database  
CREATE DATABASE testDB;  
GO  
USE testDB;  
GO  
-- Create table T1  
CREATE TABLE T1 (c VARCHAR(11));  
INSERT INTO T1 VALUES ('This is T1.');  
  
-- Create a TestUser user to own table T1  
CREATE USER TestUser WITHOUT LOGIN;  
ALTER AUTHORIZATION ON T1 TO TestUser;  
  
-- Create a certificate for signing  
CREATE CERTIFICATE csSelectT  
  ENCRYPTION BY PASSWORD = 'SimplePwd01'  
  WITH SUBJECT = 'Certificate used to grant SELECT on T1';  
CREATE USER ucsSelectT1 FROM CERTIFICATE csSelectT;  
GRANT SELECT ON T1 TO ucsSelectT1;  
  
-- Create a principal with low privileges  
CREATE LOGIN Alice WITH PASSWORD = 'SimplePwd01';  
CREATE USER Alice;  
  
-- Verify Alice cannoT1 access T1;  
EXECUTE AS LOGIN = 'Alice';  
    SELECT * FROM T1;  
REVERT;  
  
-- Create a procedure that directly accesses T1  
CREATE PROCEDURE procSelectT1 AS  
BEGIN  
    PRINT 'Now selecting from T1...';  
    SELECT * FROM T1;  
END;  
GO  
GRANT EXECUTE ON ProcSelectT1 to public;  
  
-- Create special procedure for accessing T1  
CREATE PROCEDURE  ProcForAlice AS  
BEGIN  
   IF USER_ID() <> USER_ID('Alice')  
    BEGIN  
        PRINT 'Only Alice can use this.';  
        RETURN  
    END  
   EXEC ProcSelectT1;  
END;  
GO;  
GRANT EXECUTE ON ProcForAlice TO PUBLIC;  
  
-- Verify procedure works for a sysadmin user  
EXEC ProcForAlice;  
  
-- Alice still can't use the procedure yet  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
REVERT;  
  
-- Sign procedure to grant it SELECT permission  
ADD SIGNATURE TO ProcForAlice BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Counter sign ProcSelectT1, to make this work  
ADD COUNTER SIGNATURE TO ProcSelectT1 BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Now the proc works.   
-- Note that calling ProcSelectT1 directly still doesn't work  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
    EXEC ProcSelectT1;  
REVERT;  
  
-- Cleanup  
USE master;  
GO  
DROP DATABASE testDB;  
DROP LOGIN Alice;  
```  
  
## <a name="see-also"></a>Vedere anche

- [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
- [DROP SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-signature-transact-sql.md)
