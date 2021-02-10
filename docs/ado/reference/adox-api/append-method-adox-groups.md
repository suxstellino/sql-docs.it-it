---
description: Metodo Append (raccolta Groups ADOX)
title: Metodo Append (gruppi ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Groups::raw_Append
- Groups::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 56b94fc6-7ef0-4e4a-82a3-033b94c46036
author: rothja
ms.author: jroth
ms.openlocfilehash: 846475567fb15e549f96094ca55a1881d8888ce5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050532"
---
# <a name="append-method-adox-groups"></a>Metodo Append (raccolta Groups ADOX)
Aggiunge un nuovo oggetto [gruppo](./group-object-adox.md) alla raccolta di [gruppi](./groups-collection-adox.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Groups.Append Group  
```  
  
#### <a name="parameters"></a>Parametri  
 *Gruppo*  
 Oggetto **gruppo** da accodare o il nome del gruppo da creare e accodare.  
  
## <a name="remarks"></a>Commenti  
 La raccolta di **gruppi** di un [Catalogo](./catalog-object-adox.md) rappresenta tutti gli account di gruppo del catalogo. La raccolta **gruppi** per un [utente](./user-object-adox.md) rappresenta solo il gruppo a cui appartiene l'utente.  
  
 Se il provider non supporta la creazione di gruppi, si verificherà un errore.  
  
> [!NOTE]
>  Prima di aggiungere un **oggetto gruppo** alla raccolta **di gruppi** di un oggetto **utente** , un oggetto **gruppo** con lo stesso [nome](./name-property-adox.md) di quello da accodare deve esistere già nella raccolta di **gruppi** del **Catalogo**.  
  
## <a name="applies-to"></a>Si applica a  
 [Raccolta di Groups (ADOX)](./groups-collection-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Accodamento di gruppi e utenti, esempio di Metodi ChangePassword (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Metodo Append (colonne ADOX)](./append-method-adox-columns.md)   
 [Metodo Append (indici ADOX)](./append-method-adox-indexes.md)   
 [Metodo Append (chiavi ADOX)](./append-method-adox-keys.md)   
 [Metodo Append (routine ADOX)](./append-method-adox-procedures.md)   
 [Metodo Append (tabelle ADOX)](./append-method-adox-tables.md)   
 [Metodo Append (utenti ADOX)](./append-method-adox-users.md)   
 [Metodo Append (raccolta Views ADOX)](./append-method-adox-views.md)