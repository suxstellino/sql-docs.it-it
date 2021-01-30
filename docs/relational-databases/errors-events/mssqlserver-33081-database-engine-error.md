---
description: MSSQLSERVER_33081
title: MSSQLSERVER_33081 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33081 (Database Engine error)
ms.assetid: 839705e7-fa37-4c0d-9f3f-95a9eab98bcf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8be9f2bec59b51f75a7832768663535175946f60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208420"
---
# <a name="mssqlserver_33081"></a>MSSQLSERVER_33081
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|33081|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SEC_DLL_TRUST_VERIFICATION_FAILED|  
|Testo del messaggio|Impossibile caricare il provider del servizio di crittografia '%.*ls' a causa di una firma Authenticode o un percorso file non valido.  Controllare i messaggi precedenti per altri errori.|  
  
## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è stato in grado di caricare il provider del servizio di crittografia elencato nel messaggio di errore. Questo errore può essere provocato da numerose API Windows non corrette. Il buffer circolare contiene il nome dell'API non corretta e il codice di errore Windows restituito dall'API. Per ottenere altre informazioni, eseguire una query sulla vista sys.dm_os_ring_buffers nel modo seguente.  
  
```  
SELECT * FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_SECURITY_ERROR';  
```  
  
## <a name="user-action"></a>Azione dell'utente  
Per ricercare la causa del problema, individuare il codice di errore Windows in MSDN (https://msdn.microsoft.com/). Risolvere l'errore o contattare il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)] per ulteriori informazioni. Se è necessario contattare il Servizio Supporto Tecnico Clienti Microsoft, raccogliere le informazioni seguenti che dovranno essere comunicate al personale di supporto:  
  
-   Log degli errori che mostra l'errore relativo all'impossibilità di caricare il provider di crittografia.  
  
-   Output dell'istruzione seguente:  
  
    ```  
    SELECT * FROM sys.dm_os_ring_buffers   
    WHERE ring_buffer_type = 'RING_BUFFER_SECURITY_ERROR';  
    ```  
  
> [!NOTE]  
> Tentare di raccogliere subito le informazioni sul buffer circolare poiché possono essere perse durante un riavvio.  
  
