---
description: 'ISSCommandWithParameters:: GetParameterProperties in SQL Server Native Client (OLE DB)'
title: ISSCommandWithParameters::GetParameterProperties (OLE DB)
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
ms.assetid: 7f4cc5ea-d028-4fe5-9192-bd153ab3c26c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c641a32268a793b17a9b000d7aa6cdcd663e5e28
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750081"
---
# <a name="isscommandwithparametersgetparameterproperties-in-sql-server-native-client-ole-db"></a>ISSCommandWithParameters:: GetParameterProperties in SQL Server Native Client (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una matrice di strutture di set di proprietà SSPARAMPROPS, un set di proprietà SSPARAMPROPS per ogni tipo definito dall'utente o parametro XML.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>Argomenti  
 *pcParams*[out][in]  
 Puntatore alla memoria contenente il numero di strutture SSPARAMPROPS restituite in *prgParamProperties*.  
  
 *prgParamProperties*[out]  
 Puntatore alla memoria nel quale viene restituita una matrice di strutture SSPARAMPROPS. Il provider alloca memoria per le strutture e restituisce l'indirizzo alla memoria. il consumer rilascia la memoria con **IMalloc:: Free** quando non necessita più delle strutture. Prima di chiamare **IMalloc:: Free** per *prgParamProperties*, il consumer deve chiamare anche **VariantClear** per la proprietà *vValue* di ogni struttura DBPROP per evitare una perdita di memoria nei casi in cui la variante contiene un tipo di riferimento, ad esempio un BSTR. Se *pcParams* è zero nell'output o si verifica un errore diverso da DB_E_ERRORSOCCURRED, il provider non alloca alcuna memoria e assicura che *prgParamProperties* sia un puntatore null nell'output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Il metodo **GetParameterProperties** restituisce gli stessi codici di errore del metodo Core OLE DB **ICommandProperties:: GetProperties** , ad eccezione del fatto che non è possibile generare DB_S_ERRORSOCCURRED e DB_E_ERRORSOCCURED.  
  
## <a name="remarks"></a>Commenti  
 **ISSCommandWithParameters:: GetParameterProperties** si comporta in modo coerente rispetto a **GetParameterInfo**. Se [ISSCommandWithParameters:: SetParameterProperties](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) o **sqlparameterinfo** non sono stati chiamati o sono stati chiamati con cParams uguale a zero, **GetParameterInfo** deriva le informazioni sui parametri e restituisce this. Se **ISSCommandWithParameters:: SetParameterProperties** o **separameterinfo** è stato chiamato per almeno un parametro, **ISSCommandWithParameters:: GetParameterProperties** restituisce proprietà solo per i parametri per i quali è stato chiamato **ISSCommandWithParameters:: SetParameterProperties** . Se **ISSCommandWithParameters:: SetParameterProperties** viene chiamato dopo **ISSCommandWithParameters:: GetParameterProperties** o **GetParameterInfo**, le chiamate successive a **ISSCommandWithParameters:: GetParameterProperties** restituiscono i valori sottoposti a override per i parametri per i quali è stato chiamato **ISSCommandWithParameters:: SetParameterProperties** .  
  
 La struttura SSPARAMPROPS viene definita nel modo seguente:  

```cpp
struct SSPARAMPROPS {
    DBORDINAL iOrdinal;
    ULONG cPropertySets;
    DBPROPSET *rgPropertySets;
};
```

|Membro|Descrizione|  
|------------|-----------------|  
|*iOrdinal*|Numero ordinale del parametro passato.|  
|*cPropertySets*|Numero di strutture DBPROPSET in *rgPropertySets*.|  
|*rgPropertySets*|Puntatore alla memoria nel quale restituire una matrice di strutture DBPROPSET.|  
|||

## <a name="see-also"></a>Vedere anche  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
