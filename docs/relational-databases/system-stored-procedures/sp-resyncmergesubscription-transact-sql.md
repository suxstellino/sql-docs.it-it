---
description: sp_resyncmergesubscription (Transact-SQL)
title: sp_resyncmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resyncmergesubscription_TSQL
- sp_resyncmergesubscription
helpviewer_keywords:
- sp_resyncmergesubscription
ms.assetid: e04d464a-60ab-4b39-a710-c066025708e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530c93cbb99db63c7ced6e454ea78b4f6a0f1ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183081"
---
# <a name="sp_resyncmergesubscription-transact-sql"></a>sp_resyncmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Risincronizza una sottoscrizione di tipo merge in base a uno stato di convalida noto specificato. In questo modo è possibile forzare la convergenza oppure sincronizzare il database di sottoscrizione fino a un momento specifico, ad esempio fino all'ultima convalida riuscita oppure fino a una data specificata. Quando si risincronizza una sottoscrizione in base a questa modalità, lo snapshot non viene riapplicato. Questa stored procedure non viene utilizzata per sottoscrizioni di replica snapshot o transazionali. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_resyncmergesubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
        , [ @publication = ] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @resync_type = ] resync_type ]  
    [ , [ @resync_date_str = ] resync_date_string ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e il valore predefinito è null. Il valore NULL è valido se la stored procedure viene eseguita nel server di pubblicazione. Se la stored procedure viene eseguita nel Sottoscrittore, è necessario specificare un server di pubblicazione.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e il valore predefinito è null. Il valore NULL è valido se la stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione. Se la stored procedure viene eseguita nel Sottoscrittore, è necessario specificare un server di pubblicazione.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @subscriber = ] 'subscriber'` Nome del Sottoscrittore. *Subscriber* è di **tipo sysname** e il valore predefinito è null. Il valore NULL è valido se la stored procedure viene eseguita nel Sottoscrittore. Se la stored procedure viene eseguita nel server di pubblicazione, è necessario specificare un Sottoscrittore.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nome del database di sottoscrizione. *subscription_db* è di **tipo sysname** e il valore predefinito è null. Il valore NULL è valido se la stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore. Se la stored procedure viene eseguita nel server di pubblicazione, è necessario specificare un Sottoscrittore.  
  
`[ @resync_type = ] resync_type` Definisce quando deve iniziare la risincronizzazione. *resync_type* è di **tipo int**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|La sincronizzazione ha inizio dopo lo snapshot iniziale. Questa opzione comporta il maggior utilizzo di risorse, in quanto tutte le modifiche apportate dopo lo snapshot iniziale vengono riapplicate nel Sottoscrittore.|  
|**1**|La sincronizzazione ha inizio dopo l'ultima convalida riuscita. Tutte le generazioni nuove o incomplete eseguite dopo l'ultima convalida riuscita vengono riapplicate nel Sottoscrittore.|  
|**2**|La sincronizzazione inizia dalla data specificata in *resync_date_str*. Tutte le generazioni nuove o incomplete eseguite dopo tale data vengono riapplicate nel Sottoscrittore.|  
  
`[ @resync_date_str = ] resync_date_string` Definisce la data di inizio della risincronizzazione. *resync_date_string* è di **tipo nvarchar (30)** e il valore predefinito è null. Questo parametro viene utilizzato quando il *resync_type* è un valore di **2**. La data specificata viene convertita nel valore **DateTime** equivalente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_resyncmergesubscription** viene utilizzata nella replica di tipo merge.  
  
 Il valore **0** per il parametro *resync_type* , che riapplica tutte le modifiche apportate dopo lo snapshot iniziale, può richiedere un utilizzo intensivo delle risorse, ma probabilmente molto inferiore a una reinizializzazione completa. Se, ad esempio, lo snapshot iniziale è stato recapitato un mese fa, vengono riapplicati i dati relativi all'ultimo mese. Se lo snapshot iniziale contiene 1 gigabyte (GB) di dati, ma i dati modificati nell'ultimo mese corrispondono a 2 megabyte (MB), risulta più efficiente riapplicare i dati anziché l'intero snapshot da 1 GB.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_resyncmergesubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
