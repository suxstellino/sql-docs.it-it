---
description: Oggetto Group (ADOX)
title: Oggetto Group (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Group
helpviewer_keywords:
- group object [ADOX]
ms.assetid: 55ef0ade-68ea-4da5-8aa5-4cd27d1f6d1e
author: rothja
ms.author: jroth
ms.openlocfilehash: b72179e91ad4c9742ffd42a70ff37e33f402d8e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054196"
---
# <a name="group-object-adox"></a>Oggetto Group (ADOX)
Rappresenta un account di gruppo che dispone di autorizzazioni di accesso all'interno di un database protetto.  
  
## <a name="remarks"></a>Commenti  
 La raccolta di [gruppi](./groups-collection-adox.md) di un [Catalogo](./catalog-object-adox.md) rappresenta tutti gli account di gruppo del catalogo. La raccolta **gruppi** per un [utente](./user-object-adox.md) rappresenta solo il gruppo a cui appartiene l'utente.  
  
 Con le proprietà, le raccolte e i metodi di un oggetto **gruppo** , è possibile:  
  
-   Identificare il gruppo con la proprietà [Name](./name-property-adox.md) .  
  
-   Determinare se un gruppo dispone di autorizzazioni di lettura, scrittura o eliminazione con i metodi [GetPermissions](./getpermissions-method-adox.md) e [sepermissions](./setpermissions-method-adox.md) .  
  
-   Accedere agli account utente che dispongono di appartenenze al gruppo con la raccolta [Users](./users-collection-adox.md) .  
  
-   Accedere alle proprietà specifiche del provider con la raccolta [Properties](../ado-api/properties-collection-ado.md) .  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Group](./group-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Raccolta Groups (ADOX)](./groups-collection-adox.md)   
 [Raccolta Users (ADOX)](./users-collection-adox.md)