---
title: Visualizzare lo stato di un database con mirroring
description: Informazioni su come visualizzare lo stato di un database configurato per il mirroring all'interno dell'interfaccia utente grafica di SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- database mirroring [SQL Server], states
ms.assetid: 544f4194-253e-4c57-96ca-31c16301434f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 991a87b45632c49853d9ac4c97b29c743e18e781
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352160"
---
# <a name="view-the-state-of-a-mirrored-database-sql-server-management-studio"></a>Visualizzazione dello stato di un database con mirroring (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Durante una sessione di mirroring del database, è possibile visualizzare lo stato nella pagina **Mirroring** della finestra di dialogo **Proprietà database** .  
  
### <a name="to-view-the-status-of-a-database-mirroring-session"></a>Per visualizzare lo stato di una sessione di mirroring del database  
  
1.  Dopo aver attivato la connessione all'istanza del server principale, in Esplora oggetti fare clic sul nome del server per espandere l'albero.  
  
2.  Espandere **Database** e selezionare il database per il mirroring.  
  
3.  Fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi fare clic su **Server mirror**. Viene visualizzata la pagina **Mirroring** della finestra di dialogo **Proprietà database** .  
  
4.  In seguito all'avvio del mirroring, nel pannello **Stato** verrà visualizzato lo stato della sessione di mirroring del database nel momento in cui si è selezionata la pagina **Mirroring** o si è fatto clic sul pulsante **Aggiorna** . Gli stati possibili sono indicati di seguito:  
  
    |Stati|Spiegazione|  
    |------------|-----------------|  
    |\<blank>|Non esiste alcuna sessione di mirroring del database e non ci sono attività da segnalare nella pagina **Mirroring** .|  
    |Paused|Il database principale è in esecuzione, ma non viene inviato alcun log al server mirror. La copia mirror del database non è disponibile.|  
    |Nessuna connessione|L'istanza del server principale non può connettersi ai partner o all'istanza del server di controllo del mirroring (se disponibile).|  
    |Sincronizzazione in corso|Il contenuto del database mirror è in ritardo rispetto a quello del database principale. L'istanza del server principale invia record di log all'istanza del server mirror, che applica le modifiche al database mirror per eseguirne il rollforward.<br /><br /> All'avvio della sessione di mirroring del database, i database mirror e principale sono in stato di sincronizzazione.|  
    |Failover|Nell'istanza del server principale è iniziato un failover manuale (inversione di ruolo), che non è stato ancora accettato dal server mirror.|  
    |Sincronizzato|Il database mirror contiene gli stessi dati del database principale. I failover manuale e automatico sono possibili *solo* in stato sincronizzato.|  
  
## <a name="see-also"></a>Vedere anche  
 [Stati di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
  
