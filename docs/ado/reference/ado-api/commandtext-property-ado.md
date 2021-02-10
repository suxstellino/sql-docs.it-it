---
description: Proprietà CommandText (ADO)
title: Proprietà CommandText (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandText
helpviewer_keywords:
- CommandText property [ADO]
ms.assetid: 4dd7e82a-8da5-4a4e-b439-11a29286fa0e
author: rothja
ms.author: jroth
ms.openlocfilehash: fb418696392fe250bd659ba68306df10a7c36a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026616"
---
# <a name="commandtext-property-ado"></a>Proprietà CommandText (ADO)
Indica il testo di un comando da emettere a fronte di un provider.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Ottiene o imposta un valore **stringa** che contiene un comando del provider, ad esempio un'istruzione SQL, un nome di tabella, un URL relativo o una chiamata stored procedure. Il valore predefinito è la stringa vuota ("").  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **CommandText** per impostare o restituire il testo di un comando rappresentato da un oggetto [comando](./command-object-ado.md) . Si tratta in genere di un'istruzione SQL, ma può anche essere qualsiasi altro tipo di istruzione di comando riconosciuta dal provider, ad esempio una chiamata stored procedure. Un'istruzione SQL deve essere del dialetto o della versione particolare supportata da query processor del provider.  
  
 Se la proprietà [preparata](./prepared-property-ado.md) dell'oggetto **Command** è impostata su **true** e l'oggetto **Command** è associato a una connessione aperta quando si imposta la proprietà **CommandText** , ADO prepara la query, ovvero un form compilato della query archiviata dal provider, quando si chiamano i metodi [Execute](./execute-method-ado-command.md) o [Open](./open-method-ado-connection.md) .  
  
 A seconda dell'impostazione della proprietà [CommandType](./commandtype-property-ado.md) , ADO potrebbe modificare la proprietà **CommandText** . È possibile leggere la proprietà **CommandText** in qualsiasi momento per visualizzare il testo effettivo del comando che ADO utilizzerà durante l'esecuzione.  
  
 Utilizzare la proprietà **CommandText** per impostare o restituire un URL relativo che specifichi una risorsa, ad esempio un file o una directory. La risorsa è relativa a una posizione specificata in modo esplicito da un URL assoluto o in modo implicito da un oggetto [connessione](./connection-object-ado.md) aperta.  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Metodo Requery](./requery-method.md)   
 [Esempio di proprietà ActiveConnection, CommandText, CommandTimeout, CommandType, Size e Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)