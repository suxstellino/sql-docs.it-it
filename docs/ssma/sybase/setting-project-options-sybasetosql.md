---
description: Impostazione delle opzioni del progetto (SybaseToSQL)
title: Impostazione delle opzioni del progetto (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Project Options Setting
ms.assetid: 97b70fc8-1f68-4f15-8e22-db5b784ea4ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: fa8e45af93faa4ef175d129487a1d655a6d02ead
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030759"
---
# <a name="setting-project-options-sybasetosql"></a>Impostazione delle opzioni del progetto (SybaseToSQL)
Per ogni progetto SSMA, è possibile impostare le opzioni a livello di progetto. Queste opzioni specificano la conversione degli oggetti, il caricamento di oggetti, SQL Azure, l'interfaccia utente e le impostazioni di migrazione dei dati. Prima di convertire o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure o migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, verificare che le opzioni di configurazione siano appropriate per il progetto.  
  
SSMA consente di configurare le opzioni predefinite per tutti i progetti. Queste opzioni vengono applicate a qualsiasi nuovo progetto creato. È quindi possibile personalizzare le opzioni per ogni progetto.  
  
## <a name="configuration-options-and-modes"></a>Opzioni e modalità di configurazione  
SSMA include cinque set di impostazioni di progetto:  
  
1.  Informazioni progetto  
  
2.  Generale (conversione, migrazione e raccolta di dati)  
  
3.  Synchronization  
  
4.  Interfaccia utente grafica  
  
5.  Mapping dei tipi  
  
Sono inoltre disponibili quattro modalità per la configurazione di queste impostazioni:  
  
1.  Predefinito  
  
2.  Optimistic  
  
3.  Full  
  
4.  Personalizzato  
  
La modalità predefinita è consigliata per la maggior parte degli utenti. La modalità ottimistica mantiene più della sintassi attuale di Sybase Adaptive Server Enterprise (ASE) ed è più facile da leggere. Tuttavia, mantenere la sintassi corrente potrebbe non essere accurata. Se la sintassi dell'ambiente del servizio app deve essere convertita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] una sintassi equivalente o SQL Azure, la modalità completa esegue una conversione completa, ma il codice risultante può essere più difficile da leggere. In modalità personalizzata è possibile impostare le opzioni.  
  
Le impostazioni sono descritte nella sezione riferimento dell'interfaccia utente di questa documentazione. Per ulteriori informazioni sulle impostazioni e sul modo in cui le impostazioni vengono applicate in ogni modalità, vedere gli argomenti seguenti:  
  
-   [Impostazioni progetto &#40;conversione&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;migrazione&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;sincronizzazione&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;GUI&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;database SQL di Azure &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)  
  
## <a name="setting-project-options"></a>Impostazione delle opzioni del progetto  
In SSMA è possibile configurare le impostazioni predefinite per tutti i progetti. Queste impostazioni vengono salvate nel file di configurazione SSMA e applicate a qualsiasi nuovo progetto creato.  
  
**Per impostare le opzioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **Impostazioni progetto predefinite** utilizzare una delle procedure seguenti:  
  
    -   Selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare o modificare le impostazioni dall'elenco a discesa della **versione di destinazione della migrazione** fare clic su generale nella parte inferiore del riquadro sinistro, quindi selezionare conversione o migrazione o SQL Azure.  
  
    -   Per selezionare una modalità **predefinita**, nella casella di riepilogo a discesa **modalità** selezionare predefinito, **ottimistico** o **completo**.  
  
    -   Per specificare le impostazioni personalizzate, è sufficiente selezionare o immettere le nuove impostazioni o i nuovi valori.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
È inoltre possibile personalizzare le impostazioni per il progetto corrente. Queste impostazioni vengono salvate nel file di progetto corrente.  
  
**Per personalizzare le impostazioni per il progetto corrente**  
  
1.  Scegliere **Impostazioni progetto** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **Impostazioni progetto** utilizzare una delle procedure seguenti:  
  
    -   Per selezionare una modalità **predefinita**, nella casella di riepilogo a discesa **modalità** selezionare predefinito, **ottimistico** o **completo**.  
  
    -   Per specificare una modalità personalizzata, nella casella di riepilogo a discesa **modalità** selezionare **personalizzata**, selezionare un'opzione nel riquadro a sinistra, fare clic sull'impostazione o sul valore nel riquadro destro, quindi selezionare o immettere il nuovo valore o impostazione.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
## <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo della migrazione dipende dalle esigenze del progetto:  
  
-   Se si vuole personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere Mapping dei tipi di [dati di Sybase ASE e SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
-   In caso contrario, è possibile convertire le definizioni degli oggetti di database Sybase in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure le definizioni degli oggetti. Per altre informazioni, vedere [conversione di oggetti di database Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database Sybase ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
