---
description: Proprietà ActiveConnection (ADO MD)
title: Proprietà ActiveConnection (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ActiveConnection
- Cellset::ActiveConnection
- Catalog::ActiveConnection
helpviewer_keywords:
- ActiveConnection property [ADO MD]
ms.assetid: 2509b32c-a995-4364-9152-d8c83129bdd8
author: rothja
ms.author: jroth
ms.openlocfilehash: 879e8ddd84eb7c4fc386cb0828191ce0441b5f38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056006"
---
# <a name="activeconnection-property-ado-md"></a>Proprietà ActiveConnection (ADO MD)
Indica a quale oggetto della [connessione](../ado-api/connection-object-ado.md) ADO appartiene attualmente il Cell o il catalogo corrente.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce una **variante** che contiene una stringa che definisce una connessione o un oggetto **connessione** . Il valore predefinito è vuoto.  
  
## <a name="remarks"></a>Commenti  
 È possibile impostare questa proprietà su un oggetto **connessione** ADO valido o su una stringa di connessione valida. Quando questa proprietà è impostata su una stringa di connessione, il provider crea un nuovo oggetto **connessione** utilizzando questa definizione e apre la connessione.  
  
 Se si usa l'argomento *ActiveConnection* del metodo [Open](./open-method-ado-md.md) per aprire un oggetto [cellt](./cellset-object-ado-md.md) , la proprietà **ActiveConnection** erediterà il valore dell'argomento.  
  
 Se si imposta la proprietà **ActiveConnection** di un oggetto [Catalog](./catalog-object-ado-md.md) su Nothing, i dati associati **non** vengono rilasciati, inclusi i dati nella raccolta [CubeDefs](./cubedefs-collection-ado-md.md) e gli oggetti [dimensione](./dimension-object-ado-md.md), [gerarchia](./hierarchy-object-ado-md.md), [livello](./level-object-ado-md.md)e [membro](./member-object-ado-md.md) correlati. La chiusura di un oggetto **Connection** utilizzato per aprire un **Catalogo** ha lo stesso effetto dell'impostazione della proprietà **ActiveConnection** su **Nothing**.  
  
 La modifica del database predefinito della connessione a cui fa riferimento la proprietà **ActiveConnection** di un oggetto **Catalog** invalida il contenuto del **Catalogo**.  
  
 Si verificherà un errore se si tenta di modificare la proprietà **ActiveConnection** per un oggetto del **celle** aperto.  
  
> [!NOTE]
>  In Visual Basic, ricordarsi di usare la parola chiave **set** quando si imposta la proprietà **ActiveConnection** su un oggetto **Connection** . Se si omette la parola chiave **set** , si imposta la proprietà **ActiveConnection** uguale alla proprietà predefinita dell'oggetto **Connection** , **ConnectionString**. Il codice funzionerà; Tuttavia, si creerà una connessione aggiuntiva all'origine dati, che può avere implicazioni negative sulle prestazioni.  
  
 Quando si utilizza il provider di dati MSOLAP, impostare l'origine dati in una stringa di connessione su un nome di server e impostare il catalogo iniziale sul nome di un catalogo dall'origine dati. Per connettersi a un file di cubo disconnesso da un server, impostare il percorso sul percorso completo della. File CUB. In entrambi i casi, impostare il provider sul nome del provider. Ad esempio, la stringa seguente usa il provider MSOLAP per connettersi a un catalogo denominato Bob video Store in un server denominato **ServerName**:  
  
```  
"Data Source=Servername;Initial Catalog=Bobs Video Store;Provider=msolap"  
```  
  
 La stringa seguente si connette a un file di cubo locale nel percorso C:\MSDASDK\samples\oledb\olap\data\bobsvid.cub:  
  
```  
"Location=C:\MSDASDK\samples\oledb\olap\data\bobsvid.cub;Provider=msolap"  
```  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto Catalog (ADO MD)](./catalog-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Oggetto Cellset (ADO MD)](./cellset-object-ado-md.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di celle (VB)](./cellset-example-vb.md)   
 [Oggetto Connection (ADO)](../ado-api/connection-object-ado.md)   
 [Metodo Open (ADO MD)](./open-method-ado-md.md)