---
description: Raccolta Parameters (ADO)
title: Raccolta Parameters (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::get_Parameters
- Command15::Parameters
- Command15::GetParameters
helpviewer_keywords:
- Parameters collection [ADO]
ms.assetid: 497cae10-3913-422a-9753-dcbb0a639b1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 93d34581e7ea19ad584161e12375101a06256cbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041111"
---
# <a name="parameters-collection-ado"></a>Raccolta Parameters (ADO)
Contiene tutti gli oggetti [Parameter](./parameter-object.md) di un oggetto [Command](./command-object-ado.md) .  
  
## <a name="remarks"></a>Commenti  
 Un oggetto **Command** ha una raccolta **Parameters** costituita da oggetti **Parameter** .  
  
 L'utilizzo del metodo [Refresh](./refresh-method-ado.md) sulla raccolta **Parameters** di un oggetto **Command** recupera le informazioni sui parametri del provider per la query stored procedure o con parametri specificata nell'oggetto **Command** . Alcuni provider non supportano chiamate di stored procedure o query con parametri; Se si chiama il metodo **Refresh** sulla raccolta **Parameters** quando si utilizza tale provider, verrà restituito un errore.  
  
 Se non sono stati definiti oggetti **Parameter** personalizzati e si accede alla raccolta **Parameters** prima di chiamare il metodo **Refresh** , ADO chiamerà automaticamente il metodo e compilerà la raccolta.  
  
 È possibile ridurre al minimo le chiamate al provider per migliorare le prestazioni se si conoscono le proprietà dei parametri associati alla stored procedure o alla query con parametri che si desidera chiamare. Usare il metodo [CreateParameter](./createparameter-method-ado.md) per creare oggetti **Parameter** con le impostazioni delle proprietà appropriate e usare il metodo [Append](./append-method-ado.md) per aggiungerli alla raccolta **Parameters** . In questo modo è possibile impostare e restituire i valori dei parametri senza dover chiamare il provider per le informazioni sui parametri. Se si sta scrivendo in un provider che non fornisce informazioni sui parametri, è necessario popolare manualmente la raccolta **Parameters** usando questo metodo per poter usare i parametri. Se necessario, utilizzare il metodo [Delete](./delete-method-ado-parameters-collection.md) per rimuovere gli oggetti **Parameter** dalla raccolta **Parameters** .  
  
 Gli oggetti nella raccolta **Parameters** di un **Recordset** si escono dall'ambito (pertanto non è più disponibile) quando il **Recordset** viene chiuso.  
  
 Quando si chiama un stored procedure con **Command**, il parametro return value/output di un stored procedure viene recuperato nel modo seguente:  
  
1.  Quando si chiama un stored procedure senza parametri, è necessario chiamare il metodo **Refresh** sulla raccolta **Parameters** prima di chiamare il metodo **Execute** sull'oggetto **Command** .  
  
2.  Quando si chiama un stored procedure con parametri e si accoda in modo esplicito un parametro alla raccolta **Parameters** con **Append**, il parametro value/output restituito deve essere aggiunto alla raccolta **Parameters** . Il valore restituito deve essere prima aggiunto alla raccolta **Parameters** . Utilizzare **Append** per aggiungere gli altri parametri nella raccolta **Parameters** nell'ordine di definizione. Ad esempio, il stored procedure SPWithParam ha due parametri. Il primo parametro, *InParam*, è un parametro di input definito come adVarChar (20) e il secondo parametro, *outParam*, è un parametro di output definito come adVarChar (20). È possibile recuperare il parametro di output o il valore restituito con il codice seguente.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Append ccmd.CreateParameter(, adInteger, adParamReturnValue, , NULL)   ' return value  
    ccmd.parameters.Append ccmd.CreateParameter("InParam", adVarChar, adParamInput, 20, "hello world")   ' input parameter  
    ccmd.parameters.Append ccmd.CreateParameter("OutParam", adVarChar, adParamOutput, 20, NULL)   ' output parameter  
  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of this stored procedure  
    ' Access ccmd.parameters("OutParam") as the output parameter of this stored procedure.  
  
    ```  
  
3.  Quando si chiama un stored procedure con parametri e si configurano i parametri chiamando il metodo **Item** sulla raccolta **Parameters** , il parametro restituito value/output del stored procedure può essere recuperato dalla raccolta **Parameters** . Ad esempio, il stored procedure SPWithParam ha due parametri. Il primo parametro, *InParam*, è un parametro di input definito come adVarChar (20) e il secondo parametro, *outParam*, è un parametro di output definito come adVarChar (20). È possibile recuperare il parametro di output o il valore restituito con il codice seguente.  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Item("InParam").value = "hello world" ' input parameter  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of stored procedure  
    ' Access ccmd.parameters(2) or ccmd.parameters("OutParam") as the output parameter.  
    ```  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Parameters](./parameters-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Append (ADO)](./append-method-ado.md)   
 [Metodo CreateParameter (ADO)](./createparameter-method-ado.md)   
 [Oggetto Parameter](./parameter-object.md)