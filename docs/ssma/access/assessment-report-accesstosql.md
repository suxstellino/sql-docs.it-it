---
description: Report di valutazione (AccessToSQL)
title: Report di valutazione (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment Report dialog box
- Conversion Report dialog box
ms.assetid: ba6f53aa-0049-4c49-8bb8-607a8bfaa737
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7c7e8b8e282a59144628f70e0b2efc06a1f4b59f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076392"
---
# <a name="assessment-report-accesstosql"></a>Report di valutazione (AccessToSQL)
La finestra report di valutazione Mostra i risultati della conversione degli oggetti di database nella [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi e consente inoltre di stimare la complessità e il costo dei progetti di migrazione.  
  
Per creare un report di valutazione, selezionare gli oggetti da convertire in Esplora metadati di origine, fare clic con il pulsante destro del mouse su **database** e quindi scegliere **Crea report**. È inoltre possibile visualizzare questo report automaticamente dopo la conversione degli schemi. Il nome del report sarà tuttavia un report di conversione. Per ulteriori informazioni, vedere [Impostazioni progetto (GUI) (SSMA Common)](../sybase/project-settings-gui-sybasetosql.md).  
  
## <a name="options"></a>Opzioni  
**Riquadro di esplorazione**  
Contiene una gerarchia di oggetti nel report di valutazione. Espandere cartelle per visualizzare singoli oggetti e sottocomponenti. Quando si fa clic su una categoria o un oggetto, nel riquadro dei dettagli vengono visualizzate le statistiche relative alla conversione per tale categoria o oggetto.  
  
**Riquadro dei dettagli**  
Mostra le statistiche di conversione o i messaggi di errore e di avviso per l'oggetto selezionato. Se, ad esempio, è selezionata la cartella tabelle, nel riquadro dei dettagli verrà visualizzato il numero di chiavi esterne, indici, chiavi primarie e tabelle convertite.  
  
**riquadro Messaggi**  
Mostra gli errori, gli avvisi e i messaggi informativi generati al momento della creazione del report di valutazione. I messaggi sono raggruppati per numero.  
  
Per visualizzare i dettagli del messaggio, fare clic su **errori**, **avvisi** o **messaggi**, quindi espandere un messaggio. SSMA visualizzerà l'elenco di oggetti con questo errore. Fare clic su un oggetto per visualizzare tutti i dettagli di conversione per l'oggetto.  
  
## <a name="see-also"></a>Vedere anche  
[Guida di riferimento all'interfaccia utente (accesso)](./user-interface-reference-accesstosql.md)  
