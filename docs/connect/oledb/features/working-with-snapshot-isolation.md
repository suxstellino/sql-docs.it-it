---
title: Utilizzo dell'isolamento dello snapshot | Microsoft Docs
description: Informazioni su come i miglioramenti di OLE DB Driver per SQL Server usano l'isolamento dello snapshot, che migliora la concorrenza per le applicazioni di elaborazione delle transazioni online.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], snapshot isolation
- MSOLEDBSQL, snapshot isolation
- isolation levels [SQL Server], snapshot
- DBPROPSET_SESSION property set
- DBDROPSET_DATASOURCEINFO property set
- snapshot isolation [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, snapshot isolation
- SQLGetInfo function
- concurrency [OLE DB Driver for SQL Server]
- SQLSetConnectAttr function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a31ec13ff3cf60bdb218657ebd0187f7e0e8e23
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746261"
---
# <a name="working-with-snapshot-isolation"></a>Utilizzo dell'isolamento dello snapshot
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] è stato introdotto un nuovo livello di isolamento dello "snapshot" destinato a migliorare la concorrenza delle applicazioni per l'elaborazione delle transazioni online (OLTP). Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la concorrenza è basata esclusivamente sul blocco. Questa caratteristica può causare problemi di blocco e deadlock per alcune applicazioni. L'isolamento dello snapshot dipende dai miglioramenti apportati al controllo delle versioni delle righe e ha lo scopo di migliorare le prestazioni evitando scenari di blocco in lettura/scrittura.  
  
 Le transazioni avviate con l'isolamento dello snapshot leggono uno snapshot del database che risale all'avvio della transazione. Se aperti nel contesto di una transazione snapshot, i cursori server keyset statici e dinamici si comporteranno come cursori statici aperti in transazioni serializzabili. Quando tuttavia i cursori vengono aperti con il livello di isolamento dello snapshot, non vengono acquisiti blocchi. In questo modo viene ridotto il numero di blocchi nel server.  
  
## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server  
 In OLE DB Driver per SQL Server sono disponibili miglioramenti che sfruttano l'isolamento dello snapshot introdotto in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Tra questi miglioramenti figurano le modifiche ai set di proprietà DBPROPSET_DATASOURCEINFO e DBPROPSET_SESSION.  
  
### <a name="dbpropset_datasourceinfo"></a>DBPROPSET_DATASOURCEINFO  
 Il set di proprietà DBPROPSET_DATASOURCEINFO è stato modificato per indicare che il livello di isolamento dello snapshot è supportato dall'aggiunta del valore DBPROPVAL_TI_SNAPSHOT utilizzato nella proprietà DBPROP_SUPPORTEDTXNISOLEVELS. Questo nuovo valore indica che il livello di isolamento dello snapshot è supportato indipendentemente dall'abilitazione del controllo delle versioni nel database. La tabella seguente elenca i valori DBPROP_SUPPORTEDTXNISOLEVELS:  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|DBPROP_SUPPORTEDTXNISOLEVELS|Tipo: VT_I4<br /><br /> L/S: Sola lettura<br /><br /> Descrizione: maschera di bit che specifica i livelli di isolamento delle transazioni supportati. Combinazione di zero o più dei valori seguenti:<br /><br /> DBPROPVAL_TI_CHAOS<br /><br /> DBPROPVAL_TI_READUNCOMMITTED<br /><br /> DBPROPVAL_TI_BROWSE<br /><br /> DBPROPVAL_TI_CURSORSTABILITY<br /><br /> DBPROPVAL_TI_READCOMMITTED<br /><br /> DBPROPVAL_TI_REPEATABLEREAD<br /><br /> DBPROPVAL_TI_SERIALIZABLE<br /><br /> DBPROPVAL_TI_ISOLATED<br /><br /> DBPROPVAL_TI_SNAPSHOT|  
  
### <a name="dbpropset_session"></a>DBPROPSET_SESSION  
 Il set di proprietà DBPROPSET_SESSION è stato modificato per indicare che il livello di isolamento dello snapshot è supportato dall'aggiunta del valore DBPROPVAL_TI_SNAPSHOT utilizzato nella proprietà DBPROP_SESS_AUTOCOMMITISOLEVELS. Questo nuovo valore indica che il livello di isolamento dello snapshot è supportato indipendentemente dall'abilitazione del controllo delle versioni nel database. La tabella seguente elenca i valori DBPROP_SESS_AUTOCOMMITISOLEVELS:
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Tipo: VT_I4<br /><br /> L/S: Sola lettura<br /><br /> Descrizione: specifica una maschera di bit che indica il livello di isolamento delle transazioni durante la modalità di autocommit. I valori che è possibile impostare in questa maschera di bit sono uguali a quelli che è possibile impostare per DBPROP_SUPPORTEDTXNISOLEVELS.|  
  
> [!NOTE]  
>  Si verifica l'errore DB_S_ERRORSOCCURRED o DB_E_ERRORSOCCURRED se DBPROPVAL_TI_SNAPSHOT viene impostato quando si utilizzano versioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Per altre informazioni sul supporto dell'isolamento dello snapshot nelle transazioni, vedere [Supporto delle transazioni locali](../../oledb/ole-db-transactions/supporting-local-transactions.md).  

  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Proprietà e comportamenti dei set di righe](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
  
