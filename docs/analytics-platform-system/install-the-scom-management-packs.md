---
title: Installare i Management Pack SCOM
description: Attenersi alla seguente procedura per scaricare e installare i Management Pack System Center Operations Manager (SCOM) per SQL Server PDW. I Management Pack sono necessari per monitorare SQL Server PDW da SCOM.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: d8f4145b85d505ccdf1d0fe26b22f2cdf02d9e90
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641494"
---
# <a name="install-sql-server-operations-manager-scom-management-packs-for-analytics-platform-system"></a>Installare SQL Server Management Pack Operations Manager (SCOM) per il sistema di piattaforma di analisi
Attenersi alla seguente procedura per scaricare e installare i Management Pack System Center Operations Manager (SCOM) per SQL Server PDW. I Management Pack sono necessari per monitorare SQL Server PDW da SCOM.  
  
## <a name="before-you-begin"></a><a name="BeforeBegin"></a>Prima di iniziare  
**Prerequisiti**  
  
System Center Operations Manager necessario che sia installato e in esecuzione. SQL Server PDW 2012 richiede System Center Operations Manager 2007 R2, System Center Operations Manager 2012 o System Center Operations Manager 2012 Service Pack 1.  
  
## <a name="step-1-download-the-management-packs"></a><a name="Step1"></a>Passaggio 1: scaricare i Management Pack  
Per il carico di lavoro PDW APS, scaricare il [Management Pack di System Center per la piattaforma di strumenti analitici Microsoft](https://go.microsoft.com/fwlink/?LinkId=396857).  
  
Per gestione Appliance, scaricare il [Management Pack di Base SQL Server Appliance](/previous-versions/system-center/packs/gg602398(v=technet.10)).  
  
Per le versioni precedenti di PDW senza APS, scaricare[System Center Monitoring Pack per Microsoft SQL Server 2012 Parallel Data warehouse appliance](./download-and-apply-microsoft-updates.md?view=aps-pdw-2016-au7&preserve-view=true).  
  
<!-- MISSING LINKS - For the HDInsight workload, download the [System Center Management Pack for HDInsight](https://go.microsoft.com/fwlink/?LinkId=390208).  -->
  
## <a name="step-2-install-the-management-packs"></a><a name="Step2"></a>Passaggio 2: installare i Management Pack  
  
### <a name="install-the-sql-server-appliance-base-management-pack"></a>Installare il Management Pack di base di SQL Server Appliance  
  
1.  Per eseguire l'installazione, fare doppio clic sul Management Pack di base SQL Server Appliance scaricato.  
  
2.  Accettare il contratto di licenza e fare clic su **Avanti**.  
  
    ![Accetta il contratto di licenza](./media/install-the-scom-management-packs/SCOM_licnse_agrmt.png "SCOM_licnse_agrmt")  
  
3.  Selezionare una cartella di installazione personalizzata oppure utilizzare la cartella di installazione predefinita del Management Pack.  
  
    ![Selezione cartella di installazione](./media/install-the-scom-management-packs/SCOM_licnse_agrmt2.png "SCOM_licnse_agrmt2")  
  
4.  Fare clic su **Installa**.  
  
    ![Screenshot della procedura guidata per il programma di installazione del MP di monitoraggio di base di SQL Server Appliance nel passaggio conferma installazione con l'opzione di installazione cerchio in rosso.](./media/install-the-scom-management-packs/SCOM_licnse_agrmt3.png "SCOM_licnse_agrmt3")  
  
5.  Fare clic su **Close**.  
  
    ![Fare clic su Chiudi](./media/install-the-scom-management-packs/SCOM_licnse_agrmt4.png "SCOM_licnse_agrmt4")  
  
### <a name="install-the-monitoring-pack-for-sql-server-pdw-appliance"></a>Installare il Monitoring Pack per SQL Server PDW Appliance  
  
1.  Per eseguire l'installazione, fare doppio clic sul Management Pack SQL Server PDW Appliance scaricato.  
  
2.  Accettare il contratto di licenza e fare clic su **Avanti**.  
  
    ![Accettare il Contratto di licenza](./media/install-the-scom-management-packs/SCOM_licnse_agmtB.png "SCOM_licnse_agmtB")  
  
3.  Scegliere la directory che conterrà i file estratti. Per impostazione predefinita, viene visualizzata la cartella di installazione predefinita del Management Pack. Selezionare il valore predefinito oppure selezionare la cartella di installazione.  
  
    ![Seleziona cartella di installazione](./media/install-the-scom-management-packs/SCOM_licnse_agmtB1.png "SCOM_licnse_agmtB1")  
  
4.  Fare clic su **Installa**.  
  
    ![Screenshot della procedura guidata del programma di installazione di PDWMP nel passaggio conferma installazione con l'opzione di installazione cerchio in rosso.](./media/install-the-scom-management-packs/SCOM_licnse_agmtB2.png "SCOM_licnse_agmtB2")  
  
5.  Fare clic su **Close**.  
  
    ![Installazione completata](./media/install-the-scom-management-packs/SCOM_licnse_agmtB3.png "SCOM_licnse_agmtB3")  
  
## <a name="next-step"></a>passaggio successivo  
Ora che sono stati installati i Management Pack, procedere al passaggio successivo: [importare il Management Pack SCOM per la piattaforma &#40;Analytics System&#41;](import-the-scom-management-pack-for-pdw.md).  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->