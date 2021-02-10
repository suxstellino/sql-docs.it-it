---
description: Metodo Execute (Servizi Desktop remoto)
title: Metodo Execute (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 2d9c30e9-ab5b-4920-91b8-48454c2fb5d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 862ded514855d6ac0cd3c18bcb014463eecf1e76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049481"
---
# <a name="execute-method-rds"></a>Metodo Execute (Servizi Desktop remoto)
Esegue la richiesta e crea un recordset ADO da usare in ADO 2,5 e versioni successive.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.Execute(ConnectionString As String, HandlerString As String, QueryString As String, lFetchOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters, [lcid As Long], [pInformation])  
```  
  
#### <a name="parameters"></a>Parametri  
 *ConnectionString*  
 Stringa utilizzata per la connessione al provider OLE DB in cui verrà inviata la richiesta per l'esecuzione. Se viene specificato un gestore usando *HandlerString* , è possibile modificare o sostituire la stringa di connessione.  
  
 *HandlerString*  
 Stringa in due parti che identifica il gestore da utilizzare con questa esecuzione. La stringa contiene due parti. La prima parte contiene il nome (ProgID) del gestore da usare. La seconda parte contiene gli argomenti da passare al gestore. I dettagli relativi alla modalità di interpretazione della stringa degli argomenti sono specifici per ogni gestore. Le due parti sono separate dalla prima istanza di una virgola nella stringa. La stringa arguments può contenere altre virgole. Gli argomenti sono facoltativi.  
  
 *QueryString*  
 Comando nel linguaggio di comando supportato dal provider OLE DB identificato nella stringa di connessione. Per i provider basati su SQL, *QueryString* potrebbe contenere un'istruzione del comando Transact-SQL, ma per i provider non SQL (ad esempio, MSDataShape) potrebbe non essere un' [!INCLUDE[tsql](../../../includes/tsql-md.md)] istruzione di query.  
  
 Se viene utilizzato un gestore, il gestore può modificare o sostituire il valore specificato qui. Ad esempio, il gestore in genere sostituisce *QueryString* con una stringa di query dal relativo file ini. Per impostazione predefinita, viene usato il file di Msdfmap.ini.  
  
 *lFetchOptions*  
 Indica il tipo di recupero asincrono.  
  
 Per ulteriori informazioni, vedere [Proprietà FetchOptions (RDS)](./fetchoptions-property-rds.md).  
  
 *TableID*  
 **Variant** di tipo VT_EMPTY o VT_BSTR. Se questo valore è di tipo VT_EMPTY, viene ignorato. Se è di tipo VT_BSTR, il recordset viene creato usando **adCmdTableDirect** e il valore specificato qui e il parametro *QueryString* viene ignorato.  
  
 *lExecuteOptions*  
 Maschera di bit delle opzioni di esecuzione:  
  
 1 =*ReadOnly* il recordset verrà aperto utilizzando **adLockReadOnly**.  
  
 2 =*nobatch* il recordset verrà aperto utilizzando **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* il chiamante garantisce che le informazioni sui parametri per tutti i parametri vengano fornite in *pParameters*.  
  
 8 = le informazioni sul parametro *GetInfo* per la query verranno ottenute dal provider OLE DB e restituite nel parametro *pParameters* . La query non viene eseguita e non viene restituito alcun recordset.  
  
 16 =*GetHiddenColumns* il recordset verrà aperto utilizzando **adLockBatchOptimistic** e tutte le colonne nascoste verranno incluse nel recordset.  
  
 *ReadOnly*, *nobatch* e *GetHiddenColumns* sono opzioni che si escludono a vicenda. Tuttavia, non genera un errore per impostare più di uno di essi. Se sono impostate più opzioni, *GetHiddenColumns* avrà la precedenza su tutte le altre, seguite da *ReadOnly*. Se non viene specificata alcuna opzione, per impostazione predefinita il recordset viene aperto utilizzando **adLockBatchOptimistic** e le colonne nascoste non sono incluse nel recordset.  
  
 *pParameters*  
 **Variant** che contiene una matrice sicura di definizioni di parametri. Se l'opzione *GetInfo* è stata specificata in *lExecuteOptions*, questo parametro viene utilizzato per restituire le definizioni dei parametri ottenute dal provider OLE DB. In caso contrario, questo parametro può essere vuoto.  
  
 *lcid*  
 Identificatore LCID utilizzato per compilare eventuali errori restituiti in *pInformation*.  
  
 *pInformation*  
 Puntatore a un errore di informazioni restituito da Execute. Se è NULL, non vengono restituite informazioni sull'errore.  
  
## <a name="remarks"></a>Commenti  
 Il parametro *HandlerString* può essere null. Ciò che accade in questo caso dipende dalla modalità di configurazione del server RDS. Una stringa del gestore "MSDFMAP. handler" indica che deve essere utilizzato il gestore fornito da Microsoft (Msdfmap.dll). Una stringa del gestore "MASDFMAP. Handler, sample.ini" indica che è necessario utilizzare il gestore di Msdfmap.dll e che l'argomento "sample.ini" deve essere passato al gestore. MSDFMAP.dll interpreterà l'argomento come direzione per utilizzare l'sample.ini per verificare le stringhe di connessione e di query.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)