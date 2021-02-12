---
description: Valutazione degli oggetti di database di Access per la conversione (AccessToSQL)
title: Valutazione degli oggetti di database di Access per la conversione (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- assessing SQL
- assessing syntax
- assessment reports
- creating assessment reports
- estimating migration effort
- reports
- SQL, assessing
- syntax, assessing
ms.assetid: 8b9e23d6-da62-437a-8c05-8ad2628b9441
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07adac4e991afd70b70a5f087a881f1933baaae1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076422"
---
# <a name="assessing-access-database-objects-for-conversion-accesstosql"></a>Valutazione degli oggetti di database di Access per la conversione (AccessToSQL)
Prima di caricare oggetti e migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è necessario determinare la quantità di migrazione che avrà esito positivo e il tempo necessario per la conversione. SSMA è in grado di creare un report di valutazione che mostra la percentuale di oggetti convertiti correttamente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure stime della sintassi e del tempo per l'esecuzione della migrazione. SSMA consente inoltre di visualizzare i problemi specifici che hanno causato errori di conversione.  
  
## <a name="creating-assessment-reports"></a>Creazione di report di valutazione  
Quando crea un report di valutazione, SSMA converte gli oggetti di database di Access selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure la sintassi, quindi Visualizza i risultati.  
  
**Per creare un report di valutazione**  
  
1.  In Esplora metadati di Access selezionare il database o i database che si desidera valutare.  
  
2.  Per omettere singoli oggetti, deselezionare le caselle di controllo accanto agli oggetti che non si desidera valutare.  
  
3.  Fare clic con il pulsante destro del mouse su **database** e quindi scegliere **Crea report**.  
  
    È anche possibile analizzare singoli oggetti facendo clic con il pulsante destro del mouse su un oggetto e scegliendo **Crea report**.  
  
    SSMA Mostra lo stato di avanzamento nella barra di stato nella parte inferiore della finestra. Se il riquadro di output è visibile, i messaggi vengono visualizzati anche nel riquadro di output.  
  
Al termine della valutazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene visualizzata la finestra Migration Assistant per Access: report di valutazione.  
  
## <a name="using-assessment-reports"></a>Utilizzo dei report di valutazione  
La finestra report di valutazione contiene tre riquadri: un visualizzatore, un riquadro dei dettagli e un riquadro messaggi.  
  
-   Il riquadro di esplorazione consente di visualizzare gli oggetti che sono stati valutati. È possibile fare clic su elementi in questo riquadro per eseguire il drill-down a singole tabelle, indici e chiavi.  
  
-   Il riquadro dei dettagli Mostra le statistiche di conversione per l'oggetto selezionato.  
  
-   Il riquadro messaggi Mostra gli errori, gli avvisi e i messaggi informativi per la conversione e le stime temporali per eseguire la migrazione e i singoli passaggi di correzione degli errori.  
  
È necessario correggere gli errori prima di eseguire di nuovo il report di valutazione o convertire gli schemi. Per individuare gli errori, fare clic sul pulsante **errori** nel riquadro messaggi, quindi espandere ogni errore per visualizzare un elenco di oggetti in cui si è verificato l'errore. Se si fa clic su un oggetto nel riquadro messaggi, tutti gli errori e gli avvisi relativi a tale oggetto verranno visualizzati nel riquadro dei dettagli.  
  
## <a name="next-step"></a>passaggio successivo  
[Conversione di oggetti di database di Access](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
