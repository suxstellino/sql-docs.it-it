---
description: Oggetto User (ADOX)
title: Oggetto User (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- User
helpviewer_keywords:
- User object [ADOX]
ms.assetid: f68e32ce-ef7c-407d-bdb5-d280947ae0e2
author: rothja
ms.author: jroth
ms.openlocfilehash: e37c09d4793a96056ab5d8003bf022b1d250b744
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049681"
---
# <a name="user-object-adox"></a>Oggetto User (ADOX)
Rappresenta un account utente che dispone di autorizzazioni di accesso all'interno di un database protetto.  
  
## <a name="remarks"></a>Commenti  
 La raccolta [Users](./users-collection-adox.md) di un [Catalogo](./catalog-object-adox.md) rappresenta tutti gli utenti del catalogo. La raccolta **Users** per un [gruppo](./group-object-adox.md) rappresenta solo gli utenti del gruppo specifico.  
  
 Con le proprietà, le raccolte e i metodi di un oggetto **utente** , è possibile:  
  
-   Identificare l'utente con la proprietà [Name](./name-property-adox.md) .  
  
-   Modificare la password per un utente con il metodo [ChangePassword](./changepassword-method-adox.md) .  
  
-   Determinare se un utente dispone di autorizzazioni di lettura, scrittura o eliminazione con i metodi [GetPermissions](./getpermissions-method-adox.md) e [sepermissions](./setpermissions-method-adox.md) .  
  
-   Accedere ai gruppi a cui appartiene un utente con la raccolta di [gruppi](./groups-collection-adox.md) .  
  
-   Accedere alle proprietà specifiche del provider con la raccolta [Properties](../ado-api/properties-collection-ado.md) .  
  
-   Determinare il [ParentCatalog](./parentcatalog-property-adox.md) per un utente.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto User](./user-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi GetPermissions e sepermissions (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Raccolta Groups (ADOX)](./groups-collection-adox.md)   
 [Raccolta Users (ADOX)](./users-collection-adox.md)