---
title: Archivio Blob remoto (RBS) (SQL Server) | Microsoft Docs
description: Archivio BLOB remoto (RBS) di SQL Server archivia oggetti binari di grandi dimensioni (BLOB) in risorse di archiviazione commerciali, anziché nel server di database principale. Informazioni su questo componente aggiuntivo.
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bfd234025664e7508d7d9cf942ff81a216b447a2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171753"
---
# <a name="remote-blob-store-rbs-sql-server"></a>Archivio Blob remoto (RBS) (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Archivio BLOB remoti (RBS) di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è un componente aggiuntivo facoltativo che consente agli amministratori di database di archiviare oggetti binari di grandi dimensioni in soluzioni di archiviazione apposite anziché direttamente nel server di database principale.  
  
 Tale componente è incluso nei supporti di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ma non viene installato dal programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cercare RBS.msi nel supporto di installazione per individuare il file di installazione.

 Se non è disponibile il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile scaricare RBS in una delle posizioni seguenti:

| Versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | Percorso download RBS |
|:---|:---|
| [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | [[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] | [[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] Pagina di download di RBS](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>Vantaggi di RBS  
  
### <a name="optimized-database-storage-and-performance"></a>Archiviazione e prestazioni ottimizzate del database  
 L'archiviazione di BLOB nel database può utilizzare grandi quantità di spazio file e risorse server costose. Archivio BLOB remoto consente di trasferire BLOB in una soluzione di archiviazione dedicata selezionata e di archiviare i riferimenti ai BLOB nel database. In questo modo vengono liberate l'archiviazione su server per i dati strutturati e le risorse server per le operazioni del database.  
  
### <a name="efficient-blob-management"></a>Gestione efficiente di BLOB  
 Diverse funzionalità di RBS supportano la gestione di BLOB archiviati:  
  
-   BLOB gestiti con transazioni ACID (Atomic, Consistent, Isolatable, Durable).  
  
-   BLOB organizzati in raccolte.  
  
-   Garbage Collection, verifica coerenza e altre funzioni di manutenzione.  
  
### <a name="standardized-api"></a>API standardizzata  
 RBS definisce un set di API che fornisce un modello di programmazione standardizzato affinché le applicazioni possano accedere e modificare gli archivi BLOB. Ogni archivio BLOB può specificare la propria libreria del provider che consente il collegamento alla libreria client di RBS e specifica la modalità di archiviazione e accesso ai BLOB.  
  
 Molti fornitori di soluzioni di archiviazione di terze parti hanno sviluppato provider RBS conformi a queste API standard e in grado di supportare l'archiviazione BLOB su varie piattaforme di archiviazione.  
  
## <a name="rbs-requirements"></a>Requisiti di RBS  
 - Per RBS è necessaria l'edizione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise per il server di database principale in cui vengono archiviati i metadati BLOB.  Tuttavia, se si utilizza il provider FILESTREAM fornito, è possibile archiviare BLOB nell'edizione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard. Per connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], RBS richiede almeno la versione 11 del driver ODBC per [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] e la versione 13 del driver ODBC per [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. I driver sono disponibili all'indirizzo [Download di driver ODBC per SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).    
  
 In RBS è incluso un provider FILESTREAM che consente di utilizzare tale componente per l'archiviazione di BLOB in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se si desidera utilizzare RBS per l'archiviazione di BLOB in una soluzione di archiviazione diversa, è necessario utilizzare un provider RBS di terze parti sviluppato per tale soluzione di archiviazione o sviluppare un provider RBS personalizzato utilizzando l'API di RBS. Un provider di esempio che consenta di archiviare BLOB nel file system NTFS è disponibile come risorsa per l'apprendimento in [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190).  
  
## <a name="rbs-security"></a>Sicurezza relativa a RBS  
 Il blog del team di Archiviazione BLOB remoti SQL è un'ottima fonte di informazioni su questa funzionalità. Il modello di sicurezza per RBS è descritto nel post dedicato al [modello di sicurezza di RBS](/archive/blogs/sqlrbs/rbs-security-model).  
  
### <a name="custom-providers"></a>Provider personalizzati  
 Quando si usa un provider personalizzato per archiviare BLOB all'esterno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], verificare che i BLOB archiviati siano protetti con autorizzazioni e opzioni di crittografia adatte al supporto di archiviazione usato dal provider personalizzato.  
  
### <a name="credential-store-symmetric-key"></a>Chiave simmetrica dell'archivio delle credenziali  
 Se il provider richiede la configurazione e l'uso di un segreto archiviato nell'archivio delle credenziali, RBS usa una chiave simmetrica per crittografare i segreti del provider, utilizzabile da un client per ottenere l'autorizzazione per l'archivio BLOB del provider.  
  
-   RBS 2016 usa una chiave simmetrica **AES_128** . [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] non consente la creazione di nuove chiavi **TRIPLE_DES** , se non per motivi di compatibilità con le versioni precedenti. Per altre informazioni, vedere [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
-   RBS 2014 e le versioni precedenti usano un archivio di credenziali che contiene i segreti crittografati con l'algoritmo per chiavi simmetriche **TRIPLE_DES** , ora obsoleto. Se si usa ancora **TRIPLE_DES**, [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di migliorare la sicurezza seguendo i passaggi descritti in questo argomento per adottare un metodo di crittografia più sicuro per la chiave.  
  
 È possibile determinare le proprietà della chiave simmetrica dell'archivio delle credenziali di RBS eseguendo l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente nel database di RBS:   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` Se l'output generato dall'istruzione indica che **TRIPLE_DES** è ancora in uso, è necessaria la rotazione della chiave.  
  
### <a name="rotating-the-symmetric-key"></a>Rotazione della chiave simmetrica  
 Durante l'uso di RBS è consigliabile ruotare periodicamente la chiave simmetrica dell'archivio delle credenziali. Si tratta di una comune procedura consigliata di sicurezza per soddisfare i criteri di sicurezza dell'organizzazione.  Un modo per eseguire la rotazione della chiave simmetrica dell'archivio delle credenziali di RBS consiste nell'usare lo [script riportato di seguito](#Key_rotation) nel database di RBS.  È anche possibile usare questo script per eseguire la migrazione a proprietà per aumentare la sicurezza della crittografia, come l'algoritmo o la lunghezza della chiave. Eseguire il backup del database prima della rotazione della chiave.  Al termine dello script sono previsti alcuni passaggi di verifica.  
Se i criteri di sicurezza in uso richiedono proprietà diverse per la chiave (ad esempio, algoritmo o lunghezza della chiave) da quelle specificate, lo script può essere usato come modello. Modificare le proprietà delle chiavi in due momenti, ovvero durante la creazione della chiave temporanea e durante la creazione della chiave permanente.  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> Risorse di RBS  
  
 **Esempi di RBS**  
 Negli esempi di RBS disponibili in [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190) viene illustrato come sviluppare un'applicazione RBS e come sviluppare e installare un provider RBS personalizzato.  
  
 **Blog di RBS**  
 Nel [blog di RBS](/archive/blogs/sqlrbs/) sono disponibili informazioni aggiuntive sulla distribuzione e gestione di RBS.  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> Script di rotazione della chiave  
 In questo esempio viene creata una stored procedure denominata `sp_rotate_rbs_symmetric_credential_key` per sostituire la chiave simmetrica dell'archivio delle credenziali di RBS in uso  
con una di propria scelta.  Si consiglia di eseguire questa operazione se esiste un criterio di sicurezza che richiede   
la rotazione periodica della chiave o se sono presenti requisiti specifici per gli algoritmi.  
 In questa stored procedure la chiave corrente verrà sostituita con una chiave simmetrica che usa **AES_256** .  In seguito alla  
sostituzione della chiave simmetrica, sarà necessario crittografare nuovamente i secreti con la nuova chiave.  Questa stored   
procedure eseguirà anche una nuova crittografia dei segreti.  È consigliabile eseguire un backup del database prima della rotazione della chiave.  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 È ora possibile usare la stored procedure `sp_rotate_rbs_symmetric_credential_key` per la rotazione della chiave simmetrica dell'archivio delle credenziali di RBS e i segreti rimangono invariati prima e dopo la rotazione della chiave.  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>Vedere anche  
[Archivio BLOB remoti (RBS) e gruppi di disponibilità AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
