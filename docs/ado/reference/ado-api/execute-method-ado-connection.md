---
description: Metodo Execute (Connection - ADO)
title: Metodo Execute (connessione ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Execute
- Connection15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 03c69320-96b2-4d85-8d49-a13b13e31578
author: rothja
ms.author: jroth
ms.openlocfilehash: c91fd13744afd106b2a0cb8e986d803f1b722c24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025105"
---
# <a name="execute-method-ado-connection"></a>Metodo Execute (Connection - ADO)
Esegue la query, l'istruzione SQL, il stored procedure o il testo specifico del provider specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un riferimento a un oggetto [Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
#### <a name="parameters"></a>Parametri  
 *CommandText*  
 Valore **stringa** che contiene l'istruzione SQL, stored procedure, un URL o un testo specifico del provider da eseguire. **Facoltativamente**, è possibile utilizzare i nomi di tabella, ma solo se il provider è compatibile con SQL. Se, ad esempio, viene utilizzato il nome di tabella "Customers", ADO antepone automaticamente la sintassi SQL SELECT standard al modulo e passerà "SELECT * FROM Customers" come [!INCLUDE[tsql](../../../includes/tsql-md.md)] istruzione al provider.  
  
 *RecordsAffected*  
 facoltativo. Variabile **Long** a cui il provider restituisce il numero di record interessati dall'operazione.  
  
 *Opzioni*  
 facoltativo. Valore **Long** che indica la modalità di valutazione dell'argomento CommandText da parte del provider. Può essere una maschera di maschera di uno o più valori [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) o [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) .  
  
 **Nota** Usare il  valore ExecuteOptionEnum **adExecuteNoRecords** per migliorare le prestazioni riducendo al minimo l'elaborazione interna e per le applicazioni da trasferire da Visual Basic 6,0.  
  
 Non utilizzare **adExecuteStream** con il metodo **Execute** di un oggetto **Connection** .  
  
 Non usare i valori CommandTypeEnum di adCmdFile o adCmdTableDirect con Execute. Questi valori possono essere utilizzati solo come opzioni con il [metodo Open (recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) e i metodi del [metodo di riquery](../../../ado/reference/ado-api/requery-method.md) di un **Recordset**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzando il metodo **Execute** su un oggetto [Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) viene eseguita qualsiasi query passata al metodo nell'argomento CommandText della connessione specificata. Se l'argomento CommandText specifica una query che restituisce una riga, tutti i risultati generati dall'esecuzione vengono archiviati in un nuovo oggetto **Recordset** . Se il comando non è destinato a restituire risultati (ad esempio, una query SQL UPDATE), il provider non restituisce **alcun** valore purché venga specificata l'opzione **adExecuteNoRecords** . in caso contrario Execute restituisce un **Recordset** chiuso.  
  
 L'oggetto **Recordset** restituito è sempre un cursore di sola lettura e di sola trasmissione. Se è necessario un oggetto **Recordset** con una maggiore funzionalità, creare innanzitutto un oggetto **Recordset** con le impostazioni di proprietà desiderate, quindi utilizzare il metodo [Open Method (ADO recordset)](../../../ado/reference/ado-api/open-method-ado-recordset.md) dell'oggetto **Recordset** per eseguire la query e restituire il tipo di cursore desiderato.  
  
 Il contenuto dell'argomento *CommandText* è specifico del provider e può essere una sintassi SQL standard o qualsiasi formato di comando speciale supportato dal provider.  
  
 Quando si conclude questa operazione, verrà emesso un evento ExecuteComplete.  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)
