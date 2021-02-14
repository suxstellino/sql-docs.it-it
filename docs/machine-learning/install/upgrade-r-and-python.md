---
title: Aggiornare i runtime di Python e R (binding)
description: Aggiornare i runtime di Python e R in Machine Learning Services per SQL Server o R Services per SQL Server usando sqlbindr.exe per il binding a Machine Learning Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 7cdf5b2d0c223130245604f5b27674a714ab4d8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082386"
---
# <a name="upgrade-python-and-r-runtime-with-binding-in-sql-server-machine-learning-services"></a>Aggiornare il runtime di Python e R con binding in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and 2017](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Questo articolo descrive come usare il processo di installazione denominato **binding** per aggiornare i runtime di R o Python in [SQL Server 2016 R Services](../r/sql-server-r-services.md) o [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md). È possibile ottenere [versioni più recenti di Python e R](#version-map) con il *binding* a [Microsoft Machine Learning Server](/machine-learning-server).

> [!IMPORTANT]
> Questo articolo descrive un vecchio metodo di aggiornamento dei runtime di R e Python, denominato *binding*. Se è stato installato l'**aggiornamento cumulativo (CU) 14 o versione successiva per SQL Server 2016 Services Pack (SP) 2** o l'**aggiornamento cumulativo (CU) 22 o versione successiva per SQL Server 2017**, vedere come [modificare il runtime predefinito del linguaggio R o Python impostandolo su una versione successiva](change-default-language-runtime-version.md).

## <a name="what-is-binding"></a>Che cosa si intende per binding?

Il binding è un processo di installazione che sostituisce il contenuto delle cartelle **R_SERVICES** e **PYTHON_SERVICES** con i file eseguibili, le librerie e gli strumenti più recenti di [Microsoft Machine Learning Server](/machine-learning-server).

I componenti caricati inclusi nel modello di manutenzione sono stati modificati. Gli aggiornamenti del servizio corrispondono alla [sequenza temporale del supporto per Microsoft R Server e Machine Learning Server](/machine-learning-server/resources-servicing-support) nel [ciclo di vita moderno](https://support.microsoft.com/help/30881/modern-lifecycle-policy).

Fatta eccezione per le versioni dei componenti e degli aggiornamenti di manutenzione, il binding non modifica le caratteristiche di base dell'installazione:

- L'integrazione di Python e R fa ancora parte di un'istanza del motore di database.
- Le licenze sono invariate (nessun costo aggiuntivo associato al binding).
- I criteri di supporto di SQL Server vengono mantenuti per il motore di database.

La parte restante di questo articolo illustra il meccanismo di binding e ne spiega il funzionamento per ogni versione di SQL Server.

> [!NOTE]
> Questo meccanismo si applica solo alle istanze nel database associate a istanze di SQL Server. In questo caso il binding non è necessario per un'installazione autonoma.

::: moniker range="=sql-server-2016"
**Considerazioni sul binding per SQL Server 2016**

Per i clienti di R Services per SQL Server 2016 il binding offre quanto segue:

- Pacchetti R aggiornati.
- Nuovi pacchetti che non fanno parte dell'installazione originale ([MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package))
- [Modelli](/machine-learning-server/install/microsoftml-install-pretrained-models) di Machine Learning con training preliminare per l'analisi del sentiment e il rilevamento delle immagini.

Tutti i binding possono essere aggiornati ulteriormente a ogni nuova versione principale e secondaria di [Microsoft Machine Learning Server](/machine-learning-server/index).
::: moniker-end

## <a name="version-map"></a>Mappa delle versioni

Le tabelle seguenti sono mappe delle versioni. Ogni mappa illustra le versioni del pacchetto nelle varie release. È possibile esaminare i percorsi di aggiornamento quando si esegue il binding a Microsoft Machine Learning Server (noto in precedenza come R Server, prima dell'aggiunta del supporto Python a partire da Machine Learning Server 9.2.1).

Il binding non garantisce che sia disponibile la versione più recente di R o Anaconda. Quando si esegue il binding a Microsoft Machine Learning Server si ottiene la versione di R o Python installata con il programma di installazione, che potrebbe non essere quella più recente disponibile sul Web.

::: moniker range="=sql-server-2016"
[**R Services per SQL Server 2016**](../install/sql-r-services-windows-install.md)

Componente |Versione iniziale | [R Server 9.0.1](/machine-learning-server/install/r-server-install-windows) | [R Server 9.1](/machine-learning-server/install/r-server-install-windows) | [Machine Learning Server 9.2.1](/machine-learning-server/install/machine-learning-server-windows-install) | [Machine Learning Server 9.3](/machine-learning-server/install/machine-learning-server-windows-install) |  [Machine Learning Server 9.4.7](/machine-learning-server/install/machine-learning-server-windows-install)
----------|----------------|----------------|--------------|---------|-------|-------|
Microsoft R Open (MRO) su R | R 3.2.2     | R 3.3.2   |R 3.3.3   | R 3.4.1  | R 3.4.3 | R 3.5.2
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) | 8.0.3  | 9.0.1 |  9.1 |  9.2.1 |  9.3 |  9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package)| n.d. | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[modelli con training preliminare](/machine-learning-server/install/microsoftml-install-pretrained-models)| n.d. | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| n.d. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | n.d. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
::: moniker-end

::: moniker range="=sql-server-2017"
[**Machine Learning Services per SQL Server 2017**](../install/sql-machine-learning-services-windows-install.md)

Componente |Versione iniziale | Machine Learning Server 9.3 | Machine Learning Server 9.4.7 |
----------|----------------|---------|---------|
Microsoft R Open (MRO) su R | R 3.3.3 | R 3.4.3 | R 3.5.2 |
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) |   9.2 |  9.3 | 9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| 1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0 |  1.0 | 1.0 |
Anaconda 4.2 su Python 3.5  | 4.2/3.5.2 | 4.2/3.5.2 |
[revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2  | 9.3| 9.4.7 |
[microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[modelli con training preliminare](/machine-learning-server/install/microsoftml-install-pretrained-models) | 9.2 | 9.3| 9.4.7 |
::: moniker-end

## <a name="how-component-upgrade-works"></a>Come funziona l'aggiornamento dei componenti

I file eseguibili, Python e le librerie R vengono aggiornati quando si esegue il binding di un'installazione esistente di Python e R a Machine Learning Server.

Il binding viene eseguito dal [programma di installazione di Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) quando si esegue il programma di installazione in un'istanza esistente del motore di database SQL Server con l'integrazione di Python o R. 

Il programma di installazione rileva le funzionalità esistenti e richiede di eseguire nuovamente il binding a Machine Learning Server.

Durante il binding il contenuto di `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES` e `\PYTHON_SERVICES` viene sovrascritto con i file eseguibili e le librerie più recenti di `C:\Program Files\Microsoft\ML Server\R_SERVER` e `\PYTHON_SERVER`.

Il binding si applica solo alle funzionalità Python e R. I pacchetti open source per Python e R sono costituiti da:

- Anaconda
- Microsoft R Open
- Pacchetti proprietari RevoScaleR
- Revoscalepy

Il binding non modifica il modello di supporto per l'istanza del motore di database o la versione di SQL Server.

Il binding è reversibile. È possibile tornare al modello di manutenzione di SQL Server [annullando il binding dell'istanza](#bkmk_Unbind) e ripristinando l'istanza del motore di database SQL Server.

<a name="bkmk_BindWizard"></a>

## <a name="bind-to-machine-learning-server-using-setup"></a>Eseguire il binding a Machine Learning Server usando il programma di installazione

Seguire questa procedura per eseguire il binding di SQL Server a Microsoft Machine Learning Server usando il programma di installazione. 

1. In SSMS eseguire `SELECT @@version` per verificare che il server soddisfi i requisiti minimi di build.

   Nel caso di R Services per SQL Server 2016, il requisito minimo è [Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276) e [CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1).

1. Controllare le versioni esistenti dei pacchetti R di base e RevoScaleR per verificare che siano minori di quelle con cui si prevede di sostituirle. 

    ```sql
    EXECUTE sp_execute_external_script
    @language=N'R'
    ,@script = N'str(OutputDataSet);
    packagematrix <- installed.packages();
    Name <- packagematrix[,1];
    Version <- packagematrix[,3];
    OutputDataSet <- data.frame(Name, Version);'
    , @input_data_1 = N''
    WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
    ```

1. Chiudere SSMS e tutti gli altri strumenti con una connessione aperta a SQL Server. Il binding sovrascrive i file di programma. Se SQL Server ha sessioni aperte, il binding avrà esito negativo con codice di errore 6.

1. Scaricare Microsoft Machine Learning Server nel computer in cui è presente l'istanza da aggiornare. È consigliabile scaricare la [versione più recente](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer).

1. Decomprimere la cartella e avviare ServerSetup.exe, che si trova in MLSWIN93.

1. In **Configure the installation** (Configurare l'installazione) verificare i componenti da aggiornare ed esaminare l'elenco delle istanze compatibili.

1. Nella pagina **License agreement** (Contratto di licenza) selezionare **I accept these terms** (Accetto le condizioni) per accettare le condizioni di licenza per Machine Learning Server. 

1. Nelle pagine successive fornire il proprio consenso alle condizioni di licenza aggiuntive per tutti i componenti open source selezionati, ad esempio Microsoft R Open o la distribuzione Anaconda di Python.

1. Nella pagina **Almost there** (Quasi fatto) prendere nota della cartella di installazione. La cartella predefinita è \Programmi\Microsoft\ML Server.

    Se si vuole modificare la cartella di installazione, selezionare **Advanced** (Avanzate) per tornare alla prima pagina della procedura guidata. In tal caso è tuttavia necessario ripetere tutte le selezioni precedenti.

Se l'aggiornamento non riesce, controllare i [codici di errore di SqlBindR](#sqlbindr-error-codes) per altre informazioni.

## <a name="offline-binding-no-internet-access"></a>Binding offline (senza accesso a Internet)

Per i sistemi senza connettività Internet, è possibile scaricare il programma di installazione e i file con estensione cab in un computer connesso a Internet e quindi trasferire i file nel server isolato.

Il programma di installazione (ServerSetup.exe) include i pacchetti Microsoft (RevoScaleR, MicrosoftML, olapR, sqlRUtils). I file con estensione cab includono altri componenti di base. Il file CAB "SRO", ad esempio, fornisce R Open, la distribuzione Microsoft di R open source.

Le istruzioni seguenti illustrano come posizionare i file per un'installazione offline.

1. Scaricare il programma di installazione di MLSWIN93. Il programma viene scaricato come singolo file compresso. È consigliabile usare la [versione più recente](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer), ma è possibile installare anche [versioni precedenti](/machine-learning-server/install/r-server-install-windows-offline#download-required-components).

1. Scaricare i file con estensione cab. Di seguito sono indicati i collegamenti per la versione 9.3. Se sono necessarie versioni precedenti, è possibile trovare altri collegamenti in [R Server 9.1](/machine-learning-server/install/r-server-install-windows-offline#download-required-components). Tenere presente che Python/Anaconda può essere aggiunto solo a un'istanza di Machine Learning Services per SQL Server. Sono disponibili modelli con training preliminare per entrambi i linguaggi Python e R. Il file CAB contiene modelli nei linguaggi usati.

    | Funzionalità | Download |
    |---------|----------|
    | R       | [SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186&clcid=1033) |
    | Python  | [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | 
    | Modelli con training preliminare | [MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) |

1. Trasferire i file con estensione zip e cab nel server di destinazione.

1. Nel server digitare `%temp%` nel comando Esegui per ottenere il percorso fisico della directory temp. Il percorso fisico varia a seconda del computer usato, ma è in genere `C:\Users\<your-user-name>\AppData\Local\Temp`.

1. Inserire i file con estensione cab nella cartella %temp%.

1. Decomprimere il programma di installazione.

1. Eseguire ServerSetup.exe e seguire le istruzioni visualizzate per completare l'installazione.

## <a name="command-line-operations"></a><a name="bkmk_BindCmd"></a>Operazioni dalla riga di comando


> [!TIP]
> Se non si riesce a trovare SqlBindR, è probabile che non sia stato eseguito il programma di installazione.
> SqlBindR è disponibile solo dopo l'esecuzione del programma di installazione di Machine Learning Server.

1. Aprire un prompt dei comandi come amministratore e passare alla cartella che contiene sqlbindr.exe. Il percorso predefinito è C:\Programmi\Microsoft\MLServer\Setup

2. Digitare il comando seguente per visualizzare un elenco delle istanze disponibili: `SqlBindR.exe /list`
  
   Annotare il nome completo dell'istanza indicato. Ad esempio, il nome dell'istanza potrebbe essere MSSQL14.MSSQLSERVER per un'istanza predefinita o comunque un nome nel formato NOMESERVER.ISTANZADENOMINATA.

3. Eseguire il comando **SqlBindR.exe** con l'argomento */bind*. Specificare il nome dell'istanza da aggiornare, usando il nome dell'istanza restituito nel passaggio precedente.

   Ad esempio, per aggiornare l'istanza predefinita, digitare: `SqlBindR.exe /bind MSSQL14.MSSQLSERVER`

4. Al termine dell'aggiornamento, riavviare il servizio Launchpad associato a qualsiasi istanza modificata.

## <a name="revert-or-unbind-an-instance"></a><a name="bkmk_Unbind"></a>Annullare il binding di un'istanza

È possibile annullare un'istanza con binding e ripristinare un'installazione iniziale dei componenti Python e R, come configurata dal programma di installazione di SQL Server. Il processo di ripristino del modello di manutenzione di SQL Server prevede tre passaggi.

+ [Passaggio 1: Annullare il binding da Microsoft Machine Learning Server](#step-1-unbind)
+ [Passaggio 2: Ripristinare lo stato originale dell'istanza](#step-2-restore)
+ [Passaggio 3: Reinstallare i pacchetti aggiunti all'installazione](#step-3-reinstall-packages)

<a name="step-1-unbind"></a> 

### <a name="step-1-unbind"></a>Passaggio 1: Annullare il binding

Per eseguire il rollback del binding sono disponibili due opzioni: eseguire nuovamente il programma di installazione o usare l'utilità della riga di comando SqlBindR.

#### <a name="unbind-using-setup"></a><a name="bkmk_wizunbind"></a> Annullare il binding con il programma di installazione

1. Individuare il programma di installazione per Machine Learning Server. Se il programma di installazione è stato rimosso, può essere necessario scaricarlo di nuovo o copiarlo da un altro computer.
2. Assicurarsi di eseguire il programma di installazione nel computer in cui è presente l'istanza di cui si vuole annullare il binding.
2. Il programma di installazione identifica le istanze locali candidate per l'annullamento del binding.
3. Deselezionare la casella di controllo accanto all'istanza di cui si vuole ripristinare la configurazione originale.
4. Accettare tutti i contratti di licenza.
5. Selezionare **Fine**. L'esecuzione del processo richiede alcuni minuti.

#### <a name="unbind-using-the-command-line"></a><a name="bkmk_cmdunbind"></a> Annullare il binding tramite la riga di comando

1. Aprire un prompt dei comandi e passare alla cartella contenente **sqlbindr.exe**, come descritto nella sezione precedente.

2. Eseguire il comando **SqlBindR.exe** con l'argomento */unbind* e specificare l'istanza.

   Il comando seguente, ad esempio, ripristina l'istanza predefinita:
   
    `SqlBindR.exe /unbind MSSQL14.MSSQLSERVER`

<a name="step-2-restore"></a> 

###  <a name="step-2-repair-the-sql-server-instance"></a>Passaggio 2: Ripristinare l'istanza di SQL Server

Eseguire il programma di installazione di SQL Server per ripristinare l'istanza del motore di database con le funzionalità Python e R. Gli aggiornamenti preesistenti vengono conservati. Il passaggio successivo si applica nel caso in cui non sia stato eseguito un aggiornamento per gli aggiornamenti di manutenzione per i pacchetti Python e R.

Soluzione alternativa: Disinstallare e reinstallare completamente l'istanza del motore di database e quindi applicare tutti gli aggiornamenti di manutenzione.

<a name="step-3-reinstall-packages"></a> 

### <a name="step-3-add-any-third-party-packages"></a>Passaggio 3: Aggiungere eventuali pacchetti di terze parti

È possibile che siano stati aggiunti altri pacchetti open source o di terze parti alla libreria di pacchetti. Poiché l'annullamento del binding cambia il percorso della libreria di pacchetti predefinita, è necessario reinstallare i pacchetti nella libreria effettivamente usata da Python e R. Per altre informazioni, vedere [Informazioni sui pacchetti R](../package-management/r-package-information.md) e le relative istruzioni di [installazione](../package-management/install-additional-r-packages-on-sql-server.md) e [Informazioni sui pacchetti Python](../package-management/python-package-information.md) e le relative istruzioni di [installazione](../package-management/install-additional-python-packages-on-sql-server.md).

## <a name="sqlbindrexe-command-syntax"></a>Sintassi del comando SqlBindR.exe

### <a name="usage"></a>Uso

`sqlbindr [/list] [/bind <SQL_instance_ID>] [/unbind <SQL_instance_ID>]`

### <a name="parameters"></a>Parametri

|Nome|Descrizione|
|------|------|
|*list*| Visualizza un elenco di tutti gli ID delle istanze di SQL Server nel computer corrente|
|*bind*| Aggiorna l'istanza di SQL Server specificata alla versione più recente di R Server e fa in modo che all'istanza vengano applicati automaticamente gli aggiornamenti futuri di R Server|
|*unbind*|Disinstalla la versione più recente di R Server dall'istanza di SQL Server specificata e impedisce l'applicazione degli aggiornamenti futuri di R Server all'istanza|

<a name="sqlbindr-error-codes"><a/>

## <a name="binding-errors"></a>Errori di binding

Il programma di installazione di Machine Learning Server e l'utilità SqlBindR restituiscono entrambi i codici di errore e i messaggi seguenti.

|Codice di errore  | Message           | Dettagli               |
|------------|-------------------|-----------------------|
|Errore di binding 0 | OK (success) (Operazione riuscita) | Binding completato senza errori. |
|Errore di binding 1 | Invalid arguments (Argomenti non validi) | Errore di sintassi. |
|Errore di binding 2 | Invalid action (Azione non valida) | Errore di sintassi. |
|Errore di binding 3 | Invalid instance (Istanza non valida) | È presente un'istanza, ma non è valida per il binding. |
|Errore di binding 4 | Not bindable (Istanza non associabile) | |
|Errore di binding 5 | Already bound (Istanza già associata) | È stato eseguito il comando *bind* , ma l'istanza specificata è già associata. |
|Errore di binding 6 | Bind failed (Binding non riuscito) | Si è verificato un errore durante l'annullamento del binding dell'istanza. Questo errore può verificarsi se si esegue il programma di installazione di Machine Learning Server senza selezionare alcuna funzionalità. Per il binding è necessario selezionare un'istanza di MSSQL e i componenti Python e R, purché l'istanza sia SQL Server 2017. Questo errore si verifica anche nel caso in cui SqlBindR non riesca a scrivere nella cartella Programmi. Il problema può essere dovuto a sessioni o handle aperti a SQL Server. Se viene restituito questo errore, riavviare il computer e ripetere i passaggi per il binding prima di avviare nuove sessioni.|
|Errore di binding 7 | Not bound (Istanza non associata) | Nell'istanza del motore di database è presente R Services o Machine Learning Services per SQL Server. L'istanza non è associata a Microsoft Machine Learning Server. |
|Errore di binding 8 | Unbind failed (Annullamento del binding non riuscito) | Si è verificato un errore durante l'annullamento del binding dell'istanza. |
|Errore di binding 9 | No instances found (Non è stata trovata alcuna istanza) | Non sono state trovate istanze del motore di database nel computer. |

## <a name="known-issues"></a>Problemi noti

In questa sezione sono elencati i problemi noti relativi all'uso dell'utilità SqlBindR.exe o agli aggiornamenti di Machine Learning Server che possono avere effetto sulle istanze di SQL Server.

### <a name="restoring-packages-that-were-previously-installed"></a>Ripristino dei pacchetti installati in precedenza

SqlBindR.exe non è in grado di ripristinare i pacchetti originali o i componenti R con l'aggiornamento a Microsoft R Server 9.0.1. Usare l'operazione di ripristino di SQL Server per l'istanza e applicare tutte le versioni del servizio. Riavviare l'istanza.

La versione successiva di SqlBindR ripristina automaticamente le funzionalità di R originali, eliminando la necessità di reinstallare i componenti R o di riapplicare patch al server. È tuttavia necessario installare gli eventuali aggiornamenti dei pacchetti R aggiunti dopo l'installazione iniziale.

Usare i comandi R per sincronizzare i pacchetti installati con i file system usando i record del database. Per altre informazioni, vedere [Gestione dei pacchetti R per SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

### <a name="problems-with-overwritten-sqlbinrini-file-in-sql-server"></a>Problemi con la sovrascrittura del file sqlbinr.ini in SQL Server

Scenario: Questo problema si verifica con il binding di Machine Learning Server 9.4.7 a SQL Server 2017.  Quando Python viene aggiornato e associato o quando si esegue l'aggiornamento a un nuovo CU, il binding di Python non viene riconosciuto e i file vengono sovrascritti. Non esiste un problema noto con R.

Come soluzione alternativa, creare un file `sqlbindr.ini` nella directory PYTHON_SERVICES non vuota. Il contenuto non influisce sulla modalità di funzionamento del file.

Creare un file `sqlbindr.ini` contenente **9.4.7.82** e salvarlo in questa posizione:  

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

### <a name="problems-with-multiple-upgrades-from-sql-server"></a>Problemi relativi a più aggiornamenti di SQL Server

Scenario: Istanza di R Services per SQL Server 2016 aggiornata alla versione 9.0.1. Nuovo programma di installazione eseguito per Microsoft R Server 9.1.0. Il programma di installazione visualizza un elenco di tutte le istanze valide.
Per impostazione predefinita, il programma di installazione seleziona le istanze precedentemente associate. Se si continua, il binding delle istanze associate in precedenza viene annullato. Di conseguenza, l'installazione precedente della versione 9.0.1 viene rimossa con tutti i pacchetti correlati, ma la nuova versione di Microsoft R Server (9.1.0) non viene installata.

Come soluzione alternativa, è possibile modificare l'installazione esistente di R Server nel modo seguente:
1. Nel Pannello di controllo aprire **Programmi e funzionalità**.
2. Individuare Microsoft R Server e selezionare **Cambia/Modifica**.
3. All'avvio del programma di installazione, selezionare le istanze per cui eseguire il binding alla versione 9.1.0.

Microsoft Machine Learning Server 9.2.1 e 9.3 non presentano questo problema.

### <a name="binding-or-unbinding-leaves-multiple-temporary-folders"></a>Cartelle temporanee non rimosse automaticamente dopo il binding o l'annullamento del binding

Rimuovere le cartelle temporanee al termine dell'installazione.

> [!NOTE]
> Attendere il completamento dell'installazione. Può essere necessario molto tempo per rimuovere le librerie R associate a una versione e quindi aggiungere le nuove librerie R. Al termine dell'operazione, le cartelle temporanee vengono rimosse.

## <a name="see-also"></a>Vedere anche

+ [Modificare la versione predefinita del runtime dei linguaggi R o Python](./change-default-language-runtime-version.md)
+ [Installare Machine Learning Server per Windows (con connessione Internet)](/machine-learning-server/install/machine-learning-server-windows-install)
+ [Installare Machine Learning Server per Windows (offline)](/machine-learning-server/install/machine-learning-server-windows-offline)
+ [Problemi noti di Machine Learning Server](/machine-learning-server/resources-known-issues)
+ [Annunci di funzionalità della versione precedente di R Server](/r-server/whats-new-in-r-server)
+ [Funzionalità deprecate, non più supportate o modificate](/machine-learning-server/resources-deprecated-features)