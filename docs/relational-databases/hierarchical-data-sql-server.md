---
description: Dati gerarchici [SQL Server]
title: Dati gerarchici (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 10/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [SQL Server], tables to support
- hierarchyid [Database Engine], concepts
- hierarchical tables [Database Engine]
- SqlHierarchyId
- hierarchyid [Database Engine]
- hierarchical queries [SQL Server], using hierarchyid data type
ms.assetid: 19aefa9a-fbc2-4b22-92cf-67b8bb01671c
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7fcefb597fe97e90bfda78fe934c83646414c23f
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611181"
---
# <a name="hierarchical-data-sql-server"></a>Dati gerarchici [SQL Server]

[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

  Con il tipo di dati **hierarchyid** predefinito, l'archiviazione e l'esecuzione di query sui dati gerarchici risultano più semplici. **hierarchyid** è ottimizzato per la rappresentazione di alberi, ovvero il tipo più comune di dati gerarchici.  
  
 I dati gerarchici vengono definiti come un set di elementi di dati correlati tra loro tramite relazioni gerarchiche. Si parla di relazioni gerarchiche quando un elemento di dati è l'elemento padre di un altro elemento. Di seguito sono riportati alcuni esempi dei dati gerarchici comunemente archiviati nei database:  
  
-   Una struttura organizzativa  
  
-   Un file system  
  
-   Un set di attività di un progetto  
  
-   Una tassonomia di termini del linguaggio  
  
-   Un grafico di collegamenti tra pagine Web  
  
 Usare [hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) come tipo di dati per creare tabelle con una struttura gerarchica o per descrivere la struttura gerarchica dei dati archiviati in un altro percorso. Usare le [funzioni hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) di [!INCLUDE[tsql](../includes/tsql-md.md)] per eseguire query e gestire i dati gerarchici.  
  
##  <a name="key-properties-of-hierarchyid"></a><a name="keyprops"></a> Proprietà chiave di hierarchyid  
 Un valore del tipo di dati **hierarchyid** rappresenta una posizione in un albero gerarchico. I valori per **hierarchyid** hanno le proprietà seguenti:  
  
-   Estremamente compresso  
  
     Il numero medio di bit richiesto per rappresentare un nodo in un albero con *n* nodi dipende dal fanout medio, ovvero il numero medio di elementi figlio di un nodo. Per i fanout piccoli (0-7), la dimensione è approssimativamente 6\*logA *n* bit, dove A è il fanout medio. Un nodo in una gerarchia organizzativa di 100.000 persone con un fanout medio di 6 livelli richiede circa 38 bit. Viene arrotondato a 40 bit, o 5 byte, per l'archiviazione.  
  
-   Il confronto avviene in ordine di scorrimento in profondità  
  
     Dati due valori **hierarchyid****a** e **b**, **a<b** indica che a precede b nell'attraversamento del primo livello di profondità dell'albero. Gli indici sui tipi di dati **hierarchyid** sono in ordine di scorrimento della profondità e i nodi l'uno vicino all'altro nell'attraversamento del primo livello di profondità della struttura sono archiviati l'uno vicino all'altro. Ad esempio, i figli di un record sono archiviati adiacenti al record specifico.  
  
-   Supporto per eliminazioni e inserimenti arbitrari  
  
     Usando il metodo [GetDescendant](../t-sql/data-types/getdescendant-database-engine.md) , è sempre possibile generare un elemento di pari livello a destra o a sinistra di un nodo oppure tra due elementi di pari livello. La proprietà del confronto è gestita quando un numero arbitrario di nodi viene inserito o eliminato dalla gerarchia. La maggior parte degli inserimenti e delle eliminazioni mantiene la proprietà di compattezza. Tuttavia, gli inserimenti tra due nodi produrranno i valori hierarchyid con una rappresentazione leggermente meno compatta.  
  
  
##  <a name="limitations-of-hierarchyid"></a><a name="limits"></a> Limitazioni di hierarchyid  
 Al tipo di dati **hierarchyid** vengono applicate le limitazioni seguenti:  
  
-   Una colonna di tipo **hierarchyid** non rappresenta automaticamente un albero. È compito dell'applicazione generare e assegnare i valori **hierarchyid** in maniera tale che la relazione desiderata tra le righe sia riflessa nei valori. In alcune applicazioni potrebbe essere presente una colonna di tipo **hierarchyid** in cui viene indicato il percorso in una gerarchia definita in un'altra tabella.  
  
-   La concorrenza nella generazione e nell'assegnazione dei valori **hierarchyid** deve essere gestita dall'applicazione. Non c'è garanzia che i valori **hierarchyid** di una colonna siano univoci a meno che l'applicazione usi un vincolo della chiave univoca o applichi univocità stessa tramite la logica.  
  
-   Le relazioni gerarchiche rappresentate dai valori **hierarchyid** non sono applicate come una relazione della chiave esterna. È possibile e qualche volta appropriato avere una relazione gerarchica dove A ha un figlio B, A viene eliminato e lascia a B una relazione con un record inesistente. Se questo comportamento è inaccettabile, tramite l'applicazione deve essere eseguita una query per i discendenti prima di eliminare gli elementi padre.  
  
  
##  <a name="when-to-use-alternatives-to-hierarchyid"></a><a name="alternatives"></a> Quando utilizzare le alternative a hierarchyid  
 Di seguito sono elencate due alternative a **hierarchyid** per la rappresentazione di dati gerarchici:  
  
-   Elemento padre/figlio  
  
-   XML  
  
 **hierarchyid** è generalmente superiore a queste alternative. Tuttavia, esistono determinate situazioni illustrate in dettaglio di seguito in cui le alternative sono superiori.  
  
### <a name="parentchild"></a>Elemento padre/figlio  
 Quando si utilizza l'approccio dell'elemento padre/figlio, ogni riga contiene un riferimento all'elemento padre. La tabella seguente definisce una tabella tipica utilizzata per contenere le righe padre e figlio in una relazione padre/figlio:  
  
```sql
USE AdventureWorks2012 ;  
GO  
  
CREATE TABLE ParentChildOrg  
   (  
    BusinessEntityID int PRIMARY KEY,  
    ManagerId int REFERENCES ParentChildOrg(BusinessEntityID),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
```  
  
 Confronto padre/figlio e **hierarchyid** per operazioni comuni  
  
-   Le query del sottoalbero sono notevolmente più veloci con **hierarchyid**.  
  
-   Le query discendenti dirette sono leggermente più lente con **hierarchyid**.  
  
-   Lo spostamento di nodi non foglia è più lento con **hierarchyid**.  
  
-   L'inserimento di nodi non foglia e l'inserimento o lo spostamento di nodi foglia sono caratterizzati dalla stessa complessità con **hierarchyid**.  
  
 La relazione elemento padre/figlio potrebbe essere superiore quando esistono le condizioni seguenti:  
  
-   La dimensione della chiave è importante. Per lo stesso numero di nodi, un valore **hierarchyid** è uguale a o maggiore di un valore della famiglia Integer (**smallint**, **int**, **bigint**). Questo è solo uno dei motivi per cui usare la relazione padre/figlio in casi rari, poiché **hierarchyid** si colloca meglio nell'I/O e nella complessità della CPU che nelle espressioni della tabella comune richieste quando si usa la struttura padre/figlio.  
  
-   Le query vengono eseguite raramente sulle sezioni della gerarchia. In altre parole, le query di solito vengono eseguite solo su un singolo punto della gerarchia. In questi casi la condivisione percorso non è importante. Ad esempio, la relazione elemento padre/figlio è superiore se la tabella dell'organizzazione viene utilizzata solo per l'elaborazione del libro paga per i singoli dipendenti.  
  
-   I sottoalberi non-foglia vengono spostati frequentemente e le prestazioni sono molto importanti. In una rappresentazione padre/figlio, la modifica del percorso di una riga in una gerarchia influisce su una sola riga. La modifica del percorso di una riga in un uso di **hierarchyid** influisce su *n* righe, dove *n* è il numero di nodi del sottoalbero spostato.  
  
     Se i sottoalberi non-foglia vengono spostati frequentemente e le prestazioni sono importanti, ma la maggior parte degli spostamenti è a un livello ben definito della gerarchia, suddividere i livelli superiori e inferiori in due gerarchie. In questo modo, tutti gli spostamenti si verificano a livello di foglia nella gerarchia più elevata. Ad esempio, considerare una gerarchia di siti Web ospitata da un servizio. I siti contengono molte pagine disposte in modo gerarchico. I siti ospitati potrebbero essere spostati in altri percorsi nella gerarchia del sito, tuttavia le pagine subordinate vengono raramente disposte in modo nuovo. Questo potrebbe essere rappresentato tramite:  
  
    ```sql
    CREATE TABLE HostedSites   
       (  
        SiteId hierarchyid, PageId hierarchyid  
       ) ;  
    GO  
    ```  
  
  
### <a name="xml"></a>XML  
 Un documento XML è un albero e pertanto una singola istanza del tipo di dati XML può rappresentare una gerarchia completa. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] quando viene creato un indice XML, i valori **hierarchyid** vengono usati internamente per rappresentare la posizione nella gerarchia.  
  
 L'utilizzo del tipo di dati XML può essere superiore quando tutte le seguenti condizioni sono vere:  
  
-   La gerarchia completa viene sempre archiviata e recuperata.  
  
-   I dati vengono utilizzati in un formato XML dall'applicazione.  
  
-   Le ricerche del predicato sono estremamente limitate e non sono importanti per la prestazione.  
  
 Ad esempio, se tramite un'applicazione vengono rilevate più organizzazioni, viene sempre archiviata e recuperata la gerarchia organizzativa completa e non viene eseguita alcuna query su una singola organizzazione, potrebbe essere utile una tabella con il formato seguente:  
  
```sql
CREATE TABLE XMLOrg   
    (  
    Orgid int,  
    Orgdata xml  
    ) ;  
GO  
```  
  
  
##  <a name="indexing-strategies-for-hierarchical-data"></a><a name="indexing"></a> Strategie di indicizzazione per i dati gerarchici  
 Sono disponibili due strategie per indicizzare i dati gerarchici:  
  
-   **Depth-first**  
  
     In un indice depth-first le righe di un sottoalbero vengono archiviate le une accanto alle altre. Ad esempio, tutti i dipendenti che riportano a un responsabile vengono archiviati accanto al record dei responsabili.  
  
     In un indice depth-first tutti i nodi del sottoalbero di un nodo vengono posizionati insieme. Gli indici depth-first sono pertanto utili per rispondere alle query sui sottoalberi, ad esempio "Trova tutti i file in questa cartella e nelle relative sottocartelle".  
  
-   **Breadth-first**  
  
     Un indice con larghezza-primo archivia le righe di ogni livello della gerarchia insieme. Ad esempio, i record dei dipendenti che riportano direttamente allo stesso responsabile vengono archiviati gli uni accanto agli altri.  
  
     In un indice breadth-first tutti gli elementi figlio diretti di un nodo vengono posizionati insieme. Pertanto, gli indici breadth-first sono in grado di fornire risposte alle query sugli elementi figlio immediati, ad esempio "Trova tutti i dipendenti che riportano direttamente a questo responsabile".  
  
 La scelta tra depth-first, breadth-first o entrambi e la selezione di uno di questi come chiave di clustering (se disponibile) dipendono dall'importanza relativa dei tipi di query riportati in precedenza e dall'importanza relativa delle operazioni SELECT e DML. Per un esempio dettagliato delle strategie di indicizzazione, vedere [Esercitazione: Utilizzo del tipo di dati hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md).  
  
  
### <a name="creating-indexes"></a>Creazione di indici  
 Il metodo GetLevel() può essere usato per creare un ordine breadth-first. Nell'esempio seguente vengono creati sia gli indici breadth-first sia quelli depth-first:  
  
```sql
USE AdventureWorks2012 ;   -- wmimof
GO  
  
CREATE TABLE Organization  
   (  
    BusinessEntityID hierarchyid,  
    OrgLevel as BusinessEntityID.GetLevel(),   
    EmployeeName nvarchar(50) NOT NULL  
   ) ;  
GO  
  
CREATE CLUSTERED INDEX Org_Breadth_First   
    ON Organization(OrgLevel,BusinessEntityID) ;  
GO  
  
CREATE UNIQUE INDEX Org_Depth_First   
    ON Organization(BusinessEntityID) ;  
GO  
```  
  
  
## <a name="examples"></a>Esempi  
  
### <a name="simple-example"></a>Esempio semplice  
 L'esempio seguente è intenzionalmente semplicistico per agevolare l'utilizzo iniziale. Creare innanzitutto una tabella per contenere alcuni dati geography.  
  
```sql
CREATE TABLE SimpleDemo  
(
    Level hierarchyid NOT NULL,  
    Location nvarchar(30) NOT NULL,  
    LocationType nvarchar(9) NULL
);
```  
  
 A questo punto inserire i dati per alcuni continenti, paesi, stati e città.  
  
```sql
INSERT SimpleDemo  
    VALUES   
('/1/', 'Europe', 'Continent'),  
('/2/', 'South America', 'Continent'),  
('/1/1/', 'France', 'Country'),  
('/1/1/1/', 'Paris', 'City'),  
('/1/2/1/', 'Madrid', 'City'),  
('/1/2/', 'Spain', 'Country'),  
('/3/', 'Antarctica', 'Continent'),  
('/2/1/', 'Brazil', 'Country'),  
('/2/1/1/', 'Brasilia', 'City'),  
('/2/1/2/', 'Bahia', 'State'),  
('/2/1/2/1/', 'Salvador', 'City'),  
('/3/1/', 'McMurdo Station', 'City');  
```  
  
 Selezionare i dati, aggiungendo una colonna in cui i dati del livello vengono convertiti in un valore di testo facile da capire. Tramite questa query è inoltre possibile ordinare il risultato in base al tipo di dati **hierarchyid** .  
  
```sql
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], *   
    FROM SimpleDemo ORDER BY Level;  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
Converted Level  Level     Location         LocationType  
/1/              0x58      Europe           Continent  
/1/1/            0x5AC0    France           Country  
/1/1/1/          0x5AD6    Paris            City  
/1/2/            0x5B40    Spain            Country  
/1/2/1/          0x5B56    Madrid           City  
/2/              0x68      South America    Continent  
/2/1/            0x6AC0    Brazil           Country  
/2/1/1/          0x6AD6    Brasilia         City  
/2/1/2/          0x6ADA    Bahia            State  
/2/1/2/1/        0x6ADAB0  Salvador         City  
/3/              0x78      Antarctica       Continent  
/3/1/            0x7AC0    McMurdo Station  City  
```  
  
 Si noti che la gerarchia ha una struttura valida, anche se non è coerente internamente. Bahia è l'unico stato. Viene visualizzato nella gerarchia come peer della città Brasilia. Analogamente, McMurdo Station non dispone di un paese padre. Gli utenti dovranno decidere se questo tipo di gerarchia è appropriato per l'utilizzo.  
  
 Aggiungere un'altra riga e selezionare i risultati.  
  
```sql
INSERT SimpleDemo  
    VALUES ('/1/3/1/', 'Kyoto', 'City'), ('/1/3/1/', 'London', 'City');  
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], * FROM SimpleDemo ORDER BY Level;  
```  
  
 In questo modo vengono illustrati altri possibili problemi. Kyoto può essere inserita come livello `/1/3/1/` anche se non esiste alcun livello padre `/1/3/`. Inoltre, Londra e Kyoto hanno lo stesso valore per **hierarchyid**. Anche in questo caso, gli utenti dovranno decidere se questo tipo di gerarchia è appropriato per l'utilizzo e bloccare i valori che non sono validi per lo scopo.  
  
 Inoltre, in questa tabella non è stato utilizzato il primo livello della gerarchia `'/'`. È stato omesso perché non vi è alcun elemento padre comune per tutti i continenti. È possibile inserirne uno aggiungendo l'intero pianeta.  
  
```sql
INSERT SimpleDemo  
    VALUES ('/', 'Earth', 'Planet');  
```  
  
##  <a name="related-tasks"></a><a name="tasks"></a> Attività correlate  
  
###  <a name="migrating-from-parentchild-to-hierarchyid"></a><a name="migrating"></a> Migrazione dalla relazione elemento padre/figlio a hierarchyid  
 La maggior parte degli alberi viene rappresentata utilizzando la relazione elemento padre/figlio. Il modo più semplice per eseguire la migrazione da una struttura elemento padre/figlio a una tabella tramite **hierarchyid** consiste nell'usare una colonna o una tabella temporanea per tenere traccia del numero di nodi a ogni livello della gerarchia. Per un esempio di migrazione di una tabella padre/figlio, vedere la lezione 1 di [Esercitazione: Utilizzo del tipo di dati hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md).  
  
  
###  <a name="managing-a-tree-using-hierarchyid"></a><a name="BKMK_ManagingTrees"></a> Gestione di un albero tramite hierarchyid  
 Un'applicazione può assicurarsi facilmente che una colonna **hierarchyid** rappresenti un albero anche se ciò non accade necessariamente.  
  
-   Durante la generazione di nuovi valori, eseguire una delle operazioni seguenti:  
  
    -   Monitorare l'ultimo numero figlio nella riga padre.  
  
    -   Calcolare l'ultimo elemento figlio. Ciò richiede un indice breadth-first.  
  
-   Applicare l'univocità creando un indice univoco sulla colonna, come parte di una chiave di clustering. Per garantire che vengano inseriti valori univoci, effettuare una delle operazioni seguenti:  
  
    -   Rilevare errori di violazione di chiave univoca e riprovare.  
  
    -   Determinare l'univocità di ogni nuovo nodo figlio e inserirlo come parte di una transazione serializzabile.  
  
  
#### <a name="example-using-error-detection"></a>Esempio di utilizzo del rilevamento degli errori  
 Nell'esempio seguente, il codice consente calcola il nuovo valore **EmployeeId** figlio, quindi rileva eventuali violazioni di chiave e torna al marcatore **INS_EMP** per ricalcolare il valore **EmployeeId** per la nuova riga:  
  
```sql
USE AdventureWorks ;  
GO  
  
CREATE TABLE Org_T1  
   (  
    EmployeeId hierarchyid PRIMARY KEY,  
    OrgLevel AS EmployeeId.GetLevel(),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
  
CREATE INDEX Org_BreadthFirst ON Org_T1(OrgLevel, EmployeeId);
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50) )   
AS  
BEGIN  
    DECLARE @last_child hierarchyid;
INS_EMP:   
    SELECT @last_child = MAX(EmployeeId) FROM Org_T1   
        WHERE EmployeeId.GetAncestor(1) = @mgrid;
    INSERT INTO Org_T1 (EmployeeId, EmployeeName)  
        SELECT @mgrid.GetDescendant(@last_child, NULL), @EmpName;
-- On error, return to INS_EMP to recompute @last_child  
IF @@error <> 0 GOTO INS_EMP   
END ;  
GO  
```  
  
  
#### <a name="example-using-a-serializable-transaction"></a>Esempio di utilizzo di una transazione serializzabile  
 L'indice **Org_BreadthFirst** garantisce che per la determinazione di **\@last_child** venga usata una ricerca di intervallo. Oltre ad altri casi di errore che un'applicazione potrebbe volere controllare, una violazione di chiave duplicata dopo l'inserimento indica un tentativo di aggiunta di più dipendenti con lo stesso ID. Questa situazione richiede pertanto il ricalcolo di **\@last_child**. Il codice seguente calcola il nuovo valore del nodo all'interno di una transazione serializzabile:  
  
```sql
CREATE TABLE Org_T2  
    (  
    EmployeeId hierarchyid PRIMARY KEY,  
    LastChild hierarchyid,   
    EmployeeName nvarchar(50)   
    ) ;  
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50))   
AS  
BEGIN  
DECLARE @last_child hierarchyid  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION   
  
SELECT @last_child  =  EmployeeId.GetDescendant(LastChild,NULL)
FROM Org_T2
WHERE EmployeeId = @mgrid

UPDATE Org_T2 SET LastChild = @last_child  WHERE EmployeeId = @mgrid

INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(@last_child, @EmpName)  
COMMIT  
END ;  
```  
  
 Tramite il codice seguente la tabella viene popolata con tre righe e vengono restituiti i risultati:  
  
```sql
INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(hierarchyid::GetRoot(), 'David') ;  
GO  
AddEmp 0x , 'Sariya'  
GO  
AddEmp 0x58 , 'Mary'  
GO  
SELECT * FROM Org_T2  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
EmployeeId LastChild EmployeeName  
---------- --------- ------------  
0x        0x58       David  
0x58      0x5AC0     Sariya  
0x5AC0    NULL       Mary  
```  
  
  
###  <a name="enforcing-a-tree"></a><a name="BKMK_EnforcingTrees"></a> Applicazione di un albero  
 Negli esempi riportati in precedenza si illustra la possibilità di gestione di un albero da parte di un'applicazione. Per applicare un albero tramite vincoli, è possibile creare una colonna calcolata in cui viene definito l'elemento padre di ogni nodo con un vincolo di chiave esterna relativo all'ID della chiave primaria.  
  
```sql
CREATE TABLE Org_T3  
(  
   EmployeeId hierarchyid PRIMARY KEY,  
   ParentId AS EmployeeId.GetAncestor(1) PERSISTED    
      REFERENCES Org_T3(EmployeeId),  
   LastChild hierarchyid,   
   EmployeeName nvarchar(50)  
)  
GO  
```  
  
 Questo metodo dell'applicazione di una relazione è preferito quando per il codice considerato non affidabile per la gestione dell'albero gerarchico è disponibile un accesso DML diretto alla tabella. Tuttavia, questo metodo potrebbe ridurre le prestazioni poiché è necessario controllare il vincolo in ogni operazione DML.  
  
  
###  <a name="finding-ancestors-by-using-the-clr"></a><a name="findclr"></a> Individuazione di predecessori tramite CLR  
 Un'operazione comune che interessa due nodi in una gerarchia è la ricerca del predecessore comune minore. Questo può essere scritto in [!INCLUDE[tsql](../includes/tsql-md.md)] o CLR poiché il tipo **hierarchyid** è disponibile per entrambi. Si consiglia di usare CLR poiché le prestazioni sono più rapide.  
  
 Utilizzare il codice CLR seguente per elencare i predecessori e cercare il predecessore comune minore:  
  
```csharp
using System;  
using System.Collections;  
using System.Text;  
using Microsoft.SqlServer.Server;  // SqlFunction Attribute
using Microsoft.SqlServer.Types;   // SqlHierarchyId
  
public partial class HierarchyId_Operations  
{  
    [SqlFunction(FillRowMethodName = "FillRow_ListAncestors")]
    public static IEnumerable ListAncestors(SqlHierarchyId h)
    {  
        while (!h.IsNull)  
        {  
            yield return (h);  
            h = h.GetAncestor(1);  
        }  
    }  
    public static void FillRow_ListAncestors(
        Object obj,
        out SqlHierarchyId ancestor
        )
    {  
        ancestor = (SqlHierarchyId)obj;  
    }  
  
    public static HierarchyId CommonAncestor(
        SqlHierarchyId h1,
        HierarchyId h2
        )  
    {  
        while (!h1.IsDescendantOf(h2))  
            h1 = h1.GetAncestor(1);  
  
        return h1;  
    }  
}  
```  
  
 Per usare i metodi **ListAncestor** e **CommonAncestor** negli esempi [!INCLUDE[tsql](../includes/tsql-md.md)] seguenti, compilare la DLL e creare l'assembly **HierarchyId_Operations** in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] eseguendo un codice simile al seguente:  
  
```sql
CREATE ASSEMBLY HierarchyId_Operations   
    FROM '<path to DLL>\ListAncestors.dll';
GO  
```  
  
  
###  <a name="listing-ancestors"></a><a name="ancestors"></a> Elenco dei predecessori  
 La creazione di un elenco dei predecessori di un nodo è un'operazione comune che consente, ad esempio, di mostrare le posizioni di un'organizzazione. A questo scopo, è possibile usare una funzione con valori di tabella tramite la classe **HierarchyId_Operations** definita in precedenza:  
  
 Utilizzo di [!INCLUDE[tsql](../includes/tsql-md.md)]:  
  
```sql
CREATE FUNCTION ListAncestors (@node hierarchyid)  
RETURNS TABLE (node hierarchyid)  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.ListAncestors  
GO  
```  
  
 Esempio di utilizzo:  
  
```sql
DECLARE @h hierarchyid  
SELECT @h = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0' -- /1/1/5/2/  
  
SELECT LoginID, OrgNode.ToString() AS LogicalNode  
FROM HumanResources.EmployeeDemo AS ED  
JOIN ListAncestors(@h) AS A   
   ON ED.OrgNode = A.Node  
GO  
```  
  
  
###  <a name="finding-the-lowest-common-ancestor"></a><a name="lowestcommon"></a> Ricerca del predecessore comune minore  
 Usando la classe **HierarchyId_Operations** definita in precedenza, creare la funzione [!INCLUDE[tsql](../includes/tsql-md.md)] seguente per cercare il predecessore comune minore che interessa due nodi di una gerarchia:  
  
```sql
CREATE FUNCTION CommonAncestor (@node1 hierarchyid, @node2 hierarchyid)  
RETURNS hierarchyid  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.CommonAncestor  
GO  
```  
  
 Esempio di utilizzo:  
  
```sql
DECLARE @h1 hierarchyid, @h2 hierarchyid;
  
SELECT @h1 = OrgNode   
FROM  HumanResources.EmployeeDemo   
WHERE LoginID = 'adventure-works\jossef0'; -- Node is /1/1/3/  
  
SELECT @h2 = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0'; -- Node is /1/1/5/2/  
  
SELECT OrgNode.ToString() AS LogicalNode, LoginID   
FROM HumanResources.EmployeeDemo    
WHERE OrgNode = dbo.CommonAncestor(@h1, @h2) ;  
```  
  
 Il nodo risultante è /1/1/  
  
  
###  <a name="moving-subtrees"></a><a name="BKMK_MovingSubtrees"></a> Spostamento di sottoalberi  
 Un'altra operazione comune è lo spostamento di sottoalberi. La procedura descritta di seguito prende in considerazione il sottoalbero di **\@oldMgr** e lo trasforma (includendo **\@oldMgr**) in un sottoalbero di **\@newMgr**.  
  
```sql
CREATE PROCEDURE MoveOrg(@oldMgr nvarchar(256), @newMgr nvarchar(256) )  
AS  
BEGIN  
DECLARE @nold hierarchyid, @nnew hierarchyid  
SELECT @nold = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @oldMgr ;  
  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION  
SELECT @nnew = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @newMgr ;  
  
SELECT @nnew = @nnew.GetDescendant(max(OrgNode), NULL)   
FROM HumanResources.EmployeeDemo WHERE OrgNode.GetAncestor(1)=@nnew ;  
  
UPDATE HumanResources.EmployeeDemo    
SET OrgNode = OrgNode.GetReparentedValue(@nold, @nnew)  
WHERE OrgNode.IsDescendantOf(@nold) = 1 ;  
  
COMMIT TRANSACTION;
END ;  
GO  
```  
  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento ai metodi per il tipo di dati hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md)   
 [Esercitazione: Uso del tipo di dati hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)   
 [hierarchyid &#40;Transact-SQL&#41;](../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
  
