---
description: Eseguire l'aggiornamento dal database (MySQLToSQL)
title: Aggiornare dal database (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 59a6db8f-2db6-4071-9005-928a7231de92
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 657e0b85cfb89c9a8fadf889624b75e0b064952f
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106053964"
---
# <a name="refresh-from-database-mysqltosql"></a>Eseguire l'aggiornamento dal database (MySQLToSQL)
La finestra di dialogo **Aggiorna da database** consente di selezionare gli oggetti da aggiornare dal database MySQL. Le righe nella finestra di dialogo sono codificate a colori in base allo stato dei metadati:  
  
-   Se i metadati dell'oggetto sono stati modificati localmente e nel database MySQL, la riga è blu.  
  
-   Se i metadati dell'oggetto sono stati modificati nel database MySQL ma non in SSMA, la riga è gialla.  
  
-   Se i metadati dell'oggetto sono stati modificati in locale, ma non nel database MySQL, la riga è verde.  
  
-   Se l'oggetto è nuovo nel database MySQL, la riga è rosa.  
  
È possibile specificare le impostazioni di aggiornamento oggetti predefinite nella finestra di dialogo **Impostazioni progetto** . Per altre informazioni, vedere [Impostazioni progetto &#40;sincronizzazione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
Per accedere alla finestra di dialogo **Aggiorna da database** , fare clic con il pulsante destro del mouse su un oggetto in MySQL Metadata Explorer e scegliere **Aggiorna da database**.  
  
## <a name="options"></a>Opzioni  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Comprimi (-)**|Comprime tutti i gruppi di oggetti per nascondere i singoli oggetti.|  
|**Espandi (+)**|Espandere tutti i gruppi di oggetti per visualizzare i singoli oggetti.|  
|**Nascondi/Mostra oggetti uguali**|Nasconde gli oggetti dall'elenco se i metadati dell'oggetto sono gli stessi nel database MySQL e in SSMA.|  
|**Aggiorna da database (pulsante freccia)**|Usare il pulsante freccia per specificare che i metadati per gli oggetti selezionati devono essere aggiornati in SSMA.|  
|**Non aggiornare dal database (pulsante X)**|Usare il pulsante X per specificare che i metadati per gli oggetti selezionati non devono essere aggiornati in SSMA.|  
|**Legenda**|Consente di visualizzare una finestra di dialogo **Legenda** . La legenda contiene il mapping tra i colori delle righe e gli Stati dei metadati.<br /><br />Per lasciare la finestra di dialogo **Legenda** nella parte superiore della finestra di dialogo **Aggiorna da database** , selezionare la casella di controllo **Mostra in primo piano** .|  
  
