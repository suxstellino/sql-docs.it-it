---
description: Esportazione di un inventario di accesso (AccessToSQL)
title: Esportazione di un inventario di accesso (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, exporting metadata
- exporting
- exporting Access metadata
- exporting, Access metadata
- exporting, querying exported metadata
- inventories of Access databases
- querying exported metadata
ms.assetid: 7e1941fb-3d14-4265-aff6-c77a4026d0ed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 10bff5f86bec26994c143195fcbb97aa30ba709e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044751"
---
# <a name="exporting-an-access-inventory-accesstosql"></a>Esportazione di un inventario di accesso (AccessToSQL)
Se si dispone di più database di Access e non si è certi di quali eseguire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la migrazione, è possibile esportare un inventario di tutti i database di Access in un progetto. È quindi possibile esaminare ed eseguire una query sui metadati di inventario per determinare quali database e oggetti all'interno di tali database migrare. Questo inventario consente di trovare rapidamente risposte a domande, come le seguenti:  
  
-   Quali sono i database più grandi?  
  
-   Chi appartiene alla maggior parte dei database?  
  
-   Quali database contengono le stesse tabelle?  
  
-   Quali database non sono stati modificati negli ultimi sei mesi?  
  
-   Quali database contengono informazioni private?  
  
Alla fine di questo argomento vengono forniti esempi di query utilizzati per rispondere a queste domande.  
  
## <a name="exported-metadata"></a>Metadati esportati  
SSMA Esporta i metadati relativi a database, tabelle, colonne, indici, chiavi esterne, query, report, moduli, macro e moduli di Access. I metadati relativi a ognuna di queste categorie di elementi vengono esportati in una tabella separata. Per gli schemi di queste tabelle, vedere [accedere agli schemi di inventario](access-inventory-schemas-accesstosql.md).  
  
## <a name="exporting-inventory-data"></a>Esportazione dei dati di inventario  
Per esportare un inventario di accesso, è innanzitutto necessario aprire o creare un progetto SSMA, quindi aggiungere il database di Access che si desidera analizzare. Dopo l'aggiunta di database a un progetto SSMA, i metadati relativi a tali database vengono esportati in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database e uno schema specifici. Se necessario, SSMA crea tabelle per archiviare i metadati. SSMA aggiunge quindi i metadati relativi ai database di Access al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
> [!NOTE]  
> Un database di Access può essere suddiviso in più file: un database back-end che contiene tabelle e database front-end che contengono query, moduli, report, macro, moduli e collegamenti. Se si desidera eseguire la migrazione di un database suddiviso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , aggiungere il database front-end a SSMA.  
  
Nelle istruzioni seguenti viene descritto come creare un progetto, aggiungere database al progetto, connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quindi esportare i dati di inventario.  
  
**Per creare un progetto**  
  
1.  Aprire SSMA per Access.  
  
2.  Scegliere **Nuovo progetto** dal menu **File**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
3.  Nella casella **nome** immettere un nome per il progetto.  
  
4.  Nella casella **percorso** immettere o selezionare una cartella per il progetto.  
  
5.  Nella casella combinata **Esegui migrazione a** selezionare la versione di destinazione di cui si desidera eseguire la migrazione e quindi fare clic su **OK**.  
  
Per ulteriori informazioni sulla creazione di progetti, vedere [creazione e gestione di progetti](creating-and-managing-projects-accesstosql.md).  
  
**Per trovare e aggiungere database**  
  
1.  Scegliere **trova database** dal menu **file** .  
  
2.  Nella procedura guidata trova database immettere l'unità, il percorso del file o il percorso UNC in cui si desidera eseguire la ricerca. In alternativa, fare clic su **Sfoglia** per selezionare l'unità o la cartella di rete.  
  
3.  Fare clic su **Aggiungi** per aggiungere il percorso alla casella di riepilogo.  
  
    Ripetere i due passaggi precedenti per aggiungere ulteriori percorsi di ricerca.  
  
4.  Facoltativamente, aggiungere i criteri di ricerca per ridefinire l'elenco dei database restituiti.  
  
    > [!IMPORTANT]  
    > La casella **di testo nome file o parte intera** non supporta caratteri jolly.  
  
5.  Fai clic su **Analisi**.  
  
    Viene visualizzata la pagina di analisi. Vengono visualizzati i database che sono stati trovati e lo stato di avanzamento della ricerca. Per arrestare la ricerca, fare clic su **Arresta**.  
  
6.  Nella pagina Seleziona file selezionare tutti i database che si desidera aggiungere al progetto.  
  
    È possibile utilizzare i pulsanti **Seleziona tutto** e **Cancella tutto** nella parte superiore dell'elenco per selezionare o deselezionare tutti i database. È anche possibile tenere premuto il tasto CTRL per selezionare più righe oppure tenere premuto MAIUSC per selezionare un intervallo di righe.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina verifica fare clic su **fine**.  
  
Per ulteriori informazioni sull'aggiunta di database ai progetti, vedere [aggiunta e rimozione di file di database di Access](adding-and-removing-access-database-files-accesstosql.md).  
  
**Per eseguire la connessione a SQL Server**  
  
1.  Scegliere **Connetti a SQL Server** dal menu **file** .  
  
2.  Nella finestra di dialogo connessione immettere o selezionare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Se ci si connette all'istanza predefinita nel computer locale, è possibile immettere **localhost** o un punto (**.**).  
  
    -   Se ci si connette all'istanza predefinita in un altro computer, immettere il nome del computer.  
  
    -   Se ci si connette a un'istanza denominata, immettere il nome del computer, una barra rovesciata e il nome dell'istanza. Ad esempio: MyServer\MyInstance.  
  
3.  Nella casella **database** immettere il nome del database di destinazione per i metadati esportati.  
  
4.  Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata in modo da accettare connessioni su una porta non predefinita, immettere il numero di porta utilizzato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le connessioni nella casella **porta server** . Per l'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il numero di porta predefinito è 1433. Per le istanze denominate, SSMA tenta di ottenere il numero di porta dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser.  
  
5.  Nel menu a discesa **autenticazione** selezionare il tipo di autenticazione da utilizzare per la connessione. Per utilizzare l'account di Windows corrente, selezionare **autenticazione di Windows**. Per usare un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso, selezionare **SQL Server autenticazione** e quindi specificare un nome utente e una password.  
  
Per ulteriori informazioni sulla connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere la pagina relativa [alla connessione a SQL Server &#40;AccessToSQL&#41;](../../ssma/access/connecting-to-sql-server-accesstosql.md).  
  
**Per esportare le informazioni di inventario**  
  
1.  In Esplora metadati di Access espandere **Access-metabase**.  
  
2.  Selezionare la casella di controllo accanto a **database**.  
  
    Per omettere singoli database o oggetti di database, espandere la cartella **database** , quindi deselezionare la casella di controllo accanto al database o all'oggetto di database.  
  
3.  Fare clic con il pulsante destro del mouse su **database** e selezionare **Esporta schema**.  
  
4.  Nella finestra di dialogo **Seleziona schema per esportazione** selezionare lo schema di destinazione per i metadati esportati e quindi fare clic su **OK**.  
  
Ogni volta che si esportano i metadati, SSMA accoda i dati all'inventario. I dati esistenti nell'inventario non vengono aggiornati o eliminati.  
  
## <a name="querying-the-exported-metadata"></a>Esecuzione di query sui metadati esportati  
Dopo aver esportato i metadati relativi ai database di Access, è possibile eseguire una query sui metadati. Nelle istruzioni seguenti viene descritto come utilizzare la finestra dell'editor di query in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per eseguire le query.  
  
**Per eseguire una query sui metadati**  
  
1.  Dal menu **Start** scegliere **tutti i programmi**, **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005** o Microsoft **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008** o Microsoft **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012**, quindi fare clic su **SQL Server Management Studio**.  
  
2.  Nella finestra di dialogo **Connetti al server** verificare le impostazioni e quindi fare clic su **Connetti**.  
  
3.  Sulla barra degli strumenti Management Studio fare clic su **nuova query** per aprire l'editor di query.  
  
4.  Nella finestra dell'editor di query immettere una query. Alcuni esempi sono illustrati nella sezione seguente.  
  
5.  Premere il tasto F5 per eseguire la query.  
  
## <a name="query-examples"></a>Esempi di query  
Prima di eseguire una delle query seguenti, è consigliabile eseguire una query USE *database_name* per verificare che le query vengano eseguite sul database contenente i metadati esportati. Se, ad esempio, i metadati sono stati esportati in un database denominato MyAccessMetadata, aggiungere quanto segue all'inizio del [!INCLUDE[tsql](../../includes/tsql-md.md)] codice:  
  
```  
USE MyAccessMetadata;  
GO  
```  
Negli esempi seguenti viene usato lo schema **dbo** . Se i metadati sono stati esportati in un altro schema, assicurarsi di modificare lo schema quando si eseguono tali query.  
  
### <a name="what-tables-and-columns-are-in-these-databases"></a>Quali tabelle e colonne si trovano in questi database?  
La query seguente unisce le tabelle che contengono i metadati della colonna, della tabella e del database, quindi restituisce i nomi di tutti i database, le tabelle e le colonne, ordinati in base al nome della colonna:  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D.DatabaseId  
ORDER BY ColumnName;  
```  
  
### <a name="what-are-the-largest-databases"></a>Quali sono i database più grandi?  
La query seguente restituisce il nome del database, le dimensioni del file e il numero di tabelle in ogni database di Access, ordinate in base alle dimensioni del file:  
  
```  
SELECT DatabaseName, FileSize, TablesCount  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileSize DESC;  
```  
  
### <a name="who-is-the-owner-of-most-of-the-databases"></a>Chi è il proprietario della maggior parte dei database?  
La query seguente restituisce il nome del database e il proprietario di ogni database di Access, ordinati in base al proprietario.  
  
```  
SELECT DatabaseName, FileOwner  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileOwner;  
```  
  
### <a name="which-databases-contain-the-same-tables"></a>Quali database contengono le stesse tabelle?  
Nella query seguente viene utilizzata una sottoquery per trovare tutti i nomi di tabella visualizzati più di una volta nell'elenco di tabelle, quindi viene utilizzato l'elenco di tabelle per ottenere il nome del database. I risultati vengono restituiti come nome del database e quindi il nome della tabella e sono ordinati in base al nome della tabella.  
  
```  
SELECT DatabaseName, TableName   
FROM dbo.SSMA_Access_InventoryTables T  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON D.DatabaseId = T.DatabaseId  
WHERE TableName IN (  
SELECT TableName   
FROM dbo.SSMA_Access_InventoryTables  
GROUP BY TableName   
HAVING count(*)>1  
)  
ORDER BY TableName;  
```  
  
### <a name="which-databases-were-not-modified-in-the-last-six-months"></a>Quali database non sono stati modificati negli ultimi sei mesi?  
La query seguente ottiene la data corrente, ottiene il valore del mese per sei mesi fa, quindi restituisce un elenco di database con una data modificata maggiore di sei mesi fa.  
  
```  
SELECT DatabaseName, DateModified  
FROM dbo.SSMA_Access_InventoryDatabases  
WHERE DATEDIFF(month, DateModified, GETDATE()) > 6  
ORDER BY DateModified;  
```  
  
### <a name="which-databases-contain-private-information"></a>Quali database contengono informazioni private?  
I database di Access possono contenere informazioni riservate o personali. Potrebbe essere necessario spostare questi database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per sfruttare le funzionalità di sicurezza. Se si è certi che le colonne che contengono dati sensibili hanno un nome specifico o contengono caratteri specifici, è possibile usare una query per trovare tutte le colonne che contengono tali informazioni. È possibile, ad esempio, trovare tutte le colonne che includono la stringa "Salary".  La query restituisce quindi il nome del database, il nome della tabella e il nome della colonna.  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D. DatabaseId  
WHERE ColumnName LIKE '%salary%';  
```  
Se non si conosce il nome della colonna, è possibile scrivere una query per restituire tutte le colonne. A tale scopo, rimuovere la clausola WHERE dalla query precedente.  
  
## <a name="see-also"></a>Vedere anche  
[Preparazione dei database di Access per la migrazione](preparing-access-databases-for-migration-accesstosql.md)  
  
