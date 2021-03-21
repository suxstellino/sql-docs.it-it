---
description: Recupero di righe (provider OLE DB Native Client)
title: Recupero di righe (provider OLE DB Native Client) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- fetching rows
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- IRowset interface
- SQL Server Native Client OLE DB provider, fetching
ms.assetid: 5e6dbe36-b682-464d-adfa-8e886f9bd452
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4f86131da122b5c7266eeb8d23b02016f047ec13
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753531"
---
# <a name="fetching-rows-native-client-ole-db-provider"></a>Recupero di righe (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **IRowset** è l'interfaccia di base dei set di righe. L'interfaccia **IRowset** comprende metodi che consentono di recuperare le righe in modo sequenziale, ottenere dati dalle righe e gestire le righe. I consumer utilizzano i metodi disponibili in **IRowset** per tutte le operazioni di base relative ai set di righe, incluse quelle che consentono di recuperare e rilasciare le righe nonché ottenere i valori delle colonne.  
  
 Quando un consumer ottiene un puntatore di interfaccia su un set di righe, il primo passaggio consiste, in genere, nel determinare le funzionalità del set di righe mediante il metodo **IRowsetInfo::GetProperties**. In questo modo vengono restituite le informazioni sulle interfacce esposte dal set di righe nonché le funzionalità del set di righe che non vengono visualizzate come interfacce distinte, ad esempio il numero massimo di righe attive e il numero di righe che possono presentare aggiornamenti in sospeso contemporaneamente.  
  
 Il passaggio successivo per i consumer consiste nel determinare le caratteristiche, o metadati, delle colonne nel set di righe. A questo scopo, è necessario utilizzare il metodo **IColumnsInfo** per le informazioni sulle colonne semplici o il metodo **IColumnsRowset** per le informazioni sulle colonne estese. Il metodo **GetColumnInfo** restituisce le informazioni seguenti:  
  
-   Il numero di colonne nel set di risultati.  
  
-   Una matrice di strutture DBCOLUMNINFO, una per colonna.  
  
     L'ordine delle strutture corrisponde all'ordine in cui le colonne vengono visualizzate nel set di righe. Ogni struttura DBCOLUMNINFO include metadati di colonna, ad esempio nome della colonna, ordinale della colonna, lunghezza massima consentita per un valore nella colonna, tipo di dati della colonna, precisione e lunghezza.  
  
-   Il puntatore a un archivio per tutti i valori stringa all'interno di un singolo blocco di allocazione.  
  
 Il consumer determina le colonne necessarie dai metadati o in base al comando di testo che ha generato il set di righe. Determina inoltre gli ordinali delle colonne necessarie dall'ordinamento delle informazioni sulle colonne restituite da **IColumnsInfo** o dagli ordinali nel set di righe dei metadati di colonna restituito da **IColumnsRowset**.  
  
 Le interfacce **IColumnsInfo** e **IColumnsRowset** vengono utilizzate per estrarre informazioni sulle colonne nel set di righe. L'interfaccia **IColumnsInfo** restituisce un set limitato di informazioni, mentre **IColumnsRowset** fornisce tutti i metadati.  
  
> [!NOTE]  
>  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versione 7.0 e precedenti la colonna dei metadati facoltativa DBCOLUMN_COMPUTEMODE restituita da **IColumnsInfo::GetColumnsInfo** restituisce DBSTATUS_S_ISNULL, anziché i valori che descrivono se la colonna viene calcolata, perché non è possibile stabilire se la colonna sottostante viene calcolata.  
  
 Gli ordinali vengono utilizzati per specificare un'associazione a una colonna. Un'associazione è una struttura che associa un elemento della struttura del consumer a una colonna. Può inoltre associare il valore dei dati, la lunghezza e il valore dello stato della colonna.  
  
 Un set di associazioni viene raccolto in una funzione di accesso creata mediante il metodo **IAccessor::CreateAccessor**. Una funzione di accesso può contenere più associazioni in modo da consentire il recupero o l'impostazione dei dati per più colonne in una singola chiamata. Il consumer può creare diverse funzioni di accesso in base ai differenti modelli di utilizzo nelle varie parti dell'applicazione. Può creare e rilasciare funzioni di accesso mentre il set di righe è ancora attivo.  
  
 Per recuperare righe dal database, il consumer chiama un metodo, ad esempio **IRowset::GetNextRows** o **IRowsetLocate::GetRowsAt**. Queste operazioni di recupero inseriscono i dati delle righe dal server nel buffer di riga del provider. Il consumer non dispone di accesso diretto al buffer di riga del provider e utilizza **IRowset::GetData** per copiare i dati dal buffer del provider nel proprio buffer e **IRowsetChange::SetData** per copiare le modifiche dei dati dal proprio buffer nel buffer del provider.  
  
 Il consumer chiama il metodo **GetData** e passa l'handle a una riga, l'handle a una funzione di accesso e un puntatore a un buffer allocato dal consumer. **GetData** converte i dati e restituisce le colonne come specificato nelle associazioni utilizzate per creare la funzione di accesso. Il consumer può chiamare **GetData** più volte per una riga, utilizzando buffer e funzioni di accesso differenti, e può pertanto ottenere più copie degli stessi dati.  
  
 I dati delle colonne a lunghezza variabile possono essere utilizzati in diversi modi. Innanzitutto, tali colonne possono essere associate a una sezione limitata della struttura del consumer. Questo causa il troncamento quando la lunghezza dei dati supera la lunghezza del buffer. Per stabilire se il troncamento si è verificato, il consumer può controllare lo stato DBSTATUS_S_TRUNCATED. Poiché la lunghezza restituita è sempre la lunghezza effettiva in byte, il consumer può determinare anche la quantità di dati troncati.  
  
 Al termine del recupero o dell'aggiornamento, il consumer rilascia le righe mediante il metodo **ReleaseRows**. In questo modo vengono rilasciate le risorse dalla copia delle righe nel set di righe e si crea spazio per le nuove righe. Il consumer può quindi ripetere il ciclo di recupero o creazione di righe e di accesso ai dati in esse contenuti.  
  
 Al termine delle operazioni relative al set di righe, il consumer chiama il metodo **IAccessor::ReleaseAccessor** per rilasciare le funzioni di accesso e il metodo **IUnknown::Release** su tutte le interfacce esposte dal set di righe per rilasciare il set di righe. Quando il set di righe viene rilasciato, forza il rilascio delle eventuali righe o funzioni di accesso rimanenti ancora in gestione da parte del consumer.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [posizione del recupero successivo](../../relational-databases/native-client-ole-db-rowsets/fetching-rows-next-fetch-position.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Set di righe](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
