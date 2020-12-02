---
description: catalog.cleanup_server_log
title: catalog.cleanup_server_log | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 0dedb685-d3a6-4bd6-8afd-58d98853deee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0942fa73c29b14ba5b07b305126e4ba70bfa0cb1
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129924"
---
# <a name="catalogcleanup_server_log"></a>catalog.cleanup_server_log 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Pulisce i log operazioni per portare il database SSISDB in uno stato che permette di modificare il valore della proprietà SERVER_OPERATION_ENCRYPTION_LEVEL.  
  
## <a name="syntax"></a>Sintassi  
  
```sql
catalog.cleanup_server_log  
```  
  
## <a name="arguments"></a>Argomenti  
 No.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 per esito positivo e 1 per esito negativo.  
  
## <a name="result-sets"></a>Set di risultati  
 No.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ ed EXECUTE per il progetto e, se applicabile, autorizzazioni READ per l'ambiente a cui si fa riferimento.  
  
-   Appartenenza al ruolo del database **ssis_admin**.  
  
-   Appartenenza al ruolo del server **sysadmin**.  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Questa stored procedure genera errori negli scenari seguenti:  
  
-   Almeno una operazione è attiva nel database SSISDB.  
  
-   Il database SSISDB non è in modalità utente singolo.  
  
## <a name="remarks"></a>Osservazioni  
 In SQL Server 2012 Service Pack 2 è stata aggiunta la proprietà SERVER_OPERATION_ENCRYPTION_LEVEL alla tabella **internal.catalog_properties**. Per questa proprietà sono possibili due valori:  
  
-   **PER_EXECUTION (1)** : il certificato e la chiave simmetrica usati per la protezione dei parametri e dei log di esecuzione riservati vengono creati per ogni esecuzione. Poiché i certificati e le chiavi vengono generati per ogni esecuzione, è possibile riscontrare problemi di prestazioni (deadlock, mancata riuscita di processi di manutenzione e così via) negli ambienti di produzione. Questa impostazione, tuttavia, offre un livello di sicurezza superiore rispetto all'altro valore (2).  
  
-   **PER_PROJECT (2)** : il certificato e la chiave simmetrica usati per la protezione dei parametri riservati vengono creati per ogni progetto. PER_PROJECT (2) corrisponde all'impostazione predefinita. Questa impostazione garantisce prestazioni migliori rispetto al livello PER_EXECUTION, perché la chiave e il certificato vengono generati una volta per il progetto, anziché per ogni esecuzione.  
  
 Prima di modificare SERVER_OPERATION_ENCRYPTION_LEVEL da 2 a 1 o viceversa, è necessario eseguire [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md). Prima di eseguire questa stored procedure, effettuare le operazioni seguenti:  
  
1.  Verificare che il valore della proprietà OPERATION_CLEANUP_ENABLED sia impostato su TRUE nella tabella [catalog.catalog_properties &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
2.  Impostare il database di Integration Services (SSISDB) sulla modalità utente singolo. In SQL Server Management Studio avviare la finestra di dialogo Proprietà database per SSISDB, passare alla scheda Opzioni e impostare la proprietà Limitazione accesso sulla modalità utente singolo (SINGLE_USER). Dopo aver eseguito la stored procedure cleanup_server_log, riportare il valore della proprietà sul valore originale.  
  
3.  Eseguire la stored procedure [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md).  
  
4.  A questo punto modificare il valore della proprietà SERVER_OPERATION_ENCRYPTION_LEVEL nella tabella [catalog.catalog_properties &#40;database SSISDB&#41; ](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
5.  Eseguire la stored procedure [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md) per pulire le chiavi dei certificati dal database SSISDB. Il rilascio dei certificati e delle chiavi dal database SSISDB può richiedere molto tempo. È quindi necessario eseguire questa operazione periodicamente in orario non di punta.  
  
     È possibile specificare l'ambito o il livello (di esecuzione o di progetto) e il numero di chiavi da eliminare. La dimensione predefinita del batch per l'eliminazione è 1000. Quando si imposta il livello su 2, le chiavi e i certificati vengono eliminati solo se i progetti associati sono stati eliminati.  
  
 Per altre informazioni, vedere l'articolo della Knowledge Base [CORREZIONE: Problemi di prestazioni quando si usa SSISDB come archivio di distribuzione in SQL Server 2012](https://support.microsoft.com/kb/2972285)  
  
## <a name="example"></a>Esempio  
 L'esempio seguente chiama la stored procedure cleanup_server_log.  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
EXEC@return_value = [internal].[cleanup_server_log]  
SELECT'Return Value' = @return_value  
GO   
```  
  
  
