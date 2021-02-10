---
description: Oggetto Cellset (ADO MD)
title: Oggetto cellt (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset
helpviewer_keywords:
- Cellset object [ADO MD]
ms.assetid: 5e2452c0-cac0-49b2-8099-836c35794d50
author: rothja
ms.author: jroth
ms.openlocfilehash: dc64756d2b2ac679e185c4dff8888352a7865f83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055726"
---
# <a name="cellset-object-ado-md"></a>Oggetto Cellset (ADO MD)
Rappresenta i risultati di una query multidimensionale. Si tratta di una raccolta di celle selezionate da cubi o altri celle.  
  
## <a name="remarks"></a>Commenti  
 I dati all'interno di un insieme di **celle** vengono recuperati mediante l'accesso diretto a un tipo di matrice. È possibile eseguire il drill-down di un membro specifico per ottenere i dati relativi a tale membro. Il codice seguente, ad esempio, restituisce la didascalia del primo membro nella prima posizione sul primo asse di un celle denominato `cst` :  
  
```  
cst.Axes(0).Positions(0).Members(0).Caption  
```  
  
 Non esiste alcuna nozione di una cella corrente all'interno di un celle. Al contrario, la proprietà [Item](./item-property-ado-md-cellset.md) recupera un oggetto [cella](./cell-object-ado-md.md) specifico dal cellt. Gli argomenti della proprietà **Item** determinano quale cella viene recuperata. È possibile specificare il valore ordinale univoco di una cella. È anche possibile recuperare le celle usando i numeri di posizione lungo ogni asse del celle. Per ulteriori informazioni sul recupero di celle, vedere la proprietà [Item](./item-property-ado-md-cellset.md) .  
  
 Con le raccolte, i metodi e le proprietà di un oggetto di un insieme di **celle** , è possibile eseguire le operazioni seguenti:  
  
-   Associare una connessione aperta a un oggetto set di **celle** impostando la relativa proprietà [ActiveConnection](./activeconnection-property-ado-md.md) .  
  
-   Eseguire e recuperare i risultati di una query multidimensionale con il metodo [Open](./open-method-ado-md.md) .  
  
-   Recuperare una **cella** da un insieme di **celle** con la proprietà [Item](./item-property-ado-md-cellset.md) .  
  
-   Restituisce gli oggetti [asse](./axis-object-ado-md.md) che definiscono il gruppo di **celle** con la raccolta [assi](./axes-collection-ado-md.md) .  
  
-   Recuperare le informazioni sulle dimensioni utilizzate per filtrare i dati nel **celle** con la proprietà [FilterAxis](./filteraxis-property-ado-md.md) .  
  
-   Restituisce o specifica la query utilizzata per definire il **celle** con la proprietà di [origine](./source-property-ado-md.md) .  
  
-   Restituisce lo stato corrente del **cellt** (aperto, chiuso, in esecuzione o in connessione) con la proprietà [state](./state-property-ado-md.md) .  
  
-   Chiude un insieme di **celle** aperto con il metodo [Close](./close-method-ado-md.md) .  
  
-   Recuperare le informazioni specifiche del provider relative al **celle** con la raccolta delle [Proprietà](../ado-api/properties-collection-ado.md) ADO standard.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./cellset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di celle (VB)](./cellset-example-vb.md)   
 [Raccolta assi (ADO MD)](./axes-collection-ado-md.md)   
 [Oggetto Cell (ADO MD)](./cell-object-ado-md.md)   
 [Oggetto Connection (ADO)](../ado-api/connection-object-ado.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)