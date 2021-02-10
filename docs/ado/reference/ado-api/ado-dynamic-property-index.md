---
description: Indice delle proprietà dinamiche ADO
title: Indice proprietà dinamica ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO], index
ms.assetid: 80d389dd-46ef-459f-b0d4-6f712fc4f32d
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a074ca1e5f50e9d9b634fd67cfceeae353c436e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035751"
---
# <a name="ado-dynamic-property-index"></a>Indice delle proprietà dinamiche ADO
I provider di dati, i provider di servizi e i componenti del servizio possono aggiungere proprietà dinamiche alle raccolte di **Proprietà** degli oggetti di [connessione](./connection-object-ado.md) e [Recordset](./recordset-object-ado.md) non aperti. Un provider specificato può anche inserire proprietà aggiuntive quando questi oggetti sono aperti. Alcune di queste proprietà sono elencate nella sezione [proprietà dinamiche ADO](./ado-dynamic-properties.md) . Ulteriori informazioni sono elencate sotto i provider specifici nella sezione [appendice a: Providers](../../guide/appendixes/appendix-a-providers.md) .  
  
 Le tabelle seguenti sono indici incrociati dei nomi ADO e OLE DB per ogni proprietà dinamica del provider OLE DB standard. I provider possono aggiungere altre proprietà rispetto a quelle elencate qui. Per informazioni specifiche sulle proprietà dinamiche specifiche del provider, vedere la documentazione del provider.  
  
 Il riferimento del programmatore OLE DB fa riferimento a un nome di proprietà ADO in base al termine "Description". Per ulteriori informazioni su queste proprietà standard, cercare o esplorare l'indice nella [documentazione di OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85))per la proprietà OLE DB in base al nome.  
  
## <a name="connection-dynamic-properties"></a>Proprietà dinamiche della connessione  
  
|Nome della proprietà ADO|Nome della proprietà OLE DB|  
|-----------------------|--------------------------|  
|Sessioni attive|DBPROP_ACTIVESESSIONS|  
|Interruzione asincrona|DBPROP_ASYNCTXNABORT|  
|Commit asincrona|DBPROP_ASYNCTNXCOMMIT|  
|Livelli di isolamento autocommit|DBPROP_SESS_AUTOCOMMITISOLEVELS|  
|Percorso catalogo|DBPROP_CATALOGLOCATION|  
|Termine Catalogo|DBPROP_CATALOGTERM|  
|Definizione di colonna|DBPROP_COLUMNDEFINITION|  
|Timeout di connessione|DBPROP_INIT_TIMEOUT|  
|Catalogo corrente|DBPROP_CURRENTCATALOG|  
|origine dati|DBPROP_INIT_DATASOURCE|  
|Data Source Name|DBPROP_DATASOURCENAME|  
|Modello di threading dell'oggetto origine dati|DBPROP_DSOTHREADMODEL|  
|Nome DBMS|DBPROP_DBMSNAME|  
|Versione DBMS|DBPROP_DBMSVER|  
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|  
|Supporto per GROUP BY|DBPROP_GROUPBY|  
|Supporto tabelle eterogenee|DBPROP_HETEROGENEOUSTABLES|  
|Distinzione maiuscole/minuscole identificatore|DBPROP_IDENTIFIERCASE|  
|Catalogo iniziale|DBPROP_INIT_CATALOG|  
|Livelli di isolamento|DBPROP_SUPPORTEDTXNISOLEVELS|  
|Conservazione isolamento|DBPROP_SUPPORTEDTXNISORETAIN|  
|Locale Identifier|DBPROP_INIT_LCID|  
|Location|DBPROP_INIT_LOCATION|  
|Dimensioni massime indice|DBPROP_MAXINDEXSIZE|  
|Dimensioni massime riga|DBPROP_MAXROWSIZE|  
|Dimensioni massime righe includono BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|  
|Numero massimo di tabelle in SELECT|DBPROP_MAXTABLESINSELECT|  
|Modalità|DBPROP_INIT_MODE|  
|Set di parametri multipli|DBPROP_MULTIPLEPARAMSETS|  
|Risultati multipli|DBPROP_MULTIPLERESULTS|  
|Più oggetti di archiviazione|DBPROP_MULTIPLESTORAGEOBJECTS|  
|Aggiornamento di più tabelle|DBPROP_MULTITABLEUPDATE|  
|Ordinamento regole di confronto NULL|DBPROP_NULLCOLLATION|  
|Comportamento concatenazione NULL|DBPROP_CONCATNULLBEHAVIOR|  
|Servizi di OLE DB|DBPROP_INIT_OLEDBSERVICES|  
|Versione OLE DB|DBPROP_PROVIDEROLEDBVER|  
|Supporto per oggetti OLE|DBPROP_OLEOBJECTS|  
|Supporto per set di righe aperto|DBPROP_OPENROWSETSUPPORT|  
|Colonne ORDER BY nell'elenco di selezione|DBPROP_ORDERBYCOLUMNSINSELECT|  
|Disponibilità parametro di output|DBPROP_OUTPUTPARAMETERAVAILABILITY|  
|Passa per funzioni di accesso Ref|DBPROP_BYREFACCESSORS|  
|Password|DBPROP_AUTH_PASSWORD|  
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|  
|Tipo ID persistente|DBPROP_PERSISTENTIDTYPE|  
|Comportamento preparazione interruzione|DBPROP_PREPAREABORTBEHAVIOR|  
|Comportamento preparazione commit|DBPROP_PREPARECOMMITBEHAVIOR|  
|Termine procedura|DBPROP_PROCEDURETERM|  
|Prompt|DBPROP_INIT_PROMPT|  
|Nome descrittivo provider|DBPROP_PROVIDERFRIENDLYNAME|  
|Provider Name|DBPROP_PROVIDERFILENAME|  
|Versione del provider|DBPROP_PROVIDERVER|  
|Read-Only origine dati|DBPROP_DATASOURCEREADONLY|  
|Conversioni di set di righe nel comando|DBPROP_ROWSETCONVERSIONSONCOMMAND|  
|Termine schema|DBPROP_SCHEMATERM|  
|Utilizzo dello schema|DBPROP_SCHEMAUSAGE|  
|Supporto di SQL|DBPROP_SQLSUPPORT|  
|Archiviazione strutturata|DBPROP_STRUCTUREDSTORAGE|  
|Supporto sottoquery|DBPROP_SUBQUERIES|  
|Termine tabella|DBPROP_TABLETERM|  
|DDL transazione|DBPROP_SUPPORTEDTXNDDL|  
|ID utente|DBPROP_AUTH_USERID|  
|Nome utente|DBPROP_USERNAME|  
|Handle finestra|DBPROP_INIT_HWND|  
  
## <a name="recordset-dynamic-properties"></a>Proprietà dinamiche del recordset  
 Si noti che le **proprietà dinamiche** dell'oggetto **Recordset** non rientrano nell'ambito (diventano non disponibili) quando il **Recordset** viene chiuso.  
  
|Nome della proprietà ADO|Nome della proprietà OLE DB|  
|-----------------------|--------------------------|  
|IAccessor|DBPROP_IACCESSOR|  
|IChapteredRowset||  
|IColumnsInfo|DBPROP_ICOLUMNSINFO|  
|IColumnsRowset|DBPROP_ICOLUMNSROWSET|  
|IConnectionPointContainer|DBPROP_ICONNECTIONPOINTCONTAINER|  
|IConvertType||  
|ILockBytes|DBPROP_ILOCKBYTES|  
|IRowset|DBPROP_IROWSET|  
|IDBAsynchStatus|DBPROP_IDBASYNCHSTATUS|  
|IParentRowset||  
|IRowsetChange|DBPROP_IROWSETCHANGE|  
|IRowsetExactScroll||  
|IRowsetFind|DBPROP_IROWSETFIND|  
|IRowsetIdentity|DBPROP_IROWSETIDENTITY|  
|IRowsetInfo|DBPROP_IROWSETINFO|  
|IRowsetLocate|DBPROP_IROWSETLOCATE|  
|IRowsetRefresh|DBPROP_IROWSETREFRESH|  
|IRowsetResynch||  
|IRowsetScroll|DBPROP_IROWSETSCROLL|  
|IRowsetUpdate|DBPROP_IROWSETUPDATE|  
|IRowsetView|DBPROP_IROWSETVIEW|  
|IRowsetIndex|DBPROP_IROWSETINDEX|  
|ISequentialStream|DBPROP_ISEQUENTIALSTREAM|  
|IStorage|DBPROP_ISTORAGE|  
|IStream|DBPROP_ISTREAM|  
|ISupportErrorInfo|DBPROP_ISUPPORTERRORINFO|  
|Ordine di accesso|DBPROP_ACCESSORDER|  
|Set di righe Append-Only|DBPROP_APPENDONLY|  
|Elaborazione asincrona dei set di righe|DBPROP_ROWSET_ASYNCH|  
|Ricalcolo automatico|DBPROP_ADC_AUTORECALC|  
|Dimensioni recupero in background|DBPROP_ASYNCHFETCHSIZE|  
|Background Thread Priority|DBPROP_ASYNCHTHREADPRIORITY|  
|Dimensioni batch|DBPROP_ADC_BATCHSIZE|  
|Blocco di oggetti di archiviazione|DBPROP_BLOCKINGSTORAGEOBJECTS|  
|Tipo di segnalibro|DBPROP_BOOKMARKTYPE|  
|Bookmarkable|DBPROP_IROWSETLOCATE|  
|Segnalibri ordinati|DBPROP_ORDEREDBOOKMARKS|  
|Memorizza nella cache le righe figlio|DBPROP_ADC_CACHECHILDROWS|  
|Memorizza nella cache le colonne posticipate|DBPROP_CACHEDEFERRED|  
|Modifica righe inserite|DBPROP_CHANGEINSERTEDROWS|  
|Privilegi di colonna|DBPROP_COLUMNRESTRICT|  
|Notifica set di colonne|DBPROP_NOTIFYCOLUMNSET|  
|Colonna scrivibile|DBPROP_MAYWRITECOLUMN|  
|Timeout del comando|DBPROP_COMMANDTIMEOUT|  
|Versione del motore di cursori|DBPROP_ADC_CEVER|  
|Rinvia colonna|DBPROP_DEFERRED|  
|Ritardare gli aggiornamenti degli oggetti di archiviazione|DBPROP_DELAYSTORAGEOBJECTS|  
|Recupera all'indietro|DBPROP_CANFETCHBACKWARDS|  
|Operazioni di filtro|DBPROP_FILTERCOMPAREOPS|  
|Operazioni di ricerca|DBPROP_FINDCOMPAREOPS|  
|Colonne nascoste (conteggio)|DBPROP_HIDDENCOLUMNS|  
|Mantieni righe|DBPROP_CANHOLDROWS|  
|Righe non mobili|DBPROP_IMMOBILEROWS|  
|Dimensioni recupero iniziali|DBPROP_ASYNCHPREFETCHSIZE|  
|Segnalibri letterali|DBPROP_LITERALBOOKMARKS|  
|Identità di riga letterale|DBPROP_LITERALIDENTITY|  
|Mantieni stato modifica|DBPROP_ADC_MAINTAINCHANGESTATUS|  
|Numero massimo di righe aperte|DBPROP_MAXOPENROWS|  
|Numero massimo di righe in sospeso|DBPROP_MAXPENDINGROWS|  
|Numero massimo di righe|DBPROP_MAXROWS|  
|Utilizzo memoria|DBPROP_MEMORYUSAGE|  
|Granularità delle notifiche|DBPROP_NOTIFICATIONGRANULARITY|  
|Fasi di notifica|DBPROP_NOTIFICATIONPHASES|  
|Oggetti sottoposti a transazione|DBPROP_TRANSACTEDOBJECT|  
|Modifiche di altri utenti visibili|DBPROP_OTHERUPDATEDELETE|  
|Inserimenti di altri utenti visibili|DBPROP_OTHERINSERT|  
|Modifiche personalizzate visibili|DBPROP_OWNUPDATEDELETE|  
|Inserimenti personali visibili|DBPROP_OWNINSERT|  
|Mantieni in interruzione|DBPROP_ABORTPRESERVE|  
|Mantieni al commit|DBPROP_COMMITPRESERVE|  
|Private1||  
|Riavvio rapido|DBPROP_QUICKRESTART|  
|Eventi rientranti|DBPROP_REENTRANTEVENTS|  
|Rimuovi righe eliminate|DBPROP_REMOVEDELETED|  
|Segnala più modifiche|DBPROP_REPORTMULTIPLECHANGES|  
|Nome riforma|DBPROP_ADC_RESHAPENAME|  
|Comando di risincronizzazione|DBPROP_ADC_CUSTOMRESYNCH|  
|Restituisci inserimenti in sospeso|DBPROP_RETURNPENDINGINSERTS|  
|Notifica eliminazione riga|DBPROP_NOTIFYROWDELETE|  
|Notifica di modifica prima riga|DBPROP_NOTIFYROWFIRSTCHANGE|  
|Notifica inserimento riga|DBPROP_NOTIFYROWINSERT|  
|Privilegi di riga|DBPROP_ROWRESTRICT|  
|Notifica della risincronizzazione delle righe|DBPROP_NOTIFYROWRESYNCH|  
|Modello di threading delle righe|DBPROP_ROWTHREADMODEL|  
|Notifica modifiche annullamento riga|DBPROP_NOTIFYROWUNDOCHANGE|  
|Notifica annullamento eliminazione riga|DBPROP_NOTIFYROWUNDODELETE|  
|Notifica annullamento inserimento riga|DBPROP_NOTIFYROWUNDOINSERT|  
|Notifica aggiornamento riga|DBPROP_NOTIFYROWUPDATE|  
|Notifica di modifica posizione recupero set di righe|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|  
|Notifica di rilascio del set di righe|DBPROP_NOTIFYROWSETRELEASE|  
|Scorri indietro|DBPROP_CANSCROLLBACKWARDS|  
|Cursore server|DBPROP_SERVERCURSOR|  
|Ignora segnalibri eliminati|DBPROP_BOOKMARKSKIPPED|  
|Identità riga forte|DBPROP_STRONGIDENTITY|  
|Catalogo univoco|DBPROP_ADC_UNIQUECATALOG|  
|Righe univoche|DBPROP_UNIQUEROWS|  
|Schema univoco|DBPROP_ADC_UNIQUESCHEMA|  
|tabella univoca|DBPROP_ADC_UNIQUETABLE|  
|Aggiornabilità|DBPROP_UPDATABILITY|  
|Criteri di aggiornamento|DBPROP_ADC_UPDATECRITERIA|  
|Aggiornamento della risincronizzazione|DBPROP_ADC_UPDATERESYNC|  
|Usare i segnalibri|DBPROP_BOOKMARKS|