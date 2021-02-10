---
description: Impostazione delle opzioni progetto (OracleToSQL)
title: Impostazione delle opzioni del progetto (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Configuration Options and Modes
ms.assetid: a324d07d-cfdf-43bd-98a0-acf332c5a4db
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7de2ab3447a35f9d55489673adaa532866a4f89f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067676"
---
# <a name="setting-project-options-oracletosql"></a>Impostazione delle opzioni progetto (OracleToSQL)
Per ogni progetto SSMA è possibile impostare le opzioni a livello di progetto. Queste opzioni specificano conversione di oggetti, caricamento di oggetti, interfaccia utente e impostazioni di migrazione dei dati. Prima di convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che le opzioni di configurazione siano appropriate per il progetto.  
  
SSMA consente di configurare le opzioni predefinite per tutti i progetti. Queste opzioni vengono applicate a qualsiasi nuovo progetto creato. È quindi possibile personalizzare le opzioni per ogni progetto.  
  
## <a name="configuration-options-and-modes"></a>Opzioni e modalità di configurazione  
SSMA include cinque set di impostazioni di progetto:  
  
-   Informazioni progetto  
  
-   Generale (conversione, migrazione, caricamento di oggetti)  
  
-   Synchronization  
  
-   Interfaccia utente grafica  
  
-   Mapping dei tipi  
  
Sono inoltre disponibili quattro modalità per la configurazione di queste impostazioni:  
  
-   Predefinito  
  
-   Optimistic  
  
-   Full  
  
-   Personalizzato  
  
La modalità predefinita è consigliata per la maggior parte degli utenti. La modalità ottimistica mantiene più della sintassi Oracle corrente ed è più facile da leggere. Tuttavia, mantenere la sintassi corrente potrebbe non essere accurata. Se la sintassi Oracle deve essere convertita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi equivalente, la modalità completa esegue la conversione più completa, ma il codice risultante potrebbe essere più difficile da leggere. In modalità personalizzata è possibile impostare le opzioni.  
  
Per ulteriori informazioni sulle impostazioni e sul modo in cui le impostazioni vengono applicate in ogni modalità, vedere gli argomenti seguenti:  
  
-   [Impostazioni progetto &#40;conversione&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md)  
  
-   [Impostazioni progetto &#40;migrazione&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-migration-oracletosql.md)  
  
-   [Impostazioni progetto&#40;sincronizzazione&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-synchronization-oracletosql.md)  
  
-   [Impostazioni progetto &#40;GUI&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-gui-oracletosql.md)  
  
-   [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md)  
  
## <a name="setting-project-options"></a>Impostazione delle opzioni del progetto  
In SSMA è possibile configurare le impostazioni predefinite per tutti i progetti. Queste impostazioni vengono salvate nel file di configurazione SSMA e applicate a qualsiasi nuovo progetto creato.  
  
**Per impostare le opzioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **Impostazioni progetto predefinite** utilizzare una delle procedure seguenti:  
  
    -   Selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare o modificare le impostazioni dall'elenco a discesa della **versione di destinazione della migrazione** fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi selezionare conversione o migrazione.  
  
    -   Per selezionare una modalità **predefinita**, nella casella di riepilogo a discesa **modalità** selezionare predefinito, **ottimistico** o **completo**.  
  
    -   Per specificare le impostazioni personalizzate, selezionare o immettere le nuove impostazioni o i nuovi valori.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
È inoltre possibile personalizzare le impostazioni per il progetto corrente. Queste impostazioni vengono salvate nel file di progetto corrente.  
  
**Per personalizzare le impostazioni per il progetto corrente**  
  
1.  Scegliere **Impostazioni progetto** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **Impostazioni progetto** utilizzare una delle procedure seguenti:  
  
    -   Per selezionare una modalità **predefinita**, nella casella di riepilogo a discesa **modalità** selezionare predefinito, **ottimistico** o **completo**.  
  
    -   Per specificare una modalità personalizzata, nella casella **modalità** selezionare **personalizzato**, quindi selezionare le impostazioni appropriate per il progetto.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
## <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo della migrazione dipende dalle esigenze del progetto:  
  
-   Per personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere [mapping di tipi di dati Oracle e SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
-   In caso contrario, è possibile convertire le definizioni degli oggetti di database Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni di oggetti. Per ulteriori informazioni, vedere la pagina relativa alla [conversione di schemi Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Mapping di tipi di dati Oracle e SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md)  
  
