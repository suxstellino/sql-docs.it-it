---
description: Mapping di schemi DB2 a schemi SQL Server (DB2ToSQL)
title: Mapping di schemi DB2 a schemi SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 05ff7bd4-e60b-4f48-a893-bc2346aa9a8a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 489a026fe610b298803db088e7f8c9953215a7c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060586"
---
# <a name="mapping-db2-schemas-to-sql-server-schemas-db2tosql"></a>Mapping di schemi DB2 a schemi SQL Server (DB2ToSQL)
In DB2 ogni database dispone di uno o più schemi. Per impostazione predefinita, SSMA esegue la migrazione di tutti gli oggetti in uno schema DB2 a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database denominato per lo schema. Tuttavia, è possibile personalizzare il mapping tra gli schemi DB2 e i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
## <a name="db2-and-sql-server-schemas"></a>Schemi DB2 e SQL Server  
Un database DB2 contiene schemi. Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene più database, ognuno dei quali può disporre di più schemi.  
  
Il concetto DB2 di uno schema viene mappato al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] concetto di database e di uno dei relativi schemi. Ad esempio, DB2 potrebbe avere uno schema denominato **HR**. Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe disporre di un database denominato **HR** e all'interno di tale database sono schemi. Uno schema è lo schema **dbo** (o Owner database). Per impostazione predefinita, lo schema DB2 **HR** verrà mappato al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database e allo schema **HR. dbo**. SSMA fa riferimento alla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinazione di database e schema come schema.  
  
È possibile modificare il mapping tra DB2 e gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modifica del database di destinazione e dello schema  
In SSMA è possibile eseguire il mapping di uno schema DB2 a qualsiasi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schema disponibile.  
  
**Per modificare il database e lo schema**  
  
1.  In DB2 Metadata Explorer selezionare **schemas**.  
  
    La scheda **mapping dello schema** è disponibile anche quando si seleziona un singolo database, la cartella **schemi** o singoli schemi. L'elenco nella scheda **mapping dello schema** è personalizzato per l'oggetto selezionato.  
  
2.  Nel riquadro di destra fare clic sulla scheda **mapping dello schema** .  
  
    Viene visualizzato un elenco di tutti gli schemi DB2, seguiti da un valore di destinazione. Questa destinazione è indicata in una notazione in due parti (*database. Schema*) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui verrà eseguita la migrazione degli oggetti e dei dati.  
  
3.  Selezionare la riga che contiene il mapping che si desidera modificare, quindi fare clic su **modifica**.  
  
    Nella finestra di dialogo **Scegli schema di destinazione** è possibile cercare il database e lo schema di destinazione disponibili oppure digitare il nome del database e dello schema nella casella di testo in una notazione in due parti (database. Schema), quindi fare clic su **OK**.  
  
4.  La destinazione cambia nella scheda **mapping dello schema** .  
  
**Modalità di mapping**  
  
-   Mapping a SQL Server  
  
È possibile eseguire il mapping del database di origine a qualsiasi database di destinazione. Per impostazione predefinita, il database di origine viene mappato al database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il quale è stata effettuata la connessione tramite SSMA. Se il database di destinazione di cui è stato eseguito il mapping non è presente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verrà visualizzato un messaggio che indica che **il database e/o lo schema non esiste nei metadati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Verrà creata durante la sincronizzazione. Continuare? "** Fare clic su Sì. Analogamente, è possibile eseguire il mapping dello schema a uno schema non esistente nel database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che verrà creato durante la sincronizzazione.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Ripristino del database e dello schema predefiniti  
Se si Personalizza il mapping tra uno schema DB2 e uno [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schema, è possibile ripristinare i valori predefiniti del mapping.  
  
**Per ripristinare il database e lo schema predefiniti**  
  
1.  Nella scheda mapping dello schema selezionare una riga qualsiasi e fare clic su **Reimposta per impostazione predefinita** per ripristinare il database e lo schema predefiniti.  
  
## <a name="next-steps"></a>Passaggi successivi  
Se si desidera analizzare la conversione degli oggetti DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti, è possibile creare [report di migrazione dei dati (SSMA Common)](../sybase/data-migration-report-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Connessione a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
[Migrazione di database DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
