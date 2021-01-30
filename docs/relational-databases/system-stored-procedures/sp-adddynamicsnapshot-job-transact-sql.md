---
description: sp_adddynamicsnapshot_job (Transact-SQL)
title: sp_adddynamicsnapshot_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddynamicsnapshot_job
- sp_adddynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_adddynamicsnapshot_job
ms.assetid: ef50ccf6-e360-4e4b-91b9-6706b8fabefa
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 9798613901be96552813ebe6f419f2e2c24229fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182790"
---
# <a name="sp_adddynamicsnapshot_job-transact-sql"></a>sp_adddynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un processo di agente che genera uno snapshot dei dati filtrati per una pubblicazione con filtri di riga con parametri. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione. Questa stored procedure viene utilizzata da un amministratore per creare in modo manuale processi di snapshot dei dati filtrati per Sottoscrittori.  
  
> [!NOTE]  
>  Affinché venga creato un processo di snapshot dei dati filtrati, è necessario che esista già un processo di snapshot standard per la pubblicazione.  
  
 Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adddynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]   
    [ , [ @host_name = ] 'host_name' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' OUTPUT ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' OUTPUT ]   
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione a cui viene aggiunto il processo di snapshot dei dati filtrati. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @suser_sname = ] 'suser_sname'` Valore utilizzato per la creazione di uno snapshot dei dati filtrati per una sottoscrizione filtrata in base al valore della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nel Sottoscrittore. *SUSER_SNAME* è di **tipo sysname** e non prevede alcun valore predefinito. *SUSER_SNAME* deve essere null se questa funzione non viene utilizzata per filtrare in modo dinamico la pubblicazione.  
  
`[ @host_name = ] 'host_name'` Valore utilizzato per la creazione di uno snapshot dei dati filtrati per una sottoscrizione filtrata in base al valore della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) nel Sottoscrittore. *HOST_NAME* è di **tipo sysname** e non prevede alcun valore predefinito. *HOST_NAME* deve essere null se questa funzione non viene utilizzata per filtrare in modo dinamico la pubblicazione.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Nome del processo di snapshot dei dati filtrati creato. *dynamic_snapshot_jobname* è di **tipo sysname** e il valore predefinito è null. si tratta di un parametro di output facoltativo. Se specificato, *dynamic_snapshot_jobname* necessario risolvere un processo univoco nel database di distribuzione. Se non viene specificato, verrà generato automaticamente e restituito nel set di risultati un nome di processo con il formato seguente:  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
> [!NOTE]  
>  Quando viene generato il nome del processo di snapshot dinamico, è possibile troncare il nome del processo di snapshot standard.  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Identificatore del processo di snapshot dei dati filtrati creato. *dynamic_snapshot_jobid* è di tipo **uniqueidentifier** e il valore predefinito è null. si tratta di un parametro di output facoltativo.  
  
`[ @frequency_type = ] frequency_type` Frequenza con cui pianificare il processo di snapshot dei dati filtrati. *frequency_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Singola occorrenza|  
|**2**|On demand|  
|**4** (impostazione predefinita)|Ogni giorno|  
|**8**|Settimanale|  
|**16**|Ogni mese|  
|**32**|Mensile relativa|  
|**64**|Avvio automatico|  
|**128**|Periodica|  
  
`[ @frequency_interval = ] frequency_interval` Periodo, espresso in giorni, durante il quale viene eseguito il processo di snapshot dei dati filtrati. *frequency_interval* è di **tipo int** e il valore predefinito è 1 e dipende dal valore di *frequency_type*.  
  
|Valore di *frequency_type*|Effetti sui *frequency_interval*|  
|--------------------------------|-------------------------------------|  
|**1**|*frequency_interval* non è utilizzato.|  
|**4** (impostazione predefinita)|Ogni *frequency_interval* giorni e il valore predefinito è giornaliero.|  
|**8**|*frequency_interval* è uno o più degli elementi seguenti, combinati con un operatore logico [&#124; &#40;bit per bit o&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md) :<br /><br /> **1** = domenica &#124; **2** = lunedì &#124; **4** = martedì &#124; **8** = mercoledì &#124; **16** = giovedì &#124; **32** = venerdì &#124; **64** = sabato|  
|**16**|Il *frequency_interval* giorno del mese.|  
|**32**|*frequency_interval* è uno dei seguenti:<br /><br /> **1** = domenica &#124; **2** = lunedì &#124; **3** = martedì &#124; **4** = mercoledì &#124; **5** = giovedì &#124; **6** = venerdì &#124; **7** = sabato &#124; **8** = giorno &#124; **9** = giorno feriale &#124; **10** = giorno festivo|  
|**64**|*frequency_interval* non è utilizzato.|  
|**128**|*frequency_interval* non è utilizzato.|  
  
`[ @frequency_subday = ] frequency_subday` Specifica le unità per *frequency_subday_interval*. *frequency_subday* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1**|Una sola volta|  
|**2**|Second|  
|**4** (impostazione predefinita)|Minuto|  
|**8**|Ora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Numero di periodi di *frequency_subday* che si verificano tra ogni esecuzione del processo. *frequency_subday_interval* è di **tipo int** e il valore predefinito è 5.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Occorrenza del processo di snapshot dei dati filtrati in ogni mese. Questo parametro viene usato quando *frequency_type* è impostato su **32** (mensile relativo). *frequency_relative_interval* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**1** (impostazione predefinita)|First (Primo)|  
|**2**|Second|  
|**4**|Terzo|  
|**8**|Quarto|  
|**16**|Last (Ultimo)|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fattore di occorrenza utilizzato da *frequency_type*. *frequency_recurrence_factor* è di **tipo int** e il valore predefinito è 0.  
  
`[ @active_start_date = ] active_start_date` Data della prima pianificazione del processo di snapshot dei dati filtrati, nel formato AAAAMMGG. *active_start_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_date = ] active_end_date` Data di arresto della pianificazione del processo di snapshot dei dati filtrati, nel formato AAAAMMGG. *active_end_date* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Ora del giorno in cui il processo di snapshot dei dati filtrati viene pianificato per la prima volta, formattato come HHMMSS. *active_start_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Ora del giorno in cui il processo di snapshot dei dati filtrati smette di essere pianificato, formattato come HHMMSS. *active_end_time_of_day* è di **tipo int** e il valore predefinito è null.  
  
## <a name="result-set"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica il processo di snapshot dei dati filtrati nella tabella di sistema [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) .|  
|**dynamic_snapshot_jobname**|**sysname**|Nome del processo di snapshot dei dati filtrati.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifica in modo univoco il [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processo di Agent nel server di distribuzione.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 **sp_adddynamicsnapshot_job** viene utilizzata nella replica di tipo merge per le pubblicazioni che utilizzano un filtro con parametri.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-adddynamicsnapshot-jo_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_adddynamicsnapshot_job**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [sp_dropdynamicsnapshot_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)   
 [sp_helpdynamicsnapshot_job &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md)  
  
  
