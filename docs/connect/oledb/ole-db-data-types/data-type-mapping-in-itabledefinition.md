---
title: Mapping dei tipi di dati in ITableDefinition (OLE DB Driver) | Microsoft Docs
description: Informazioni sul modo in cui un consumer OLE DB Driver per SQL Server può specificare tipi di dati SQL Server durante la creazione di tabelle con il metodo ITableDefinition::CreateTable.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- OLE DB Driver for SQL Server, data types
- ITableDefinition interface
- DBCOLUMNDESC structure
- data types [OLE DB]
- CreateTable function
- OLE DB, data types
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e111f1760415a3511686dce17d8dec5784c9a991
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755561"
---
# <a name="data-type-mapping-in-itabledefinition"></a>Mapping dei tipi di dati in ITableDefinition
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In caso di creazione di tabelle tramite la funzione **ITableDefinition::CreateTable**, il consumer del driver OLE DB per SQL Server può specificare tipi di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel membro *pwszTypeName* della matrice DBCOLUMNDESC passata. Se il consumer specifica il tipo di dati di una colonna in base al nome, il mapping del tipo di dati OLE DB rappresentato dal membro *wType* della struttura DBCOLUMNDESC viene ignorato.  
  
 Per specificare nuovi tipi di dati di colonna con tipi di dati OLE DB mediante il membro *wType* della struttura DBCOLUMNDESC, il driver OLE DB per SQL Server esegue il mapping dei tipi di dati OLE DB come segue.  
  
|Tipo di dati OLE DB|SQL Server<br /><br /> Tipo di dati|Informazioni aggiuntive|  
|----------------------|------------------------------|----------------------------|  
|DBTYPE_BOOL|**bit**||  
|DBTYPE_BYTES|**binary**, **varbinary**, **image,** o **varbinary(max)**|OLE DB Driver per SQL Server controlla il membro *ulColumnSize* della struttura DBCOLUMNDESC. In base al valore e alla versione dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], OLE DB Driver per SQL Server esegue il mapping del tipo a **image**.<br /><br /> Se il valore di *ulColumnSize* è minore della lunghezza massima di una colonna del tipo di dati **binary**, il driver OLE DB per SQL Server controlla il membro *rgPropertySets* di DBCOLUMNDESC. Se DBPROP_COL_FIXEDLENGTH è VARIANT_TRUE, OLE DB Driver per SQL Server esegue il mapping del tipo a **binary**. Se il valore della proprietà è VARIANT_FALSE, OLE DB Driver per SQL Server esegue il mapping del tipo a **varbinary**. In entrambi i casi il membro *ulColumnSize* di DBCOLUMNDESC determina la larghezza della colonna di SQL Server creata.|  
|DBTYPE_CY|**money**||  
|DBTYPE_DBTIMESTAMP|**datetime2**||  
|DBTYPE_GUID|**uniqueidentifier**||  
|DBTYPE_I2|**smallint**||  
|DBTYPE_I4|**int**||  
|DBTYPE_I8|**bigint**||
|DBTYPE_NUMERIC|**numeric**|Il driver OLE DB per SQL Server controlla i membri *bPrecision* e *bScale* di DBCOLUMDESC per determinare la precisione e la scala per la colonna **numeric**.|  
|DBTYPE_R4|**real**||  
|DBTYPE_R8|**float**||  
|DBTYPE_STR|**char**, **varchar**, **text,** o **varchar(max)**|OLE DB Driver per SQL Server controlla il membro *ulColumnSize* della struttura DBCOLUMNDESC. In base al valore e alla versione dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], OLE DB Driver per SQL Server esegue il mapping del tipo a **text**.<br /><br /> Se il valore di *ulColumnSize* è minore della lunghezza massima di una colonna del tipo di dati multibyte, il driver OLE DB per SQL Server controlla il membro *rgPropertySets* di DBCOLUMNDESC. Se DBPROP_COL_FIXEDLENGTH è VARIANT_TRUE, OLE DB Driver per SQL Server esegue il mapping del tipo a **char**. Se il valore della proprietà è VARIANT_FALSE, OLE DB Driver per SQL Server esegue il mapping del tipo a **varchar**. In entrambi i casi, il membro *ulColumnSize* di DBCOLUMNDESC determina la larghezza della colonna di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] creata.|  
|DBTYPE_UDT|**UDT**|Le informazioni seguenti vengono usate nelle strutture **DBCOLUMNDESC** impiegate da **ITableDefinition::CreateTable** quando sono necessarie colonne di tipo definito dall'utente:<br /><br /> *pwSzTypeName* viene ignorato.<br /><br /> *rgPropertySets* deve includere un set di proprietà **DBPROPSET_SQLSERVERCOLUMN**, come descritto nella sezione **DBPROPSET_SQLSERVERCOLUMN**, in [Uso dei tipi definiti dall'utente](../../oledb/features/using-user-defined-types.md).|  
|DBTYPE_UI1|**tinyint**||  
|DBTYPE_VARIANT|**sql_variant**||
|DBTYPE_WSTR|**nchar**, **nvarchar**, **ntext,** o **nvarchar(max)**|OLE DB Driver per SQL Server controlla il membro *ulColumnSize* della struttura DBCOLUMNDESC. In base al valore, OLE DB Driver per SQL Server esegue il mapping del tipo a **ntext**.<br /><br /> Se il valore di *ulColumnSize* è minore della lunghezza massima di una colonna del tipo di dati Unicode, il driver OLE DB per SQL Server controlla il membro *rgPropertySets* di DBCOLUMNDESC. Se DBPROP_COL_FIXEDLENGTH è VARIANT_TRUE, OLE DB Driver per SQL Server esegue il mapping del tipo a **nchar**. Se il valore della proprietà è VARIANT_FALSE, OLE DB Driver per SQL Server esegue il mapping del tipo a **nvarchar**. In entrambi i casi, il membro *ulColumnSize* di DBCOLUMNDESC determina la larghezza della colonna di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] creata.|  
|DBTYPE_XML|**XML**||  

> [!NOTE]  
>  In caso di creazione di una nuova tabella, il driver OLE DB per SQL Server esegue il mapping solo dei valori di enumerazione del tipo di dati OLE DB specificati nella tabella precedente. Il tentativo di creare una tabella con una colonna di qualsiasi altro tipo di dati OLE DB DB genera un errore.  

## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;OLE DB&#41;](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
  
