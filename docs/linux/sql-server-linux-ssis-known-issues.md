---
title: Limitazioni e problemi noti di SSIS in Linux
description: Questo articolo descrive le limitazioni e i problemi noti di SQL Server Integration Services (SSIS) nei computer Linux
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 06/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ce07fb02a81b4e44375d80f643713b9a0b62d80d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838998"
---
# <a name="limitations-and-known-issues-for-ssis-on-linux"></a>Limitazioni e problemi noti di SSIS in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive le limitazioni e i problemi noti di SQL Server Integration Services (SSIS) in Linux.

## <a name="general-limitations-and-known-issues"></a>Limitazioni e problemi noti generali

Le funzionalità seguenti non sono supportate in questa versione di SSIS in Linux:
  - Database di catalogo SSIS
  - Esecuzione pianificata dei pacchetti tramite SQL Agent
  - Autenticazione di Windows
  - Componenti di terze parti
  - Change Data Capture (CDC)
  - SSIS Scale Out
  - Feature Pack di Azure per SSIS
  - Supporto di Hadoop e HDFS
  - Microsoft Connector for SAP BW

Per altre limitazioni e problemi noti di SSIS in Linux, vedere le [note sulla versione](sql-server-linux-release-notes.md#ssis).

## <a name="supported-and-unsupported-components"></a><a name="components"></a> Componenti supportati e non supportati

I componenti di Integration Services predefiniti seguenti sono supportati in Linux. Alcuni di questi presentano limitazioni nella piattaforma Linux. I componenti predefiniti non elencati qui non sono supportati in Linux.

## <a name="supported-control-flow-tasks"></a>Attività di flusso di controllo
- Inserimento bulk - attività
- Attività Flusso di dati
- Attività Profiling dati
- Attività Esegui SQL
- Attività Esegui istruzione T-SQL
- Attività Espressione
- Attività FTP
- Attività Servizio Web
- Attività XML

## <a name="control-flow-tasks-supported-with-limitations"></a>Attività di flusso di controllo supportate con limitazioni

| Attività | Limitazioni |
|------------|---|
| Execute Process Task | Supporta solo la modalità in-process. |
| Attività File system | Le azioni *Sposta directory* e *Set file attributes* (Imposta attributi file) non sono supportate. |
| attività Script | Supporta solo API .NET Framework standard. |
| Invia messaggi - attività | Supporta solo la modalità utente anonimo. |
| Attività Trasferisci database | I percorsi UNC non sono supportati. |
| | |

## <a name="supported-and-unsupported-maintenance-plan-tasks"></a>Attività di piano di manutenzione supportate e non supportate

In un piano di manutenzione di SQL Server è in genere possibile usare un'ampia gamma di attività SSIS.

Le attività di piano di manutenzione seguenti non sono supportate in Linux:
- Notifica operatore
- Esecuzione processo di SQL Server Agent

Le attività di piano di manutenzione seguenti sono supportate in Linux:
- Controlla integrità database
- Compatta database
- Riorganizza indice
- Ricompila indice
- Aggiorna statistiche
- Pulisci contenuto cronologia
- Backup database
- Istruzione T-SQL

## <a name="supported-control-flow-containers"></a>Contenitori di flusso di controllo supportati
- Sequenza - contenitore
- Contenitore Ciclo For
- Contenitore Ciclo Foreach

## <a name="supported-data-flow-sources-and-destinations"></a>Origini e destinazioni dei dati supportate
- Origine e destinazione file non elaborato
- Origine XML

## <a name="data-flow-sources-and-destinations-supported-with-limitations"></a>Origini e destinazioni di flusso di dati supportate con limitazioni

| Componente | Limitazioni |
|------------|---|
| Origine e destinazione ADO.NET | Supporta solo il provider di dati SQLClient. |
| Origine e destinazione file flat | 1. supportano solo i percorsi di file di tipo Windows a cui viene applicata la regola di mapping del percorso predefinito. Ad esempio, `D:\home\ssis\travel.csv` diventa `/home/ssis/travel.csv`. 2. La condivisione file di Azure montata in Red Hat 7 non è supportata. |
| Origine OData | Supporta solo l'autenticazione di base. |
| Origine e destinazione ODBC | Supporta i driver ODBC Unicode a 64 bit in Linux. Dipende dalla gestione driver UnixODBC in Linux. |
| Origine e destinazione OLE DB | Supporta solo SQL Server Native Client 11.0 e Provider Microsoft OLE DB per SQL Server. |
| | |

## <a name="supported-data-flow-transformations"></a>Trasformazioni di flusso di dati supportate
- Aggregate
- Audit
- Server di distribuzione di dati bilanciati
- Mappa caratteri
- Suddivisione condizionale
- Copia colonna
- Conversione dati
- Colonna derivata
- Esportazione colonna
- Raggruppamento fuzzy
- Ricerca fuzzy
- Importa colonna
- Ricerca
- Unione
- Merge Join
- Multicast
- Pivot
- Row Count
- Dimensione a modifica lenta
- Ordina
- Ricerca termini
- Union All
- UnPivot

## <a name="data-flow-transformations-supported-with-limitations"></a>Trasformazioni di flusso di dati supportate con limitazioni

| Componente | Limitazioni |
|------------|---|
| Comando OLE DB - trasformazione | Stesse limitazioni di origine e destinazione OLE DB. |
| componente script | Supporta solo API .NET Framework standard. |
| | |

## <a name="supported-and-unsupported-log-providers"></a>Provider di log supportati e non supportati
Tutti i provider di log SSIS predefiniti sono supportati in Linux, ad eccezione del provider di log eventi di Windows.

Il provider di log di SQL Server supporta solo l'autenticazione SQL e non l'autenticazione di Windows.

I provider di log SSIS per i file di testo, per i file XML e per SQL Server Profiler scrivono l'output in un file specificato dall'utente. Al percorso del file si applicano le considerazioni seguenti:
-   Se non si specifica un percorso, il provider di log scrive nella directory corrente dell'host. Se l'utente corrente non dispone delle autorizzazioni necessarie per scrivere nella directory corrente dell'host, il provider di log genera un errore.
-   Non è possibile usare una variabile di ambiente in un percorso di file. Se si specifica una variabile di ambiente, il testo letterale specificato viene visualizzato nel percorso del file. Se, ad esempio, si specifica `%TMP%/log.txt`, il provider di log accoda il testo letterale `/%TMP%/log.txt` alla directory host corrente.

## <a name="related-content-about-ssis-on-linux"></a>Contenuto correlato su SSIS in Linux
-   [Estrarre, trasformare e caricare i dati in Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md)
-   [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Pianificare l'esecuzione del pacchetto SQL Server Integration Services in Linux con cron](sql-server-linux-schedule-ssis-packages.md)
