---
description: Valutazione dei database MySQL per la conversione (MySQLToSQL)
title: Valutazione dei database MySQL per la conversione (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment reports
ms.assetid: 2a56a003-3b0f-453a-963c-00c9e40933ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61e8cd99640b45cfdfd74de60beb2fcbd9393308
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069196"
---
# <a name="assessing-mysql-databases-for-conversion-mysqltosql"></a>Valutazione dei database MySQL per la conversione (MySQLToSQL)
Prima di caricare oggetti ed eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è necessario determinare la complessità della migrazione e il tempo necessario per la migrazione. SSMA è in grado di creare un report di valutazione che mostra la percentuale di oggetti che verranno convertiti correttamente. SSMA consente inoltre di visualizzare i problemi specifici che provocano errori di conversione.  
  
## <a name="creating-assessment-reports"></a>Creazione di report di valutazione  
Quando crea il report di valutazione, SSMA converte gli oggetti di database MySQL selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure la sintassi, quindi Visualizza i risultati.  
  
**Per creare un report di valutazione**  
  
1.  In MySQL Metadata Explorer selezionare gli schemi da valutare.  
  
2.  Per omettere singoli oggetti, deselezionare le caselle di controllo accanto a tali oggetti.  
  
3.  Fare clic con il pulsante destro del mouse su **schemi** e quindi scegliere **Crea report**.  
  
    Fare clic con il pulsante destro del mouse su un oggetto per analizzare i singoli oggetti. Selezionare quindi **Crea report**.  
  
    SSMA indicherà lo stato di avanzamento nella barra di stato nella parte inferiore della finestra. Se il riquadro di output è visibile, i messaggi vengono visualizzati anche nel riquadro di output.  
  
    Al termine della valutazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà visualizzata la finestra Migration Assistant per MySQL, report di valutazione.  
  
## <a name="using-assessment-reports"></a>Utilizzo dei report di valutazione  
La finestra report di valutazione contiene tre riquadri:  
  
-   Il riquadro sinistro contiene la gerarchia di oggetti inclusi nel report di valutazione. È possibile esplorare la gerarchia e selezionare oggetti e categorie di oggetti per visualizzare le statistiche di conversione e il codice.  
  
-   Il contenuto del riquadro destro dipende dall'elemento selezionato nel riquadro sinistro.  
  
    Se si seleziona un gruppo di oggetti, ad esempio schema, il riquadro destro contiene un riquadro statistiche di conversione e il riquadro oggetti per categorie. Il riquadro statistiche di conversione Mostra le statistiche di conversione per gli oggetti selezionati. Il riquadro oggetti per categorie Mostra le statistiche di conversione per l'oggetto o le categorie di oggetti.  
  
    Se è selezionata una funzione, una stored procedure, una tabella o una vista, il riquadro destro contiene le statistiche, il codice sorgente e il codice di destinazione.  
  
    -   L'area superiore mostra le statistiche complessive per l'oggetto. Potrebbe essere necessario espandere **statistiche** per visualizzare queste informazioni.  
  
    -   L'area di origine Mostra il codice sorgente dell'oggetto selezionato nel riquadro sinistro. Le aree evidenziate mostrano codice sorgente problematico.  
  
    -   Nell'area di destinazione viene visualizzato il codice convertito. Il testo rosso Mostra codice problematico e messaggi di errore.  
  
-   Il riquadro inferiore mostra i messaggi di conversione raggruppati per numero di messaggio. È possibile fare clic su **errori**, **avvisi** o **informazioni** per visualizzare le categorie di messaggi, quindi espandere un gruppo di messaggi. Fare clic su un singolo messaggio per selezionare l'oggetto nel riquadro a sinistra e visualizzare i dettagli nel riquadro di destra.  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>Analisi dei problemi di conversione tramite il report di valutazione  
Il riquadro statistiche di conversione Mostra le statistiche di conversione. Se la percentuale di una categoria è inferiore al 100%, è necessario determinare il motivo per cui la conversione non è riuscita.  
  
**Per visualizzare i problemi di conversione**  
  
1.  Creare il report di valutazione utilizzando le istruzioni riportate nella procedura precedente.  
  
2.  Nel riquadro sinistro espandere schemi o cartelle con un'icona di errore rossa. Continuare a espandere gli elementi fino a quando non si seleziona un singolo elemento che non è riuscito a convertire.  
  
3.  Nella parte superiore del riquadro di origine fare clic su **problema successivo**.  
  
    Il codice problematico è evidenziato, così come il codice correlato nel riquadro di spostamento di destinazione.  
  
4.  Esaminare tutti i messaggi di errore e quindi determinare cosa si vuole fare con l'oggetto che ha causato il problema di conversione.  
  
-   Aggiornare la sintassi di MySQL in SSMA. È possibile aggiornare la sintassi solo per le procedure e le funzioni. Per aggiornare la sintassi, selezionare l'oggetto nel riquadro MySQL Metadata Explorer, fare clic sulla scheda **SQL** , quindi modificare il codice SQL. Quando ci si allontana dall'elemento, verrà richiesto di salvare la sintassi aggiornata. È possibile visualizzare gli errori segnalati per l'oggetto nella scheda **report** .  
  
-   In MySQL è possibile modificare l'oggetto MySQL per rimuovere o modificare il codice problematico. Per caricare il codice aggiornato in SSMA, sarà necessario aggiornare i metadati. Per altre informazioni, vedere [connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).  
  
-   È possibile escludere l'oggetto dalla migrazione. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Metadata Explorer e MySQL Metadata Explorer deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure ed eseguire la migrazione dei dati da MySQL.  
  
## <a name="next-step"></a>passaggio successivo  
[Conversione dei database MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
