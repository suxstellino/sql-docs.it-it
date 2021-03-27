---
title: Mapping di schemi Oracle a schemi SQL Server (OracleToSQL) | Microsoft Docs
description: Informazioni su come personalizzare SSMA per i mapping Oracle tra gli schemi Oracle e SQL Server oppure accettare il valore predefinito.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 0edeaa08-9c5d-4e3a-bc15-b9a1f0c8a9dc
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: dc144017c6bf45a21721fe647dbf5affa486f8ef
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105632910"
---
# <a name="mapping-oracle-schemas-to-sql-server-schemas-oracletosql"></a>Mapping di schemi Oracle a schemi SQL Server (OracleToSQL)
In Oracle ogni database dispone di uno o più schemi. Per impostazione predefinita, SSMA esegue la migrazione di tutti gli oggetti in uno schema Oracle a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database denominato per lo schema. Tuttavia, è possibile personalizzare il mapping tra gli schemi e i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database Oracle.  
  
## <a name="oracle-and-sql-server-schemas"></a>Schemi Oracle e SQL Server  
Un database Oracle contiene schemi. Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene più database, ognuno dei quali può disporre di più schemi.  
  
Il concetto Oracle di uno schema viene mappato al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] concetto di database e di uno dei relativi schemi. Ad esempio, Oracle potrebbe avere uno schema denominato **HR**. Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe disporre di un database denominato **SampleDatabase** e all'interno di tale database sono presenti più schemi. Per impostazione predefinita, verrà eseguito il mapping dell' **ora** dello schema Oracle al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database e allo schema **SampleDatabase.hr**. SSMA fa riferimento alla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinazione di database e schema come schema.  
  
È possibile modificare il mapping tra Oracle e gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modifica del database di destinazione e dello schema  
In SSMA è possibile eseguire il mapping di uno schema Oracle a qualsiasi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schema disponibile.  
  
**Per modificare il database e lo schema**  
  
1.  In Esplora metadati Oracle selezionare **schemi**.  
  
    La scheda **mapping dello schema** è disponibile anche quando si seleziona un singolo database, la cartella **schemi** o singoli schemi. L'elenco nella scheda **mapping dello schema** è personalizzato per l'oggetto selezionato.  
  
2.  Nel riquadro di destra fare clic sulla scheda **mapping dello schema** .  
  
    Viene visualizzato un elenco di tutti gli schemi Oracle, seguiti da un valore di destinazione. Questa destinazione è indicata in una notazione in due parti (*database. Schema*) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui verrà eseguita la migrazione degli oggetti e dei dati.  
  
3.  Selezionare la riga che contiene il mapping che si desidera modificare, quindi fare clic su **modifica**.  
  
    Nella finestra di dialogo **Scegli schema di destinazione** è possibile cercare il database e lo schema di destinazione disponibili oppure digitare il nome del database e dello schema nella casella di testo in una notazione in due parti (database. Schema), quindi fare clic su **OK**.  
  
4.  La destinazione cambia nella scheda **mapping dello schema** .  
  
**Modalità di mapping**  
  
-   Mapping a SQL Server  
  
È possibile eseguire il mapping del database di origine a qualsiasi database di destinazione. Per impostazione predefinita, il database di origine viene mappato al database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il quale è stata effettuata la connessione tramite SSMA. Se il database di destinazione di cui è stato eseguito il mapping non è presente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verrà visualizzato un messaggio che indica che **il database e/o lo schema non esiste nei metadati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Verrà creata durante la sincronizzazione. Continuare? "** Fare clic su Sì. Analogamente, è possibile eseguire il mapping dello schema a uno schema non esistente nel database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che verrà creato durante la sincronizzazione.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Ripristino del database e dello schema predefiniti  
Se si Personalizza il mapping tra uno schema Oracle e uno [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schema, è possibile ripristinare i valori predefiniti del mapping.  
  
**Per ripristinare il database e lo schema predefiniti**  
  
1.  Nella scheda mapping dello schema selezionare una riga qualsiasi e fare clic su **Reimposta per impostazione predefinita** per ripristinare il database e lo schema predefiniti.  
  
## <a name="next-steps"></a>Passaggi successivi  
Se si desidera analizzare la conversione degli oggetti Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti, è possibile [creare un report di conversione](assessing-oracle-schemas-for-conversion-oracletosql.md). In caso contrario, è possibile [convertire le definizioni degli oggetti di database Oracle](converting-oracle-schemas-oracletosql.md) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni di oggetti.  
  
## <a name="see-also"></a>Vedere anche  
[Connessione a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
