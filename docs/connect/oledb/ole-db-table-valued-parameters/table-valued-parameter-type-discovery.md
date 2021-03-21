---
title: Individuazione dei tipi di parametro con valori di tabella (OLE DB Driver)
description: Informazioni su come un consumer può trovare le informazioni dei metadati per ogni singola colonna del parametro con valori di tabella in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, type discovery
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33525b50f0f0320d8753d12afa9bfca5fb832a72
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750761"
---
# <a name="table-valued-parameter-type-discovery-ole-db-driver"></a>Individuazione dei tipi di parametro con valori di tabella (OLE DB Driver)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il consumer, ovvero l'applicazione client che usa il driver OLE DB per SQL Server, può individuare il tipo di ogni parametro del comando se al provider OLE DB è stato indicato il testo del comando. Una volta noto il tipo di un parametro con valori di tabella, il consumer può individuare le informazioni sui metadati per ogni singola colonna del parametro con valori di tabella.  
  
 Per la maggior parte dei tipi di parametri le informazioni sul tipo dei parametri di procedura sono supportate da ICommandWithParameters::GetParameterInfo. A partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], con l'introduzione dei tipi definiti dall'utente e del tipo di dati **xml**, il metodo GetParameterInfo non era sufficiente per questo scopo poiché non era possibile specificare le informazioni sul tipo definito dall'utente (nome, schema e catalogo) tramite ICommandWithParameters. Per fornire informazioni estese sul tipo è stata definita la nuova interfaccia ISSCommandWithParameters.  
  
 Per i parametri con valori di tabella, l'interfaccia ISSCommandWithParameters viene usata anche per individuare informazioni dettagliate. Il client chiama ISSCommandWithParameters::GetParameterInfo dopo aver preparato l'oggetto commando. Per i parametri con valori di tabella il membro *wType* della struttura DBPARAMINFO viene impostato su DBTYPE_TABLE dal provider. Il valore del campo *ulParamSize* della struttura DBPARAMINFO è ~0.  
  
 Il consumer può quindi richiedere proprietà aggiuntive (nome del catalogo del tipo di parametro con valori di tabella, nome dello schema del tipo di parametro con valori di tabella, nome del tipo di parametro con valori di tabella, ordinamento colonne e colonne predefinite) mediante ISSCommandWithParameters::GetParameterProperties.  
  
 Una volta noto il nome del tipo, per recuperare informazioni sulle singole colonne il consumer deve chiamare IOpenRowset::OpenRowset oppure ottenere il set di righe DBSCHEMA_TABLE_TYPE_COLUMNS specificando il nome del tipo di parametro con valori di tabella come nome della tabella.  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
