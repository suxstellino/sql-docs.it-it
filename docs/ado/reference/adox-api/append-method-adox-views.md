---
description: Metodo Append (raccolta Views ADOX)
title: Metodo Append (viste ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Views::raw_Append
- Views::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6070fd58-3237-4c77-a966-5b39ce5d57e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 757778961d7b66dd147b8b013b48062dae0cbd91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172280"
---
# <a name="append-method-adox-views"></a>Metodo Append (raccolta Views ADOX)
Crea un nuovo oggetto [visualizzazione](./view-object-adox.md) e lo aggiunge alla raccolta [views](./views-collection-adox.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Views.Append Name, Command  
```  
  
#### <a name="parameters"></a>Parametri  
 *Nome*  
 Valore **stringa** che specifica il nome della visualizzazione da creare.  
  
 *Comando*  
 Oggetto [comando](../ado-api/command-object-ado.md) ADO che rappresenta la visualizzazione da creare.  
  
## <a name="remarks"></a>Commenti  
 Crea una nuova visualizzazione nell'origine dati con il nome e gli attributi specificati nell'oggetto **comando** .  
  
 Se il testo del comando specificato dall'utente rappresenta una procedura anziché una vista, il comportamento dipende dal provider. L' **Accodamento** avrà esito negativo se il provider non supporta la persistenza dei comandi.  
  
> [!NOTE]
>  Quando si usa il provider di OLE DB per Microsoft Jet, il metodo **Append** della raccolta **views** consente di specificare una **procedura** anziché una **vista** nel parametro *Command* . La **procedura** verrà aggiunta all'origine dati e verrà aggiunta alla raccolta **views** . Dopo l' **Accodamento**, se le raccolte **procedure** e **viste** vengono aggiornate, la **procedura** non sarà più presente nella raccolta **views** e verrà visualizzata nella raccolta **Procedures** .  
  
## <a name="applies-to"></a>Si applica a  
 [Raccolta Views (ADOX)](./views-collection-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Append views (VB)](./views-append-method-example-vb.md)   
 [Metodo Append (colonne ADOX)](./append-method-adox-columns.md)   
 [Metodo Append (gruppi ADOX)](./append-method-adox-groups.md)   
 [Metodo Append (indici ADOX)](./append-method-adox-indexes.md)   
 [Metodo Append (chiavi ADOX)](./append-method-adox-keys.md)   
 [Metodo Append (routine ADOX)](./append-method-adox-procedures.md)   
 [Metodo Append (tabelle ADOX)](./append-method-adox-tables.md)   
 [Metodo Append (raccolta Users ADOX)](./append-method-adox-users.md)