---
title: Abilitare e configurare FILESTREAM | Microsoft Docs
description: Per usare FILESTREAM, è prima necessario abilitarlo nell'istanza di motore di database di SQL Server. Informazioni su come abilitare FILESTREAM usando Gestione configurazione SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], enabling
ms.assetid: 78737e19-c65b-48d9-8fa9-aa6f1e1bce73
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea678a62f4453232fe8442cf2053c3b951e361ef
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250660"
---
# <a name="enable-and-configure-filestream"></a>Abilitare e configurare FILESTREAM

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Prima di iniziare a utilizzare FILESTREAM, è necessario abilitarlo nell'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. In questo argomento viene descritto come abilitare FILESTREAM utilizzando Gestione configurazione SQL Server.  
  
##  <a name="enabling-filestream"></a><a name="enabling"></a> Abilitazione di FILESTREAM  
  
#### <a name="to-enable-and-change-filestream-settings"></a>Per abilitare e modificare le impostazioni FILESTREAM  
  
1.  Fare clic sul menu **Start** , scegliere **Tutti i programmi**, [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], **Strumenti di configurazione** e quindi **Gestione configurazione SQL Server**.  
  
2.  Nell'elenco dei servizi fare clic con il pulsante destro del mouse su **Servizi di SQL Server** e quindi scegliere **Apri**.  
  
3.  Nello snap-in **Gestione configurazione SQL Server** trovare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si vuole abilitare FILESTREAM.  
  
4.  Fare clic con il pulsante destro sull'istanza e quindi scegliere **Proprietà**.  
  
5.  Nella finestra di dialogo delle **proprietà di SQL Server** fare clic sulla scheda **FILESTREAM** .  
  
6.  Selezionare la casella di controllo **Abilita FILESTREAM per l'accesso Transact-SQL** .  
  
7.  Se si vogliono leggere e scrivere dati FILESTREAM da Windows, fare clic su **Abilita FILESTREAM per l'accesso tramite il flusso di I/O dei file**. Immettere il nome della condivisione di Windows nella casella **Nome condivisione di Windows** .  
  
8.  Se ai dati FILESTREAM archiviati in tale condivisione devono accedere client remoti, selezionare **Consenti ai client remoti l'accesso tramite flusso ai dati FILESTREAM**.  
  
9. Fare clic su **Applica**.  
  
10. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]fare clic su **Nuova query** per visualizzare l'editor di query.  
  
11. Nell'editor di query immettere il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```sql  
    EXEC sp_configure filestream_access_level, 2  
    RECONFIGURE  
    ```  
  
12. Fare clic su **Execute**.  
  
13. Riavviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

##  <a name="best-practices"></a><a name="best"></a> Procedure consigliate  
  
###  <a name="physical-configuration-and-maintenance"></a><a name="config"></a> Configurazione fisica e manutenzione  
 Quando si configurano i volumi di archiviazione FILESTREAM, tenere presenti le linee guida seguenti:  
  
-   Disabilitare i nomi di file brevi nei sistemi FILESTREAM. La creazione di nomi di file brevi richiede tempi sensibilmente più lunghi. Per disabilitare i nomi di file brevi, usare l'utilità **fsutil** di Windows.  
  
-   Deframmentare regolarmente i sistemi FILESTREAM.  
  
-   Utilizzare cluster NTFS da 64 KB. I volumi compressi devono essere impostati su cluster NTFS da 4 KB.  
  
-   Disabilitare l'indicizzazione nei volumi FILESTREAM e impostare **disablelastaccess**. Per impostare **disablelastaccess**, usare l'utilità **fsutil** di Windows.  
  
-   Disabilitare l'analisi per la ricerca di virus nei volumi FILESTREAM quando non è non necessaria. Se tale analisi è necessaria, evitare di impostare criteri per l'eliminazione automatica dei file infetti.  
  
-   Configurare e ottimizzare il livello RAID per la tolleranza di errore e le prestazioni richieste da un'applicazione.  
  
|Livello RAID|Prestazioni di scrittura|Prestazioni di lettura|Tolleranza di errore|Osservazioni|  
|-|-|-|-|-|   
|RAID 5|Normale|Normale|Eccellenti|Le prestazioni sono più elevate rispetto all'utilizzo di un unico disco o di JBOD e meno elevate rispetto all'utilizzo di RAID 0 o RAID 5 con striping.|  
|RAID 0|Eccellenti|Eccellenti|nessuno||  
|RAID 5 + striping|Eccellenti|Eccellenti|Eccellenti|Opzione più costosa.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
  
###  <a name="physical-database-design"></a><a name="database"></a> Progettazione fisica di database  
 Quando si progetta un database FILESTREAM, tenere presenti le linee guida seguenti:  
  
-   Le colonne FILESTREAM devono essere associate a una corrispondente colonna ROWGUID **uniqueidentifier**. Questi tipi di tabelle devono inoltre essere associati a un indice univoco. Solitamente questo indice non è cluster. Se la logica di business dei database richiede un indice cluster, è necessario assicurarsi che i valori archiviati nell'indice non siano casuali. In caso contrario, l'indice verrà riordinato ogni volta che viene aggiunta o rimossa una riga dalla tabella.  
  
-   Ai fini delle prestazioni, i contenitori e i filegroup FILESTREAM dovrebbero risiedere in volumi anziché nel sistema operativo, nel database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nel log di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel file di paging.  
  
-   La gestione dello spazio e i criteri non sono supportati direttamente da FILESTREAM. È tuttavia possibile gestire lo spazio e applicare criteri in modo indiretto assegnando ogni filegroup FILESTREAM a un volume distinto e utilizzando le funzionalità di gestione di quest'ultimo.  
  
  
