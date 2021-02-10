---
description: Impostazione delle opzioni progetto (MySQLToSQL)
title: Impostazione delle opzioni del progetto (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Setting project options,configuration options
ms.assetid: 08820d88-e157-4d49-9401-38580dd7ec2d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75c4f6ca899135141aa68ec147889fcce8cfa549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074512"
---
# <a name="setting-project-options-mysqltosql"></a>Impostazione delle opzioni progetto (MySQLToSQL)
Per ogni progetto SSMA, è possibile impostare le opzioni a livello di progetto. Queste opzioni specificano la modalità di conversione degli oggetti, la modalità di migrazione dei dati e la modalità di mapping dei tipi di dati di origine ai tipi di dati di destinazione.  Prima di convertire gli oggetti in SQL Server o SQL Azure o migrare i dati in SQL Server o SQL Azure, verificare che le opzioni di configurazione siano appropriate per il progetto.  
  
SSMA consente di configurare le opzioni predefinite per tutti i progetti. Queste opzioni vengono applicate a qualsiasi nuovo progetto creato. È quindi possibile personalizzare le opzioni per ogni progetto.  
  
## <a name="configuration-options-and-modes"></a>Opzioni e modalità di configurazione  
SSMA include cinque set di impostazioni di progetto:  
  
-   Informazioni progetto  
  
-   Generale (conversione, migrazione e SQL Azure)  
  
-   Synchronization  
  
-   Interfaccia utente grafica  
  
-   Mapping dei tipi  
  
Le impostazioni del progetto possono essere configurate in quattro modi:  
  
-   Predefinito  
  
-   Optimistic  
  
-   Full  
  
-   Personalizzato  
  
La modalità predefinita è consigliata per la maggior parte degli utenti. La modalità ottimistica mantiene più la sintassi attuale di MySQL ed è più facile da leggere. Tuttavia, mantenere la sintassi corrente potrebbe non essere accurata. Se la sintassi di MySQL deve essere convertita in SQL Server equivalente o SQL Azure sintassi, la modalità completa esegue la conversione più completa. Il codice risultante, tuttavia, potrebbe essere più difficile da leggere. In modalità personalizzata è possibile impostare le opzioni.  
  
Per ulteriori informazioni sulle impostazioni e sul modo in cui le impostazioni vengono applicate in ogni modalità, vedere gli argomenti seguenti:  
  
-   [Impostazioni progetto &#40;conversione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
-   [Impostazioni progetto &#40;migrazione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)  
  
-   [Impostazioni progetto (GUI) (SSMA comune)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)  
  
-   [Impostazioni progetto &#40;sincronizzazione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
-   [Impostazioni progetto &#40;database SQL di Azure&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)  
  
## <a name="setting-project-options"></a>Impostazione delle opzioni del progetto  
In SSMA è possibile configurare le impostazioni predefinite per tutti i progetti. Queste impostazioni vengono salvate nel file di configurazione SSMA e applicate a qualsiasi nuovo progetto creato.  
  
**Per impostare le opzioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **Impostazioni progetto predefinite** utilizzare una delle procedure seguenti:  
  
    1.  Selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare/modificare le impostazioni dall'elenco a discesa della **versione di destinazione della migrazione** , fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi selezionare **conversione o migrazione o SQL Azure** opzione.  
  
    2.  Per selezionare una modalità predefinita, selezionare **predefinita**, **ottimistica** o **completa** nella casella di riepilogo a discesa **modalità** .  
  
    3.  Per specificare le impostazioni personalizzate, selezionare o immettere le nuove impostazioni o i nuovi valori.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
È inoltre possibile personalizzare le impostazioni per il progetto corrente. Le impostazioni vengono salvate nel file di progetto corrente.  
  
**Per personalizzare le impostazioni per il progetto corrente**  
  
1.  Scegliere **ProjectSettings** dal menu **strumenti** .  
  
2.  Nella finestra di dialogo **ProjectSettings** utilizzare una delle procedure seguenti:  
  
    1.  Per selezionare una modalità predefinita, selezionare **predefinita**, **ottimistica** o **completa** nella casella di riepilogo a discesa **modalità** .  
  
    2.  Per specificare una modalità personalizzata, selezionare **personalizzata** nella casella di riepilogo a discesa **modalità** . Quindi selezionare le impostazioni del progetto appropriate.  
  
3.  Fare clic su **OK** per salvare le impostazioni.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo della migrazione dipende dalle esigenze del progetto:  
  
-   Per personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere [mapping di tipi di dati MySQL e SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   In caso contrario, è possibile convertire le definizioni degli oggetti di database MySQL in definizioni di oggetti SQL Server o SQL Azure. Per altre informazioni, vedere [conversione di database MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Mapping di tipi di dati MySQL e SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
