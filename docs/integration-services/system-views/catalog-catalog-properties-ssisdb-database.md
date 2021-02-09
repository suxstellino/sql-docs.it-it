---
description: catalog.catalog_properties (database SSISDB)
title: catalog.catalog_properties (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/11/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e604a382-95c8-4764-b268-742eb5c6d4cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4028247051220e1e45d054e400da050601254fb2
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835852"
---
# <a name="catalogcatalog_properties-ssisdb-database"></a>catalog.catalog_properties (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Vengono visualizzate le proprietà del catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|property_name|**nvarchar(256)**|Nome della proprietà del catalogo.|  
|property_value|**nvarchar(256)**|Valore della proprietà del catalogo.|  
  
## <a name="remarks"></a>Commenti  
 In questa vista viene visualizzata una riga per ogni proprietà del catalogo.
  
|Nome proprietà|Descrizione|  
|-------------------|-----------------|  
|**DEFAULT_EXECUTION_MODE**|Modalità di esecuzione predefinita a livello di server per i pacchetti, ovvero `Server` (0) o `Scale Out` (1). |
|**ENCRYPTION_ALGORITHM**|Tipo di algoritmo di crittografia utilizzato per crittografare i dati sensibili. Nei valori supportati sono inclusi: `DES`, `TRIPLE_DES`, `TRIPLE_DES_3KEY`, `DESX`, `AES_128`, `AES_192` e `AES_256`. Nota: il database del catalogo deve essere in modalità utente singolo per la modifica di questa proprietà.|
|**IS_SCALEOUT_ENABLED**|Quando il valore è `True`, la funzionalità SSIS Scale Out è abilitata. Se questa funzionalità non è stata abilitata, questa proprietà potrebbe non comparire nella vista.|
|**MAX_PROJECT_VERSIONS**|Numero di nuove versioni del progetto che vengono mantenute per un progetto singolo. Se la pulizia delle versioni è abilitata, le versioni precedenti oltre questo conteggio vengono eliminate.|  
|**OPERATION_CLEANUP_ENABLED**|Se il valore è `TRUE`, i dettagli e i messaggi delle operazioni anteriori a **RETENTION_WINDOW** (giorni) vengono eliminati dal catalogo. Quando il valore è `FALSE`, tutti i dettagli e i messaggi dell'operazione vengono archiviati nel catalogo. Nota: la pulizia dell'operazione viene eseguita da un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**RETENTION_WINDOW**|Numero di giorni di archiviazione nel catalogo dei dettagli e dei messaggi dell'operazione. Quando il valore è `-1`, il periodo di memorizzazione è infinito. Nota: se non si vuole eseguire la pulizia, impostare **OPERATION_CLEANUP_ENABLED** su **FALSE**.|
|**SCHEMA_BUILD**|Numero di build dello schema del database del catalogo SSISDB. Questo numero cambia ogni volta che il catalogo SSISDB viene creato o aggiornato.|
|**SCHEMA_VERSION**|Numero di versione principale dello schema del database del catalogo SSISDB. Questo numero cambia ogni volta che il catalogo SSISDB viene creato o che la versione principale viene aggiornata.|
|**VALIDATION_TIMEOUT**|La convalida viene interrotta se non viene completata nel numero di secondi specificato da questa proprietà.|  
|**SERVER_CUSTOMIZED_LOGGING_LEVEL**|Livello di registrazione personalizzato predefinito per il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se non sono stati creati livelli di registrazione personalizzati, questa proprietà potrebbe non comparire nella vista.|
|**SERVER_LOGGING_LEVEL**|Livello di registrazione predefinito per il server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].|
|**SERVER_OPERATION_ENCRYPTION_LEVEL**|Se il valore è 1 (`PER_EXECUTION`), il certificato e la chiave simmetrica usati per la protezione dei parametri e dei log di esecuzione riservati vengono creati per ogni *esecuzione*. Se il valore è 2 (`PER_PROJECT`), il certificato e la chiave simmetrica vengono creati una sola volta per ogni *progetto*. Per altre informazioni su questa proprietà, vedere i commenti sulla stored procedure SSIS [catalog.cleanup_server_log](../system-stored-procedures/catalog-cleanup-server-log.md#remarks).|
|**VERSION_CLEANUP_ENABLED**|Se il valore è `TRUE`, solo il numero di versioni di progetto **MAX_PROJECT_VERSIONS** viene archiviato nel catalogo, mentre tutte le altre versioni vengono eliminate. Se il valore è **FALSE**, tutte le versioni del progetto vengono archiviate nel catalogo. Nota: la pulizia dell'operazione viene eseguita da un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|||
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
  
