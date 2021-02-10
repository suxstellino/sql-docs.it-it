---
description: Provider Microsoft OLE DB Remoting (provider di servizi ADO)
title: Provider Microsoft OLE DB Remoting (provider di servizi ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB remoting provider [ADO]
- providers [ADO], OLE DB remoting provider
- remoting provider [ADO]
ms.assetid: a4360ed4-b70f-4734-9041-4025d033346b
author: rothja
ms.author: jroth
ms.openlocfilehash: c73122e5fe1b06fb44ad69219145972ef4009202
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029245"
---
# <a name="microsoft-ole-db-remoting-provider-overview"></a>Panoramica del provider Microsoft OLE DB Remoting
Il provider Microsoft OLE DB Remoting consente a un utente locale in un computer client di richiamare i provider di dati in un computer remoto. Specificare i parametri del provider di dati per il computer remoto come se si trattasse di un utente locale nel computer remoto. Specificare quindi i parametri utilizzati dal provider remoto per accedere al computer remoto. È quindi possibile accedere al computer remoto come se si trattasse di un utente locale.

> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a  [WCF Data Services](/dotnet/framework/wcf/).

## <a name="provider-keyword"></a>Parola chiave provider
 Per richiamare il provider di OLE DB Remoting, specificare la parola chiave e il valore seguenti nella stringa di connessione. Prendere nota dello spazio vuoto nel nome del provider.

```vb
"Provider=MS Remote"
```

## <a name="additional-keywords"></a>Parole chiave aggiuntive
 Quando viene richiamato questo provider di servizi, sono rilevanti le seguenti parole chiave aggiuntive.

|Parola chiave|Descrizione|
|-------------|-----------------|
|**Origine dati**|Specifica il nome dell'origine dati remota. Viene passato al provider di servizi remoti di OLE DB per l'elaborazione.<br /><br /> Questa parola chiave equivale a [RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Proprietà [Connect](../../reference/rds-api/connect-property-rds.md) dell'oggetto DataControl.|

## <a name="dynamic-properties"></a>Proprietà dinamiche
 Quando il provider di servizi viene richiamato, le seguenti proprietà dinamiche vengono aggiunte alla raccolta [Properties](../../reference/ado-api/properties-collection-ado.md) dell'oggetto [Connection](../../reference/ado-api/connection-object-ado.md).

|Nome proprietà dinamica|Descrizione|
|---------------------------|-----------------|
|**DFMode**|Indica la modalità DataFactory. Stringa che specifica la versione desiderata dell'oggetto [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) nel server. Impostare questa proprietà prima di aprire una connessione per richiedere una versione specifica della data **Factory**. Se la versione richiesta non è disponibile, verrà effettuato un tentativo di usare la versione precedente. Se non è presente alcuna versione precedente, si verificherà un errore. Se **DFMode** è minore della versione disponibile, si verificherà un errore. Questa proprietà è di sola lettura dopo che è stata effettuata una connessione.<br /><br /> Può essere uno dei valori stringa validi seguenti:<br /><br /> -"25"-versione 2,5 (impostazione predefinita)<br />-"21"-versione 2,1<br />-"20"-versione 2,0<br />-"15"-versione 1,5|
|**Proprietà del comando**|Indica i valori che verranno aggiunti alla stringa di proprietà del comando (set di righe) inviate al server dal provider MS remoto. Il valore predefinito per questa stringa è vt_empty.|
|**DFMode corrente**|Indica il numero di versione effettivo della **DataFactory** nel server. Controllare questa proprietà per verificare se la versione richiesta nella proprietà **DFMode** è stata rispettata.<br /><br /> Può essere uno dei valori long integer validi seguenti:<br /><br /> -25-versione 2,5 (impostazione predefinita)<br />-21-versione 2,1<br />-20-versione 2,0<br />-15-versione 1,5<br /><br /> L'aggiunta di "DFMode = 20;" alla stringa di connessione quando si usa il provider **MSRemote** può migliorare le prestazioni del server durante l'aggiornamento dei dati. Con questa impostazione, l'oggetto **RDSServer. DataFactory** sul server usa una modalità con utilizzo intensivo di risorse minore. Tuttavia, le funzionalità seguenti non sono disponibili in questa configurazione:<br /><br /> -Uso di query con parametri.<br />-Recupero delle informazioni sul parametro o sulla colonna prima della chiamata al metodo **Execute** .<br />-Impostazione degli **aggiornamenti Transact** su **true**.<br />-Recupero dello stato delle righe.<br />-Chiamata del metodo **Resync** .<br />-Aggiornamento (in modo esplicito o automatico) tramite la proprietà **Aggiorna risincronizzazione** .<br />-Impostazione delle proprietà del **comando** o del **Recordset** .<br />-Uso di **adCmdTableDirect**.|
|**Gestore**|Indica il nome di un programma di personalizzazione lato server (o gestore) che estende la funzionalità di [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)ed eventuali parametri usati dal gestore, tutti separati da virgole (","). Valore **String**.|
|**Timeout Internet**|Indica il numero massimo di millisecondi di attesa per una richiesta di trasferimento da e verso il server. Il valore predefinito è 5 minuti.|
|**Provider remoto**|Indica il nome del provider di dati da utilizzare nel server remoto.|
|**Server remoto**|Indica il nome del server e il protocollo di comunicazione che devono essere utilizzati da questa connessione. Questa proprietà è equivalente a [RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Proprietà del [Server](../../reference/rds-api/server-property-rds.md) oggetti datacontro.|
|**Aggiornamenti Transact**|Se impostato su **true**, questo valore indica che quando [UpdateBatch](../../reference/ado-api/updatebatch-method.md) viene eseguito sul server, verrà eseguito all'interno di una transazione. Il valore predefinito per questa proprietà dinamica booleana è **false**.|

 È inoltre possibile impostare le proprietà dinamiche scrivibili specificandone i nomi come parole chiave nella stringa di connessione. Ad esempio, impostare la proprietà dinamica **timeout Internet** su cinque secondi specificando:

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MS Remote;Internet Timeout=5000"
```

 È anche possibile impostare o recuperare una proprietà dinamica specificandone il nome come indice della proprietà **Properties** . Nell'esempio seguente viene illustrato come ottenere e stampare il valore corrente della proprietà dinamica **timeout Internet** , quindi impostare un nuovo valore:

```vb
Debug.Print cn.Properties("Internet Timeout")
cn.Properties("Internet Timeout") = 5000
```

## <a name="remarks"></a>Commenti
 In ADO 2,0, il provider di OLE DB Remoting può essere specificato solo nel parametro *ActiveConnection* del metodo **Open** dell'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) . A partire da ADO 2,1, il provider può anche essere specificato nel parametro *ConnectionString* del metodo **Open** dell'oggetto [Connection](../../reference/ado-api/connection-object-ado.md) .

 Equivalente di **RDS.** La proprietà [SQL](../../reference/rds-api/sql-property.md) dell'oggetto datacontrollo non è disponibile. Viene invece utilizzato l'argomento di *origine* del metodo **Open** dell'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) .

 **Nota** Specifica di "...; Provider remoto = MS Remote;... " creerebbe uno scenario a quattro livelli. Gli scenari oltre i tre livelli non sono stati testati e non dovrebbero essere necessari.

## <a name="example"></a>Esempio
 In questo esempio viene eseguita una query sulla tabella **authors** del database **pubs** in un server denominato *yourserver*. I nomi dell'origine dati remota e del server remoto vengono forniti nel metodo [Open](../../reference/ado-api/open-method-ado-connection.md) dell'oggetto[Connection](../../reference/ado-api/connection-object-ado.md) e la query SQL viene specificata nel metodo[Open](../../reference/ado-api/open-method-ado-recordset.md) dell'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) . Un oggetto **Recordset** viene restituito, modificato e utilizzato per aggiornare l'origine dati.

```vb
Dim rs as New ADODB.Recordset
Dim cn as New ADODB.Connection
cn.Open  "Provider=MS Remote;Data Source=pubs;" & _
         "Remote Server=https://YourServer"
rs.Open "SELECT * FROM authors", cn
...                'Edit the recordset
rs.UpdateBatch     'Equivalent of RDS SubmitChanges
...
```

## <a name="see-also"></a>Vedere anche
 [Panoramica del provider di OLE DB Remoting](/previous-versions/windows/desktop/ms713673(v=vs.85))