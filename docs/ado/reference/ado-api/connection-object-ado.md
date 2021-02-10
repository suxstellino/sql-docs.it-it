---
description: Oggetto Connection (ADO)
title: Oggetto Connection (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection
helpviewer_keywords:
- Connection object [ADO]
ms.assetid: ef6b1824-5b12-43db-89d7-8f3d13896d4d
author: rothja
ms.author: jroth
ms.openlocfilehash: b09ba2f8f026b6c08d64acab6701a0d3da4013f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034771"
---
# <a name="connection-object-ado"></a>Oggetto Connection (ADO)
Rappresenta una connessione aperta a un'origine dati.  
  
## <a name="remarks"></a>Commenti  
 Un oggetto **Connection** rappresenta una sessione univoca con un'origine dati. In un sistema di database client/server, può essere equivalente a una connessione di rete effettiva al server. A seconda della funzionalità supportata dal provider, alcune raccolte, metodi o proprietà di un oggetto **connessione** potrebbero non essere disponibili.  
  
 Con le raccolte, i metodi e le proprietà di un oggetto **connessione** , è possibile eseguire le operazioni seguenti:  
  
-   Configurare la connessione prima di aprirla con le proprietà [ConnectionString](./connectionstring-property-ado.md), [ConnectionTimeout](./connectiontimeout-property-ado.md)e [mode](./mode-property-ado.md) . **ConnectionString** è la proprietà predefinita dell'oggetto **Connection** .  
  
-   Impostare la proprietà [CursorLocation](./cursorlocation-property-ado.md) su client per richiamare il [servizio Microsoft Cursor per OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md), che supporta gli aggiornamenti in batch.  
  
-   Impostare il database predefinito per la connessione con la proprietà [DefaultDatabase](./defaultdatabase-property.md) .  
  
-   Imposta il livello di isolamento per le transazioni aperte sulla connessione con la proprietà [IsolationLevel](./isolationlevel-property.md) .  
  
-   Specificare un provider OLE DB con la proprietà [provider](./provider-property-ado.md) .  
  
-   Stabilire e successivamente interrompere la connessione fisica all'origine dati con i metodi [Open](./open-method-ado-connection.md) e [Close](./close-method-ado.md) .  
  
-   Eseguire un comando sulla connessione con il metodo [Execute](./execute-method-ado-connection.md) e configurare l'esecuzione con la proprietà [CommandTimeout](./commandtimeout-property-ado.md) .  
  
    > [!NOTE]
    >  Per eseguire una query senza usare un oggetto Command, passare una stringa di query al metodo **Execute** di un oggetto **Connection** . Tuttavia, è necessario un oggetto [comando](./command-object-ado.md) quando si desidera salvare in modo permanente il testo del comando ed eseguirlo di nuovo oppure utilizzare parametri di query.  
  
-   Gestire le transazioni sulla connessione aperta, incluse le transazioni nidificate se supportate dal provider, con i metodi [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md), [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md)e [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) e la proprietà [Attributes](./attributes-property-ado.md) .  
  
-   Esaminare gli errori restituiti dall'origine dati con la raccolta [Errors](./errors-collection-ado.md) .  
  
-   Leggere la versione dall'implementazione ADO utilizzata con la proprietà [Version](./version-property-ado.md) .  
  
-   Ottenere informazioni sullo schema relative al database con il metodo [OpenSchema](./openschema-method.md) .  
  
 È possibile creare oggetti **connessione** indipendentemente da qualsiasi altro oggetto definito in precedenza.  
  
 È possibile eseguire comandi denominati o stored procedure come se fossero metodi nativi su un oggetto **Connection** , come illustrato nella sezione successiva. Quando un comando denominato ha lo stesso nome di un stored procedure, richiamare "Native Method Call" in un oggetto **Connection** sempre eseguire il comando denominato invece della stored procedure.  
  
> [!NOTE]
>  Non usare questa funzionalità (chiamando un comando denominato o stored procedure come se fosse un metodo nativo sull'oggetto **Connection** ) in un'applicazione Microsoft® .NET Framework, perché l'implementazione sottostante della funzionalità è in conflitto con il modo in cui il .NET Framework interagisce con com.  
  
## <a name="execute-a-command-as-a-native-method-of-a-connection-object"></a>Eseguire un comando come metodo nativo di un oggetto Connection  
 Per eseguire un comando, assegnare un nome al comando utilizzando la proprietà [nome](./name-property-ado.md) oggetto **comando** . Impostare la proprietà **ActiveConnection** dell'oggetto **Command** sulla connessione. Eseguire quindi un'istruzione in cui il nome del comando viene usato come se fosse un metodo nell'oggetto **Connection** , seguito da qualsiasi parametro e da un oggetto **Recordset** se vengono restituite righe. Impostare le proprietà del **Recordset** per personalizzare il **Recordset** risultante. Ad esempio:  
  
```  
Dim cnn As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
...  
cnn.Open "..."  
cmd.Name = "yourCommandName"  
cmd.ActiveConnection = cnn  
...  
'Your command name, any parameters, and an optional Recordset.  
cnn. "parameter", rst  
```  
  
## <a name="execute-a-stored-procedure-as-a-native-method-of-a-connection-object"></a>Eseguire un stored procedure come metodo nativo di un oggetto Connection  
 Per eseguire una stored procedure, emettere un'istruzione in cui il nome del stored procedure viene usato come se fosse un metodo nell'oggetto **Connection** , seguito da qualsiasi parametro. ADO effettuerà una "ipotesi migliore" dei tipi di parametro. Ad esempio:  
  
```  
Dim cnn As New ADODB.Connection  
...  
'Your stored procedure name and any parameters.  
cnn. "parameter"  
```  
  
 L'oggetto **Connection** è sicuro per lo scripting.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Connection](./connection-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Command (ADO)](./command-object-ado.md)   
 [Raccolta Errors (ADO)](./errors-collection-ado.md)   
 [Raccolta Properties (ADO)](./properties-collection-ado.md)   
 [Oggetto recordset (ADO)](./recordset-object-ado.md)   
 [Appendice A: Provider](../../guide/appendixes/appendix-a-providers.md)