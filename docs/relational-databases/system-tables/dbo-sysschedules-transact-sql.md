---
description: dbo.sysschedules (Transact-SQL)
title: Pianificazioni dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysschedules_TSQL
- sysschedules
- sysschedules_TSQL
- dbo.sysschedules
dev_langs:
- TSQL
helpviewer_keywords:
- sysschedules system table
ms.assetid: 4cac9237-7a69-4035-bb3e-928b76aad698
author: cawrites
ms.author: chadam
ms.openlocfilehash: 31c49370b1e2ed2b5685bf46b0918fd0173f7119
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096236"
---
# <a name="dbosysschedules-transact-sql"></a>dbo.sysschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni sulle pianificazioni dei processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questa tabella è archiviata nel database **msdb** .  
  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|ID della pianificazione dei processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|  
|**schedule_uid**|**uniqueidentifier**|Identificatore univoco della pianificazione dei processi. Questo valore viene utilizzato per identificare una pianificazione per i processi distribuiti.|  
|**originating_server_id**|**int**|ID del server master di provenienza della pianificazione dei processi.|  
|**nome**|**sysname (nvarchar (128))**|Nome definito dall'utente per la pianificazione dei processi. Il nome deve essere univoco all'interno di un processo.|  
|**owner_sid**|**varbinary(85)**|Microsoft Windows *security_identifier* dell'utente o del gruppo a cui appartiene la pianificazione del processo.|  
|**abilitato**|**int**|Stato della pianificazione dei processi:<br /><br /> **0** = non abilitato.<br /><br /> **1** = abilitata.<br /><br /> Quando la pianificazione non è abilitata, non verrà eseguito alcun processo su questa pianificazione.|  
|**freq_type**|**int**|Frequenza di esecuzione di un processo per questa pianificazione.<br /><br /> **1** = solo una volta<br /><br /> **4** = giornaliero<br /><br /> **8** = settimanale<br /><br /> **16** = mensile<br /><br /> **32** = mensile, relativo a **freq_interval**<br /><br /> **64** = viene eseguito all'avvio del servizio SQL Server Agent<br /><br /> **128** = viene eseguito quando il computer è inattivo|  
|**freq_interval**|**int**|Giorni in cui viene eseguito il processo. Dipende dal valore di **freq_type**. Il valore predefinito è **0**, che indica che **freq_interval** è inutilizzato. Per i valori possibili e i relativi effetti, vedere la tabella seguente.|  
|**freq_subday_type**|**int**|Unità per la **freq_subday_interval**. Di seguito sono riportati i valori possibili e le relative descrizioni.<br /><br /> <br /><br /> **1** : all'ora specificata<br /><br /> **2** : secondi<br /><br /> **4** : minuti<br /><br /> **8** : ore|  
|**freq_subday_interval**|**int**|Numero di periodi di **freq_subday_type** tra le esecuzioni del processo.|  
|**freq_relative_interval**|**int**|Quando **freq_interval** si verifica ogni mese, se **freq_type** è **32** (mensile relativo). I possibili valori sono i seguenti:<br /><br /> **0**  =  **freq_relative_interval** non è usato<br /><br /> **1** = prima<br /><br /> **2** = secondo<br /><br /> **4** = terzo<br /><br /> **8** = quarto<br /><br /> **16** = Ultima|  
|**freq_recurrence_**<br /><br /> **Factor**|**int**|Numero di settimane o mesi tra le esecuzioni pianificate di un processo. **freq_recurrence_factor** viene utilizzato solo se **freq_type** è **8**, **16** o **32**. Se questa colonna contiene **0**, **freq_recurrence_factor** non è utilizzata.|  
|**active_start_date**|**int**|Data dalla quale è possibile avviare l'esecuzione del processo. La data è nel formato AAAAMMGG. NULL indica la data odierna.|  
|**active_end_date**|**int**|Data dalla quale è possibile arrestare l'esecuzione del processo. La data è nel formato AAAAMMGG.|  
|**active_start_time**|**int**|Tempo che intercorre tra **active_start_date** e **active_end_date** avvio dell'esecuzione del processo. L'ora è nel formato HHMMSS, a 24 ore.|  
|**active_end_time**|**int**|Tempo che intercorre tra **active_start_date** e **active_end_date** interruzione dell'esecuzione del processo. L'ora è nel formato HHMMSS, a 24 ore.|  
|**date_created**|**datetime**|Data e ora di creazione della pianificazione.|  
|**date_modified**|**datetime**|Data e ora dell'ultima modifica della pianificazione.|  
|**version_number**|**int**|Numero di versione corrente della pianificazione. Se, ad esempio, una pianificazione è stata modificata 10 volte, il **version_number** è 10.|  
  
|Valore di freq_type|Effetto su freq_interval|  
|-------------------------|------------------------------|  
|**1** (una volta)|**freq_interval** non usato (**0**)|  
|**4** (giornaliera)|Ogni **freq_interval** giorni|  
|**8** (settimanale)|**freq_interval** è uno o più degli elementi seguenti:<br /><br /> **1** = domenica<br /><br /> **2** = lunedì<br /><br /> **4** = martedì<br /><br /> **8** = mercoledì<br /><br /> **16** = giovedì<br /><br /> **32** = venerdì<br /><br /> **64** = sabato|  
|**16** (ogni mese)|Il **freq_interval** giorno del mese|  
|**32** (mensile, relativo)|**freq_interval** è uno dei seguenti:<br /><br /> **1** = domenica<br /><br /> **2** = lunedì<br /><br /> **3** = martedì<br /><br /> **4** = mercoledì<br /><br /> **5** = giovedì<br /><br /> **6** = venerdì<br /><br /> **7** = sabato<br /><br /> **8** = giorno<br /><br /> **9** = giorno feriale<br /><br /> **10** = giorno festivo|  
|**64** (inizia all'avvio del servizio SQL Server Agent)|**freq_interval** non usato (**0**)|  
|**128** (viene eseguito quando il computer è inattivo)|**freq_interval** non usato (**0**)|  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.sysJobSchedules &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
  
  
