---
description: Raccolta Users (ADOX)
title: Raccolta Users (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Group::Users
- Users
- Catalog::Users
helpviewer_keywords:
- Users collection [ADOX]
ms.assetid: 0a30fa74-6f10-4410-bd70-882e7c43cd46
author: rothja
ms.author: jroth
ms.openlocfilehash: 781f9eb90c621cf8daf4afb0cdf8dc4f1f7b47b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049671"
---
# <a name="users-collection-adox"></a>Raccolta Users (ADOX)
Contiene tutti gli oggetti [utente](./user-object-adox.md) archiviati di un [Catalogo](./catalog-object-adox.md) o di un [gruppo](./group-object-adox.md).  
  
## <a name="remarks"></a>Commenti  
 La raccolta **Users** di un [Catalogo](./catalog-object-adox.md) rappresenta tutti gli utenti del catalogo. La raccolta **Users** per un [gruppo](./group-object-adox.md) rappresenta solo gli utenti che dispongono di un'appartenenza al gruppo specifico.  
  
 Il metodo [Append](./append-method-adox-users.md) per una raccolta **Users** è univoco per ADOX. È possibile:  
  
-   Aggiungere un nuovo utente alla raccolta usando il metodo **Append** .  
  
 Le proprietà e i metodi rimanenti sono standard per le raccolte ADO. È possibile:  
  
-   Accedere a un utente nella raccolta con la proprietà [Item](../ado-api/item-property-ado.md) .  
  
-   Restituisce il numero di utenti contenuti nella raccolta con la proprietà [count](../ado-api/count-property-ado.md) .  
  
-   Rimuovere un utente dalla raccolta con il metodo [Delete](./delete-method-adox-collections.md) .  
  
-   Aggiornare gli oggetti della raccolta in modo che corrispondano allo schema del database corrente con il metodo [Refresh](../ado-api/refresh-method-ado.md) .  
  
> [!NOTE]
>  Prima di aggiungere un oggetto **utente** alla raccolta **Users** di un oggetto **gruppo** , deve esistere già un oggetto **utente** con lo stesso [nome](./name-property-adox.md) di quello da accodare nella raccolta **Users** del **Catalogo**.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Users](./users-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi GetPermissions e sepermissions (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)   
 [Oggetto User (ADOX)](./user-object-adox.md)