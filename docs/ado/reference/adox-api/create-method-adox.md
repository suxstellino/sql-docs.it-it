---
description: Metodo Create (ADOX)
title: Metodo Create (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Catalog::raw_Create
- _Catalog::Create
helpviewer_keywords:
- Create method [ADOX]
ms.assetid: 64f5c21c-b581-42d8-bdc7-c4f1bebaf105
author: rothja
ms.author: jroth
ms.openlocfilehash: 364af90384476b8da388c5ea5f5bfc8467ca97e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054316"
---
# <a name="create-method-adox"></a>Metodo Create (ADOX)
Crea un nuovo catalogo.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Catalog.Create ConnectString  
```  
  
#### <a name="parameters"></a>Parametri  
 *ConnectString*  
 Valore **stringa** utilizzato per la connessione all'origine dati.  
  
## <a name="remarks"></a>Commenti  
 Il metodo **create** crea e apre una nuova [connessione](../ado-api/connection-object-ado.md) ADO all'origine dati specificata in *ConnectString*. In caso di esito positivo, il nuovo oggetto **Connection** viene assegnato alla proprietà [ActiveConnection](./activeconnection-property-adox.md) .  
  
 Se il provider non supporta la creazione di nuovi cataloghi, si verificherà un errore.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Catalog (ADOX)](./catalog-object-adox.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Create (VB)](./create-method-example-vb.md)   
 [Proprietà ActiveConnection (ADOX)](./activeconnection-property-adox.md)