---
title: Creazione di un set di righe di parametri con valori di tabella (OLE DB Driver)
description: Informazioni sul set di righe di parametri con valori di tabella, un oggetto in memoria che OLE DB Driver per SQL Server consente ai consumer di creare.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, rowset creation
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bbde1d2007e18cefba01a43522cbaf5f07a6cee1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750791"
---
# <a name="table-valued-parameter-rowset-creation"></a>Creazione di un set di righe di parametri con valori di tabella
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Sebbene i consumer siano in grado di fornire qualsiasi oggetto set di righe per i parametri con valori di tabella, vengono implementati gli oggetti set di righe tipici rispetto agli archivi dati di back-end, con conseguente limitazione delle prestazioni. Per questo motivo, il driver OLE DB per SQL Server consente ai consumer di creare un oggetto set di righe specializzato nei dati in memoria. Questo oggetto set di righe speciale in memoria rappresenta un nuovo oggetto COM definito set di righe di parametri con valori di tabella. Tale oggetto fornisce funzionalità simili ai set di parametri.  
  
 Gli oggetti set di righe di parametri con valori di tabella vengono creati in modo esplicito dal consumer per i parametri di input tramite più interfacce a livello di sessione. Esiste una sola istanza di un oggetto set di righe di parametri con valori di tabella per parametro con valori di tabella. Il consumer può creare gli oggetti set di righe di parametri con valori di tabella fornendo le informazioni sui metadati già note (scenario statico) o individuandole tramite le interfacce del provider (scenario dinamico). Nelle sezioni seguenti vengono descritti questi due scenari.  
  
## <a name="static-scenario"></a>Scenario statico  
 Quando le informazioni sul tipo sono note, il consumer usa ITableDefinitionWithConstraints::CreateTableWithConstraints per creare un'istanza dell'oggetto set di righe di parametri con valori di tabella che corrisponde a un parametro con valori di tabella.  
  
 Il campo *guid* (parametro *pTableID*) contiene il GUID speciale (CLSID_ROWSET_TVP). Il membro *pwszName* contiene il nome del tipo di parametro con valori di tabella di cui il consumer intende creare un'istanza. Il campo *eKind* sarà impostato su DBKIND_GUID_NAME. Questo nome è necessario quando si tratta di un'istruzione SQL ad hoc; il nome è facoltativo se è una chiamata di procedura.  
  
 Per l'aggregazione il consumer passa il parametro *pUnkOuter* con IUnknown per il controllo.  
  
 Le proprietà dell'oggetto set di righe di parametri con valori di tabella sono di sola lettura, pertanto il consumer non deve impostare alcuna proprietà in *rgPropertySets*.  
  
 Per il membro *rgPropertySets* di ogni struttura DBCOLUMNDESC, il consumer può specificare proprietà aggiuntive per ogni colonna. Queste proprietà appartengono al set di proprietà DBPROPSET_SQLSERVERCOLUMN. Tali proprietà consentono di specificare le impostazioni predefinite e calcolate per ogni colonna. Supportano inoltre le proprietà esistenti della colonna, ad esempio nullability e identity.  
  
 Per recuperare le informazioni corrispondenti da un oggetto set di righe di parametri con valori di tabella, il consumer usa IRowsetInfo::GetProperties.  
  
 Per recuperare informazioni sullo stato null, univoco, calcolato e di aggiornamento di ogni colonna, il consumer può usare IColumnsRowset::GetColumnsRowset o IColumnsInfo::GetColumnInfo. Questi metodi forniscono informazioni dettagliate su ogni colonna del set di righe di parametri con valori di tabella.  
  
 Il consumer specifica il tipo di ogni colonna del parametro con valori di tabella, analogamente al modo in cui vengono specificate le colonne quando viene creata una tabella in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il consumer ottiene un oggetto set di righe di parametri con valori di tabella da OLE DB Driver per SQL Server tramite il parametro di output *ppRowset*.  
  
## <a name="dynamic-scenario"></a>Scenario dinamico  
 Quando il consumer non dispone di informazioni sul tipo, deve usare IOpenRowset::OpenRowset per creare un'istanza degli oggetti set di righe di parametri con valori di tabella. L'unica informazione che il consumer deve fornire al provider è il nome del tipo.  
  
 In questo scenario, il provider ottiene le informazioni sul tipo relative a un oggetto set di righe di parametri con valori di tabella dal server per conto del consumer.  
  
 I parametri *pTableID* e *pUnkOuter* devono essere impostati come nello scenario statico. Il driver OLE DB per SQL Server ottiene quindi le informazioni sul tipo (vincoli e informazioni sulla colonna) dal server e restituisce un oggetto set di righe di parametri con valori di tabella tramite il parametro *ppRowset*. Questa operazione richiede la comunicazione con il server e pertanto viene eseguita in modo diverso rispetto allo scenario statico. Lo scenario dinamico funziona solo con le chiamate di procedura con parametri.  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
