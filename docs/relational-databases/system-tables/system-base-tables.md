---
description: Tabelle di base di sistema
title: Tabelle di base di sistema | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system base tables [SQL Server]
- hobt [SQL Server]
- base tables
ms.assetid: 31f2df90-651f-4699-8067-19f59b60904f
author: cawrites
ms.author: chadam
ms.openlocfilehash: e01c89ef3903e78b8ba312914c97eed61bd8757e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208897"
---
# <a name="system-base-tables"></a>Tabelle di base di sistema
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tabelle di base di sistema sono le tabelle sottostanti in cui vengono effettivamente archiviati i metadati per un database specifico. Il database **Master** è particolarmente speciale in quanto contiene alcune tabelle aggiuntive non disponibili in nessuno degli altri database. Queste tabelle contengono metadati persistenti il cui ambito è esteso all'intero server.  
  
> [!IMPORTANT]  
>  Le tabelle di base di sistema sono utilizzate solo in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e non sono destinate all'utilizzo da parte di tutti gli utenti. Sono soggette a modifiche e la compatibilità non è garantita.  
  
## <a name="system-base-table-metadata"></a>Metadati della tabella di base di sistema  
 Un utente autorizzato che dispone dell'autorizzazione CONTROL, ALTER o VIEW DEFINITION per un database può visualizzare i metadati della tabella di base di sistema nella vista del catalogo **sys. Objects** . L'utente autorizzato può inoltre risolvere i nomi e gli ID oggetto delle tabelle di base di sistema utilizzando funzioni predefinite, ad esempio [object_name](../../t-sql/functions/object-name-transact-sql.md) e [object_id](../../t-sql/functions/object-id-transact-sql.md).  
  
 Per associare una tabella di base di sistema, un utente deve connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando la connessione amministrativa dedicata (DAC). Se si tenta di eseguire una query SELECT da una tabella di base di sistema senza connettersi tramite DAC, viene generato un errore.  
  
> [!IMPORTANT]  
>  L'accesso alle tabelle di base di sistema tramite DAC è progettato solo per il personale di [!INCLUDE[msCoName](../../includes/msconame-md.md)] e non è uno scenario di supporto per i clienti.  
  
## <a name="system-base-tables"></a>Tabelle di base di sistema  
 Nella tabella seguente viene indicata e descritta ogni un tabella di base sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Tabella di base|Descrizione|  
|----------------|-----------------|  
|**sys.sysschobjs**|Esiste in ogni database. Ogni riga rappresenta un oggetto del database.|  
|**sys.sysbinobjs**|Esiste in ogni database. Contiene una riga per ogni entità Service Broker del database. Le entità Service Broker includono gli elementi seguenti:<br /><br /> Tipo di messaggio<br /><br /> Contratto servizio<br /><br /> Servizio<br /><br /> I nomi e i tipi utilizzano regole di confronto binarie predefinite.|  
|**sys.sysclsobjs**|Esiste in ogni database. Contiene una riga per ogni entità classificata che condivide le stesse proprietà comuni che includono gli elementi seguenti:<br /><br /> Assembly<br /><br /> Dispositivo di backup<br /><br /> Catalogo full-text<br /><br /> Funzione di partizione<br /><br /> Schema di partizione<br /><br /> Gruppo di file<br /><br /> Chiave di offuscamento|  
|**sys.sysnsobjs**|Esiste in ogni database. Contiene una riga per ogni entità dell'ambito dello spazio dei nomi. Questa tabella viene utilizzata per archiviare le entità di raccolta XML.|  
|**sys.syscolpars**|Esiste in ogni database. Contiene una riga per ogni colonna di una tabella, vista o funzione con valori di tabella. Contiene anche righe per ogni parametro di una procedura o funzione.|  
|**sys.systypedsubobjs**|Esiste in ogni database. Contiene una riga per ogni sottoentità tipizzata. In questa categoria rientrano solo i parametri relativi alla funzione di partizione.|  
|**sys.sysidxstats**|Esiste in ogni database. Contiene una riga per ogni indice o statistica di tabelle e viste indicizzate<br /><br /> Nota: ogni indice (eccetto heap) è associato a una statistica con lo stesso nome dell'indice.|  
|**sys.sysiscols**|Esiste in ogni database. Contiene una riga per ogni indice persistente e colonna delle statistiche.|  
|**sys.sysscalartypes**|Esiste in ogni database. Contiene una riga per ogni tipo di sistema o tipo definito dall'utente.|  
|**sys.sysdbreg**|Esiste solo nel database **Master** . Contiene una riga per ogni database registrato.|  
|**sys.sysxsrvs**|Esiste solo nel database **Master** . Contiene una riga per ogni server locale, collegato o remoto.|  
|**sys.sysrmtlgns**|Questa tabella di base di sistema esiste solo nel database **Master** . Contiene una riga per ogni mapping di account di accesso remoto. Viene utilizzata per eseguire il mapping tra gli account di accesso in ingresso che risultano provenire da un server corrispondente e un account di accesso locale effettivo.|  
|**sys.syslnklgns**|Esiste solo nel database **Master** . Contiene una riga per ogni mapping di account di accesso collegato. I mapping di account di accesso collegati vengono utilizzati da chiamate di procedure remote e query distribuite che provengono da un server locale a un server collegato corrispondente.|  
|**sys.sysxlgns**|Esiste solo nel database **Master** . Contiene una riga per ogni entità di server.|  
|**sys.sysdbfiles**|Esiste in ogni database. Se la colonna **dbid** è zero, la riga rappresenta un file che appartiene a questo database. Nel database **Master** la colonna **dbid** può essere diversa da zero. In questo caso, la riga rappresenta un file master.|  
|**sys.sysusermsg**|Esiste solo nel database **Master** . Ogni riga rappresenta un messaggio di errore definito dall'utente.|  
|**sys.sysprivs**|Esiste in ogni database. Contiene una riga per ogni database o autorizzazione a livello di server.<br /><br /> Nota: le autorizzazioni a livello di server sono archiviate nel database **Master** .|  
|**sys.sysowners**|Esiste in ogni database. Ogni riga rappresenta un'entità di database.|  
|**sys.sysobjkeycrypts**|Esiste in ogni database. Contiene una riga per ogni chiave simmetrica, crittografia o proprietà crittografica associata a un oggetto.|  
|**sys.syscerts**|Esiste in ogni database. Contiene una riga per ogni certificato di un database.|  
|**sys.sysasymkeys**|Esiste in ogni database. Ogni riga rappresenta una chiave asimmetrica.|  
|**sys.ftinds**|Esiste in ogni database. Contiene una riga per ogni indice full-text del database.|  
|**sys.sysxprops**|Esiste in ogni database. Contiene una riga per ogni proprietà estesa.|  
|**sys.sysallocunits**|Esiste in ogni database. Contiene una riga per ogni unità di allocazione di archiviazione.|  
|**sys.sysrowsets**|Esiste in ogni database. Contiene una riga per ogni set di righe della partizione per un indice o un heap.|  
|**sys.sysrowsetrefs**|Esiste in ogni database. Contiene una riga per ogni riferimento di indice a un set di righe.|  
|**sys.syslogshippers**|Esiste solo nel database **Master** . Contiene una riga per ogni server di controllo del mirroring del database.|  
|**sys.sysremsvcbinds**|Esiste in ogni database. Contiene una riga per ogni associazione al servizio remoto.|  
|**sys.sysconvgroup**|Esiste in ogni database. Contiene una riga per ogni istanza di servizio di Service Broker.|  
|**sys.sysxmitqueue**|Esiste in ogni database. Contiene una riga per ogni coda di trasmissione di Service Broker.|  
|**sys.sysdesend**|Esiste in ogni database. Contiene una riga per ogni endpoint di invio di una conversazione di Service Broker.|  
|**sys.sysdercv**|Esiste in ogni database. Contiene una riga per ogni endpoint di ricezione di una conversazione di Service Broker.|  
|**sys.sysendpts**|Esiste solo nel database **Master** . Contiene una riga per ogni endpoint creato nel server.|  
|**sys.syswebmethods**|Esiste solo nel database **Master** . Contiene una riga per ogni metodo SOAP definito in un endpoint HTTP attivato per SOAP creato nel server.|  
|**sys.sysqnames**|Esiste in ogni database. Contiene una riga per ogni spazio dei nomi o nome completo a un token ID di 4 byte.|  
|**sys.sysxmlcomponent**|Esiste in ogni database. Ogni riga rappresenta un componente di XML Schema.|  
|**sys.sysxmlfacet**|Esiste in ogni database. Contiene una riga per ogni facet XML (restrizione) di definizione del tipo XML.|  
|**sys.sysxmlplacement**|Esiste in ogni database. Contiene una riga per ogni posizione XML dei componenti XML.|  
|**sys.syssingleobjrefs**|Esiste in ogni database. Contiene una riga per ogni riferimento N-a-1 generale.|  
|**sys.sysmultiobjrefs**|Esiste in ogni database. Contiene una riga per ogni riferimento N-a-N generale.|  
|**sys.sysobjvalues**|Esiste in ogni database. Contiene una riga per ogni proprietà del valore generale di un'entità.|  
|**sys.sysguidrefs**|Esiste in ogni database. Contiene una riga per ogni riferimento a ID classificati GUID.|  
  
## <a name="updating-system-base-tables"></a>Aggiornamento delle tabelle di base di sistema    
È possibile visualizzare i dati nelle tabelle di sistema tramite le viste del catalogo di sistema. Per aggiornare i metadati in una tabella di base di sistema, utilizzare l'interfaccia TSQL appropriata, ad esempio istruzioni DDL. Non è possibile aggiornare manualmente le tabelle di sistema. SQL Server segnala i messaggi seguenti quando si eseguono gli aggiornamenti diretti delle tabelle di sistema.

### <a name="a-system-table-is-manually-updated"></a>Una tabella di sistema viene aggiornata manualmente
Messaggio 17659: Avviso: la tabella di sistema con ID <id> è stata aggiornata direttamente nel database con ID <id> ed è possibile che non sia stata mantenuta la coerenza a livello della cache. È necessario riavviare SQL Server.

### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Avvio di un database con una tabella di sistema aggiornata manualmente
Messaggio 3859: avviso: il catalogo di sistema è stato aggiornato direttamente nel database con ID 17, più di recente in date_time.

### <a name="executing-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Esecuzione del comando DBCC_CHECKDB dopo l'aggiornamento manuale di una tabella di sistema
Messaggio 3859: avviso: il catalogo di sistema è stato aggiornato direttamente nel database con ID 17, più di recente in date_time.

Se si eseguono aggiornamenti manuali a una tabella di sistema e si verifica un problema, potrebbe essere richiesto di eseguire il ripristino da un backup o di copiare i dati dal database interessato in un nuovo database. Ulteriori informazioni sui [messaggi di errore dell'azione dell'utente](../errors-events/mssqlserver-8992-database-engine-error.md#user-action).