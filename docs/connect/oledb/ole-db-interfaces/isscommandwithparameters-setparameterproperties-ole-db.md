---
title: ISSCommandWithParameters::SetParameterProperties (OLE DB) | Microsoft Docs
description: Informazioni sul modo in cui il metodo ISSCommandWithParameters::SetParameterProperties imposta le proprietà dei parametri in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters::SetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- SetParameterProperties method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a4cd90ca969231452112b2e41fc0521066199480
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199351"
---
# <a name="isscommandwithparameterssetparameterproperties-ole-db"></a>ISSCommandWithParameters::SetParameterProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Imposta le proprietà dei parametri per i singoli parametri in base al numero ordinale oppure imposta proprietà dei parametri bulk specificando una matrice di strutture SSPARAMPROPS.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT SetParameterProperties(  
      DB_UPARAMS cParams,   
      SSPARAMPROPS rgParamProperties[]);  
```  
  
## <a name="arguments"></a>Argomenti  
 *cParams*[in]  
 Numero di strutture SSPARAMPROPS nella matrice *rgParamProperties*. Se questo numero è zero, **ISSCommandWithParameters::SetParameterProperties** eliminerà tutte le proprietà che possono essere state specificate per i parametri nel comando.  
  
 *rgParamProperties*[in]  
 Matrice di strutture SSPARAMPROPS da impostare.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Il metodo **ISSCommandWithParameters::SetParameterProperties** restituisce gli stessi codici di errore del metodo **ICommandProperties::SetProperties** OLE DB di base.  
  
## <a name="remarks"></a>Osservazioni  
 L'impostazione delle proprietà del parametro con questo metodo è consentita per i singoli parametri in base al numero ordinale oppure con una sola chiamata a **ISSCommandWithParameters::SetParameterProperties** dopo che la struttura SSPARAMPROPS è stata compilata dalla matrice della proprietà.  
  
 Il metodo **SetParameterInfo** deve essere chiamato prima del metodo **ISSCommandWithParameters::SetParameterProperties**. Se si chiama `SetParameterProperties(0, NULL)`, si cancellano tutte le proprietà di parametro specificate, mentre se si chiama `SetParameterInfo(0,NULL,NULL)`, si cancellano tutte le informazioni di parametro che includono le proprietà che potrebbero essere associate a un parametro.  
  
 Se si chiama **ISSCommandWithParameters::SetParameterProperties** per specificare le proprietà per un parametro che non è del tipo DBTYPE_XML o DBTYPE_UDT viene restituito DB_E_ERRORSOCCURRED o DB_S_ERRORSOCCURRED e il campo *dwStatus* di tutte le matrici DBPROP contenute nella struttura SSPARAMPROPS per quel parametro viene contrassegnato con DBPROPSTATUS_NOTSET. È necessario attraversare la matrice DBPROP di ogni proprietà DBPROPSET contenuta nella struttura SSPARAMPROPS per individuare il parametro al quale DB_E_ERRORSOCCURRED o DB_S_ERRORSOCCURRED fa riferimento.  
  
 Se viene chiamato **ISSCommandWithParameters::SetParameterProperties** per specificare le proprietà di parametri le cui informazioni non sono state ancora impostate con **SetParameterInfo**, il provider restituisce E_UNEXPECTED con il messaggio di errore seguente:  
  
 Impossibile chiamare il metodo SetParameterProperties per i parametri specificati senza chiamare prima il metodo SetParameterInfo. È necessario impostare le informazioni sui parametri prima di impostare le proprietà dei parametri stessi.  
  
 Se la chiamata a **ISSCommandWithParameters::SetParameterProperties** contiene alcuni parametri per i quali le informazioni sono state impostate e alcuni parametri per i quali non sono state impostate, le proprietà dwStatus nella struttura DBPROPSET del set di proprietà SSPARAMPROPS verranno restituite con DBSTATUS_NOTSET.  
  
 La struttura SSPARAMPROPS viene definita nel modo seguente:  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
 I miglioramenti apportati al motore di database a partire da [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] consentono a ISSCommandWithParameters::SetParameterProperties di ottenere descrizioni più accurate dei risultati previsti. È possibile che questi risultati più accurati differiscano dai valori restituiti da ISSCommandWithParameters::SetParameterProperties nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Metadata Discovery](../../oledb/features/metadata-discovery.md).  
  
|Membro|Descrizione|  
|------------|-----------------|  
|*iOrdinal*|Numero ordinale del parametro passato.|  
|*cPropertySets*|Numero di strutture DBPROPSET in *rgPropertySets*.|  
|*rgPropertySets*|Puntatore alla memoria nel quale restituire una matrice di strutture DBPROPSET.|  
  
## <a name="see-also"></a>Vedere anche  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
