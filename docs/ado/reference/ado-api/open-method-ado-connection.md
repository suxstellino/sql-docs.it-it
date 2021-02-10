---
description: Metodo Open (Connection - ADO)
title: Metodo Open (connessione ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::raw_Open
- Connection15::Open
- _Connection::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 663defab-5545-4973-9036-24d5882c9737
author: rothja
ms.author: jroth
ms.openlocfilehash: ada69c67cbd6d76973391ba1076129e5ee6baa24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041431"
---
# <a name="open-method-ado-connection"></a>Metodo Open (Connection - ADO)
Apre una connessione a un'origine dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
connection.Open ConnectionString, UserID, Password, Options  
```  
  
#### <a name="parameters"></a>Parametri  
 *ConnectionString*  
 facoltativo. Valore **stringa** che contiene le informazioni di connessione. Per informazioni dettagliate sulle impostazioni valide, vedere la proprietà [ConnectionString](./connectionstring-property-ado.md) .  
  
 *UserID*  
 facoltativo. Valore **stringa** che contiene un nome utente da utilizzare per stabilire la connessione.  
  
 *Password*  
 facoltativo. Valore **stringa** che contiene una password da utilizzare per stabilire la connessione.  
  
 *Opzioni*  
 facoltativo. Valore [ConnectOptionEnum](./connectoptionenum.md) che determina se il metodo deve restituire dopo (in modo sincrono) o prima (in modo asincrono) che la connessione viene stabilita.  
  
## <a name="remarks"></a>Commenti  
 L'utilizzo del metodo **Open** su un oggetto [Connection](./connection-object-ado.md) stabilisce la connessione fisica a un'origine dati. Una volta completato correttamente questo metodo, la connessione è attiva ed è possibile eseguire comandi su di essa ed elaborare i risultati.  
  
 Usare l'argomento facoltativo *ConnectionString* per specificare una stringa di connessione contenente una serie di istruzioni *argument* *= value* separate da punti e virgola o una risorsa di file o directory identificata con un URL. La proprietà **ConnectionString** eredita automaticamente il valore usato per l'argomento *ConnectionString* . Pertanto, è possibile impostare la proprietà **ConnectionString** dell'oggetto **Connection** prima di aprirla oppure utilizzare l'argomento *ConnectionString* per impostare o eseguire l'override dei parametri di connessione correnti durante la chiamata al metodo **Open** .  
  
 Se si passano informazioni sull'utente e sulla password sia nell'argomento *ConnectionString* che negli argomenti facoltativi *userid* e *password* , gli argomenti *userid* e *password* eseguiranno l'override dei valori specificati in *ConnectionString*.  
  
 Una volta terminate le operazioni su una **connessione** aperta, utilizzare il metodo [Close](./close-method-ado.md) per liberare tutte le risorse di sistema associate. La chiusura di un oggetto non comporta la rimozione dalla memoria; è possibile modificare le impostazioni delle proprietà e usare il metodo **Open** per aprirlo di nuovo in un secondo momento. Per eliminare completamente un oggetto dalla memoria, impostare la variabile oggetto su *Nothing*.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Quando viene utilizzato su un oggetto **connessione** lato client, il metodo **Open** non stabilisce effettivamente una connessione al server fino a quando non viene aperto un [Recordset](./recordset-object-ado.md) nell'oggetto **Connection** .  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Open e Close (VB)](./open-and-close-methods-example-vb.md)   
 [Esempio di metodi Open e Close (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Esempio di metodi Open e Close (VC + +)](./open-and-close-methods-example-vc.md)   
 [Metodo Open (record ADO)](./open-method-ado-record.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Metodo Open (flusso ADO)](./open-method-ado-stream.md)   
 [Metodo OpenSchema](./openschema-method.md)