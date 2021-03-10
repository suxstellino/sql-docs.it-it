---
title: Regole di confronto dei database indipendenti | Microsoft Docs
description: Informazioni sul funzionamento delle regole di confronto nei database indipendenti e non indipendenti. Vedere i problemi che possono verificarsi quando le sessioni sono una via di mezzo tra contesti indipendenti e non indipendenti.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database, collations
ms.assetid: 4b44f6b9-2359-452f-8bb1-5520f2528483
author: stevestein
ms.author: sstein
ms.openlocfilehash: 260c4afa90b28aa8efc22a15fbda13cfa7f35417
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532323"
---
# <a name="contained-database-collations"></a>Regole di confronto dei database indipendenti
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Varie proprietà incidono sull'ordinamento e sulla semantica di uguaglianza dei dati testuali, ad esempio distinzione maiuscole/minuscole, distinzione caratteri accentati/non accentati e linguaggio di base utilizzato. Queste qualità vengono espresse in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite la scelta di regole di confronto per i dati. Per informazioni dettagliate sulle regole di confronto, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Le regole di confronto si applicano non solo ai dati archiviati nelle tabelle utente, ma a tutto il testo gestito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], inclusi metadati, oggetti temporanei, nomi di variabili e così via. La gestione di tali elementi risulta diversa nei database indipendenti rispetto a quelli non indipendenti. Questa modifica non interesserà molti utenti, ma contribuirà ad assicurare uniformità e indipendenza dell'istanza. È tuttavia possibile che sia anche causa di qualche confusione, nonché di problemi per le sessioni che accedono sia a database indipendenti che non indipendenti.  
  
 In questo argomento si illustra il contenuto della modifica e si esaminano alcune aree in cui tale modifica potrebbe causare problemi.  
  
## <a name="non-contained-databases"></a>Database non indipendenti  
 A tutti i database sono associate regole di confronto predefinite, che possono essere impostate quando si crea o si modifica un database. Queste regole di confronto vengono utilizzate per tutti i metadati presenti nel database e come impostazione predefinita per tutte le colonne stringa all'interno del database. Gli utenti possono scegliere regole di confronto diverse per qualsiasi colonna specifica usando la clausola **COLLATE** .  
  
### <a name="example-1"></a>Esempio 1  
 Se si lavora ad esempio a Pechino, è possibile che venga utilizzata una regola di confronto cinese:  
  
```sql  
ALTER DATABASE MyDB COLLATE Chinese_Simplified_Pinyin_100_CI_AS;  
```  
  
 Se ora si crea una colonna, le relative regole di confronto predefinite saranno quelle cinesi, ma è possibile sceglierne altre, se lo si desidera:  
  
```sql  
CREATE TABLE MyTable  
      (mycolumn1 nvarchar,  
      mycolumn2 nvarchar COLLATE Frisian_100_CS_AS);  
GO  
SELECT name, collation_name  
FROM sys.columns  
WHERE name LIKE 'mycolumn%' ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```sql  
name            collation_name  
--------------- ----------------------------------  
mycolumn1       Chinese_Simplified_Pinyin_100_CI_AS  
mycolumn2       Frisian_100_CS_AS  
```  
  
 Questo approccio sembra relativamente semplice, ma insorgono vari problemi. Poiché le regole di confronto per una colonna sono dipendenti dal database nel quale viene creata la tabella, i problemi insorgono se si usano tabelle temporanee archiviate in **tempdb**. Le regole di confronto di **tempdb** corrispondono solitamente a quelle dell'istanza, che non devono corrispondere alle regole di confronto del database.  
  
### <a name="example-2"></a>Esempio 2  
 Si consideri, ad esempio, il database precedente (cinese) usato in un'istanza con regole di confronto **Latin1_General** :  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max)) ;  
GO  
```  
  
 A prima vista queste due tabelle hanno apparentemente lo stesso schema, ma poiché le regole di confronto dei database sono diverse, i valori sono in effetti incompatibili:  
  
```  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Messaggio 468, livello 16, stato 9, riga 2  
  
 Impossibile risolvere il conflitto tra le regole di confronto "Latin1_General_100_CI_AS_KS_WS_SC" e "Chinese_Simplified_Pinyin_100_CI_AS" nell'operazione uguale a.  
  
 Per correggere questo problema è necessario confrontare esplicitamente la tabella temporanea. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa operazione è resa più semplice perché viene fornita la parola chiave **DATABASE_DEFAULT** per la clausola **COLLATE** .  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max) COLLATE DATABASE_DEFAULT);  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 A questo punto l'esecuzione avviene senza errori.  
  
 Il comportamento dipendente dalle regole di confronto è rilevabile anche con le variabili. Si consideri la funzione seguente:  
  
```  
CREATE FUNCTION f(@x INT) RETURNS INT  
AS BEGIN   
      DECLARE @I INT = 1  
      DECLARE @İ INT = 2  
      RETURN @x * @i  
END;  
```  
  
 Si tratta di una funzione piuttosto particolare. Nelle regole di confronto con distinzione tra maiuscole e minuscole, @i nella clausola RETURN non può essere associato a @I o @İ. Nelle regole di confronto Latin1_General senza distinzione tra maiuscole e minuscole, @i viene associato a @I e la funzione restituisce 1. Tuttavia, nelle regole di confronto Turkish senza distinzione tra maiuscole e minuscole, @i viene associato a @I e la funzione restituisce 2. Questa funzione può causare seri problemi in un database che passa da un'istanza all'altra con regole di confronto diverse.  
  
## <a name="contained-databases"></a>Database indipendenti  
 Poiché uno degli obiettivi di progettazione dei database indipendenti è di renderli autosufficienti, è necessario eliminare la dipendenza dalle regole di confronto di istanza e **tempdb** . A questo scopo, nei database indipendenti viene introdotto il concetto di regole di confronto del catalogo. Tali regole vengono utilizzate per metadati di sistema e oggetti temporanei. Di seguito vengono illustrati i dettagli.  
  
 In un database indipendente vengono usate le regole di confronto del catalogo **Latin1_General_100_CI_AS_WS_KS_SC**. Si tratta delle stesse regole di confronto per tutti i database indipendenti in tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e non è possibile modificarle.  
  
 Le regole di confronto del database vengono mantenute, ma utilizzate solo come regole di confronto predefinite per i dati utente. Per impostazione predefinita, le regole di confronto del database sono uguali a quelle del modello, ma possono essere modificate dall'utente tramite un comando **CREATE** o **ALTER DATABASE** come con i database non indipendenti.  
  
 Nella clausola **COLLATE** è disponibile una nuova parola chiave, **CATALOG_DEFAULT** . che viene utilizzata come collegamento alle regole di confronto correnti per i metadati nei database indipendenti e non indipendenti. Vale a dire che, in un database non indipendente, la parola chiave **CATALOG_DEFAULT** restituirà le regole di confronto del database correnti, poiché i metadati vengono confrontati nelle regole di confronto del database. In un database indipendente questi due valori potrebbero essere diversi, in quanto l'utente può modificare le regole di confronto del database in modo che non corrispondano alle regole di confronto del catalogo.  
  
 Nella tabella seguente viene riepilogato il comportamento di vari oggetti sia nei database non indipendenti che in quelli indipendenti:  
  
|Elemento|Database non indipendente|Database indipendente|  
|-|-|-|  
|Dati utente (impostazione predefinita)|DATABASE_DEFAULT|DATABASE_DEFAULT|  
|Dati temporanei (impostazione predefinita)|Regole di confronto TempDB|DATABASE_DEFAULT|  
|Metadati|DATABASE_DEFAULT/CATALOG_DEFAULT|COLLATE|  
|Metadati temporanei|Regole di confronto TempDB|COLLATE|  
|variables|Regole di confronto di istanza|COLLATE|  
|Etichette Goto|Regole di confronto di istanza|COLLATE|  
|Nomi di cursori|Regole di confronto di istanza|COLLATE|  
  
 Nell'esempio di tabella temporanea descritto in precedenza, si noterà che questo comportamento delle regole di confronto Elimina la necessità di una clausola **COLLATE** esplicita nella maggior parte degli usi della tabella temporanea. In un database indipendente questo codice ora viene eseguito senza errori, anche se le regole di confronto di database e istanza sono diverse:  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max));  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Il codice funziona perché il confronto di `T1_txt` e di `T2_txt` viene effettuato nelle regole di confronto del database relative al database indipendente.  
  
## <a name="crossing-between-contained-and-uncontained-contexts"></a>Passaggio tra contesti indipendenti e non indipendenti  
 Finché una sessione in un database indipendente rimane indipendente, dovrà rimanere all'interno del database al quale è connessa. In questo caso il comportamento è molto chiaro. Se, tuttavia, avviene il passaggio di una sessione tra contesti indipendenti e non indipendenti, il comportamento diventa più complesso, poiché è necessario creare un collegamento tra i due set di regole. Questa condizione può verificarsi in un database parzialmente indipendente, poiché un utente può usare **USE** su un altro database. In questo caso, la differenza nelle regole di confronto viene gestita in base al principio seguente.  
  
-   Il comportamento delle regole di confronto per un batch viene determinato dal database in cui inizia il batch.  
  
 Si noti che questa decisione avviene prima dell'esecuzione di qualsiasi comando, incluso un comando **USE** iniziale. Ciò significa che se un batch inizia in un database indipendente, ma il primo comando è **USE** su un database non indipendente, per il batch verrà comunque usato il comportamento delle regole di confronto indipendenti. In questo caso, un riferimento a una variabile, ad esempio, può avere più risultati possibili:  
  
-   Il riferimento può trovare esattamente una corrispondenza. In questo caso il riferimento funzionerà senza errori.  
  
-   Il riferimento può non trovare una corrispondenza nelle regole di confronto correnti dove in precedenza ne era presente una. In questo caso verrà generato un errore nel quale è indicato che la variabile non esiste, anche se apparentemente è stata creata.  
  
-   Il riferimento più trovare più corrispondenze originariamente distinte. Anche in questo caso verrà generato un errore.  
  
 Di seguito vengono usati alcuni esempi per illustrare questo comportamento, presupponendo l'uso di un database parzialmente indipendente, denominato `MyCDB` , con regole di confronto del database impostate sulle regole di confronto predefinite **Latin1_General_100_CI_AS_WS_KS_SC**. Si supponga che le regole di confronto dell'istanza siano **Latin1_General_100_CS_AS_WS_KS_SC**. la sola differenza tra le due regole di confronto riguarda la distinzione tra maiuscole e minuscole.  
  
### <a name="example-1"></a>Esempio 1  
 Nell'esempio seguente viene illustrato il caso in cui il riferimento trova esattamente una corrispondenza.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #a VALUES(1);  
GO  
  
USE master;  
GO  
  
SELECT * FROM #a;  
GO  
  
Results:  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
x  
-----------  
1  
```  
  
 In questo caso, il risultato #a identificato viene associato sia nelle regole di confronto del catalogo senza distinzione tra maiuscole e minuscole che nelle regole di confronto di istanza con distinzione tra maiuscole e minuscole e il codice funziona correttamente.  
  
### <a name="example-2"></a>Esempio 2  
 Nell'esempio seguente viene illustrato il caso in cui il riferimento non trova una corrispondenza nelle regole di confronto correnti dove in precedenza ne era presente una.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #A VALUES(1);  
GO  
```  
  
 In questo caso #A viene associato ad #a nelle regole di confronto predefinite senza distinzione tra maiuscole e minuscole e l'inserimento funziona:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
```  
  
 Se tuttavia si continua lo script...  
  
```  
USE master;  
GO  
  
SELECT * FROM #A;  
GO  
```  
  
 il tentativo di associazione ad #A nelle regole di confronto di istanza con distinzione tra maiuscole e minuscole restituisce un errore:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Messaggio 208, livello 16, stato 0, riga 2  
  
 Il nome di oggetto '#A' non è valido.  
  
### <a name="example-3"></a>Esempio 3  
 Nell'esempio seguente viene illustrato il caso in cui il riferimento trova più corrispondenze originariamente distinte. Si inizierà innanzitutto in **tempdb** (per cui sono valide le stesse regole di confronto con distinzione tra maiuscole e minuscole dell'istanza) e verranno eseguite le istruzioni seguenti.  
  
```  
USE tempdb;  
GO  
  
CREATE TABLE #a(x int);  
GO  
CREATE TABLE #A(x int);  
GO  
INSERT INTO #a VALUES(1);  
GO  
INSERT INTO #A VALUES(2);  
GO  
```  
  
 L'esito è positivo poiché in queste regole di confronto le tabelle sono distinte:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
(1 row(s) affected)  
```  
  
 Se si passa al database indipendente, si noterà tuttavia che l'associazione a queste tabelle non è più possibile.  
  
```  
USE MyCDB;  
GO  
SELECT * FROM #a;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Messaggio 12800, livello 16, stato 1, riga 2  
  
 Il riferimento al nome della tabella temporanea '#a' è ambiguo e non può essere risolto. I possibili candidati sono '#a' e '#A'.  
  
## <a name="conclusion"></a>Conclusioni  
 Il comportamento delle regole di confronto dei database indipendenti è leggermente diverso da quello nei database non indipendenti. Questo comportamento è generalmente utile, in quanto consente l'indipendenza dell'istanza e favorisce la semplicità. È possibile che alcuni utenti riscontrino problemi, specialmente quando una sessione accede a database indipendenti e non indipendenti.  
  
## <a name="see-also"></a>Vedere anche  
 [Database indipendenti](../../relational-databases/databases/contained-databases.md)  
  
  
