---
title: Supporto per colonne di tipo sparse nel driver OLE DB per SQL Server | Microsoft Docs
description: Informazioni sulle modalità di supporto delle colonne di tipo sparse in OLE DB Driver per SQL Server e sulle colonne di tipo sparse in SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sparse columns, OLE DB Driver for SQL Server
- sparse columns, OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b6af363b6879a892170dc0f4c96e7ca176b4c788
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751141"
---
# <a name="sparse-columns-support-in-ole-db-driver-for-sql-server"></a>Supporto per colonne di tipo sparse nel driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server supporta le colonne di tipo sparse. Per altre informazioni sulle colonne di tipo sparse in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [Usare le colonne di tipo sparse ](../../../relational-databases/tables/use-sparse-columns.md)e [Usare set di colonne](../../../relational-databases/tables/use-column-sets.md).  
  
 Per altre informazioni sul supporto per le colonne di tipo sparse in OLE DB Driver per SQL Server, vedere [Supporto per colonne di tipo sparse &#40;OLE DB&#41;](../../oledb/ole-db/sparse-columns-support-ole-db.md).  
  
 Per informazioni sulle applicazioni di esempio in cui viene illustrata questa funzionalità, vedere la [pagina relativa agli esempi di programmazione dati di SQL Server](https://msftdpprodsamples.codeplex.com/).  
  
## <a name="user-scenarios-for-sparse-columns-and-ole-db-driver-for-sql-server"></a>Scenari utente per colonne di tipo sparse e OLE DB Driver per SQL Server  
 Nella tabella seguente vengono descritti in breve gli scenari comuni per gli utenti di OLE DB Driver per SQL Server che usano colonne di tipo sparse:  
  
|Scenario|Comportamento|  
|--------------|--------------|  
|**select \* from table** o IOpenRowset::OpenRowset.|Restituisce tutte le colonne che non sono membri del set di colonne **column_set** di tipo sparse, nonché una colonna XML che contiene i valori di tutte le colonne non Null che sono membri del set di colonne **column_set** di tipo sparse.|  
|Fare riferimento a una colonna in base al nome.|È possibile fare riferimento alla colonna indipendentemente dal relativo stato di colonna di tipo sparse o dall'appartenenza al set di colonne **column_set**.|  
|Accedere alle colonne che sono membri del set di colonne **column_set** tramite una colonna XML calcolata.|È possibile accedere alle colonne che sono membri del set di colonne **column_set** di tipo sparse selezionando il set di colonne **column_set** in base al nome; inoltre, i valori di tali colonne possono essere inseriti e aggiornati aggiornando i dati XML nella colonna **column_set**.<br /><br /> Il valore deve essere conforme allo schema per le colonne **column_set**.|  
|Recuperare metadati per tutte le colonne in una tabella usando il set di righe dello schema DBSCHEMA_COLUMNS senza alcuna restrizione relativa alle colonne (OLE DB).|Restituisce una riga per tutte le colonne che non sono membri di un set di colonne **column_set**. Se la tabella include un set di colonne **column_set** di tipo sparse, verrà restituita una riga per la tabella.<br /><br /> Si noti che questo comportamento non restituisce metadati per colonne membri di un set di colonne **column_set**.|  
|Recuperare metadati per tutte le colonne, indipendentemente dal fatto che siano o meno di tipo sparse o dall'appartenenza a un set di colonne **column_set**. È possibile che venga restituito un numero molto elevato di righe.|Chiamare IDBSchemaRowset::GetRowset per ottenere il set di righe dello schema DBSCHEMA_COLUMNS_EXTENDED.|  
|Recuperare metadati solo per le colonne che sono membri di un set di colonne **column_set**. È possibile che venga restituito un numero molto elevato di righe.|Chiamare IDBSchemaRowset::GetRowset per ottenere il set di righe dello schema DBSCHEMA_SPARSE_COLUMN_SET.|  
|Determinare se una colonna è di tipo sparse.|Esaminare la colonna SS_IS_SPARSE del set di righe dello schema DBSCHEMA_COLUMNS (OLE DB).|  
|Determinare se una colonna è un **column_set**.|Esaminare la colonna SS_IS_COLUMN_SET del set di righe dello schema DBSCHEMA_COLUMNS. In alternativa, esaminare i valori *dwFlags* restituiti da IColumnsinfo::GetColumnInfo o DBCOLUMNFLAGS nel set di righe restituito da IColumnsRowset::GetColumnsRowset. Per le colonne **column_set** sarà impostato DBCOLUMNFLAGS_SS_ISCOLUMNSET.|  
|Importazione ed esportazione di colonne di tipo sparse tramite BCP per una tabella senza **column_set**.|Nessuna modifica nel comportamento rispetto alle versioni precedenti di OLE DB Driver per SQL Server.|  
|Importazione ed esportazione di colonne di tipo sparse tramite BCP per una tabella con **column_set**.|Il valore **column_set** viene importato ed esportato in modo analogo a XML, ovvero come **varbinary(max)** , se associato come tipo binario, o come **nvarchar(max)** se associato come tipo **char** o **wchar**.<br /><br /> Le colonne che sono membri del set di colonne **column_set** di tipo sparse non vengono esportate come colonne distinte, ma vengono esportate solo nel valore di **column_set**.|  
|Comportamento di **queryout** per BCP.|Nessuna modifica nella gestione di colonne denominate in modo esplicito rispetto alle versioni precedenti di OLE DB Driver per SQL Server.<br /><br /> Gli scenari che comportano l'importazione e l'esportazione tra tabelle con schemi diversi possono richiedere una gestione speciale.<br /><br /> Per ulteriori informazioni su BCP, vedere Supporto per la copia bulk (BCP) per colonne di tipo sparse più avanti in questo argomento.|  
  
## <a name="down-level-client-behavior"></a>Comportamento dei client legacy  
 I client restituiscono metadati solo per le colonne che non sono membri del set di colonne **column_set** di tipo sparse per SQLColumns e DBSCHMA_COLUMNS.
  
 I client legacy possono accedere per nome alle colonne che sono membri del set di colonne **column_set** di tipo sparse e la colonna **column_set** sarà accessibile come colonna XML per i client e come colonna per i client [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
## <a name="bulk-copy-bcp-support-for-sparse-columns"></a>Supporto per la copia bulk (BCP) per colonne di tipo sparse  
 Non è stata apportata alcuna modifica all'API BCP in OLE DB per le colonne di tipo sparse e per le caratteristiche **column_set**.  
  
 Se una tabella include un set di colonne **column_set**, le colonne di tipo sparse non verranno gestite come colonne distinte. I valori di tutte le colonne di tipo sparse sono inclusi nel valore di **column_set**, che viene esportato come una colonna XML, ovvero come **varbinary(max)** , se associato come tipo binario, oppure come **nvarchar(max)** se associato come tipo **char** o **wchar**. Per l'importazione, il valore **column_set** deve essere conforme allo schema di **column_set**.  
  
 Per le operazioni **queryout** non esiste alcuna modifica al modo in cui sono gestite le colonne a cui si fa esplicitamente riferimento. Le colonne **queryout** dispongono dello stesso comportamento delle colonne XML e il fatto che siano o meno di tipo sparse non ha effetto sulla gestione delle colonne di tipo sparse denominate.  
  
 Se, tuttavia, si utilizza **queryout** per l'esportazione e si fa riferimento a colonne di tipo sparse che sono membri del set di colonne di tipo sparse in base al nome, non è possibile eseguire un'importazione diretta in una tabella dalla struttura analoga. Questo perché BCP usa i metadati coerenti con un' **\* *operazione SELECT _ per l'importazione e non è in grado di trovare la corrispondenza con i metadati _* column_set** colonne membro con questi metadati. Per importare colonne membri del set di colonne **column_set** singolarmente, è necessario definire una vista nella tabella che fa riferimento alle colonne **column_set** desiderate ed eseguire l'operazione di importazione utilizzando la vista.  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per SQL Server](../../oledb/oledb-driver-for-sql-server.md)  
  
  
