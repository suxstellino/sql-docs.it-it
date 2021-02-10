---
description: Oggetto Command (ADO)
title: Oggetto Command (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command
helpviewer_keywords:
- Command object [ADO]
ms.assetid: a02c22fb-542d-465e-a629-30fd59dcbebf
author: rothja
ms.author: jroth
ms.openlocfilehash: c8cb910eef7ef810b444813c1e6678b745c524e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026957"
---
# <a name="command-object-ado"></a>Oggetto Command (ADO)
Definisce un comando specifico che si desidera eseguire su un'origine dati.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare un oggetto **comando** per eseguire una query su un database e restituire record in un oggetto [Recordset](./recordset-object-ado.md) , per eseguire un'operazione bulk o per modificare la struttura di un database. A seconda della funzionalità del provider, alcune raccolte di **comandi** , metodi o proprietà possono generare un errore quando vi si fa riferimento.  
  
 Con le raccolte, i metodi e le proprietà di un oggetto **Command** , è possibile eseguire le operazioni seguenti:  
  
-   Definire il testo eseguibile del comando (ad esempio, un'istruzione SQL) con la proprietà [CommandText](./commandtext-property-ado.md) . In alternativa, per le strutture di comando o di query diverse dalle stringhe semplici (ad esempio, query modello XML) definire il comando con la proprietà [CommandStream](./commandstream-property-ado.md) .  
  
-   Facoltativamente, indicare il dialetto del comando utilizzato in **CommandText** o **CommandStream** con la proprietà [dialect](./dialect-property.md) .  
  
-   Definire query con parametri o argomenti di stored procedure con oggetti [Parameter](./parameter-object.md) e la raccolta [Parameters](./parameters-collection-ado.md) .  
  
-   Indica se i nomi dei parametri devono essere passati al provider con la proprietà [namedParameters](./namedparameters-property-ado.md) .  
  
-   Eseguire un comando e restituire un oggetto **Recordset** se appropriato con il metodo [Execute](./execute-method-ado-command.md) .  
  
-   Specificare il tipo di comando con la proprietà [CommandType](./commandtype-property-ado.md) prima dell'esecuzione per ottimizzare le prestazioni.  
  
-   Controllare se il provider salva una versione preparata o compilata del comando prima dell'esecuzione con la proprietà [preparata](./prepared-property-ado.md) .  
  
-   Impostare il numero di secondi durante i quali un provider attende che un comando venga eseguito con la proprietà [CommandTimeout](./commandtimeout-property-ado.md) .  
  
-   Associare una connessione aperta a un oggetto **Command** impostando la relativa proprietà [ActiveConnection](./activeconnection-property-ado.md) .  
  
-   Impostare la proprietà [Name](./name-property-ado.md) per identificare l'oggetto **Command** come metodo sull'oggetto [Connection](./connection-object-ado.md) associato.  
  
-   Passare un oggetto **Command** alla proprietà [source](./source-property-ado-recordset.md) di un **Recordset** per ottenere i dati.  
  
-   Accedere agli attributi specifici del provider con la raccolta [Properties](./properties-collection-ado.md) .  
  
> [!NOTE]
>  Per eseguire una query senza usare un oggetto **Command** , passare una stringa di query al metodo [Execute](./execute-method-ado-connection.md) di un oggetto **Connection** o al metodo [Open](./open-method-ado-recordset.md) di un oggetto **Recordset** . Tuttavia, è necessario un oggetto **comando** quando si desidera salvare in modo permanente il testo del comando ed eseguirlo di nuovo oppure utilizzare parametri di query.  
  
 Per creare un oggetto **comando** indipendentemente da un oggetto **connessione** definito in precedenza, impostare la relativa proprietà **ActiveConnection** su una stringa di connessione valida. ADO crea ancora un oggetto **connessione** , ma non assegna tale oggetto a una variabile oggetto. Tuttavia, se si associano più oggetti **Command** con la stessa connessione, è necessario creare e aprire in modo esplicito un oggetto **Connection** . in questo modo l'oggetto **connessione** viene assegnato a una variabile oggetto. Verificare che l'oggetto **connessione** sia stato aperto correttamente prima di assegnarlo alla proprietà **ActiveConnection** dell'oggetto **Command** , perché l'assegnazione di un oggetto **connessione** chiusa causa un errore. Se non si imposta la proprietà **ActiveConnection** dell'oggetto **Command** su questa variabile oggetto, ADO crea un nuovo oggetto **Connection** per ogni oggetto **Command** , anche se si utilizza la stessa stringa di connessione.  
  
 Per eseguire un **comando**, chiamarlo tramite la relativa proprietà [Name](./name-property-ado.md) nell'oggetto **Connection** associato. Per il **comando** è necessario impostare la proprietà **ActiveConnection** sull'oggetto **Connection** . Se il **comando** dispone di parametri, passare i relativi valori come argomenti al metodo.  
  
 Se due o più oggetti **Command** vengono eseguiti sulla stessa connessione e l'oggetto **command** è un stored procedure con parametri di output, si verificherà un errore. Per eseguire ogni oggetto **Command** , utilizzare connessioni separate o disconnettere tutti gli altri oggetti **Command** dalla connessione.  
  
 La raccolta **Parameters** è il membro predefinito dell'oggetto **Command** . Di conseguenza, le due istruzioni di codice seguenti sono equivalenti.  
  
```  
objCmd.Parameters.Item(0)  
objCmd(0)  
```  
  
-   L'oggetto **Command** non è sicuro per lo scripting.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Command](./command-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Connection (ADO)](./connection-object-ado.md)   
 [Raccolta Parameters (ADO)](./parameters-collection-ado.md)   
 [Raccolta Properties (ADO)](./properties-collection-ado.md)   
 [Appendice A: Provider](../../guide/appendixes/appendix-a-providers.md)