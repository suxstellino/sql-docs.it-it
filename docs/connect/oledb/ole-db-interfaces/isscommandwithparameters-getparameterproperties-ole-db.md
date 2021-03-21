---
title: ISSCommandWithParameters::GetParameterProperties (OLE DB) | Microsoft Docs
description: ISSCommandWithParameters::GetParameterProperties restituisce una matrice di strutture di set di proprietà in OLE DB Driver per SQL Server, una per ogni parametro XML o definito dall'utente.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a982287a57dd033021bd806fc0c9aaba98212439
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753161"
---
# <a name="isscommandwithparametersgetparameterproperties-ole-db"></a>ISSCommandWithParameters::GetParameterProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Restituisce una matrice di strutture di set di proprietà SSPARAMPROPS, un set di proprietà SSPARAMPROPS per ogni tipo definito dall'utente o parametro XML.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>Argomenti  
 *pcParams*[out][in]  
 Puntatore alla memoria contenente il numero di strutture SSPARAMPROPS restituite in *prgParamProperties*.  
  
 *prgParamProperties*[out]  
 Puntatore alla memoria nel quale viene restituita una matrice di strutture SSPARAMPROPS. Il provider alloca la memoria per le strutture e restituisce l'indirizzo alla memoria. Il consumer rilascia questa memoria con `IMalloc::Free` quando le strutture non sono più necessarie. Prima di chiamare `IMalloc::Free` per *prgParamProperties*, il consumer deve chiamare anche `VariantClear` per la proprietà *vValue* di ogni struttura DBPROP in modo da impedire una perdita di memoria nei casi in cui la variante contiene un tipo di riferimento, ad esempio BSTR. Se *pcParams* è zero nell'output o si verifica un errore diverso da DB_E_ERRORSOCCURRED, il provider non alloca alcuna memoria e verifica che *prgParamProperties* sia un puntatore Null nell'output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Il metodo `GetParameterProperties` restituisce gli stessi codici di errore del metodo `ICommandProperties::GetProperties` di OLE DB, a meno che non sia possibile generare DB_S_ERRORSOCCURRED e DB_E_ERRORSOCCURED.  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo `ISSCommandWithParameters::GetParameterProperties` si comporta in modo coerente rispetto a `GetParameterInfo`. Se [`ISSCommandWithParameters::SetParameterProperties`](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) o `SetParameterInfo` non sono stati chiamati o sono stati chiamati con cParams uguale a zero, `GetParameterInfo` deriva le informazioni sui parametri e le restituisce. Se `ISSCommandWithParameters::SetParameterProperties` o `SetParameterInfo` sono stati chiamati per almeno un parametro, il metodo `ISSCommandWithParameters::GetParameterProperties` restituisce proprietà solo per i parametri per i quali è stato chiamato `ISSCommandWithParameters::SetParameterProperties`. Se `ISSCommandWithParameters::SetParameterProperties` viene chiamato dopo `ISSCommandWithParameters::GetParameterProperties` o `GetParameterInfo`, le chiamate successive a `ISSCommandWithParameters::GetParameterProperties` restituiscono i valori sottoposti a override per i parametri per i quali è stato chiamato il metodo `ISSCommandWithParameters::SetParameterProperties`.  
  
 La struttura SSPARAMPROPS viene definita nel modo seguente:  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
|Membro|Descrizione|  
|------------|-----------------|  
|*iOrdinal*|Numero ordinale del parametro passato.|  
|*cPropertySets*|Numero di strutture DBPROPSET in *rgPropertySets*.|  
|*rgPropertySets*|Puntatore alla memoria nel quale restituire una matrice di strutture DBPROPSET.|  
  
## <a name="see-also"></a>Vedere anche  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
