---
title: ISSDataClassification::GetSensitivityClassification | Microsoft Docs
description: ISSDataClassification::GetSensitivityClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification::GetSensitivityClassification
apitype: COM
helpviewer_keywords:
- GetSensitivityClassification method
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 93c630b2164015c9efedc3a00e51a67ff7fdce25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342477"
---
# <a name="issdataclassificationgetsensitivityclassification"></a>ISSDataClassification::GetSensitivityClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Recupera i dati di classificazione di riservatezza per il set di righe attivo. Per altre informazioni ed esempi di codice, vedere [Uso della classificazione dei dati](../features/using-data-classification.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT GetSensitivityClassification(
    SENSITIVITYCLASSIFICATION** ppSensitivityClassification);
```  
  
## <a name="arguments"></a>Argomenti  
  *ppSensitivityClassification*[out]  
 Puntatore a un puntatore della struttura SENSITIVITYCLASSIFICATION. Se il metodo non riesce o non sono disponibili informazioni relative alla classificazione dei dati, il provider non alloca memoria e verifica che l'argomento ppSensitivityClassification sia un puntatore Null nell'output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.    
  
 E_INVALIDARG  
 L'argomento *ppSensitivityClassification* era Null.  
  
 E_OUTOFMEMORY  
 OLE DB Driver per SQL Server non è riuscito ad allocare una quantità di memoria sufficiente per completare la richiesta.  

  
## <a name="remarks"></a>Osservazioni  
OLE DB Driver per SQL Server alloca un blocco di memoria per conservare la struttura SENSITIVITYCLASSIFICATION e i dati a cui la struttura fa riferimento. Quando l'accesso ai dati di classificazione non è più necessario, il consumer deve deallocare questa memoria chiamando il metodo [IMalloc::Free](/windows/win32/api/objidl/nf-objidl-imalloc-free).  
  
 La struttura SENSITIVITYCLASSIFICATION è definita nel modo seguente:
  
```cpp
typedef struct tagSensitivityClassification
{
    USHORT                     cSensitivityLabels;
    SENSITIVITYLABEL          *rgSensitivityLabels;
    USHORT                     cInformationTypes;
    INFORMATIONTYPE           *rgInformationTypes;
    USHORT                     cColumnSensitivityMetadata;
    COLUMNSENSITIVITYMETADATA *rgColumnSensitivityMetadata;
    SENSITIVITYRANKENUM        eQuerySensitivityRank;
} SENSITIVITYCLASSIFICATION;
```  

|Membro|Descrizione|  
|------------|-----------------|  
|*cSensitivityLabels*|Numero di strutture SENSITIVITYLABEL in *rgSensitivityLabels*.|  
|*rgSensitivityLabels*|Matrice di strutture SENSITIVITYLABEL.|  
|*cInformationTypes*|Numero di strutture INFORMATIONTYPE in *rgInformationTypes*.|  
|*rgInformationTypes*|Matrice di strutture INFORMATIONTYPE.|  
|*cColumnSensitivityMetadata*|Numero di strutture COLUMNSENSITIVITYMETADATA in *rgColumnSensitivityMetadata*.|  
|*rgColumnSensitivityMetadata*|Matrice di strutture COLUMNSENSITIVITYMETADATA.|  
|*eQuerySensitivityRank*|Classificazione relativa della riservatezza di una query eseguita per ottenere il set di righe.|  

La struttura SENSITIVITYLABEL è definita nel modo seguente:
```cpp
typedef struct tagSENSITIVITYLABEL
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} SENSITIVITYLABEL;
```

|Membro|Descrizione|  
|------------|-----------------|  
|*pwszName*|Nome di un'etichetta di riservatezza.|  
|*pwszId*|Identificatore per un'etichetta di riservatezza.|  

La struttura INFORMATIONTYPE è definita nel modo seguente:
```cpp
typedef struct tagINFORMATIONTYPE
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} INFORMATIONTYPE;
```

|Membro|Descrizione|  
|------------|-----------------|  
|*pwszName*|Nome di un tipo di informazioni.|  
|*pwszId*|Identificatore per un tipo di informazioni.|  

La struttura COLUMNSENSITIVITYMETADATA è definita nel modo seguente:
```cpp
typedef struct tagCOLUMNSENSITIVITYMETADATA
{
    SENSITIVITYPROPERTY* rgSensitivityProperties;
    USHORT cSensitivityProperties;
} COLUMNSENSITIVITYMETADATA;
```

|Membro|Descrizione|  
|------------|-----------------|  
|*cSensitivityProperties*|Numero di strutture SENSITIVITYPROPERTY in *rgSensitivityProperties*.|  
|*rgSensitivityProperties*|Matrice di strutture SENSITIVITYPROPERTY.|  

L'enumerazione SENSITIVITYRANKENUM è definita nel modo seguente:
```cpp
typedef enum tagSENSITIVITYRANKENUM
{
    SENSITIVITYRANK_NOT_DEFINED = -1,
    SENSITIVITYRANK_NONE = 0,
    SENSITIVITYRANK_LOW = 10,
    SENSITIVITYRANK_MEDIUM = 20,
    SENSITIVITYRANK_HIGH = 30,
    SENSITIVITYRANK_CRITICAL = 40
} SENSITIVITYRANKENUM;
```

La struttura SENSITIVITYPROPERTY è definita nel modo seguente:
```cpp
typedef struct tagSENSITIVITYPROPERTY
{
    SENSITIVITYLABEL* pSensitivityLabel;
    INFORMATIONTYPE* pInformationType;
    SENSITIVITYRANKENUM eSensitivityRank;
} SENSITIVITYPROPERTY;
```

|Membro|Descrizione|  
|------------|-----------------|  
|*pSensitivityLabel*|Puntatore a una struttura SENSITIVITYLABEL.|  
|*pInformationType*|Puntatore a una struttura INFORMATIONTYPE.|  
|*eSensitivityRank*|Classificazione relativa della riservatezza di una colonna che fa parte dei dati per colonna.|  

## <a name="see-also"></a>Vedere anche  
 [ISSDataClassification](../../oledb/ole-db-interfaces/issdataclassification-ole-db.md)  
 [Set di righe](../ole-db-rowsets/rowsets.md)  
