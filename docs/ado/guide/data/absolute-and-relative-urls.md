---
description: URL relativi e assoluti
title: URL assoluti e relativi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- relative URLs [ADO]
- absolute URLs [ADO]
- URLs [ADO]
ms.assetid: 6a34a7ef-50cc-4c3d-82f7-106b9a8f3caf
author: rothja
ms.author: jroth
ms.openlocfilehash: 50535a71c1062bc04aa009bea1aab08d3cf1faf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028696"
---
# <a name="absolute-and-relative-urls"></a>URL relativi e assoluti
Un URL specifica il percorso di una destinazione archiviata in un computer locale o in rete. La destinazione può essere un file, una directory, una pagina HTML, un'immagine, un programma e così via.  
  
 Un *URL assoluto* contiene tutte le informazioni necessarie per individuare una risorsa.  
  
 Un *URL relativo* individua una risorsa usando un URL assoluto come punto di partenza. In effetti, il "URL completo" della destinazione viene specificato concatenando gli URL assoluti e relativi.  
  
 Un *URL assoluto* usa il formato seguente: *Scheme://server/path/Resource*  
  
 Un URL relativo è in genere costituito solo dal *percorso* e, facoltativamente, dalla *risorsa*, ma non da uno *schema* o un *Server*. Nelle tabelle seguenti vengono definite le singole parti del formato completo dell'URL.  
  
 *scheme*  
 Specifica la modalità di accesso alla *risorsa* .  
  
 *server*  
 Specifica il nome del computer in cui si trova la *risorsa* .  
  
 *path*  
 Specifica la sequenza di directory che conduce alla destinazione. Se la *risorsa* viene omessa, la destinazione è l'ultima directory nel *percorso*.  
  
 *risorse*  
 Se incluso, *Resource* è la destinazione ed è in genere il nome di un file. Potrebbe trattarsi di un *file semplice,* contenente un singolo flusso binario di byte, o un *documento strutturato,* contenente uno o più archivi e flussi binari di byte.  
  
## <a name="url-scheme-registration"></a>Registrazione dello schema URL  
 Se un provider supporta gli URL, il provider registrerà uno o più schemi URL. La registrazione indica che tutti gli URL che usano lo schema richiameranno automaticamente il provider registrato. Ad esempio, lo schema *http* viene registrato nel [provider Microsoft OLE DB per la pubblicazione Internet](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md). ADO presuppone che tutti gli URL con prefisso "http" rappresentino cartelle o file Web da utilizzare con il provider di pubblicazione Internet. Per informazioni sugli schemi registrati dal provider, vedere la documentazione del provider.  
  
## <a name="defining-context-with-a-url"></a>Definizione del contesto con un URL  
 Una funzione di una connessione aperta, rappresentata da un oggetto [connessione](../../reference/ado-api/connection-object-ado.md) , consiste nel limitare le operazioni successive all'origine dati rappresentata da tale connessione. Ovvero la connessione definisce il contesto per le operazioni successive.  
  
 Con ADO 2,7 o versioni successive, un URL assoluto può anche definire un contesto. Ad esempio, quando un oggetto [record](../../reference/ado-api/record-object-ado.md) viene aperto con un URL assoluto, viene creato in modo implicito un oggetto **connessione** per rappresentare la risorsa specificata dall'URL.  
  
 Un URL assoluto che definisce un contesto può essere specificato nel parametro *ActiveConnection* del metodo [Open](../../reference/ado-api/open-method-ado-record.md) dell'oggetto **record** . Un URL assoluto può essere specificato anche come valore della parola chiave "URL =" nel parametro di [apertura](../../reference/ado-api/open-method-ado-connection.md) del metodo *ConnectionString* del metodo di **connessione** e il parametro *ActiveConnection* del metodo [Open](../../reference/ado-api/open-method-ado-recordset.md) dell'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) .  
  
 Il contesto può essere definito anche aprendo un oggetto **record** o **Recordset** che rappresenta una directory, perché questi oggetti dispongono già di un oggetto **connessione** dichiarato in modo implicito o esplicito che specifica il contesto.  
  
## <a name="scoped-operations"></a>Operazioni con ambito  
 Il contesto definisce anche l'ambito, ovvero la directory e le relative sottodirectory che possono partecipare alle operazioni successive. L'oggetto **record** dispone di diversi metodi con ambito che operano su una directory e tutte le relative sottodirectory. Questi metodi includono [CopyRecord](../../reference/ado-api/copyrecord-method-ado.md), [MoveRecord](../../reference/ado-api/moverecord-method-ado.md)e [DeleteRecord](../../reference/ado-api/deleterecord-method-ado.md).  
  
## <a name="relative-urls-as-command-text"></a>URL relativi come testo del comando  
 È possibile specificare un comando da eseguire sull'origine dati digitando una stringa nel parametro *CommandText* del metodo [Execute](../../reference/ado-api/execute-method-ado-connection.md) dell'oggetto **Connection** e nel parametro *source* del metodo [Open](../../reference/ado-api/open-method-ado-recordset.md) dell'oggetto **Recordset** .  
  
 È possibile specificare un URL relativo nel parametro *CommandText* o *source* . L'URL relativo non rappresenta effettivamente un comando, ad esempio un comando SQL. specifica semplicemente i parametri. Il contesto della connessione attiva deve essere un URL assoluto e il parametro *Option* deve essere impostato su **adCmdTableDirect**.  
  
 Nell'esempio di codice seguente viene illustrato come aprire un **Recordset** sul file Readme25.txt della directory Winnt/system32:  
  
```  
recordset.Open "system32/Readme25.txt", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
 L'URL assoluto nella stringa di connessione specifica il server ( `YourServer` ) e il percorso ( `Winnt` ). Questo URL definisce anche il contesto.  
  
 L'URL relativo nel testo del comando usa l'URL assoluto come punto di partenza e specifica il resto del percorso ( `system32` ) e il file da aprire ( `Readme25.txt` ).  
  
 Il campo Options ( `adCmdTableDirect` ) indica che il tipo di comando è un URL relativo.  
  
 Come altro esempio, il codice seguente apre un **Recordset** sul contenuto della `Winnt` Directory:  
  
```  
recordset.Open "", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
## <a name="ole-db-provider-supplied-url-schemes"></a>Schemi URL Provider-Supplied OLE DB  
 La parte principale di un URL completo è lo *schema* usato per accedere alla risorsa identificata dal resto dell'URL. Esempi sono HTTP (Hypertext Transfer Protocol) e FTP (File Transfer Protocol).  
  
 ADO supporta provider di OLE DB che riconoscono i propri schemi URL. Ad esempio, il [provider Microsoft OLE DB per Internet Publishing](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md)*,* che accede ai file di Windows 2000 "pubblicati", riconosce lo schema HTTP esistente.  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Connection (ADO)](../../reference/ado-api/connection-object-ado.md)   
 [Oggetto record (ADO)](../../reference/ado-api/record-object-ado.md)   
 [Oggetto Recordset (ADO)](../../reference/ado-api/recordset-object-ado.md)