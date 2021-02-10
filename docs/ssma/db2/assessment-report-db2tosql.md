---
description: Report di valutazione (DB2ToSQL)
title: Report di valutazione (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9e13eba0-e3cf-4205-974f-c00f982061de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 56e4d034fde766784fb218429e1cf5a5a5e01b73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065926"
---
# <a name="assessment-report-db2tosql"></a>Report di valutazione (DB2ToSQL)
La finestra report di valutazione Mostra i risultati della conversione degli oggetti di database nella [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi e consente inoltre di stimare la complessità e il costo dei progetti di migrazione.  
  
Per accedere al report di valutazione, selezionare gli oggetti da convertire in Esplora metadati di origine, fare clic con il pulsante destro del mouse su **schemi** o **sinonimi** e quindi scegliere **Crea report**.  
  
## <a name="options"></a>Opzioni  
  
|Termine|Definizione|  
|-|-|  
|**Statistiche di conversione**|Mostra le statistiche di conversione per tipo di istruzione. Questo riquadro è visibile quando si seleziona un oggetto gruppo, ad esempio uno schema, o un oggetto senza codice nel riquadro sinistro.|  
|**Oggetti per categorie**|Mostra il numero di oggetti per categoria. Questo riquadro è visibile solo quando si seleziona un oggetto gruppo, ad esempio uno schema, o un oggetto senza codice nel riquadro sinistro.|  
|**Statistiche**|Mostra le statistiche di conversione per l'oggetto selezionato. Questo riquadro è visibile solo quando si seleziona un singolo oggetto con codice nel riquadro sinistro. Per visualizzare questo riquadro, potrebbe essere necessario espandere **Statistics**, che si trova immediatamente sopra il riquadro di **origine** .|  
|**Origine**|Mostra il codice DB2 per l'oggetto selezionato ed evidenzia il codice che non è stato convertito in [!INCLUDE[tsql](../../includes/tsql-md.md)] . Questo riquadro è visibile solo quando si seleziona un singolo oggetto con codice nel riquadro sinistro.<br /><br />Per impostare o cancellare i segnalibri, fare clic sui numeri di riga. Utilizzare i pulsanti nella parte superiore del riquadro per spostarsi nel codice.|  
|**Destinazione**|Mostra il codice risultante della conversione [!INCLUDE[tsql](../../includes/tsql-md.md)] per l'oggetto selezionato e i messaggi di errore per il codice che non è stato convertito. Questo riquadro è visibile solo quando si seleziona un singolo oggetto con codice nel riquadro sinistro.<br /><br />Per impostare o cancellare i segnalibri, fare clic sui numeri di riga. Utilizzare i pulsanti nella parte superiore del riquadro per spostarsi nel codice.|  
|**riquadro Messaggi**|Mostra gli errori, gli avvisi e i messaggi informativi generati durante la creazione del report di valutazione. I messaggi sono raggruppati per numero. Per visualizzare il codice che ha causato l'errore, fare clic su **errori**, **avvisi** o **informazioni**, espandere la categoria di messaggi e quindi fare clic su un messaggio.|  
  
