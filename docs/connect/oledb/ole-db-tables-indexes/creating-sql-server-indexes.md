---
title: Creare indici di SQL Server (OLE DB Driver) | Microsoft Docs
description: OLE DB Driver per SQL Server espone la funzione IIndexDefinition::CreateIndex, che consente ai consumer di definire nuovi indici sulle tabelle di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CreateIndex function
- constraints [OLE DB]
- OLE DB Driver for SQL Server, indexes
- indexes [OLE DB]
- adding indexes
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8ac23ac73b8eb24ac6d8efd93416a46cb8aab4d3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749201"
---
# <a name="creating-sql-server-indexes"></a>Creazione di indici SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server espone la funzione **IIndexDefinition::CreateIndex**, consentendo ai consumer di definire nuovi indici nelle tabelle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 OLE DB Driver per SQL Server crea gli indici di tabella come indici o vincoli. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fornisce privilegio di creazione del vincolo al proprietario della tabella, del database e ai membri di determinati ruoli amministrativi. Per impostazione predefinita, solo il proprietario di tabella può creare un indice in una tabella. L'esito positivo o negativo di **CreateIndex** dipende quindi non solo dai diritti di accesso dell'utente dell'applicazione ma anche dal tipo di indice creato.  
  
 I consumer specificano il nome della tabella come stringa di caratteri Unicode nel membro *pwszName* dell'unione *uName* nel parametro *pTableID*. Il membro *eKind* di *pTableID* deve essere DBKIND_NAME.  
  
 Il parametro *pIndexID* può essere NULL e, se lo è, il driver OLE DB per SQL Server crea un nome univoco per l'indice. Il consumer può acquisire il nome dell'indice specificando un puntatore valido a un DBID nel parametro *ppIndexID*.  
  
 Il consumer può specificare il nome dell'indice come stringa di caratteri Unicode nel membro *pwszName* dell'unione *uName* del parametro *pTableID*. Il membro *eKind* di *pIndexID* deve essere DBKIND_NAME.  
  
 Il consumer specifica la colonna o le colonne utilizzate nell'indice in base al nome. Per ogni struttura DBINDEXCOLUMNDESC usata in **CreateIndex**, il membro *eKind* di *pColumnID* deve essere DBKIND_NAME. Il nome della colonna viene specificato come stringa di caratteri Unicode nel membro *pwszName* dell'unione *uName* in *pColumnID*.  
  
 OLE DB Driver per SQL Server e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supportano l'ordine crescente nei valori dell'indice. Il driver OLE DB per SQL Server restituisce E_INVALIDARG se il consumer specifica DBINDEX_COL_ORDER_DESC in una qualsiasi struttura DBINDEXCOLUMNDESC.  
  
 **CreateIndex** interpreta le proprietà di indice come segue.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|DBPROP_INDEX_AUTOUPDATE|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_CLUSTERED|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: VARIANT_FALSE<br /><br /> Descrizione: controlla il clustering dell'indice.<br /><br /> VARIANT_TRUE: OLE DB Driver per SQL Server tenta di creare un indice cluster nella tabella [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supporta al massimo un indice con cluster su una tabella.<br /><br /> VARIANT_FALSE: OLE DB Driver per SQL Server tenta di creare un indice non cluster nella tabella [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|DBPROP_INDEX_FILLFACTOR|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: 0<br /><br /> Descrizione: specifica la percentuale di una pagina di indice usata per l'archiviazione. Per altre informazioni, vedere [CREATE INDEX](../../../t-sql/statements/create-index-transact-sql.md).<br /><br /> Il tipo della variante è VT_I4. Deve essere maggiore o uguale a 1 e minore o uguale a 100.|  
|DBPROP_INDEX_INITIALIZE|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_NULLCOLLATION|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_NULLS|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_PRIMARYKEY|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: VARIANT_FALSE Descrizione: crea l'indice come integrità referenziale, vincolo PRIMARY KEY.<br /><br /> VARIANT_TRUE: l'indice viene creato per supportare il vincolo PRIMARY KEY della tabella. È necessario che le colonne non ammettano valori Null.<br /><br /> VARIANT_FALSE: l'indice non viene usato come vincolo PRIMARY KEY per i valori di riga nella tabella.|  
|DBPROP_INDEX_SORTBOOKMARKS|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_TEMPINDEX|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_TYPE|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: nessuno<br /><br /> Descrizione: OLE DB Driver per SQL Server non supporta questa proprietà. I tentativi di impostare la proprietà in **CreateIndex** determinano un valore restituito DB_S_ERRORSOCCURRED. Il membro *dwStatus* della struttura di proprietà indica DBPROPSTATUS_BADVALUE.|  
|DBPROP_INDEX_UNIQUE|R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: VARIANT_FALSE<br /><br /> Descrizione: crea l'indice come vincolo UNIQUE nella colonna o nelle colonne usate.<br /><br /> VARIANT_TRUE: l'indice viene usato per vincolare in modo univoco i valori di riga della tabella.<br /><br /> VARIANT_FALSE: l'indice non vincola in modo univoco i valori di riga.|  
  
 Nel set di proprietà DBPROPSET_SQLSERVERINDEX specifico del provider il driver OLE DB per SQL Server definisce la seguente proprietà delle informazioni sulle origini dati.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|SSPROP_INDEX_XML|Digitare: VT_BOOL (R/W)<br /><br /> Predefinito: VARIANT_FALSE<br /><br /> Descrizione: quando questa proprietà viene specificata con un valore VARIANT_TRUE con IIndexDefinition::CreateIndex, determina la creazione di un indice xml primario corrispondente alla colonna da indicizzare. Se questa proprietà è VARIANT_TRUE, cIndexColumnDescs deve essere 1, in caso contrario si tratta di un errore.|  
  
 In questo esempio viene creato un indice di chiave primaria:  
  
```  
// This CREATE TABLE statement shows the referential integrity and   
// PRIMARY KEY constraint on the OrderDetails table that will be created   
// by the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//        CONSTRAINT PK_OrderDetails  
//        PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
HRESULT CreatePrimaryKey  
    (  
    IIndexDefinition* pIIndexDefinition  
    )  
    {  
    HRESULT             hr = S_OK;  
  
    DBID                dbidTable;  
    DBID                dbidIndex;  
    const ULONG         nCols = 2;  
    ULONG               nCol;  
    const ULONG         nProps = 2;  
    ULONG               nProp;  
  
    DBINDEXCOLUMNDESC   dbidxcoldesc[nCols];  
    DBPROP              dbpropIndex[nProps];  
    DBPROPSET           dbpropset;  
  
    DBID*               pdbidIndexOut = NULL;  
  
    // Set up identifiers for the table and index.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    dbidIndex.eKind = DBKIND_NAME;  
    dbidIndex.uName.pwszName = L"PK_OrderDetails";  
  
    // Set up column identifiers.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbidxcoldesc[nCol].pColumnID = new DBID;  
        dbidxcoldesc[nCol].pColumnID->eKind = DBKIND_NAME;  
  
        dbidxcoldesc[nCol].eIndexColOrder = DBINDEX_COL_ORDER_ASC;  
        }  
    dbidxcoldesc[0].pColumnID->uName.pwszName = L"OrderID";  
    dbidxcoldesc[1].pColumnID->uName.pwszName = L"ProductID";  
  
    // Set properties for the index. The index is clustered,  
    // PRIMARY KEY.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        dbpropIndex[nProp].dwOptions = DBPROPOPTIONS_REQUIRED;  
        dbpropIndex[nProp].colid = DB_NULLID;  
  
        VariantInit(&(dbpropIndex[nProp].vValue));  
  
        dbpropIndex[nProp].vValue.vt = VT_BOOL;  
        }  
    dbpropIndex[0].dwPropertyID = DBPROP_INDEX_CLUSTERED;  
    dbpropIndex[0].vValue.boolVal = VARIANT_TRUE;  
  
    dbpropIndex[1].dwPropertyID = DBPROP_INDEX_PRIMARYKEY;  
    dbpropIndex[1].vValue.boolVal = VARIANT_TRUE;  
  
    dbpropset.rgProperties = dbpropIndex;  
    dbpropset.cProperties = nProps;  
    dbpropset.guidPropertySet = DBPROPSET_INDEX;  
  
    hr = pIIndexDefinition->CreateIndex(&dbidTable, &dbidIndex, nCols,  
        dbidxcoldesc, 1, &dbpropset, &pdbidIndexOut);  
  
    // Clean up dynamically allocated DBIDs.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        delete dbidxcoldesc[nCol].pColumnID;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle e indici](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)  
  
  
