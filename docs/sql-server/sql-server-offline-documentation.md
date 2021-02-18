---
title: Installare la documentazione di SQL Server per la visualizzazione offline
description: Informazioni su come installare la documentazione offline per SQL Server 2019, 2017, 2016, 2014 e 2012. Usare SQL Server Management Studio (SSMS) per visualizzare il contenuto offline.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
author: markingmyname
ms.author: maghan
ms.date: 08/12/2020
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: ab5ee55bf91e1a9287f22584da83ca7112dadece
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336773"
---
# <a name="install-sql-server-documentation-to-view-offline-in-ssms"></a>Installare la documentazione di SQL Server per la visualizzazione offline in SSMS

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Questo articolo descrive come scaricare e visualizzare offline il contenuto di SQL Server in [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). Il contenuto offline consente di accedere alla documentazione senza una connessione a Internet, nonostante questa sia inizialmente necessaria per scaricare il contenuto.

La documentazione offline è disponibile per SQL Server 2012 e versioni successive. Sebbene sia possibile [visualizzare il contenuto per le versioni precedenti online](/previous-versions/sql/), l'opzione offline costituisce un modo più pratico per accedere al contenuto meno recente.

- [SQL Server 2016 e versioni successive](#sql-server-2016-and-later-offline-content)
- [SQL Server 2014](#sql-server-2014-offline-content)
- [SQL Server 2012](#sql-server-2012-offline-content)

## <a name="sql-server-2016-and-later-offline-content"></a>Contenuto offline per SQL Server 2016 e versioni successive

I passaggi seguenti illustrano come caricare il contenuto offline per SQL Server 2016 e versioni successive.

1. In SSMS selezionare **Aggiungi e rimuovi contenuto della Guida** nel menu di Help Viewer.

   ![Aggiungere e rimuovere contenuto della Guida](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer si apre con la scheda Gestisci contenuto visualizzata.

2. Per trovare il contenuto della Guida più recente per SQL Server 2016 e versioni successive, nella scheda **Gestisci contenuto** scegliere **Online** in Origine dell'installazione e quindi digitare *sql server* nella barra di ricerca.

   ![Ricerca nella documentazione di SQL Server](../sql-server/media/sql-server-offline-documentation/sql-online-search.png)

   > [!Note]
   > Percorso archivio locale nella scheda Gestisci contenuto indica dove viene installato il contenuto nel computer locale. Per modificare il percorso, selezionare **Sposta**, immettere il percorso di un'altra cartella nel campo **in** e quindi fare clic su **OK**. Se l'installazione della Guida non riesce dopo aver modificato Percorso archivio locale, chiudere e riaprire Help Viewer. Verificare che la nuova posizione venga visualizzata nel percorso dell'archivio locale e tentare nuovamente l'installazione.

3. Per installare il contenuto della Guida più recente per SQL Server 2016 e versioni successive, selezionare **Aggiungi** accanto a ogni pacchetto di contenuto (documentazione) che si vuole installare e selezionare **Aggiorna** in basso a destra.

   ![Aggiunta e aggiornamento della documentazione online di SQL Server](../sql-server/media/sql-server-offline-documentation/sql-add-update.png)

   > [!NOTE]
   > Se Help Viewer si blocca durante l'aggiunta di contenuto, cambiare la riga Cache LastRefreshed="\<mm/dd/yyyy>00:00:00" nel file %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings specificando una data futura. Per altre informazioni su questo problema, vedere [Il visualizzatore della Guida di Visual Studio si blocca](/visualstudio/welcome-to-visual-studio).

4. È possibile verificare che il contenuto di SQL Server 2016 e versioni successive sia stato caricato cercando *sql server 2016* nel riquadro del contenuto a sinistra.

   ![Documentazione di SQL Server 2016 aggiornata automaticamente](../sql-server/media/sql-server-offline-documentation/sql-2016-content.png)

## <a name="sql-server-2014-offline-content"></a>Contenuto offline di SQL Server 2014

> [!IMPORTANT]
> Il contenuto Transact-SQL di SQL 2014 è disponibile solo offline.

I passaggi seguenti illustrano come caricare il contenuto offline per SQL Server 2014.

1. Scaricare il contenuto della [documentazione del prodotto per Microsoft SQL Server 2014 per ambienti firewall e proxy con restrizioni](https://www.microsoft.com/download/details.aspx?id=42557) dall'area download e salvarlo in una cartella.

2. Decomprimere il file per visualizzare il file con estensione *msha*.

   ![File di installazione della documentazione della Guida di SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-help-content-setup-msha.png)

3. In SSMS selezionare **Aggiungi e rimuovi contenuto della Guida** nel menu di Help Viewer.

   ![Aggiungi e rimuovi contenuto in Help Viewer](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer si apre con la scheda Gestisci contenuto visualizzata.

4. Per installare il pacchetto di contenuto della Guida più recente, scegliere **Disco** in Origine dell'installazione e selezionare i puntini di sospensione (...).

   ![Origine Disco in Gestisci contenuto in Help Viewer](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > Percorso archivio locale nella scheda Gestisci contenuto indica dove viene salvato il contenuto nel computer locale. Per modificare il percorso, selezionare **Sposta**, immettere il percorso di un'altra cartella nel campo **in** e quindi fare clic su **OK**.
   Se l'installazione della Guida non riesce dopo aver modificato il percorso dell'archivio locale, chiudere e riaprire Help Viewer. Verificare che la nuova posizione venga visualizzata nel percorso dell'archivio locale e tentare nuovamente l'installazione.

5. Individuare la cartella in cui è stato decompresso il contenuto. Selezionare il file **HelpContentSetup.msha** nella cartella e quindi selezionare **Apri**.

   ![Aprire il file del contenuto della Guida per SQL Server 2014 Setup.msha](../sql-server/media/sql-server-offline-documentation/sql-2014-open-msha.png)

6. Digitare *sql server 2014* nella barra di ricerca. Una volta visualizzato il contenuto della versione 2014 disponibile, selezionare **Aggiungi** accanto a ogni pacchetto di contenuto (documentazione) che si vuole installare in Help Viewer e quindi selezionare **Aggiorna**.

   ![Ricerca della documentazione di SQL Server 2014 in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2014-search.png)

   ![Aggiunta e aggiornamento della documentazione di SQL Server 2014 in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2014-add-update.png)

    > [!NOTE]
    > Se Help Viewer si blocca durante l'aggiunta di contenuto, cambiare la riga Cache LastRefreshed="\<mm/dd/yyyy>00:00:00" nel file %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings specificando una data futura. Per altre informazioni su questo problema, vedere [Il visualizzatore della Guida di Visual Studio si blocca](/visualstudio/welcome-to-visual-studio).

7. È possibile verificare che il contenuto di SQL Server 2014 sia stato installato cercando *sql server 2014* nel riquadro del contenuto a sinistra.

   ![Documentazione di SQL Server 2014 aggiornata automaticamente](../sql-server/media/sql-server-offline-documentation/sql-2014-content.png)

## <a name="sql-server-2012-offline-content"></a>Contenuto offline di SQL Server 2012

I passaggi seguenti illustrano come caricare il contenuto offline per SQL Server 2012

1. Scaricare il contenuto della [documentazione del prodotto per Microsoft SQL Server 2012 per ambienti firewall e proxy con restrizioni](https://www.microsoft.com/download/details.aspx?id=35750) dall'area download e salvarlo in una cartella.

2. Decomprimere il file per visualizzare il file con estensione *msha*.

   ![File di installazione del contenuto della Guida di SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-help-content-setup-msha.png)

3. In SSMS selezionare **Aggiungi e rimuovi contenuto della Guida** nel menu di Help Viewer.

   ![Aggiungi e rimuovi contenuto in Help Viewer](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer si apre con la scheda Gestisci contenuto visualizzata.

4. Per installare il pacchetto di contenuto della Guida più recente, scegliere **Disco** in Origine dell'installazione e selezionare i puntini di sospensione (...).

   ![Origine Disco in Gestisci contenuto in Help Viewer](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > Percorso archivio locale nella scheda Gestisci contenuto indica dove viene salvato il contenuto nel computer locale. Per modificare il percorso, selezionare **Sposta**, immettere il percorso di un'altra cartella nel campo **in** e quindi fare clic su **OK**.
   Se l'installazione della Guida non riesce dopo aver modificato il percorso dell'archivio locale, chiudere e riaprire Help Viewer. Verificare che la nuova posizione venga visualizzata nel percorso dell'archivio locale e tentare nuovamente l'installazione.

5. Individuare la cartella in cui è stato decompresso il contenuto. Selezionare il file **HelpContentSetup.msha** nella cartella e quindi selezionare **Apri**.

   ![Aprire il file del contenuto della Guida per SQL Server 2012 Setup.msha](../sql-server/media/sql-server-offline-documentation/sql-2012-open-msha.png)

6. Digitare *sql server 2012* nella barra di ricerca. Una volta visualizzato il contenuto della versione 2012 disponibile, selezionare **Aggiungi** accanto a ogni pacchetto di contenuto (documentazione) che si vuole installare in Help Viewer e quindi selezionare **Aggiorna**.

   ![Ricerca della documentazione di SQL Server 2012 in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2012-search.png)

   ![Aggiunta e aggiornamento della documentazione di SQL Server 2012 in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2012-add-update.png)

    > [!NOTE]
    > Se Help Viewer si blocca durante l'aggiunta di contenuto, cambiare la riga Cache LastRefreshed="\<mm/dd/yyyy>00:00:00" nel file %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings specificando una data futura. Per altre informazioni su questo problema, vedere [Il visualizzatore della Guida di Visual Studio si blocca](/visualstudio/welcome-to-visual-studio).

7. È possibile verificare che il contenuto di SQL Server 2012 sia stato caricato cercando *sql server 2012* nel riquadro del contenuto a sinistra.

   ![Documentazione di SQL Server 2012 aggiornata automaticamente](../sql-server/media/sql-server-offline-documentation/sql-2012-content.png)

## <a name="view-offline-documentation"></a>Visualizzare la documentazione offline

È possibile visualizzare il contenuto della Guida di SQL Server usando il menu **Guida** nella versione più recente di [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

### <a name="view-offline-help-content-in-ssms"></a>Visualizzare il contenuto della Guida offline in SSMS

Per visualizzare la Guida installata in SSMS, selezionare **Avvia in Help Viewer** dal menu Guida per avviare Help Viewer.

   ![Avvia in Help Viewer](../sql-server/media/sql-server-offline-documentation/helpviewer-view-offline.png)  

Help Viewer si apre con la scheda Gestisci contenuto visualizzata, con il sommario della Guida installata riportato nel riquadro a sinistra. Selezionare gli articoli nel sommario per visualizzarli nel riquadro a destra.

> [!Important]
> Se il riquadro del contenuto non è visibile, selezionare Contenuto sul margine sinistro. Selezionare l'icona a forma di puntina da disegno per mantenere aperto il riquadro del contenuto.  

   ![Help Viewer con contenuto](../sql-server/media/sql-server-offline-documentation/view-offline-all.png)

## <a name="life-cycle-policy"></a>Criteri del ciclo di vita

Esaminare il ciclo di vita del prodotto Microsoft per informazioni sul supporto di un prodotto, un servizio o una tecnologia specifici:

- [Criteri relativi al ciclo di vita Microsoft](https://support.microsoft.com/lifecycle/selectindex)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul contenuto archiviato e su Help Viewer, fare riferimento ai collegamenti seguenti.

- [Documentazione online di SQL Server](../sql-server/index.yml?view=sql-server-2016&preserve-view=true)
- [Documentazione online di SQL Server 2014](/previous-versions/sql/2014)
- [Versioni precedenti della documentazione online di SQL Server](previous-versions-sql-server.md)
- [Sistema di versioni per la documentazione SQL](../sql-server/versioning-system-monikers-ui-sql-server.md?view=sql-server-2016&preserve-view=true)