---
description: sp_copysubscription (Transact-SQL)
title: sp_copysubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysubscription
- sp_copysubscription_TSQL
helpviewer_keywords:
- sp_copysubscription
ms.assetid: 3c56cd62-2966-4e87-a986-44cb3fd0b760
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7377048df6d32715b8f91e26b5b21617c22a0d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192015"
---
# <a name="sp_copysubscription-transact-sql"></a>sp_copysubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

    
> [!IMPORTANT]  
>  La funzionalità di sottoscrizione collegabile è deprecata e verrà rimossa a partire da una delle prossime versioni. Evitare di utilizzare questa funzionalità in un nuovo progetto di sviluppo. Nel caso di pubblicazioni di tipo merge partizionate mediante filtri con parametri, è consigliabile utilizzare la nuove funzionalità degli snapshot partizionati, che semplificano l'inizializzazione di un ampio numero di sottoscrizioni. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Per le pubblicazioni non partizionate, è possibile inizializzare una sottoscrizione con un backup. Per altre informazioni, vedere [Inizializzazione di una sottoscrizione transazionale senza uno snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 Copia un database di sottoscrizione che include sottoscrizioni pull, ma non push. È possibile copiare solo database a file singolo. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_copysubscription [ @filename = ] 'file_name'  
    [ , [ @temp_dir = ] 'temp_dir' ]  
    [ , [ @overwrite_existing_file = ] overwrite_existing_file]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @filename = ] 'file_name'` Stringa che specifica il percorso completo, incluso il nome del file, in cui viene salvata una copia del file di dati (con estensione MDF). il *nome del file* è **nvarchar (260)** e non prevede alcun valore predefinito.  
  
`[ @temp_dir = ] 'temp_dir'` Nome della directory che contiene i file temporanei. *temp_dir* è di **tipo nvarchar (260)** e il valore predefinito è null. Se è NULL, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà utilizzata la directory dei dati predefinita. Nella directory deve essere disponibile spazio sufficiente per l'archiviazione di un file le cui dimensioni sono pari alla somma delle dimensioni di tutti i file di database del Sottoscrittore.  
  
`[ @overwrite_existing_file = ] 'overwrite_existing_file'`Flag booleano facoltativo che specifica se sovrascrivere o meno un file esistente con lo stesso nome specificato nel **\@ nome file**. *overwrite_existing_file* è di **bit** e il valore predefinito è **0**. Se è **1**, sovrascrive il file specificato da **\@ filename**, se esistente. Se è **0**, il stored procedure ha esito negativo se il file esiste e il file non viene sovrascritto.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_copysubscription** viene utilizzato in tutti i tipi di replica per copiare un database di sottoscrizione in un file come alternativa all'applicazione di uno snapshot nel Sottoscrittore. Il database deve essere configurato in modo che supporti solo le sottoscrizioni pull. Gli utenti che dispongono delle autorizzazioni appropriate possono eseguire copie del database di sottoscrizione e quindi inviare tramite posta elettronica, copiare o trasferire il file ottenuto (con estensione msf) in un altro Sottoscrittore, dove è possibile collegarlo come sottoscrizione.  
  
 Le dimensioni del database di sottoscrizione copiato devono essere inferiori a 2 gigabyte (GB)  
  
 **sp_copysubscription** è supportata solo per i database con sottoscrizioni client e non può essere eseguita quando il database dispone di sottoscrizioni server.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_copysubscription**.  
  
## <a name="see-also"></a>Vedere anche  
 [Percorsi alternativi cartella snapshot](../../relational-databases/replication/snapshot-options.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
