---
description: Tabelle applicazioni livello dati - sysdac_history_internal
title: sysdac_history_internal (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdac_history_internal
- sysdac_history_internal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdac_history_internal
ms.assetid: 774a1678-0b27-42be-8adc-a6d7a4a56510
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c1770701b0373e56bac0a6c7c5bb2e9eac44ad3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187938"
---
# <a name="data-tier-application-tables---sysdac_history_internal"></a>Tabelle applicazioni livello dati - sysdac_history_internal
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni sulle azioni eseguite per gestire le applicazioni livello dati. Questa tabella è archiviata nello schema **dbo** del database **msdb** .  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**action_id**|**int**|Identificatore dell'azione|  
|**sequence_id**|**int**|Consente di identificare un passaggio all'interno di un'azione.|  
|**instance_id**|**uniqueidentifier**|Identificatore dell'istanza di applicazione livello dati. Questa colonna può essere unita in join alla colonna **Instance_Id** in [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md).|  
|**action_type**|**tinyint**|Identificatore del tipo di azione:<br /><br /> **0** = distribuzione<br /><br /> **1** = creazione<br /><br /> **2** = Rinomina<br /><br /> **3** = scollegamento<br /><br /> **4** = Elimina|  
|**action_type_name**|**varchar (19)**|Nome del tipo di azione:<br /><br /> **deploy**<br /><br /> **create**<br /><br /> **rename**<br /><br /> **Scollegare**<br /><br /> **delete**|  
|**dac_object_type**|**tinyint**|Identificatore del tipo di oggetto interessato dall'azione:<br /><br /> **0** = dacpac<br /><br /> **1** = account di accesso<br /><br /> **2** = database|  
|**dac_object_type_name**|**varchar (8)**|Nome del tipo di oggetto interessato dall'azione:<br /><br /> **dacpac** = istanza DAC<br /><br /> **accesso**<br /><br /> **database**|  
|**action_status**|**tinyint**|Codice di identificazione dello stato corrente dell'azione:<br /><br /> **0** = in sospeso<br /><br /> **1** = esito positivo<br /><br /> **2** = esito negativo|  
|**action_status_name**|**varchar (11)**|Stato corrente dell'azione:<br /><br /> **in sospeso**<br /><br /> **successo**<br /><br /> **esito negativo**|  
|**Obbligatorio**|**bit**|Utilizzato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] per il rollback di un'operazione dell'applicazione livello dati.|  
|**dac_object_name_pretran**|**sysname**|Nome dell'oggetto prima dell'esecuzione del commit della transazione contenente l'azione. Utilizzato solo per database e account di accesso.|  
|**dac_object_name_posttran**|**sysname**|Nome dell'oggetto dopo l'esecuzione del commit della transazione contenente l'azione. Utilizzato solo per database e account di accesso.|  
|**sqlscript**|**nvarchar(max)**|Script [!INCLUDE[tsql](../../includes/tsql-md.md)] che implementa un'azione in un database o account di accesso.|  
|**payload**|**varbinary(max)**|Definizione del pacchetto di applicazione livello dati salvata in una stringa codificata binaria.|  
|**Commenti**|**ntext**|Consente di registrare l'accesso di un utente che ha accettato la possibile perdita dei dati in un aggiornamento dell'applicazione livello dati.|  
|**error_string**|**nvarchar(max)**|Messaggio di errore generato se l'azione rileva un errore.|  
|**created_by**|**sysname**|Account di accesso che ha avviato l'azione di creazione questa voce.|  
|**date_created**|**datetime**|Data e ora di creazione della voce.|  
|**date_modified**|**datetime**|Data e ora dell'ultima modifica della voce.|  
  
## <a name="remarks"></a>Commenti  
 Le azioni di gestione di applicazione livello dati, quali ad esempio la distribuzione o l'eliminazione di un'applicazione livello dati, generano più passaggi. A ogni azione viene assegnato un identificatore dell'azione. A ogni passaggio vengono assegnati un numero di sequenza e una riga in **sysdac_history_internal**, in cui viene registrato lo stato del passaggio. Ogni riga viene creata all'avvio del passaggio dell'azione e aggiornata in base alle necessità per riflettere lo stato dell'operazione. Ad esempio, un'azione Distribuisci DAC può essere assegnata **action_id** 12 e ottenere quattro righe **sysdac_history_internal**:  
  
| action_id | sequence_id | action_type_name | dac_object_type_name |
| --------- | ----------- | ---------------- | -------------------- |
|12|0|create|dacpac|  
|12|1|create|login|  
|12|2|create|database|  
|12|3|ridenominazione|database|  
  
 Le operazioni dell'applicazione livello dati, ad esempio delete, non rimuovono righe dal **sysdac_history_internal**. È possibile utilizzare la query seguente per eliminare manualmente le righe per le applicazioni livello dati non più distribuite su un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)]:  
  
```sql  
DELETE FROM msdb.dbo.sysdac_history_internal  
WHERE instance_id NOT IN  
   (SELECT instance_id  
    FROM msdb.dbo.sysdac_instances_internal);  
```  
  
 L'eliminazione delle righe per le applicazioni livello dati attive non influisce sulle operazioni di applicazione livello dati; l'unico effetto è che non sarà possibile registrare la cronologia completa dell'applicazione livello dati.  
  
> [!NOTE]  
>  Attualmente non è disponibile alcun meccanismo per l'eliminazione di **sysdac_history_internal** righe in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin. L'accesso in sola lettura a questa vista è disponibile per tutti gli utenti che dispongono delle autorizzazioni per connettersi al database master.  
  
## <a name="see-also"></a>Vedere anche  
 [Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)   
 [sysdac_instances_internal &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/data-tier-application-tables-sysdac-instances-internal.md)  
  
  
