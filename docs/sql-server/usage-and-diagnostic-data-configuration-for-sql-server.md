---
title: Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server (Analisi utilizzo software) | Microsoft Docs
description: Informazioni sui dati che SQL Server raccoglie dagli utenti per migliorare i prodotti. Vedere come configurare SQL Server per non inviare queste informazioni.
author: MikeRayMSFT
ms.author: mikeray
ms.date: 03/27/2019
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: configuration
ms.openlocfilehash: 0fc8437d6ffb3382dbcc53edc816b00ec22b7a07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353570"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-ceip"></a>Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server (Analisi utilizzo software)

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

## <a name="summary"></a>Summary

Per impostazione predefinita, Microsoft SQL Server raccoglie informazioni su come i clienti usano l'applicazione. In particolare, SQL Server raccoglie informazioni sull'esperienza di installazione, l'utilizzo e le prestazioni. Queste informazioni consentono a Microsoft di migliorare il prodotto per meglio soddisfare le esigenze dei clienti. Ad esempio, Microsoft raccoglie informazioni sui tipi di codici di errore riscontrati dai clienti in modo da poter correggere i bug correlati, migliorare la documentazione su come usare SQL Server e determinare se occorre aggiungere funzionalità al prodotto per offrire un'esperienza migliore ai clienti.

In particolare, Microsoft non invia alcuna informazione dei tipi seguenti tramite questo meccanismo:
- Qualsiasi valore all'interno delle tabelle utente
- Credenziali di accesso o altre informazioni di autenticazione
- Informazioni personali

Lo scenario di esempio seguente include informazioni sull'utilizzo di funzionalità che consentono di migliorare il prodotto.

SQL Server 2017 supporta indici ColumnStore per consentire scenari di analisi veloce. Gli indici ColumnStore combinano una tradizionale struttura dell'indice ad "albero B" per i nuovi dati inseriti con una speciale struttura compressa orientata alle colonne per comprimere i dati e velocizzare l'esecuzione di query. Il prodotto usa l'euristica per la migrazione dei dati dalla struttura ad albero B alla struttura compressa in background, velocizzando così i risultati di query future.

Se l'operazione in background non resta al passo con la velocità di inserimento dei dati, le prestazioni delle query potrebbero essere più lente del previsto. Per migliorare il prodotto, Microsoft raccoglie informazioni sull'efficienza con cui SQL Server gestisce il processo di compressione dei dati automatico. Il team del prodotto usa queste informazioni per mettere a punto frequenza e parallelismo del codice che esegue la compressione. Questa query viene eseguita occasionalmente per raccogliere queste informazioni in modo che Microsoft possibile valutare la velocità di spostamento dei dati. Ciò consente di ottimizzare l'euristica del prodotto.  

```sql
SELECT object_id, type_desc, data_space_id, db_id() AS database_id FROM sys.indexes WITH(nolock) WHERE type = 5 or type = 6 
```

```sql
SELECT cntr_value as merge_policy_evaluation
FROM sys.dm_os_performance_counters WITH(nolock)
WHERE object_name LIKE '%columnstore%' 
AND counter_name ='Total Merge Policy Evaluations' 
AND instance_name = '_Total'
```

Tenere presente che questo processo è incentrato sui meccanismi necessari per offrire valore ai clienti. Il team del prodotto non esamina i dati nell'indice o invia tali dati a Microsoft. SQL Server 2017 raccoglie e invia sempre informazioni sull'esperienza di installazione dal processo di installazione, in modo che sia possibile individuare e correggere eventuali problemi di installazione riscontrati dai clienti. SQL Server 2017 può essere configurato (per ogni singola istanza) per non inviare informazioni a Microsoft tramite i meccanismi seguenti:
- Uso dell'applicazione Segnalazione errori e utilizzo funzionalità
- Impostazione di sottochiavi del Registro di sistema nel server

Per SQL Server in Linux, fare riferimento a [Customer Feedback for SQL Server on Linux](../linux/usage-and-diagnostic-data-configuration-for-sql-server-linux.md) (Commenti e suggerimenti dei clienti per SQL Server in Linux)

> [!NOTE]
> È possibile disabilitare l'invio di informazioni a Microsoft solo nelle versioni a pagamento di SQL Server.

## <a name="remarks"></a>Osservazioni
 - La rimozione o la disabilitazione del servizio Analisi utilizzo software di SQL non è supportata. 
 - La rimozione di risorse di Analisi utilizzo software di SQL dal gruppo di cluster non è supportata. 

Per rifiutare esplicitamente la raccolta di dati, vedere [Attivazione o disattivazione del controllo locale](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off)

## <a name="error-and-usage-reporting-application"></a>Applicazione Segnalazione errori e utilizzo funzionalità 

Dopo l'installazione, l'impostazione per la raccolta dei dati di diagnostica e di utilizzo per i componenti e le istanze di SQL Server può essere modificata tramite l'applicazione Segnalazione errori e utilizzo funzionalità. L'applicazione è disponibile come parte dell'installazione di SQL Server. Questo strumento consente a ogni istanza di SQL Server di configurare un'impostazione specifica per i report di utilizzo.

> [!NOTE]
> L'applicazione Segnalazione errori e utilizzo funzionalità è inclusa nell'elenco Strumenti di configurazione di SQL Server. È possibile usare questo strumento per gestire le preferenze per la raccolta dei dati di segnalazione errori e di utilizzo e diagnostica, come in SQL Server 2017. La raccolta dei dati di segnalazione errori è separata dalla raccolta dei dati di utilizzo e diagnostica, pertanto può essere attivata o disattivata indipendentemente dalla raccolta dei dati di utilizzo e diagnostica. La funzionalità di segnalazione errori raccoglie dump di arresto anomalo del sistema che vengono inviati a Microsoft e che possono contenere informazioni riservate, come indicato nell'[informativa sulla privacy](./sql-server-privacy.md).

Per avviare Segnalazione errori e utilizzo funzionalità di SQL Server, fare clic o toccare **Start** e quindi cercare "Errore" nella casella di ricerca. Verrà visualizzato l'elemento Segnalazione errori e utilizzo funzionalità di SQL Server. Dopo avere avviato lo strumento, è possibile gestire i dati di utilizzo e diagnostica, così come gli errori gravi raccolti per le istanze e i componenti installati nel computer.

Per le versioni a pagamento, usare le caselle di controllo "Segnalazioni utilizzo funzionalità" per gestire l'invio dei dati di utilizzo e diagnostica a Microsoft.

Per le versioni a pagamento o gratuite, usare le caselle di controllo "Segnalazioni errori" per gestire l'invio di commenti e suggerimenti per errori gravi e dump di arresto anomalo a Microsoft.

## <a name="set-registry-subkeys-on-the-server"></a>Impostare le sottochiavi del Registro di sistema nel server

I clienti aziendali possono configurare impostazioni di Criteri di gruppo per consentire o meno in modo esplicito alla raccolta dei dati di utilizzo e diagnostica. Questa operazione viene eseguita tramite la configurazione di un criterio basato sul Registro di sistema. Le sottochiavi del Registro di sistema e le impostazioni pertinenti sono le seguenti:

- Per le funzionalità dell'istanza di SQL Server:
    
    Sottochiave = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{IDIstanza}\CPE
    
    Nome RegEntry = CustomerFeedback
    
    Tipo voce DWORD: 0 rifiuto esplicito; 1 consenso esplicito
    
    {IDIstanza} fa riferimento al tipo di istanza e all'istanza, come negli esempi seguenti:

    - MSSQL14.CANBERRA per il motore di database di SQL Server 2017 e il nome di istanza "CANBERRA"
    - MSAS14.CANBERRA per SQL Server 2017 Analysis Services e il nome di istanza "CANBERRA"
    - MSRS14.CANBERRA per SQL Server 2017 Reporting Services e il nome di istanza "CANBERRA"

- Per tutte le funzionalità condivise:
    
    Sottochiave = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Versione principale}
    
    Nome RegEntry = CustomerFeedback
    
    Tipo voce DWORD: 0 rifiuto esplicito; 1 consenso esplicito

> [!NOTE]
> {Versione principale} fa riferimento alla versione di SQL Server, ad esempio 140 per SQL Server 2017

- Per SQL Server Management Studio 17 e SQL Server Management Studio 18, fare riferimento a [Documentazione per gli utenti in SQL Server Management Studio](../ssms/sql-server-management-studio-telemetry-ssms.md)

## <a name="set-registry-subkeys-for-crash-dump-collection"></a>Impostare le sottochiavi del Registro di sistema per la raccolta di dump di arresto anomalo del sistema

In modo analogo al comportamento in una versione precedente di SQL Server, i clienti aziendali di SQL Server 2017 possono configurare impostazioni di Criteri di gruppo nel server per acconsentire o meno in modo esplicito alla raccolta di dump di arresto anomalo del sistema. Questa operazione viene eseguita tramite la configurazione di un criterio basato sul Registro di sistema. Le sottochiavi del Registro di sistema e le impostazioni pertinenti sono le seguenti: 

- Per le funzionalità dell'istanza di SQL Server:

    Sottochiave = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{IDIstanza}\CPE

    Nome RegEntry = EnableErrorReporting

    Tipo voce DWORD: 0 rifiuto esplicito; 1 consenso esplicito
 
    {IDIstanza} fa riferimento al tipo di istanza e all'istanza, come negli esempi seguenti: 

    - MSSQL14.CANBERRA per il motore di database di SQL Server 2017 e il nome di istanza "CANBERRA"
    - MSAS14.CANBERRA per SQL Server 2017 Analysis Services e il nome di istanza "CANBERRA"
    - MSRS14.CANBERRA per SQL Server 2017 Reporting Services e il nome di istanza "CANBERRA"
 

- Per tutte le funzionalità condivise:
    
    Sottochiave = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Versione principale}

    Nome RegEntry = EnableErrorReporting

    Tipo voce DWORD: 0 rifiuto esplicito; 1 consenso esplicito

> [!NOTE]
> {Versione principale} fa riferimento alla versione di SQL Server. Ad esempio, "140" indica SQL Server 2017.

I Criteri di gruppo basati sul Registro di sistema per queste sottochiavi del Registro di sistema vengono rispettati dalla raccolta di dump di arresto anomalo del sistema di SQL Server 2017. 

## <a name="crash-dump-collection-for-ssms"></a>Raccolta di dump di arresto anomalo del sistema per SQL Server Management Studio
SQL Server Management Studio non raccoglie autonomamente dump di arresto anomalo del sistema. Qualsiasi dump di arresto anomalo del sistema correlato a SQL Server Management Studio viene raccolto nell'ambito della funzionalità Segnalazione errori di Windows.

La procedura per attivare o disattivare questa funzionalità dipende dalla versione del sistema operativo. Per attivare o disattivare la funzionalità, seguire la procedura indicata nell'articolo appropriato per la versione di Windows in uso.
 
- Windows Server 2016 e Windows 10

    [Configurare i dati di diagnostica di Windows nell'organizzazione](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)
- Windows Server 2008 R2 e Windows 7

    [WER Settings](/windows/desktop/wer/wer-settings) (Impostazioni di segnalazione errori Windows)
 
## <a name="feedback-for-analysis-services"></a>Commenti e suggerimenti per Analysis Services

Durante l'installazione, SQL Server 2016 Analysis Services aggiunge un account speciale all'istanza di Analysis Services. Questo account è un membro del ruolo di amministratore del server di Analysis Services. L'account viene usato per raccogliere informazioni per commenti e suggerimenti dall'istanza di Analysis Services.  

È possibile configurare il servizio in modo che non invii dati di utilizzo e diagnostica, come descritto nella sezione "Impostare le sottochiavi del Registro di sistema nel server". In questo modo, tuttavia, non viene rimosso l'account del servizio. 
 
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]