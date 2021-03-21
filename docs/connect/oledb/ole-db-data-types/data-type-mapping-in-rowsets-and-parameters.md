---
title: Mapping dei tipi di dati in set di righe e parametri (OLE DB Driver) | Microsoft Docs
description: Informazioni su come OLE DB Driver per SQL Server rappresenta i dati SQL Server nei set di righe e come valori dei parametri, usando i tipi di dati definiti in OLE DB.
ms.custom: ''
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- OLE DB Driver for SQL Server, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 466e379b2ce809f72fb39c4d7594a85ce85ebad5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749161"
---
# <a name="data-type-mapping-in-rowsets-and-parameters"></a>Mapping dei tipi di dati in set di righe e parametri
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Nei set di righe e come valori di parametro, il driver OLE DB per SQL Server rappresenta i dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando i tipi di dati definiti da OLE DB seguenti, registrati nelle funzioni **IColumnsInfo::GetColumnInfo** e **ICommandWithParameters::GetParameterInfo**.  
  
|Tipo di dati di SQL Server|Tipo di dati OLE DB|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binary**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIMESTAMP|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**image**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT, DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**text**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 OLE DB Driver per SQL Server supporta le conversioni di dati richieste dal consumer, come mostrato nell'illustrazione.  
  
 Gli oggetti **sql_variant** possono contenere dati di qualsiasi tipo di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ad eccezione di text, ntext, image, varchar(max), nvarchar(max), varbinary(max), xml, timestamp e dei tipi CLR definiti dall'utente di Microsoft .NET Framework. Per un'istanza di dati sql_variant, inoltre, il tipo di dati di base sottostante non può corrispondere a sql_variant. La colonna può ad esempio contenere valori **smallint** per alcune righe, valori **float** per altre righe e valori **char**/**nchar** per la parte restante delle righe.  
  
> [!NOTE]  
>  Il tipo di dati **sql_variant** è simile al tipo Variant di Microsoft Visual Basic® e ai tipi DBTYPE_VARIANT e DBTYPE_SQLVARIANT di OLE DB.  
  
 Quando i dati **sql_variant** vengono recuperati come DBTYPE_VARIANT, vengono inseriti in una struttura VARIANT nel buffer. Tuttavia i sottotipi presenti nella struttura VARIANT non possono eseguire il mapping a sottotipi definiti nel tipo di dati **sql_variant**. Perché tutti i sottotipi siano corrispondenti, è necessario che i dati **sql_variant** vengano quindi recuperati come DBTYPE_SQLVARIANT.  
  
## <a name="dbtype_sqlvariant-data-type"></a>Tipo di dati DBTYPE_SQLVARIANT  
 Per supportare il tipo di dati **sql_variant**, il driver OLE DB per SQL Server espone un tipo di dati specifico del provider denominato DBTYPE_SQLVARIANT. Quando i dati **sql_variant** vengono recuperati come DBTYPE_SQLVARIANT, vengono archiviati in una struttura SSVARIANT specifica del provider. La struttura SSVARIANT contiene tutti i sottotipi che corrispondono ai sottotipi del tipo di dati **sql_variant**.  
  
 La proprietà di sessione SSPROP_ALLOWNATIVEVARIANT deve essere inoltre impostata su TRUE.  
  
## <a name="provider-specific-property-ssprop_allownativevariant"></a>Proprietà SSPROP_ALLOWNATIVEVARIANT specifica del provider  
 Durante il recupero dei dati è possibile specificare in modo esplicito il tipo di dati da restituire per una colonna o un parametro. È anche possibile servirsi di **IColumnsInfo** per ottenere le informazioni sulle colonne da usare per definire l'associazione. Quando **IColumnsInfo** viene usato a tale scopo, se la proprietà di sessione SSPROP_ALLOWNATIVEVARIANT è FALSE (valore predefinito), viene restituito DBTYPE_VARIANT per le colonne **sql_variant**. Se la proprietà SSPROP_ALLOWNATIVEVARIANT è FALSE, DBTYPE_SQLVARIANT non è supportato. Se la proprietà SSPROP_ALLOWNATIVEVARIANT è impostata su TRUE, il tipo di colonna viene restituito come DBTYPE_SQLVARIANT, nel qual caso il buffer conterrà la struttura SSVARIANT. Durante il recupero dei dati **sql_variant** come DBTYPE_SQLVARIANT la proprietà di sessione SSPROP_ALLOWNATIVEVARIANT deve essere impostata su TRUE.  
  
 La proprietà SSPROP_ALLOWNATIVEVARIANT fa parte del set di proprietà DBPROPSET_SQLSERVERSESSION specifico del provider ed è una proprietà di sessione.  
  
 La proprietà DBTYPE_VARIANT si applica a tutti gli altri provider OLE DB.  
  
## <a name="ssprop_allownativevariant"></a>SSPROP_ALLOWNATIVEVARIANT  
 La proprietà SSPROP_ALLOWNATIVEVARIANT è una proprietà di sessione e fa parte del set di proprietà DBPROPSET_SQLSERVERSESSION.  
  
|Proprietà|Descrizione|  
|-|-|  
|SSPROP_ALLOWNATIVEVARIANT|Digitare: VT_BOOL<br /><br /> R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: VARIANT_FALSE<br /><br /> Descrizione: determina se i dati recuperati appartengono alla categoria DBTYPE_VARIANT o DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE: il tipo di colonna viene restituito come DBTYPE_SQLVARIANT e in tal caso il buffer conserva la struttura SSVARIANT.<br /><br /> VARIANT_FALSE: il tipo di colonna viene restituito come DBTYPE_VARIANT e il buffer presenta la struttura VARIANT.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;OLE DB&#41;](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
  
