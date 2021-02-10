---
description: Proprietà ActiveConnection (ADO)
title: Proprietà ActiveConnection (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::ActiveConnection
- Recordset15::get_ActiveConnection
- _Record::ActiveConnection
helpviewer_keywords:
- ActiveConnection property [ADO]
ms.assetid: 52d0a96c-14fb-4ad9-b004-4d821bc0a6db
author: rothja
ms.author: jroth
ms.openlocfilehash: 318bb7478bdbc6f3b4007f788045438cdb91dfe3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035921"
---
# <a name="activeconnection-property-ado"></a>Proprietà ActiveConnection (ADO)
Indica a quale oggetto [connessione](./connection-object-ado.md) appartiene attualmente il [comando](./command-object-ado.md), il [Recordset](./recordset-object-ado.md)o l'oggetto [record](./record-object-ado.md) specificato.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **stringa** che contiene una definizione per una connessione se la connessione viene chiusa o una **variante** contenente l'oggetto **connessione** corrente se la connessione è aperta. Il valore predefinito è un riferimento a un oggetto null. Vedere la proprietà [ConnectionString](./connectionstring-property-ado.md) .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **ActiveConnection** per determinare l'oggetto **connessione** in cui verrà eseguito l'oggetto **comando** specificato oppure verrà aperto il **Recordset** specificato.  
  
## <a name="command"></a>Comando  
 Per gli oggetti **Command** , la proprietà **ActiveConnection** è di lettura/scrittura.  
  
 Se si tenta di chiamare il metodo [Execute](./execute-method-ado-command.md) su un oggetto **Command** prima di impostare questa proprietà su un oggetto **Connection** aperto o su una stringa di connessione valida, si verificherà un errore.  
  
 Se un oggetto **connessione** viene assegnato alla proprietà **ActiveConnection** , l'oggetto deve essere aperto. L'assegnazione di un oggetto connessione chiusa causa un errore.  
  
### <a name="note"></a>Nota  
 **Visual Basic Microsoft** Se si imposta la proprietà **ActiveConnection** su *Nothing* , l'oggetto **Command** viene **scollegato dalla connessione** corrente e il provider rilascia tutte le risorse associate nell'origine dati. È quindi possibile associare l'oggetto **Command** allo stesso oggetto o a un altro oggetto **Connection** . Alcuni provider consentono di modificare l'impostazione della proprietà da una **connessione** a un'altra, senza dover prima impostare la proprietà su *Nothing*.  
  
 Se la raccolta [Parameters](./parameters-collection-ado.md) dell'oggetto **Command** contiene parametri forniti dal provider, la raccolta viene cancellata se si imposta la proprietà **ActiveConnection** su *Nothing* o su un altro oggetto **Connection** . Se si creano manualmente [oggetti Parameter](./parameter-object.md) e li si usa per compilare la raccolta **Parameters** dell'oggetto **Command** , l'impostazione della proprietà **ActiveConnection** su *Nothing* o su un altro oggetto **Connection** lascia intatta la raccolta **Parameters** .  
  
 La chiusura dell'oggetto **Connection** a cui è associato un oggetto **Command** imposta la proprietà **ActiveConnection** su *Nothing*. L'impostazione di questa proprietà su un oggetto **connessione** chiuso genera un errore.  
  
## <a name="recordset"></a>recordset  
 Per gli oggetti **Recordset** aperti o per gli oggetti **Recordset** la cui proprietà [source](./source-property-ado-recordset.md) è impostata su un oggetto **Command** valido, la proprietà **ActiveConnection** è di sola lettura. In caso contrario, è di lettura/scrittura.  
  
 È possibile impostare questa proprietà su un oggetto **connessione** valido o su una stringa di connessione valida. In questo caso, il provider crea un nuovo oggetto **connessione** utilizzando questa definizione e apre la connessione. Inoltre, il provider può impostare questa proprietà sul nuovo oggetto **connessione** per fornire un modo per accedere all'oggetto **connessione** per ottenere informazioni estese sugli errori o per eseguire altri comandi.  
  
 Se si usa l'argomento *ActiveConnection* del metodo [Open](./open-method-ado-recordset.md) per aprire un oggetto **Recordset** , la proprietà **ActiveConnection** erediterà il valore dell'argomento.  
  
 Se si imposta la proprietà **source** dell'oggetto **Recordset** su una variabile oggetto **Command** valida, la proprietà **ActiveConnection** del **Recordset** eredita l'impostazione della proprietà **ActiveConnection** dell'oggetto **comando** .  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata su un oggetto **Recordset** sul lato client, questa proprietà può essere impostata solo su una *stringa di connessione* o (in Microsoft Visual Basic o Visual Basic, Scripting Edition).  
  
## <a name="record"></a>Registra  
 Questa proprietà è di lettura/scrittura quando l'oggetto **record** è chiuso e può contenere una stringa di connessione o un riferimento a un oggetto **Connection** aperto. Questa proprietà è di sola lettura quando l'oggetto **record** è aperto e contiene un riferimento a un oggetto **Connection** aperto.  
  
 Un oggetto **connessione** viene creato in modo implicito quando l'oggetto **record** viene aperto da un URL. Aprire il **record** con un oggetto di **connessione** aperto esistente assegnando l'oggetto **connessione** a questa proprietà o utilizzando l'oggetto **connessione** come parametro nella chiamata al metodo [Open](./open-method-ado-record.md) . Se il **record** viene aperto da un **record** o da un [Recordset](./recordset-object-ado.md)esistente, viene automaticamente associato all'oggetto di **connessione** del **record** o dell'oggetto **Recordset** .  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Command (ADO)](./command-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Oggetto Recordset (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Oggetto Connection (ADO)](./connection-object-ado.md)   
 [Proprietà ConnectionString (ADO)](./connectionstring-property-ado.md)