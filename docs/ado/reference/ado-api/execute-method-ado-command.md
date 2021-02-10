---
description: Metodo Execute (Command - ADO)
title: Metodo Execute (comando ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Execute
- Command15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: f84a5ff3-0528-4ad7-9bea-9a15103378dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 74e86bfb03ca77d103594009b7ddbbddb6b5b894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034161"
---
# <a name="execute-method-ado-command"></a>Metodo Execute (Command - ADO)
Esegue la query, l'istruzione SQL o stored procedure specificata nella proprietà [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) dell' [oggetto Command](../../../ado/reference/ado-api/command-object-ado.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Set recordset = command.Execute( RecordsAffected, Parameters, Options )  
```  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un riferimento a un oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) , un flusso o **Nothing**.  
  
#### <a name="parameters"></a>Parametri  
 *RecordsAffected*  
 facoltativo. Variabile **Long** a cui il provider restituisce il numero di record interessati dall'operazione. Il parametro *RecordsAffected* è valido solo per le query di azione o le stored procedure. *RecordsAffected* non restituisce il numero di record restituiti da una query che restituisce un risultato o stored procedure. Per ottenere queste informazioni, utilizzare la proprietà [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) . Il metodo **Execute** non restituirà le informazioni corrette quando viene usato con **adAsyncExecute**, semplicemente perché quando un comando viene eseguito in modo asincrono, il numero di record interessati potrebbe non essere ancora noto nel momento in cui il metodo restituisce.  
  
 *Parametri*  
 facoltativo. Matrice **Variant** di valori di parametro utilizzata insieme alla stringa o al flusso di input specificato in **CommandText** o **CommandStream**. I parametri di output non restituiranno valori corretti quando vengono passati in questo argomento.  
  
 *Opzioni*  
 facoltativo. Valore **Long** che indica il modo in cui il provider deve valutare il valore [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o la proprietà [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) dell'oggetto [Command](../../../ado/reference/ado-api/command-object-ado.md) . Può essere un valore di maschera di maschera creato con valori [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) e/o [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) . Ad esempio, è possibile utilizzare **adCmdText** e **adExecuteNoRecords** in combinazione se si desidera che ADO valuti il valore della proprietà **CommandText** come testo e indichi che il comando deve eliminare e non restituire i record che potrebbero essere generati quando viene eseguito il testo del comando.  
  
> [!NOTE]
>  Usare il  valore ExecuteOptionEnum **adExecuteNoRecords** per migliorare le prestazioni riducendo al minimo l'elaborazione interna. Se è stato specificato **adExecuteStream** , le opzioni **adAsyncFetch** e **adAsynchFetchNonBlocking** verranno ignorate. Non usare i valori **CommandTypeEnum** di **adCmdFile** o **adCmdTableDirect** con **Execute**. Questi valori possono essere usati solo come opzioni con i metodi [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) e [Requery](../../../ado/reference/ado-api/requery-method.md) di un **Recordset**.  
  
## <a name="remarks"></a>Commenti  
 Utilizzando il metodo **Execute** su un oggetto **Command** viene eseguita la query specificata nella proprietà **CommandText** o **CommandStream** dell'oggetto.  
  
 I risultati vengono restituiti in un **Recordset** (per impostazione predefinita) o come flusso di informazioni binarie. Per ottenere un flusso binario, specificare **adExecuteStream** in *options*, quindi fornire un flusso impostando **Command. Properties ("Stream di output")**. È possibile specificare un oggetto **flusso** ADO per ricevere i risultati oppure è possibile specificare un altro oggetto flusso, ad esempio l'oggetto Response di IIS. Se non è stato specificato alcun flusso prima di chiamare **Execute** con **adExecuteStream**, si verifica un errore. La posizione del flusso al ritorno da **Execute** è specifica del provider.  
  
 Se il comando non è destinato a restituire risultati (ad esempio, una query SQL UPDATE), il provider non restituisce **alcun** valore purché venga specificata l'opzione **adExecuteNoRecords** . in caso contrario Execute restituisce un **Recordset** chiuso. Alcuni linguaggi applicativi consentono di ignorare questo valore restituito se non si desidera alcun **Recordset** .  
  
 **Execute** genera un errore se l'utente specifica un valore per **CommandStream** quando **CommandType** è **adCmdStoredProc**, **adCmdTable** o **adCmdTableDirect**.  
  
 Se la query include parametri, verranno utilizzati i valori correnti per i parametri dell'oggetto **comando** , a meno che non vengano sostituiti con i valori dei parametri passati con la chiamata **Execute** . È possibile eseguire l'override di un subset di parametri omettendo i nuovi valori per alcuni parametri quando si chiama il metodo **Execute** . L'ordine in cui vengono specificati i parametri è lo stesso ordine in cui vengono passati dal metodo. Se ad esempio sono presenti quattro o più parametri e si desidera passare nuovi valori solo per il primo e il quarto parametro, passare `Array(var1,,,var4)` come argomento *Parameters* .  
  
> [!NOTE]
>  I parametri di output non restituiranno valori corretti quando vengono passati nell'argomento *Parameters* .  
  
 Quando si conclude questa operazione, verrà emesso un evento [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) .  
  
> [!NOTE]
>  Quando si inviano comandi contenenti URL, quelli che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Execute, Requery e Clear (VB)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [Esempio di metodi Execute, Requery e Clear (VBScript)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [Esempio di metodi Execute, Requery e Clear (VC + +)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [Proprietà CommandStream (ADO)](../../../ado/reference/ado-api/commandstream-property-ado.md)   
 [Proprietà CommandText (ADO)](../../../ado/reference/ado-api/commandtext-property-ado.md)   
 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)   
 [Metodo Execute (connessione ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [Evento ExecuteComplete (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)
