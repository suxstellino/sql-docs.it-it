---
description: Metodo OpenSchema
title: Metodo OpenSchema | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
author: rothja
ms.author: jroth
ms.openlocfilehash: b764f9a28665d31667e9c502c7ad8bdf0b9bbcad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041311"
---
# <a name="openschema-method"></a>Metodo OpenSchema
Ottiene le informazioni sullo schema del database dal provider.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un oggetto [Recordset](./recordset-object-ado.md) che contiene informazioni sullo schema. Il **Recordset** verrà aperto come cursore statico di sola lettura. *QueryType* determina quali colonne vengono visualizzate nel **Recordset**.  
  
#### <a name="parameters"></a>Parametri  
 *QueryType*  
 Qualsiasi valore di [SchemaEnum](./schemaenum.md) che rappresenta il tipo di query dello schema da eseguire.  
  
 *Criteri*  
 facoltativo. Matrice di vincoli di query per ogni opzione *QueryType* , come elencato in [SchemaEnum](./schemaenum.md).  
  
 *SchemaID*  
 GUID per una query di schema del provider non definita dalla specifica OLE DB. Questo parametro è obbligatorio se *QueryType* è impostato su **adSchemaProviderSpecific**; in caso contrario, non viene utilizzato.  
  
## <a name="remarks"></a>Commenti  
 Il metodo **OpenSchema** restituisce informazioni descrittive sull'origine dati, ad esempio le tabelle presenti nell'origine dati, le colonne nelle tabelle e i tipi di dati supportati.  
  
 L'argomento *QueryType* è un GUID che indica le colonne (schemi) restituite. La specifica OLE DB dispone di un elenco completo di schemi.  
  
 L'argomento *criteri* limita i risultati di una query di schema. *Criteri* specifica una matrice di valori che devono essere presenti in un subset di colonne corrispondente, denominate colonne vincolo, nel **Recordset** risultante.  
  
 La costante **adSchemaProviderSpecific** viene utilizzata per l'argomento *QueryType* se il provider definisce le proprie query dello schema non standard al di fuori di quelle elencate in precedenza. Quando si utilizza questa costante, l'argomento *SchemaID* è necessario per passare il GUID della query dello schema da eseguire. Se *QueryType* è impostato su **adSchemaProviderSpecific** ma *SchemaID* non è specificato, verrà generato un errore.  
  
 I provider non devono supportare tutte le query dello schema OLE DB standard. In particolare, solo **adSchemaTables**, **adSchemaColumns** e **adSchemaProviderTypes** sono necessari per la specifica OLE DB. Tuttavia, non è necessario che il provider supporti i vincoli di *criteri* elencati in precedenza per tali query dello schema.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Il metodo **OpenSchema** non è disponibile su un oggetto [connessione](./connection-object-ado.md) sul lato client.  
  
> [!NOTE]
>  In Visual Basic, le colonne con Unsigned Integer a quattro byte (DBTYPE UI4) nel **Recordset** restituito dal metodo **OpenSchema** sull'oggetto **Connection** non possono essere confrontate con altre variabili. Per ulteriori informazioni sui tipi di dati OLE DB, vedere [tipi di dati in OLE DB (OLE DB)](/previous-versions/windows/desktop/ms714931(v=vs.85)) e [appendice a: tipi di dati](/previous-versions/windows/desktop/ms723969(v=vs.85)) di Microsoft OLE DB Programmer ' s Reference.  
  
> [!NOTE]
>  **Utenti di Visual C/C++** Quando non si utilizzano cursori sul lato client, il recupero della "ORDINAL_POSITION" di uno schema di colonna in ADO restituisce una variante di tipo VT_R8 in MDAC 2,7, MDAC 2,8 e Windows Data Access Components (Windows DAC) 6,0, mentre il tipo utilizzato in MDAC 2,6 è stato VT_I4. I programmi scritti per MDAC 2,6 che cercano solo una variante restituita di tipo VT_I4 otterrebbero uno zero per ogni ordinale se eseguiti in MDAC 2,7, MDAC 2,8 e Windows DAC 6,0 senza modifiche. Questa modifica è stata apportata perché il tipo di dati restituito da OLE DB è DBTYPE_UI4 e nel tipo di VT_I4 firmato non è disponibile spazio sufficiente per contenere tutti i valori possibili senza troncamenti che si verificano, causando così una perdita di dati.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo OpenSchema (VB)](./openschema-method-example-vb.md)   
 [Esempio di metodo OpenSchema (VC + +)](./openschema-method-example-vc.md)   
 [Metodo Open (connessione ADO)](./open-method-ado-connection.md)   
 [Metodo Open (record ADO)](./open-method-ado-record.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Metodo Open (flusso ADO)](./open-method-ado-stream.md)   
 [Appendice A: Provider](../../guide/appendixes/appendix-a-providers.md)