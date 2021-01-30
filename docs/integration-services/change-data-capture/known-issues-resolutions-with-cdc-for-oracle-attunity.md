---
description: Errori noti e soluzioni con Change Data Capture per Oracle di Attunity
title: Errori noti e soluzioni con Change Data Capture per Oracle di Attunity | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e89a3638dc304bdda8bc11c55897975fb006e3ca
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076839"
---
# <a name="known-errors-and-resolutions-with-change-data-capture-for-oracle-by-attunity"></a>Errori noti e soluzioni con Change Data Capture per Oracle di Attunity
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]

In questo argomento vengono elencati i problemi principali e le soluzioni note quando si visualizza un'istanza di Change Data Capture (CDC) nello strumento di configurazione Oracle CDC Designer. Questo strumento fa parte di Change Data Capture per Oracle di Attunity, incluso a partire da SQL Server 2012. 

## <a name="bug-fixes"></a>Correzioni di bug
Prima di dedicare troppo tempo alla risoluzione dei problemi, è importante usare le build più recenti di CDC per Oracle di Attunity per evitare problemi noti, come i seguenti:

### <a name="sql-server-2017"></a>SQL Server 2017

La **versione 5.0.0.111** contiene le correzioni seguenti:
- Correzione di bug: errore di Oracle CDC Designer con il messaggio "Sintassi non corretta in prossimità della parola chiave 'KEY'" durante l'aggiunta di una tabella Oracle. 
- Miglioramento: supporto migliorato per RAC, che include una gestione migliore del riavvio di un nodo RAC. 
- Correzione di bug: CDC non funziona con Oracle 10.2 a causa della richiesta di NEXT_CHANGE# da v$log. 


La **versione 5.0.0.93** contiene le correzioni seguenti: 
- Errore di Microsoft CDC per Oracle di Attunity Designer con il messaggio "Sintassi non corretta in prossimità della parola chiave 'KEY'" durante l'aggiunta di una tabella Oracle. 

 
### <a name="sql-server-2016"></a>SQL Server 2016

La **versione 4.0.107** contiene le correzioni seguenti:
- Correzioni di bug: errore di Oracle CDC Designer con il messaggio "Sintassi non corretta in prossimità della parola chiave 'KEY'" durante l'aggiunta di una tabella Oracle.
- Miglioramento: supporto migliorato per RAC, che include una gestione migliore del riavvio di un nodo RAC.
- Correzioni di bug: CDC non funziona con Oracle 10.2 a causa della richiesta di NEXT_CHANGE# da v$log.

La **versione 4.0.0.95** contiene le correzioni seguenti: 
- Correzioni di bug: errore di Oracle CDC Designer con il messaggio "Sintassi non corretta in prossimità della parola chiave 'KEY'" durante l'aggiunta di una tabella Oracle.

La **versione 4.0.0.88** contiene le correzioni seguenti:
-  Le proprietà aggiunte nelle opzioni avanzate dell'istanza di Attunity CDC vengono rimosse quando una tabella viene aggiunta o rimossa da CDC. 
- Attunity CDC smette di funzionare dopo l'applicazione della correzione SQL che aggiunge la colonna __$command_id

### <a name="sql-server-2014"></a>SQL Server 2014 

La **versione 2.0.0.114** contiene le correzioni seguenti:
- Correzioni di bug: errore di Oracle CDC Designer con il messaggio "Sintassi non corretta in prossimità della parola chiave 'KEY'" durante l'aggiunta di una tabella Oracle.
- Miglioramento: supporto migliorato per RAC, che include una gestione migliore del riavvio di un nodo RAC.
- Correzioni di bug: CDC non funziona con Oracle 10.2 a causa della richiesta di NEXT_CHANGE# da v$log.

La **versione 2.0.0.92** contiene le correzioni seguenti: 
- Le proprietà aggiunte nelle opzioni avanzate dell'istanza di Attunity CDC vengono rimosse quando una tabella viene aggiunta o rimossa da CDC. Attunity CDC smette di funzionare dopo l'applicazione della correzione SQL che aggiunge la colonna __$command_id
- Convalida dei metadati non riuscita per la tabella Oracle cdc.table_name. L'indice della colonna column_name non è compreso nell'intervallo.  E questo problema: Il servizio Oracle CDC mostra lo stato interrotto quando si usa CDC per Oracle di Attunity
    - Problema risolto nell'_aggiornamento cumulativo 1 per SQL Server 2014 RTM_, come descritto nell'articolo della Knowledge Base [2894025](https://support.microsoft.com/kb/2894025).
- Alcune modifiche vengono perse e non vengono replicate nel database di SQL Server. Questo problema si verifica quando una tabella contiene più di un oggetto CLOB (Character Large Binary Object) e uno dei CLOB ha un valore elevato. 
    - Problema risolto nell'_aggiornamento cumulativo 1 per SQL Server 2014 SP1_ e nell'_aggiornamento cumulativo 8 per SQL Server 2014 RTM_, come descritto nell'articolo della Knowledge Base [3029096](https://support.microsoft.com/kb/3029096). 
- Change Data Capture per Oracle di Attunity smette di funzionare quando le tabelle Oracle hanno una colonna con tipo di dati Long.
    - Correzione nell' _aggiornamento cumulativo 5 per SQL Server 2014 SP1_ e _aggiornamento cumulativo 12 per SQL 2014 RTM_ , come descritto in KB [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d).

### <a name="sql-server-2012"></a>SQL Server 2012

La **versione 1.1.0.102** contiene le correzioni seguenti: 
 
- Le proprietà aggiunte nelle opzioni avanzate dell'istanza di Attunity CDC vengono rimosse quando una tabella viene aggiunta o rimossa da CDC. Attunity CDC smette di funzionare dopo l'applicazione della correzione SQL che aggiunge la colonna __$command_id
- L'istanza di CDC per Oracle si blocca quando viene avviata e non acquisisce le modifiche. È possibile che la memoria del server Oracle aumenti fino a esaurire la memoria o fino all'arresto anomalo.
- [2672759](https://support.microsoft.com/kb/2672759): Messaggio di errore quando si usa il servizio Microsoft Change Data Capture per Oracle di Attunity: "ORA-00600: codice di errore interno". Aggiungere la traccia di livello SOURCE e verificare se si ottiene lo stesso errore ORA-00600. Problema risolto con il download di una patch Oracle.
- Più partizioni
    - Quando si usano più di 10 partizioni in una tabella Oracle, l'istanza di CDC non è in grado di acquisire tutte le modifiche per la tabella. Quando la tabella Oracle è definita con più di 10 partizioni, le modifiche vengono acquisite solo dalle ultime 10 partizioni. Problema risolto nel _Service Pack 1 per SQL Server 2012_. Vedere la [pagina di download di SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575). 
- Modifiche non acquisite
    - L'acquisizione di eventi può entrare in un ciclo infinito e arrestare l'acquisizione di nuove modifiche dei dati (problema correlato al bug Oracle 5623813). Quando un ambiente Oracle RAC esegue un arresto o una ripresa dell'istanza di CDC, le modifiche possono essere ignorate/perse. Questo significa che in SQL Server Change Data Capture mancheranno righe importanti e pertanto si verifica una perdita di dati nel sistema di data warehouse o di sottoscrizione. Problema risolto nel _Service Pack 1 per SQL Server 2012_. Vedere la [pagina di download di SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)
- Larghezze doppie per le colonne in SQL
    - Quando si crea un'istanza di CDC per Oracle, negli script da eseguire su SQL Server, la lunghezza di una colonna con tipo di dati a larghezza variabile viene raddoppiata nelle tabelle SQL Server create nello script. Ad esempio, se si tenta di rilevare le modifiche in una colonna VARCHAR2(10) in una tabella Oracle, la colonna corrispondente nella tabella di SQL Server sarà di tipo NVARCHAR(20) nello script di distribuzione. Problema risolto nell'_aggiornamento cumulativo 2 per SQL Server 2012 SP1_ o nell'_aggiornamento cumulativo 5 per SQL Server 2012_, come descritto nell'articolo della Knowledge Base [2769673](https://support.microsoft.com/kb/2769673). 
- Dati DDL troncati
    - Quando si esegue un'istruzione DDL (Data Definition Language) superiore a 4.000 byte su un database Oracle che contiene caratteri non latini, si verifica un errore in CDC per Oracle di Attunity. Viene inoltre visualizzato il messaggio di errore `ORA-01406: fetched column value was truncated.`. Problema risolto nell'_aggiornamento cumulativo 4 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2839806](https://support.microsoft.com/kb/2839806). 
- Modifiche perse nelle ultime due colonne
    - Problema risolto nell'_aggiornamento cumulativo 6 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2874879](https://support.microsoft.com/kb/2874879). 
- Le dimensioni del log delle transazioni SQL aumentano quando si usa CDC per Oracle
     - Quando vengono configurate le istanze di Change Data Capture per Oracle, il database SQL che riceve i dati modificati avrà tabelle con mirroring, con transazioni contrassegnate per la replica. Questo comportamento si verifica perché CDC per Oracle si basa sulle stored procedure di sistema sottostanti, simili a quelle usate in CDC per SQL Server. Tuttavia, non essendo prevista la replica di SQL CDC quando CDC per Oracle viene usato da solo, non è disponibile alcun lettore di log per cancellare le transazioni contrassegnate per la replica. Dato che non è necessario replicare la transazione in SQL Server, è possibile contrassegnare manualmente la transazione come distribuita usando la soluzione alternativa descritta più avanti in questo articolo. Altre informazioni sono disponibili nell'articolo della Knowledge Base [2871474](https://support.microsoft.com/kb/2871474). 
- Errore `ORACDC000T: Error encountered at position to change event - SCN not found - EOF simulated`. Problema risolto nell'_aggiornamento cumulativo 7 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2883524](https://support.microsoft.com/kb/2883524). 
- Convalida dei metadati non riuscita per la tabella Oracle cdc.table_name. L'indice della colonna column_name non è compreso nell'intervallo. Problema risolto nell'_aggiornamento cumulativo 7 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2883524](https://support.microsoft.com/kb/2883524).
- Il servizio Oracle CDC mostra lo stato interrotto quando si usa CDC per Oracle di Attunity in SQL Server 2012. Problema risolto nell'_aggiornamento cumulativo 8 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2923839](https://support.microsoft.com/kb/2923839).  
- Alcune modifiche vengono perse e non vengono replicate nei database di SQL Server. Questo problema si verifica quando una tabella contiene più di un oggetto CLOB (Character Large Binary Object) e uno dei CLOB ha un valore elevato. Problema risolto nell'_aggiornamento cumulativo 8 per SQL Server 2012 SP1_, come descritto nell'articolo della Knowledge Base [2923839](https://support.microsoft.com/kb/2923839).   
- Change Data Capture per Oracle di Attunity smette di funzionare quando le tabelle Oracle hanno una colonna con tipo di dati Long. Correzione nell' _aggiornamento cumulativo 2 per SQL Server 2012 SP3_ e _aggiornamento cumulativo 11 per SQL 2012 SP2_  come descritto in KB [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d). 

## <a name="collect-detailed-logs"></a>Raccogliere log dettagliati 

In questa sezione viene spiegato come raccogliere errori ed eventi dall'istanza di CDC. 

### <a name="management-console"></a>Console di gestione

È possibile visualizzare gli errori nei messaggi **Status** (Stato) aggiunti nella console di gestione di Oracle Change Data Capture Designer quando un'istanza di CDC è evidenziata nel riquadro sinistro. 

### <a name="query-trace-table"></a>Eseguire query nella tabella di traccia

È possibile eseguire una query nella tabella di traccia nel database CDC all'interno SQL Server per visualizzare gli errori registrati. 

### <a name="save-output-from-basic-logging"></a>Salvare l'output dalla registrazione di base 

Per acquisire la diagnostica, selezionare **Collect Diagnostics** (Raccogli diagnostica) nella scheda dello stato nella console di gestione di Oracle Change Data Capture. 

![Screenshot che mostra la scheda Status (Stato) nella console di gestione di Oracle Change Data Capture con l'opzione Collect Diagnostics (Raccogli diagnostica) evidenziata.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

Scegliere un'ora di inizio e selezionare un percorso per il file di log. Selezionare quindi **Create** (Crea) per avviare la raccolta di diagnostica. 

![Screenshot della finestra di dialogo Collect Diagnostics for testTA (Raccogli diagnostica per testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/start-diagnostics.png)

### <a name="detailed-errors"></a>Errori dettagliati

È possibile aumentare il livello di traccia raccolto dall'istanza e ripetere lo scenario per ottenere una registrazione più dettagliata. A tale scopo, selezionare **Properties** (Proprietà) in **Actions** (Azioni) e quindi aggiungere una nuova proprietà nella griglia **Advanced Settings** (Impostazioni avanzate) nella scheda **Advanced** (Avanzate). Impostare il nome della proprietà su `trace` e quindi impostare il valore su `SOURCE`. 

![Screenshot che mostra l'opzione Properties (Proprietà) in Actions (Azioni).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/properties.png)

Riprodurre l'errore e quindi selezionare l'opzione **Collect Diagnostics** (Raccogli diagnostica) per raccogliere i log. 

![Un altro screenshot della scheda Status (Stato) nella console di gestione di Oracle Change Data Capture con l'opzione Collect Diagnostics (Raccogli diagnostica) evidenziata.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

## <a name="ora-00942-table-of-view-does-not-exist"></a>ORA-00942 La tabella della vista non esiste 

Si tratta di un errore comune visualizzato nel campo del messaggio **Status** (Stato) dell'istanza di CDC. L'istanza esegue vari tentativi, quindi l'icona dello stato diventerà verde momentaneamente, ma poi l'operazione avrà esito negativo con il punto esclamativo rosso e lo stato UNEXPECTED (Imprevisto). 

![Screenshot che mostra l'errore comune visualizzato nel campo del messaggio Status (Stato) dell'istanza di CDC.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-error.png)

```
"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC508E:Oracle method OCIStmtExecute failed with error: ORA-00942: table or view does not exist ","source","",""

"ERROR","computername","RUNNING","IDLE",
"ORACDC518E:Failed to verify archive log mode.","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC517E:Oracle Call Interface (OCI) method failed: ORA-00942: table or view does not exist .","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC414E:The Engine component failed with return code 1.","engine","",""
```

Questo errore si verifica quando l'account Oracle che si connette dall'istanza di CDC al server Oracle non è autorizzato a visualizzare le viste del registro di sistema. 

### <a name="resolution"></a>Soluzione

Per correggere l'errore, concedere all'utente attualmente configurato le autorizzazioni appropriate all'interno del sistema di database Oracle o cambiare l'account usato per la connessione al server Oracle dall'istanza di CDC. 

L'elenco di tutte le autorizzazioni necessarie è riportato nei dettagli nel file della Guida incluso nella cartella Programmi di installazione `C:\Program Files\Change Data Capture for Oracle by Attunity\Attunity.SqlServer.XdbCdcDesigner.chm`.  Per l'elenco completo, vedere la pagina "Connettersi a un database di origine Oracle" all'interno del file con estensione chm.

È possibile impostare l'account utente selezionando l'istanza CDCInstance nel riquadro a sinistra e selezionando il pulsante delle proprietà nel riquadro delle azioni all'estrema destra nella finestra **CDC Designer**. È possibile modificare l'account di autenticazione di log mining di Oracle dalla finestra di dialogo delle proprietà.

![Screenshot che mostra la scheda Oracle della finestra di dialogo testTA Properties (Proprietà testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-connection.png)


  
## <a name="see-also"></a>Vedi anche  
 [Tenere traccia delle modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Usare Change Data &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Amministrare e monitorare Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
