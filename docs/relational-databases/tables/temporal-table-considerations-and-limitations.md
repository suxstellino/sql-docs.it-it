---
description: Considerazioni e limitazioni delle tabelle temporali
title: Considerazioni e limitazioni delle tabelle temporali | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c8a21481-0f0e-41e3-a1ad-49a84091b422
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c39e0d3bc84bd469d599ada0ecd5884e37193a08
ms.sourcegitcommit: 5f9d682924624fe1e1a091995cd3a673605a4e31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98860924"
---
# <a name="temporal-table-considerations-and-limitations"></a>Considerazioni e limitazioni delle tabelle temporali


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Esistono alcune considerazioni e limitazioni da tenere presenti quando si lavora con le tabelle temporali, a causa della natura del controllo delle versioni di sistema.

Quando si usano le tabelle temporali, tenere presenti le considerazioni seguenti:

- Una tabella temporale deve avere una chiave primaria definita per correlare i record compresi tra la tabella corrente e la tabella di cronologia e la tabella di cronologia non può avere una chiave primaria definita.
- Le colonne periodo SYSTEM_TIME usate per registrare i valori **SysStartTime** e **SysEndTime** devono essere definite con un tipo di dati datetime2.
- Se durante la creazione della tabella di cronologia viene specificato il nome di una tabella di cronologia, è necessario specificare il nome tabella e il nome schema.
- Per impostazione predefinita, la tabella di cronologia è **PAGE** compresso.
- Se la tabella corrente è partizionata, la tabella di cronologia viene creata nel filegroup predefinito perché la configurazione del partizionamento non viene replicata automaticamente dalla tabella corrente nella tabella di cronologia.
- Le tabelle temporali e di cronologia non possono essere **FILETABLE** e possono contenere colonne di qualsiasi tipo di dati supportato diverso da **FILESTREAM** poiché **FILETABLE** e **FILESTREAM** consentono la manipolazione dei dati all'esterno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi non può essere garantito il controllo delle versioni di sistema.
- Non è possibile creare una tabella nodi o archi come tabella temporale o modificarla in tabella temporale.
- Mentre le tabelle temporali supportano i tipi di dati BLOB, ad esempio **(n)varchar(max)** , **varbinary(max)** , **(n)text** e **image**, a causa delle loro dimensioni non si potranno evitare costi di archiviazione significativi e implicazioni sulle prestazioni. Di conseguenza, quando si progetta il sistema è importante prestare attenzione mentre si usano questi tipi di dati.
- La tabella di cronologia deve essere creata nello stesso database della tabella corrente. L'esecuzione di query temporali su **Linked Server** non è supportata.
- La tabella di cronologia non può includere vincoli (chiave primaria, chiave esterna, vincoli di colonna o tabella).
- Le viste indicizzate non sono supportate per le query temporali (query che usano la clausola **FOR SYSTEM_TIME**).
- L'opzione online (**WITH (ONLINE = ON**) non ha alcun effetto su **ALTER TABLE ALTER COLUMN** nel caso di una tabella temporale con controllo delle versioni di sistema. La colonna ALTER non viene eseguita come online indipendentemente dal valore che è stato specificato per l'opzione ONLINE.
- Le istruzioni **INSERT** e **UPDATE** non possono fare riferimento a colonne periodo SYSTEM_TIME. Eventuali tentativi di inserire valori direttamente in tali colonne verranno bloccati.
- **TRUNCATE TABLE** non è supportata quando l'opzione **SYSTEM_VERSIONING** è impostata su **ON**
- La modifica diretta dei dati in una tabella di cronologia non è consentita.
- **ON DELETE CASCADE** e **ON UPDATE CASCADE** non sono consentiti nella tabella corrente. In altre parole, quando la tabella temporale fa riferimento alla tabella nella relazione di chiave esterna (corrispondente a *parent_object_id* in sys.foreign_keys) non sono consentite le opzioni CASCADE. Per risolvere questa limitazione, usare la logica dell'applicazione oppure i trigger AFTER per mantenere la coerenza in caso di eliminazione nella tabella di chiave primaria (corrispondente a *referenced_object_id* in sys.foreign_keys). Se la tabella di chiave primaria è temporale e la tabella di riferimento non lo è, questa limitazione non si applica.

  > [!NOTE]
  > questa limitazione si applica solo a SQL Server 2016. Le opzioni CASCADE sono supportate in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] e SQL Server 2017 a partire dalla versione CTP 2.0.

- Per non invalidare la logica DML, i trigger **INSTEAD OF** non sono consentiti né per la tabella corrente né per quella di cronologia. I trigger **AFTER** sono consentiti solo per la tabella corrente. Sono bloccati nella tabella di cronologia per evitare di invalidare la logica DML.
- L'utilizzo di tecnologie di replica è limitato:

  - **Always On:** supporto completo
  - **Change Data Capture e rilevamento modifiche:** Supportato solo nella tabella corrente
  - **Replica snapshot e transazionale**: supportata solo per un singolo server di pubblicazione senza attivazione di tabella temporale e per un sottoscrittore con attivazione di tabella temporale. In questo caso, il server di pubblicazione viene usato per un carico di lavoro OLTP, mentre il sottoscrittore viene usato per la ripartizione di report, inclusa l'esecuzione di query 'AS OF'. All'avvio dell'agente di distribuzione viene aperta una transazione che viene mantenuta aperta fino a quando l'agente di distribuzione non è interrotto. A causa di questo comportamento, SysStartTime e SysEndTime vengono popolati con l'ora di inizio della prima transazione avviata dall'agente di distribuzione. Di conseguenza, può essere preferibile eseguire l'agente di distribuzione in base a una pianificazione anziché usare il comportamento predefinito di esecuzione continua, se per l'applicazione o l'organizzazione è importante che SysStartTime e SysEndTime vengano popolati con un'ora vicina all'ora di sistema corrente. L'uso di più sottoscrittori non è supportato poiché potrebbe comportare dati temporali incoerenti a causa della dipendenza dall'orologio di sistema locale.
  - **Replica di tipo merge:** non supportata per le tabelle temporali

- Le query normali influiscono solo sui dati della tabella corrente. Per eseguire query sui dati della tabella di cronologia, è necessario usare le query temporali. Questo argomento verrà approfondito più avanti in questo documento nella sezione Esecuzione di query sui dati temporali.
- Una strategia di indicizzazione ottimale includerà un indice columnstore cluster e/o un indice rowstore con albero B nella tabella corrente, oltre a un indice columnstore cluster nella tabella di cronologia per dimensioni di archiviazione e prestazioni ottimali. Se si crea o si usa una tabella di cronologia propria, è consigliabile creare questo tipo di indice costituito da colonne periodo a partire dalla fine della colonna periodo; questo allo scopo di velocizzare non solo l'esecuzione di query temporali, ma anche le query incluse nella verifica di coerenza dei dati. La tabella di cronologia predefinita presenta un indice rowstore cluster creato in base alle colonne periodo (inizio, fine). Come minimo, è consigliabile un indice rowstore non cluster.
- Le proprietà o gli oggetti seguenti non vengono replicati dalla tabella corrente alla tabella di cronologia quando si crea quest'ultima:

  - Definizione di periodo
  - Definizione di identità
  - Indici
  - Statistiche
  - Vincoli CHECK
  - Trigger
  - Configurazione del partizionamento
  - Autorizzazioni
  - Predicati di sicurezza a livello di riga

- Una tabella di cronologia non può essere configurata come tabella corrente in una catena di tabelle di cronologia.

## <a name="see-also"></a>Vedere anche

- [Tabelle temporali](../../relational-databases/tables/temporal-tables.md)
- [Introduzione alle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Verifiche di coerenza del sistema della tabella temporale](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partizionamento con le tabelle temporali](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Sicurezza di una tabella temporale](../../relational-databases/tables/temporal-table-security.md)
- [Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Funzioni e viste per i metadati delle tabelle temporali](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
