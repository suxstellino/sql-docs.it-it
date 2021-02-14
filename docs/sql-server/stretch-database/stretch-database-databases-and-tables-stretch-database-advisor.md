---
description: Identificare i database e le tabelle per Stretch Database con Data Migration Assistant
title: Identificare database e tabelle
ms.date: 10/30/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, identifying databases
- Stretch Database, identifying tables
- identifying databases for Stretch Database
- identifying tables for Stretch Database
ms.assetid: 81bd93d8-eef8-4572-88d7-5c37ab5ac2bf
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0535dd0dbc410444f0dd9423da6e1bf40a4026bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079872"
---
# <a name="identify-databases-and-tables-for-stretch-database-with-data-migration-assistant"></a>Identificare i database e le tabelle per Stretch Database con Data Migration Assistant
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Per identificare i database e le tabelle candidati per Stretch Database e i problemi di blocco potenziali, scaricare ed eseguire Microsoft Data Migration Assistant.
  
## <a name="get-data-migration-assistant"></a>Ottenere Data Migration Assistant
 Scaricare e installare Data Migration Assistant da [qui](https://www.microsoft.com/download/details.aspx?id=53595). Questo strumento non è incluso nel supporto di installazione di SQL Server.  
  
## <a name="run-data-migration-assistant"></a>Eseguire Data Migration Assistant  
  
1.  Eseguire Microsoft Data Migration Assistant.  

2.  Creare un nuovo progetto di tipo **Valutazione** e assegnargli un nome.

3.  Selezionare **SQL Server** come **Source server type** (Tipo server di origine) e **Target server type** (Tipo server di destinazione).

4.  Selezionare **Crea**. 

5. Nella pagina **Options** (Opzioni) (passaggio 1) selezionare **New features recommendation** (Nuove funzionalità raccomandate). Facoltativamente, deselezionare l'opzione **Compatibility issues** (Problemi di compatibilità).

6.  Nella pagina **Select sources** (Seleziona origini) (passaggio 2) connettersi a un server, selezionare un database e quindi selezionare **Aggiungi**.

7.  Selezionare **Start Assessment** (Avvia valutazione).

## <a name="review-the-results"></a>Esaminare i risultati  
  
1.  Al termine dell'analisi, nella pagina **Review results** (Verifica risultati) (passaggio 3) selezionare l'opzione **Funzionalità consigliate** e quindi selezionare la scheda **Archiviazione**.

2.  Esaminare i consigli relativi a Stretch Database. Ogni consiglio elenca le tabelle per cui Stretch Database potrebbe essere appropriato, insieme a eventuali problemi di blocco potenziali.

## <a name="historical-note"></a>Nota storica
Stretch Database Advisor era in precedenza un componente di Gestione spazio aggiornamenti di SQL Server 2016. Stretch Database Advisor doveva essere eseguito separatamente.

Con il rilascio di Data Migration Assistant, che sostituisce ed estende Gestione spazio aggiornamenti, la funzionalità di Stretch Database Advisor è incorporata nel nuovo strumento. Non è necessario selezionare alcuna opzione per visualizzare i consigli relativi a Stretch Database. Quando si esegue una valutazione in Data Migration Assistant, i risultati relativi a Stretch Database vengono visualizzati nella scheda **Archiviazione** della finestra **Funzionalità consigliate**.
  
## <a name="next-step"></a>Passaggio successivo  
 Abilitare Stretch Database.  
  
-   Per abilitare Stretch Database in un **database**, vedere [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).  
  
-   Per abilitare Stretch Database in un'altra **tabella** quando Stretch è già abilitato nel database, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). 
  
## <a name="see-also"></a>Vedere anche  
 [Limitazioni per Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md)   
 [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
