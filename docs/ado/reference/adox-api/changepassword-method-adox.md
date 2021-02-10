---
description: Metodo ChangePassword (ADOX)
title: Metodo ChangePassword (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::raw_ChangePassword
- _User25::ChangePassword
helpviewer_keywords:
- ChangePassword method [ADOX]
ms.assetid: d187fbc6-5fac-4abb-803d-bf344dcf0302
author: rothja
ms.author: jroth
ms.openlocfilehash: c18385154c38d7e55d60cf3286e0340ceacf94c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054446"
---
# <a name="changepassword-method-adox"></a>Metodo ChangePassword (ADOX)
Modifica la password per un account [utente](./user-object-adox.md) .  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
User.ChangePassword OldPassword, NewPassword  
```  
  
#### <a name="parameters"></a>Parametri  
 *OldPassword*  
 Valore **stringa** che specifica la password esistente dell'utente. Se l'utente non dispone attualmente di una password, utilizzare una stringa vuota ("") per *oldPassword*.  
  
 *NewPassword*  
 Valore **stringa** che specifica la nuova password.  
  
## <a name="remarks"></a>Commenti  
 Per motivi di sicurezza, è necessario specificare la vecchia password oltre alla nuova password.  
  
 Si verificherà un errore se il provider non supporta l'amministrazione delle proprietà del trustee.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto User (ADOX)](./user-object-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio dei metodi Append di Groups e Users e del metodo ChangePassword (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)