---
description: Set di righe e cursori SQL Server (provider OLE DB di Native Client)
title: Set di righe e cursori SQL Server (provider OLE DB di Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB rowsets, cursors
- SQL Server Native Client OLE DB provider, rowsets
- rowsets [OLE DB], cursors
- properties [OLE DB]
- cursors [OLE DB]
ms.assetid: 26a11e26-2a3a-451e-8f78-fba51e330ecb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3de88fa5e6e662836a55a715daa33cb5c102674e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476112"
---
# <a name="rowsets-and-sql-server-cursors-native-client-ole-db-provider"></a>Set di righe e cursori SQL Server (provider OLE DB di Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce i set di risultati ai consumer utilizzando due metodi.  
  
-   Set di risultati predefiniti che:  
  
    -   Riducono al minimo l'overhead.  
  
    -   Offrono prestazioni ottimali nel recupero dei dati.  
  
    -   Supportano solo la funzionalità predefinita del cursore forward-only di sola lettura.  
  
    -   Restituiscono righe al consumer una riga alla volta.  
  
    -   Supportano solo un'istruzione attiva alla volta in una connessione.  
  
         Dopo aver eseguito un'istruzione sulla connessione, non è possibile eseguirne altre fino all'annullamento dell'istruzione o al recupero di tutti i risultati da parte del consumer.  
  
    -   Supportano tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Cursori del server che:  
  
    -   Supportano tutte le funzionalità del cursore.  
  
    -   Possono restituire blocchi di righe al consumer.  
  
    -   Supportano più istruzioni attive in una sola connessione.  
  
    -   Raggiungono un equilibrio tra le prestazioni e le funzionalità del cursore.  
  
         Il supporto delle funzionalità del cursore può ridurre le prestazioni rispetto a un set di risultati predefinito. Per controbilanciare il problema, il consumer può utilizzare la funzionalità del cursore per recuperare un set di righe più piccolo.  
  
    -   Non supportano le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che restituiscono più di un singolo set di risultati.  
  
 I consumer possono richiedere comportamenti del cursore diversi in un set di righe attraverso l'impostazione di determinate proprietà specifiche del set di righe. Se il consumer non imposta una di queste proprietà del set di righe o le imposta tutte sui valori predefiniti, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa il set di righe utilizzando un set di risultati predefinito. Se una di queste proprietà è impostata su un valore diverso da quello predefinito, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa il set di righe utilizzando un cursore del server.  
  
 Le proprietà del set di righe seguenti impostano il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sull'utilizzo dei cursori [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Alcune proprietà possono essere combinate con altre senza rischi. Un set di righe che indica le proprietà DBPROP_IRowsetScroll e DBPROP_IRowsetChange, ad esempio, sarà un set di righe del segnalibro che esibisce un comportamento di aggiornamento immediato. Altre proprietà si escludono a vicenda. Un set di righe che esibisce DBPROP_OTHERINSERT, ad esempio, non può contenere segnalibri.  
  
|ID proprietà|valore|Comportamento del set di righe|  
|-----------------|-----------|---------------------|  
|DBPROP_SERVERCURSOR|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe è sequenziale e supporta solo lo scorrimento in avanti e il recupero. Il posizionamento relativo delle righe è supportato. Il testo del comando può contenere una clausola ORDER BY.|  
|DBPROP_CANSCROLLBACKWARDS o DBPROP_CANFETCHBACKWARDS|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta lo scorrimento e il recupero in entrambe le direzioni. Il posizionamento relativo delle righe è supportato. Il testo del comando può contenere una clausola ORDER BY.|  
|DBPROP_BOOKMARKS o DBPROP_LITERALBOOKMARKS|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe è sequenziale e supporta solo lo scorrimento in avanti e il recupero. Il posizionamento relativo delle righe è supportato. Il testo del comando può contenere una clausola ORDER BY.|  
|DBPROP_OWNUPDATEDELETE o DBPROP_OWNINSERT o DBPROP_OTHERUPDATEDELETE|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta lo scorrimento e il recupero in entrambe le direzioni. Il posizionamento relativo delle righe è supportato. Il testo del comando può contenere una clausola ORDER BY.|  
|DBPROP_OTHERINSERT|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta lo scorrimento e il recupero in entrambe le direzioni. Il posizionamento relativo delle righe è supportato. Il testo del comando può includere una clausola ORDER BY se nelle colonne di riferimento esiste un indice.<br /><br /> DBPROP_OTHERINSERT non può essere VARIANT_TRUE se il set di righe contiene segnalibri. Il tentativo di creare un set di righe con questi segnalibri e questa proprietà di visibilità causa un errore.|  
|DBPROP_IRowsetLocate o DBPROP_IRowsetScroll|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta lo scorrimento e il recupero in entrambe le direzioni. I segnalibri e il posizionamento assoluto tramite l'interfaccia **IRowsetLocate** sono supportati nel set di righe. Il testo del comando può contenere una clausola ORDER BY.<br /><br /> DBPROP_IRowsetLocate e DBPROP_IRowsetScroll richiedono segnalibri nel set di righe. Il tentativo di creare un set di righe con segnalibri e DBPROP_OTHERINSERT impostato su VARIANT_TRUE causa un errore.|  
|DBPROP_IRowsetChange o DBPROP_IRowsetUpdate|VARIANT_TRUE|È possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe è sequenziale e supporta solo lo scorrimento in avanti e il recupero. Il posizionamento relativo delle righe è supportato. Tutti i comandi che supportano i cursori aggiornabili possono supportare queste interfacce.|  
|DBPROP_IRowsetLocate o DBPROP_IRowsetScroll e DBPROP_IRowsetChange o DBPROP_IRowsetUpdate|VARIANT_TRUE|È possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta lo scorrimento e il recupero in entrambe le direzioni. I segnalibri e il posizionamento assoluto tramite l'interfaccia **IRowsetLocate** sono supportati nel set di righe. Il testo del comando può contenere una clausola ORDER BY.|  
|DBPROP_IMMOBILEROWS|VARIANT_FALSE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta solo lo scorrimento in avanti. Il posizionamento relativo delle righe è supportato. Il testo del comando può includere una clausola ORDER BY se nelle colonne di riferimento esiste un indice.<br /><br /> DBPROP_IMMOBILEROWS è disponibile solo in set di righe che possono mostrare righe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inserite dai comandi in altre sessioni o da altri utenti. Il tentativo di aprire un set di righe con la proprietà impostata su VARIANT_FALSE in qualsiasi set di righe per il quale DBPROP_OTHERINSERT non può essere VARIANT_TRUE genera un errore.|  
|DBPROP_REMOVEDELETED|VARIANT_TRUE|Non è possibile aggiornare i dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite il set di righe. Il set di righe supporta solo lo scorrimento in avanti. Il posizionamento relativo delle righe è supportato. Il testo del comando può contenere una clausola ORDER BY a meno che non sia vincolato da un'altra proprietà.|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]È possibile creare facilmente un set di righe del provider di OLE DB di Native Client supportato da un cursore del server in una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vista o una tabella di base tramite il metodo **IOpenRowset:: OPENROWSET** . Specificare la tabella o la vista in base al nome, passando i set di proprietà del set di righe richiesti nel parametro *rgPropertySets*.  
  
 Il testo del comando che crea un set di righe è limitato quando il consumer prevede che il set di righe sia supportato da un cursore del server. In particolare, il testo del comando è limitato a una singola istruzione SELECT che restituisce un solo risultato del set di righe o a una stored procedure che implementa una singola istruzione SELECT che restituisce un solo risultato del set di righe.  
  
 Nelle due tabelle vengono illustrati i mapping di varie proprietà OLE DB e i modelli di cursore. Vengono mostrate inoltre le proprietà del set di righe da impostare per utilizzare un determinato tipo di modello di cursore.  
  
 Ogni cella nella tabella contiene un valore della proprietà del set di righe per il modello di cursore specifico. Il tipo di dati di tutte le proprietà del set di righe elencate in precedenza in questo argomento è VT_BOOL e il valore predefinito è VARIANT_FALSE. Nella tabella seguente vengono utilizzati i simboli seguenti.  
  
 F = valore predefinito (VARIANT_FALSE)  
  
 T = VARIANT_TRUE  
  
 \- = VARIANT_TRUE o VARIANT_FALSE  
  
 Per utilizzare un determinato tipo di modello di cursore, individuare la colonna che corrisponde al modello desiderato e trovare tutte le proprietà del set di righe con valore 'T' nella colonna. Impostare queste proprietà del set di righe su VARIANT_TRUE per utilizzare il modello di cursore specifico. Le proprietà del set di righe con '-' come valore possono essere impostate su VARIANT_TRUE o VARIANT_FALSE.  
  
|Proprietà del set di righe/modelli di cursore|Predefinito<br /><br /> result<br /><br /> set<br /><br /> (RO)|Veloce<br /><br /> forward-<br /><br /> only<br /><br /> (RO)|Statico<br /><br /> (RO)|Keyset<br /><br /> keyset<br /><br /> (RO)|  
|--------------------------------------|-------------------------------------------|--------------------------------------------|-----------------------|----------------------------------|  
|DBPROP_SERVERCURSOR|F|T|T|T|  
|DBPROP_DEFERRED|F|F|-|-|  
|DBPROP_IrowsetChange|F|F|F|F|  
|DBPROP_IrowsetLocate|F|F|-|-|  
|DBPROP_IrowsetScroll|F|F|-|-|  
|DBPROP_IrowsetUpdate|F|F|F|F|  
|DBPROP_BOOKMARKS|F|F|-|-|  
|DBPROP_CANFETCHBACKWARDS|F|F|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|F|F|-|-|  
|DBPROP_CANHOLDROWS|F|F|-|-|  
|DBPROP_LITERALBOOKMARKS|F|F|-|-|  
|DBPROP_OTHERINSERT|F|T|F|F|  
|DBPROP_OTHERUPDATEDELETE|F|T|F|T|  
|DBPROP_OWNINSERT|F|T|F|T|  
|DBPROP_OWNUPDATEDELETE|F|T|F|T|  
|DBPROP_QUICKSTART|F|F|-|-|  
|DBPROP_REMOVEDELETED|F|F|F|-|  
|DBPROP_IrowsetResynch|F|F|F|-|  
|DBPROP_CHANGEINSERTEDROWS|F|F|F|F|  
|DBPROP_SERVERDATAONINSERT|F|F|F|-|  
|DBPROP_UNIQUEROWS|-|F|F|F|  
|DBPROP_IMMOBILEROWS|-|-|-|T|  
  
|Proprietà del set di righe/modelli di cursore|Dinamico (RO)|Keyset (L/S)|Dinamico (L/S)|  
|--------------------------------------|--------------------|---------------------|----------------------|  
|DBPROP_SERVERCURSOR|T|T|T|  
|DBPROP_DEFERRED|-|-|-|  
|DBPROP_IrowsetChange|F|-|-|  
|DBPROP_IrowsetLocate|F|-|F|  
|DBPROP_IrowsetScroll|F|-|F|  
|DBPROP_IrowsetUpdate|F|-|-|  
|DBPROP_BOOKMARKS|F|-|F|  
|DBPROP_CANFETCHBACKWARDS|-|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|-|-|-|  
|DBPROP_CANHOLDROWS|F|-|F|  
|DBPROP_LITERALBOOKMARKS|F|-|F|  
|DBPROP_OTHERINSERT|T|F|T|  
|DBPROP_OTHERUPDATEDELETE|T|T|T|  
|DBPROP_OWNINSERT|T|T|T|  
|DBPROP_OWNUPDATEDELETE|T|T|T|  
|DBPROP_QUICKSTART|-|-|-|  
|DBPROP_REMOVEDELETED|T|-|T|  
|DBPROP_IrowsetResynch|-|-|-|  
|DBPROP_CHANGEINSERTEDROWS|F|-|F|  
|DBPROP_SERVERDATAONINSERT|F|-|F|  
|DBPROP_UNIQUEROWS|F|F|F|  
|DBPROP_IMMOBILEROWS|F|T|F|  
  
 Per un determinato set di proprietà del set di righe, il modello di cursore selezionato viene determinato come segue.  
  
 Dalla raccolta specificata di proprietà del set di righe, ottenere un subset di proprietà elencate nelle tabelle precedenti. Dividere queste proprietà in due sottogruppi a seconda del valore del flag, obbligatorio (T, F) o facoltativo (-), di ogni proprietà del set di righe. Per ogni modello di cursore, iniziare dalla prima tabella e spostarsi da sinistra a destra. Confrontare i valori delle proprietà nei due sottogruppi e i valori delle proprietà corrispondenti nella colonna. Verrà selezionato il modello di cursore in cui corrispondono include tutte le proprietà obbligatorie e con il minor numero di proprietà facoltative non corrispondenti. Se è disponibile più di un modello di cursore, verrà scelto quello all'estrema sinistra.  
  
## <a name="sql-server-cursor-block-size"></a>Dimensioni del blocco del cursore di SQL Server  
 Quando un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cursore supporta un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] set di righe del provider OLE DB di Native client, il numero di elementi nel parametro della matrice di handle di riga dei metodi **IRowset:: GetNextRows** o **IRowsetLocate:: GetRowsAt** definisce la dimensione del blocco del cursore. Le righe indicate dagli handle nella matrice rappresentano i membri del blocco del cursore.  
  
 Per i set di righe che supportano i segnalibri, gli handle di riga recuperati tramite il metodo **IRowsetLocate::GetRowsByBookmark** definiscono i membri del blocco del cursore.  
  
 Indipendentemente dal metodo utilizzato per popolare il set di righe e formare il blocco del cursore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il blocco del cursore è attivo fino all'esecuzione del metodo di recupero righe successivo sul set di righe.  
  
## <a name="see-also"></a>Vedere anche  
 [Set di righe](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
