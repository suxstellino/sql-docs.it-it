---
description: Garantire spazio sufficiente per TempDB
title: Verifica dello spazio TempDB sufficiente | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- TempDB space in RDS [ADO]
ms.assetid: 09130db1-6248-4234-a1e5-a9c8e1622c06
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f2ea2da9100b117a1526d7a36c6d1cc51a7a636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036491"
---
# <a name="ensuring-sufficient-tempdb-space"></a>Garantire spazio sufficiente per TempDB
Se si verificano errori durante la gestione di oggetti [Recordset](../../reference/ado-api/recordset-object-ado.md) che richiedono spazio di elaborazione in Microsoft SQL Server 6,5, potrebbe essere necessario aumentare le dimensioni di tempdb. Per alcune query è necessario uno spazio di elaborazione temporaneo. ad esempio, una query con una clausola ORDER BY richiede un tipo di **Recordset**, che richiede uno spazio temporaneo.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
> [!IMPORTANT]
>  Leggere questa procedura prima di eseguire le azioni perché non è facile compattare un dispositivo dopo che è stato espanso.  
  
> [!NOTE]
>  Per impostazione predefinita, in Microsoft SQL Server 7,0 e versioni successive, TempDB viene impostato in modo da espandersi automaticamente in base alle esigenze. Questa procedura può pertanto essere necessaria solo nei server che eseguono versioni precedenti alla 7,0.  
  
### <a name="to-increase-the-tempdb-space-on-sql-server-65"></a>Per aumentare lo spazio TempDB in SQL Server 6,5  
  
1.  Avviare Microsoft SQL Server Enterprise Manager, aprire l'albero per il server, quindi aprire l'albero dispositivi di database.  
  
2.  Selezionare un dispositivo (fisico) da espandere, ad esempio master, quindi fare doppio clic sul dispositivo per aprire la finestra di dialogo **modifica dispositivo database** .  
  
     In questa finestra di dialogo viene visualizzata la quantità di spazio utilizzata dai database correnti.  
  
3.  Nella casella **dimensioni** aumentare il dispositivo fino alla quantità desiderata (ad esempio, 50 megabyte (MB) di spazio su disco rigido).  
  
4.  Fare clic su **Cambia ora** per aumentare la quantità di spazio in cui il tempdb (Logical) può espandersi.  
  
5.  Aprire l'albero dei database nel server, quindi fare doppio clic su **tempdb** per aprire la finestra di dialogo **modifica database** . Nella scheda **database** è elencata la quantità di spazio attualmente allocata a tempdb (**dimensione dati**). Per impostazione predefinita, questo valore è pari a 2 MB.  
  
6.  Nel gruppo **dimensioni** fare clic su **Espandi**. I grafici mostrano lo spazio disponibile e allocato in ogni dispositivo fisico. Le barre in colore marrone rappresentano lo spazio disponibile.  
  
7.  Selezionare un **dispositivo di log**, ad esempio master, per visualizzare le dimensioni disponibili nella casella **dimensioni (MB)** .  
  
8.  Fare clic su **Espandi ora** per allocare lo spazio al database tempdb.  
  
     Nella finestra di dialogo **modifica database** vengono visualizzate le nuove dimensioni allocate per tempdb.  
  
 Per ulteriori informazioni su questo argomento, consultare il file della Guida di Microsoft SQL Server Enterprise Manager per la finestra di dialogo "Espandi database".  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)