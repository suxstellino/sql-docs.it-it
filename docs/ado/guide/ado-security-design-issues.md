---
description: Funzionalità di progettazione della sicurezza ADO
title: Problemi di progettazione della sicurezza ADO | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, security
ms.assetid: 86b83a38-efdf-4831-a6d5-7e470d517d1c
author: rothja
ms.author: jroth
ms.openlocfilehash: c992b58b6b84f79efa62c63353e3b8f1efcc333d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030499"
---
# <a name="ado-security-design-features"></a>Funzionalità di progettazione della sicurezza ADO
Le sezioni seguenti descrivono le funzionalità di progettazione della sicurezza in ActiveX Data Objects (ADO) 2,8 e versioni successive. Queste modifiche sono state apportate in ADO 2,8 per migliorare la sicurezza. ADO 6,0, incluso in Windows DAC 6,0 in Windows Vista, è funzionalmente equivalente a ADO 2,8, incluso in MDAC 2,8 in Windows XP e Windows Server 2003. In questo argomento vengono fornite informazioni su come proteggere meglio le applicazioni in ADO 2,8 o versioni successive.

> [!IMPORTANT]
>  Se si sta aggiornando l'applicazione da una versione precedente di ADO, è consigliabile testare l'applicazione aggiornata in un computer non di produzione prima di distribuirla ai clienti. In questo modo, è possibile assicurarsi di essere a conoscenza di eventuali problemi di compatibilità prima di distribuire l'applicazione aggiornata.

## <a name="internet-explorer-file-access-scenarios"></a>Scenari di accesso ai file di Internet Explorer
 Le funzionalità seguenti influiscono sul funzionamento di ADO 2,8 e versioni successive quando viene utilizzato nelle pagine Web con script in Internet Explorer.

### <a name="revised-and-improved-security-warning-message-box-now-used-to-alert-users"></a>Finestra di messaggio di avviso di sicurezza modificata e migliorata ora utilizzata per avvisare gli utenti
 Per ADO 2,7 e versioni precedenti, viene visualizzato il messaggio di avviso seguente quando una pagina Web con script tenta di eseguire codice ADO da un provider non attendibile:

```console
This page accesses data on another domain. Do you want to allow this? To
avoid this message in Internet Explorer, you can add a secure Web site to
your Trusted Sites zone on the Security tab of the Internet Options dialog
box.
```

 Per ADO 2,8 e versioni successive, il messaggio precedente non viene più visualizzato. In questo contesto viene invece visualizzato il messaggio seguente:

```console
This Website uses a data provider that may be unsafe. If you trust the
Website, click OK, otherwise click Cancel.
```

 Il messaggio precedente consente all'utente di prendere decisioni informate, pur conoscendo le conseguenze per entrambe le scelte:

-   Se l'utente considera attendibile il sito, facendo clic su OK si consentirà a tutto il codice indipendente dal disco (tutti i metodi e le proprietà ADO con le eccezioni delle API accessibili al disco descritte più avanti in questo argomento) per l'esecuzione e l'esecuzione nella finestra del browser.

-   Se l'utente non considera attendibile il sito, facendo clic su Annulla, il codice ADO per l'accesso ai dati non viene più eseguito ed eseguito interamente.

### <a name="disk-accessible-code-limited-now-to-trusted-sites"></a>Codice accessibile da disco limitato ora a siti attendibili
 In ADO 2,8 sono state apportate ulteriori modifiche di progettazione che limitano in modo specifico la capacità di un set limitato di API, che potrebbero esporre la possibilità di leggere o scrivere su file nel computer locale. Di seguito sono riportati i metodi API che sono stati ulteriormente limitati per garantire la sicurezza durante l'esecuzione di Internet Explorer:

-   Per l'oggetto **flusso** ADO, se vengono utilizzati i metodi [LoadFromFile](../reference/ado-api/loadfromfile-method-ado.md) o [SaveToFile](../reference/ado-api/savetofile-method.md) .

-   Per l'oggetto **Recordset** ADO, se il metodo [Save](../reference/ado-api/save-method.md) o il metodo [Open](../reference/ado-api/open-method-ado-recordset.md) , ad esempio quando viene impostata l'opzione **AdCmdFile** o viene utilizzato il [provider di persistenza Microsoft OLE DB (MSPersist)](./appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) .

 Per questi set limitati di funzioni potenzialmente accessibili al disco, si verifica il comportamento seguente per ADO 2,8 e versioni successive, se il codice che usa questi metodi viene eseguito in Internet Explorer:

-   Se il sito che ha fornito il codice è stato aggiunto in precedenza all'elenco di zone dei siti attendibili, il codice viene eseguito nel browser e l'accesso ai file locali viene concesso.

-   Se il sito non viene visualizzato nell'elenco zona siti attendibili, il codice viene bloccato e l'accesso ai file locali viene negato.

    > [!NOTE]
    >  In ADO 2,8 e versioni successive, l'utente non viene avvisato o si consiglia di aggiungere i siti all'elenco di zone dei siti attendibili. Pertanto, la gestione dell'elenco dei siti attendibili è la responsabilità di coloro che distribuiscono o supportano applicazioni basate su siti Web che richiedono l'accesso al file system locale.

### <a name="access-blocked-to-the-activecommand-property-on-recordset-objects"></a>Accesso bloccato alla proprietà ActiveCommand sugli oggetti recordset
 Quando è in esecuzione in Internet Explorer, ADO 2,8 ora blocca l'accesso alla proprietà [ActiveCommand](../reference/ado-api/activecommand-property-ado.md) per un oggetto **Recordset** attivo e restituisce un errore. L'errore si verifica indipendentemente dal fatto che la pagina provenga da un sito Web registrato nell'elenco dei siti attendibili.

### <a name="changes-in-handling-for-ole-db-providers-and-integrated-security"></a>Modifiche alla gestione per provider di OLE DB e sicurezza integrata
 Durante la revisione di ADO 2,7 e versioni precedenti per individuare potenziali problemi di sicurezza e problemi, è stato individuato lo scenario seguente:

 In alcuni casi, OLE DB provider che supportano la proprietà Integrated Security [DBPROP_AUTH_INTEGRATED](/previous-versions/windows/desktop/ms712973(v=vs.85)) possono potenzialmente consentire a pagine Web con script di riutilizzare l'oggetto connessione ADO per connettersi involontariamente ad altri server utilizzando le credenziali di accesso correnti degli utenti. Per evitare questo problema, ADO 2,8 e versioni successive gestiscono OLE DB provider a seconda della modalità con cui hanno scelto di fornire, o non fornire, per la sicurezza integrata.

 Per le pagine Web caricate da siti elencati nell'elenco zona siti attendibili, nella tabella seguente viene fornita una descrizione dettagliata del modo in cui ADO 2,8 e versioni successive gestiscono le connessioni ADO in ogni caso.

|Impostazioni IE per l'autenticazione utente, accesso|Il provider supporta "Integrated Security" e vengono specificati UID e PWD (SQLOLEDB)|Il provider non supporta la sicurezza integrata (scossa, MSDASQL, MSPersist)|Il provider supporta la "sicurezza integrata" ed è impostato su SSPI (nessun UID/PWD specificato)|
|------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
|Accesso automatico con nome utente e password correnti|Consenti connessione|Consenti connessione|Consenti connessione|
|Richiedi nome utente e password|Consenti connessione|Connessione non riuscita|Connessione non riuscita|
|Accesso automatico solo nell'area Intranet|Consenti connessione|Richiedi all'utente un avviso di sicurezza|Richiedi all'utente un avviso di sicurezza|
|Accesso anonimo|Consenti connessione|Connessione non riuscita|Connessione non riuscita|

 Nel caso in cui venga visualizzato un avviso di sicurezza, la finestra di messaggio informa gli utenti:

```console
This Website is using your identity to access a data source. If you trust this Website, click OK, otherwise click Cancel.
```

 Il messaggio precedente consente all'utente di prendere decisioni più informate e di procedere di conseguenza.

> [!NOTE]
>  Per i siti non attendibili, ovvero i siti non elencati nell'elenco di zone dei siti attendibili, se anche il provider non è attendibile, come illustrato in precedenza in questa sezione, l'utente potrebbe visualizzare due avvisi di sicurezza in una riga, un avviso sul provider unsafe e un secondo avviso sul tentativo di usare la propria identità. Se l'utente fa clic su OK per il primo avviso, verranno eseguite le impostazioni di Internet Explorer e il codice di comportamento della risposta descritti nella tabella precedente.

## <a name="controlling-whether-password-text-is-returned-in-ado-connection-strings"></a>Controllo dell'eventuale restituzione del testo della password nelle stringhe di connessione ADO
 Quando si tenta di ottenere il valore della proprietà [ConnectionString](../reference/ado-api/connectionstring-property-ado.md) su un oggetto **connessione** ADO, si verificano gli eventi seguenti:

1.  Se la connessione è aperta, viene eseguita una chiamata di inizializzazione al provider OLE DB sottostante per ottenere la stringa di connessione.

2.  A seconda dell'impostazione del provider OLE DB della proprietà [DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO](/previous-versions/windows/desktop/ms714905(v=vs.85)) , le password vengono incluse insieme ad altre informazioni sulla stringa di connessione restituite.

 Se, ad esempio, la proprietà dinamica della connessione ADO rende permanente le informazioni di **sicurezza** è impostata su **true**, le informazioni sulla password vengono incluse nella stringa di connessione restituita. In caso contrario, se il provider sottostante ha impostato la proprietà su **false** (ad esempio con il provider SQLOLEDB), le informazioni sulla password vengono omesse nella stringa di connessione restituita.

 Se si utilizzano provider di OLE DB di terze parti (ovvero non Microsoft) con il codice dell'applicazione ADO, è possibile controllare il modo in cui viene implementata la proprietà **DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO** per determinare se è consentita l'inclusione di informazioni sulla password con stringhe di connessione ADO.

## <a name="checking-for-non-file-devices-when-loading-and-saving-recordsets-or-streams"></a>Verifica della presenza di dispositivi non file durante il caricamento e il salvataggio di recordset o flussi
 Per ADO 2,7 e versioni precedenti, le operazioni di input/output dei file, ad esempio [Open](../reference/ado-api/open-method-ado-recordset.md) e [Save](../reference/ado-api/save-method.md) usate per leggere e scrivere dati basati su file, potrebbero in alcuni casi consentire l'uso di un URL o di un nome file che specifica un tipo di file non basato su disco. Ad esempio, è possibile usare LPT1, COM2, PRN.TXT, AUX come alias per l'input/output tra le stampanti e i dispositivi ausiliari del sistema con determinate

 Per ADO 2,8 e versioni successive, questa funzionalità è stata aggiornata. Per l'apertura e il salvataggio di oggetti **Recordset** e di **flusso** , ADO ora esegue un controllo del tipo di file per assicurarsi che il dispositivo di input o di output specificato in un URL o un nome di file sia un file effettivo.

> [!NOTE]
>  Il controllo dei tipi di file come descritto in questa sezione si applica solo a Windows 2000 e versioni successive. Non si applica alle situazioni in cui ADO 2,8 o versione successiva viene eseguito in versioni precedenti di Windows, ad esempio Windows 98.