---
description: Eseguire l'aggiornamento dal database (OracleToSQL)
title: Aggiornare dal database (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 84492f44-c368-4c75-954d-7307a2d2bbc0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ecf7edd43af59f2d388f83b118ad8b6a1583b0be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067646"
---
# <a name="refresh-from-database-oracletosql"></a>Eseguire l'aggiornamento dal database (OracleToSQL)
La finestra di dialogo **Aggiorna da database** consente di selezionare gli oggetti da aggiornare dal database Oracle. Le righe nella finestra di dialogo sono codificate a colori in base allo stato dei metadati:  
  
-   Se i metadati dell'oggetto sono stati modificati localmente e nel database Oracle, la riga è blu.  
  
-   Se i metadati dell'oggetto sono stati modificati nel database Oracle, ma non in SSMA, la riga è gialla.  
  
-   Se i metadati dell'oggetto sono stati modificati in locale, ma non nel database Oracle, la riga è verde.  
  
-   Se l'oggetto è nuovo nel database Oracle, la riga è rosa.  
  
È possibile specificare le impostazioni di aggiornamento oggetti predefinite nella finestra di dialogo **Impostazioni progetto** . Per altre informazioni, vedere [Impostazioni progetto&#40;sincronizzazione&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-synchronization-oracletosql.md).  
  
Per accedere alla finestra di dialogo **Aggiorna da database** , fare clic con il pulsante destro del mouse su un oggetto in Oracle Metadata Explorer e scegliere **Aggiorna da database**.  
  
## <a name="options"></a>Opzioni  
**Comprimi (-)**  
Comprime tutti i gruppi di oggetti per nascondere i singoli oggetti.  
  
**Espandi (+)**  
Espandere tutti i gruppi di oggetti per visualizzare i singoli oggetti.  
  
**Nascondi/Mostra oggetti uguali**  
Nasconde gli oggetti dall'elenco se i metadati dell'oggetto sono gli stessi nel database Oracle e in SSMA.  
  
**Aggiorna da database (pulsante freccia)**  
Usare il pulsante freccia per specificare che i metadati per gli oggetti selezionati devono essere aggiornati in SSMA.  
  
**Non aggiornare dal database (pulsante X)**  
Usare il pulsante X per specificare che i metadati per gli oggetti selezionati non devono essere aggiornati in SSMA.  
  
**Legenda**  
Consente di visualizzare una finestra di dialogo **Legenda** . La legenda contiene il mapping tra i colori delle righe e gli Stati dei metadati.  
  
Per lasciare la finestra di dialogo **Legenda** nella parte superiore della finestra di dialogo **Aggiorna da database** , selezionare la casella di controllo **Mostra in primo piano** .  
  
