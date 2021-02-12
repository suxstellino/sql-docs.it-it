---
description: Valutazione degli oggetti di database SAP ASE per la conversione (SybaseToSQL)
title: Valutazione degli oggetti di database di SAP ASE per la conversione (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eb996b7c-1eef-4f73-b5e6-2fa6faf7336c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e0926c350d02bf2cdbb5ae7fe4fdefbda3ef6863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077222"
---
# <a name="assessing-sap-ase-database-objects-for-conversion-sybasetosql"></a>Valutazione degli oggetti di database SAP ASE per la conversione (SybaseToSQL)
Prima di caricare oggetti e migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in Azure SQL, è necessario determinare la complessità della migrazione e il tempo necessario. SSMA è in grado di creare un report di valutazione che mostra la percentuale di oggetti e procedure che verranno convertiti correttamente in [!INCLUDE[tsql](../../includes/tsql-md.md)] . SSMA consente inoltre di visualizzare i problemi specifici che possono causare errori di conversione.  
  
## <a name="create-assessment-reports"></a>Creazione di report di valutazione  
Quando si crea questo report di valutazione, SSMA converte gli oggetti di database di SAP Adaptive Server Enterprise (ASE) selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la sintassi SQL di Azure e quindi Visualizza i risultati.  
  
**Per creare un report di valutazione**  
  
1.  In Sybase Metadata Explorer selezionare i database che si desidera valutare.  
  
2.  Per omettere singoli oggetti, deselezionare le caselle di controllo accanto agli oggetti che non si desidera valutare.  
  
3.  Fare clic con il pulsante destro del mouse su **database** e quindi scegliere **Crea report**.  
  
    È anche possibile analizzare singoli oggetti facendo clic con il pulsante destro del mouse su un oggetto e quindi scegliendo **Crea report**.  
  
    SSMA Mostra lo stato di avanzamento nella barra di stato nella parte inferiore della finestra. Se il riquadro di output è visibile, saranno visualizzati anche tutti i messaggi correlati.  
  
    Al termine della valutazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà visualizzata la finestra di Migration Assistant per il report Sybase: Assessment.  
  
## <a name="use-assessment-reports"></a>Usare i report di valutazione  
La finestra report di valutazione contiene tre riquadri:  
  
-   Il riquadro sinistro contiene la gerarchia di oggetti inclusi nel report di valutazione. È possibile esplorare la gerarchia e selezionare oggetti e categorie di oggetti per visualizzare le statistiche di conversione e il codice.  
  
-   Il contenuto del riquadro destro varia in base all'elemento selezionato nel riquadro sinistro.  
  
    Se si seleziona un gruppo di oggetti, ad esempio uno schema, o una tabella, nel riquadro destro verranno visualizzati due riquadri. Il riquadro **statistiche di conversione** Mostra le statistiche di conversione per gli oggetti selezionati. Il riquadro **oggetti per categorie** Mostra le statistiche di conversione per l'oggetto o le categorie di oggetti.  
  
    Se è selezionata una stored procedure, una vista o un trigger, il riquadro destro contiene le statistiche, il codice sorgente e il codice di destinazione.  
  
    -   L'area superiore mostra le statistiche complessive per l'oggetto. Potrebbe essere necessario espandere **statistiche** per visualizzare queste informazioni. 
    -   L'area di origine Mostra il codice sorgente dell'oggetto selezionato nel riquadro sinistro. Le aree evidenziate mostrano codice sorgente problematico.  
    -   Nell'area di destinazione viene visualizzato il codice convertito. Il testo rosso Mostra codice problematico e messaggi di errore.  
  
-   Il riquadro inferiore mostra i messaggi di conversione raggruppati per numero di messaggio. Selezionare **errori**, **avvisi** o **informazioni** per visualizzare le categorie di messaggi, quindi espandere un gruppo di messaggi. Fare clic su un singolo messaggio per selezionare l'oggetto nel riquadro a sinistra e quindi visualizzare i dettagli nel riquadro di destra.  
  
## <a name="analyze-conversion-problems-by-using-the-assessment-report"></a>Analizzare i problemi di conversione tramite il report di valutazione  
I **riquadri delle statistiche di conversione** mostrano le statistiche di conversione. Se la percentuale di una categoria è inferiore al 100%, è necessario determinare il motivo per cui la conversione non è riuscita.  
  
**Per visualizzare i problemi di conversione**  
  
1.  Creare il report di valutazione utilizzando le istruzioni riportate nella procedura precedente.  
  
2.  Nel riquadro sinistro espandere schemi o cartelle con un'icona di errore rossa. Continuare a espandere gli elementi fino a quando non si seleziona un singolo elemento che non è riuscito a convertire.  
  
3.  Nella parte superiore del riquadro di origine selezionare **problema successivo**.  
    Il codice problematico è evidenziato, così come il codice correlato nel riquadro di **spostamento di destinazione** .  
  
4.  Esaminare tutti i messaggi di errore e quindi determinare cosa si vuole fare con l'oggetto che ha causato il problema di conversione:  
  
    -   Aggiornare la sintassi dell'ambiente del servizio app in SSMA. È possibile aggiornare la sintassi solo per stored procedure e trigger. Per aggiornare la sintassi, selezionare l'oggetto nel riquadro Sybase Metadata Explorer, fare clic sulla scheda **SQL** , quindi modificare il codice SQL. Quando ci si allontana dall'elemento, viene richiesto di salvare la sintassi aggiornata. Visualizzare gli errori segnalati per l'oggetto nella scheda **report** .  
  
    -   Nell'ambiente del servizio app è possibile modificare l'oggetto ASE per rimuovere o rivedere il codice problematico. Per caricare il codice aggiornato in SSMA, sarà necessario aggiornare i metadati. Per altre informazioni, vedere [connessione a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
    -   È possibile escludere l'oggetto dalla migrazione. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in Esplora metadati di SQL Azure e in Esplora metadati Sybase deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL ed eseguire la migrazione dei dati dall'ambiente del servizio app.
  
## <a name="next-steps"></a>Passaggi successivi  
[Conversione di oggetti di database SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database SAP ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
