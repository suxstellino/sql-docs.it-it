---
title: Importare dati da tabelle
description: Importare dati da tabelle e apportare modifiche ai dati in blocco in un modello. Utilizzare questa procedura per aggiungere, aggiornare ed eliminare dati nel database di Master Data Services.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad5b83b1-8e40-4ef8-9ba8-4ea17a58b672
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e9faf7de8d3e7517d47c61ba3a9b6e00cd2e2b09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347736"
---
# <a name="import-data-from-tables-master-data-services"></a>Importare dati dalle tabelle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  È possibile aggiungere dati e apportare modifiche ai dati in bulk a un modello in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
 **Prerequisiti**  
  
-   È necessario disporre dell'autorizzazione per inserire i dati nel STG. \<name> _Leaf, il STG. \<name> _Consolidated, STG. \<name> _Relationship tabella nel [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] database.  
  
-   È necessario disporre delle autorizzazioni per eseguire il stg.udp_ \<name> _Leaf, STG. udp \_ \<name> _Consolidated o il _Relationship stg. UDP \_ \<name> stored procedure nel [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] database.  
  
-   Il modello non deve avere uno stato di **Commit completato**.  
  
 **Per aggiungere, aggiornare ed eliminare dati nel database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]**  
  
1.  Preparare i membri da importare nella tabella di staging appropriata del database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], ad esempio fornire valori per i campi obbligatori. Per una panoramica sulle tabelle di staging, vedere [Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
    -   Per i membri foglia la tabella è STG. \<name> _Leaf, dove \<name> fa riferimento all'entità corrispondente. Per informazioni sui campi obbligatori, vedere [Tabella di gestione temporanea dei membri foglia &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
    -   Per i membri consolidati, la tabella è STG. \<name> _Consolidated. Per informazioni sui campi obbligatori, vedere [Tabella di gestione temporanea di membri consolidati &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md).  
  
    -   Per lo spostamento della posizione dei membri nelle gerarchie esplicite, la tabella è STG. \<name> _Relationship. Per informazioni sui campi obbligatori, vedere [Tabella di gestione temporanea delle relazioni &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md).  
  
         Per una panoramica sullo spostamento dei membri nelle gerarchie esplicite, vedere [Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
    -   Usare il valore del campo **ImportType** per specificare che si stanno creando nuovi membri, disattivando membri o eliminando membri. Per altre informazioni sui valori, vedere [Tabella di gestione temporanea dei membri foglia &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md) e [Tabella di gestione temporanea di membri consolidati &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md).  
  
         Per una panoramica sulla disattivazione e sull'eliminazione dei membri, vedere [Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
2.  Aprire [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] e connettersi all'istanza del motore di database per il database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
     Per ulteriori informazioni, vedere [SQL Server Management Studio](../ssms/sql-server-management-studio-ssms.md).  
  
3.  Importare dati nelle tabelle di staging usando l'Importazione/Esportazione guidata [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
     Per altre informazioni, vedere [SQL Server Import and Export Wizard](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md).  
  
4.  Caricare i dati dalle tabelle di staging nelle tabelle di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], effettuando una delle operazioni seguenti  
  
    -   Eseguire la stored procedure di staging che corrisponde alla tabella di staging in cui spostare i dati.  
  
         Per una panoramica sulle stored procedure e sulle tabelle di staging, vedere [Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md). Per altre informazioni sui parametri delle stored procedure di gestione temporanea e un esempio di codice, vedere [Stored procedure di gestione temporanea &#40;Master Data Services&#41;](../master-data-services/staging-stored-procedure-master-data-services.md).  
  
    -   Usare l'area funzionale **Gestione integrazione** di Gestione dati master.  
  
         Nella pagina **Batch di gestione temporanea** selezionare dall'elenco a discesa il modello al quale si aggiungeranno dati e quindi fare clic su **Avvia batch**. Lo stato dell'elaborazione batch è indicato nel campo **Stato** . Per altre informazioni sugli stati, vedere [Stati di importazione &#40;Master Data Services&#41;](../master-data-services/import-statuses-master-data-services.md).  
  
         ![Batch di gestione temporanea in Gestione dati master](../master-data-services/media/mds-stagingbatchespage.png "Batch di gestione temporanea in Gestione dati master")  
  
         Il processo di gestione temporanea viene avviato a intervalli determinati dall'impostazione **Intervallo batch di gestione temporanea** in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Per altre informazioni, vedere [Impostazioni di sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
5.  Visualizzare gli errori che si sono verificati durante la gestione temporanea. Per altre informazioni, vedere [Visualizzare gli errori che si verificano durante il processo di gestione temporanea &#40;Master Data Services&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md) e [Errori del processo di gestione temporanea &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md).  
  
6.  Convalidare i dati rispetto a regole di business.  
  
     In Gestione dati master passare all'area funzionale **Visualizzatore** per il modello e quindi applicare le regole di business per convalidare i dati. Per altre informazioni, vedere [Convalidare membri specifici rispetto a regole di business &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md). È anche possibile usare una stored procedure per convalidare i dati. Per altre informazioni, vedere [Stored procedure di convalida &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md).  
  
     Quando si caricano dati dalle tabelle di staging, i dati non vengono convalidati automaticamente rispetto alle regole di business. Per altre informazioni sulla convalida e sulla sua esecuzione, vedere [Convalida &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md).  
  
