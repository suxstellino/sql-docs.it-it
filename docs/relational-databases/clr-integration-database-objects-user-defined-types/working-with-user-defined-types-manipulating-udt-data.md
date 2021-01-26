---
title: Manipolazione dei dati UDT | Microsoft Docs
description: Questo articolo descrive come inserire, selezionare e aggiornare i dati nelle colonne UDT di un database di SQL Server.
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- data deletions [CLR integration]
- data insertions [CLR integration]
- CONVERT function
- data updates [CLR integration]
- comparing data
- updating data [CLR integration]
- removing data
- IsByteOrdered property
- variables [CLR integration]
- data manipulation [CLR integration]
- user-defined types [CLR integration], data manipulation
- deleting data
- UDTs [CLR integration], data manipulation
- invoking UDT methods
- inserting data
ms.assetid: 51b1a5f2-7591-4e11-bfe2-d88e0836403f
author: rothja
ms.author: jroth
ms.openlocfilehash: d23880a7ea6a1e8f4c1beccc5ec82f40303b9b76
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783637"
---
# <a name="working-with-user-defined-types---manipulating-udt-data"></a>Uso di tipi definiti dall'utente (UDT) - Modifica di dati UDT
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] non fornisce una sintassi specifica per l'istruzione INSERT, UPDATE o DELETE quando si modificano i dati nelle colonne con tipo definito dall'utente (UDT). Le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST o CONVERT vengono utilizzate per eseguire il cast dei tipi di dati nativi al tipo UDT.  
  
## <a name="inserting-data-in-a-udt-column"></a>Inserimento di dati in una colonna con tipo definito dall'utente  
 Le istruzioni seguenti consentono [!INCLUDE[tsql](../../includes/tsql-md.md)] di inserire tre righe di dati di esempio nella tabella **Points** . Il tipo di dati **Point** è costituito da valori integer X e Y esposti come proprietà del tipo definito dall'utente. È necessario utilizzare la funzione CAST o CONVERT per eseguire il cast dei valori X e Y delimitati da virgole al tipo di **punto** . Le prime due istruzioni utilizzano la funzione CONVERT per convertire un valore stringa nel tipo **Point** e la terza istruzione utilizza la funzione cast:  
  
```sql  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
## <a name="selecting-data"></a>Selezione dei dati  
 L'istruzione SELECT seguente consente di selezionare il valore binario del tipo definito dall'utente.  
  
```sql  
SELECT ID, PointValue FROM dbo.Points  
```  
  
 Per visualizzare l'output visualizzato in un formato leggibile, chiamare il metodo **ToString** del tipo definito dall'utente **Point** , che converte il valore nella relativa rappresentazione di stringa.  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points;  
```  
  
 Vengono prodotti i risultati seguenti:  
  
```  
ID PointValue  
-- ----------  
 1 3,4  
 2 1,5  
 3 1,99  
```  
  
 Per ottenere gli stessi risultati, è anche possibile utilizzare le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST e CONVERT.  
  
```sql  
SELECT ID, CAST(PointValue AS varchar)   
FROM dbo.Points;  
  
SELECT ID, CONVERT(varchar, PointValue)   
FROM dbo.Points;  
```  
  
 Il tipo definito dall'utente **Point** espone le coordinate X e Y come proprietà, che è quindi possibile selezionare singolarmente. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente consente di selezionare le coordinate X e Y separatamente:  
  
```sql  
SELECT ID, PointValue.X AS xVal, PointValue.Y AS yVal   
FROM dbo.Points;  
```  
  
 Le proprietà X e Y restituiscono un valore integer visualizzato nel set di risultati.  
  
```  
ID xVal yVal  
-- ---- ----  
 1    3    4  
 2    1    5  
 3    1   99  
```  
  
## <a name="working-with-variables"></a>Gestione delle variabili  
 È possibile utilizzare le variabili specificando l'istruzione DECLARE per assegnare una variabile a un tipo definito dall'utente. Le istruzioni seguenti assegnano un valore usando l' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione set e visualizzano i risultati chiamando il metodo **ToString** del tipo definito dall'utente sulla variabile:  
  
```sql  
DECLARE @PointValue Point;  
SET @PointValue = (SELECT PointValue FROM dbo.Points  
    WHERE ID = 2);  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 Nel set di risultati viene visualizzato il valore della variabile:  
  
```  
PointValue  
----------  
-1,5  
```  
  
 Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] seguenti consentono di ottenere lo stesso risultato utilizzando SELECT anziché SET per l'assegnazione delle variabili:  
  
```sql  
DECLARE @PointValue Point;  
SELECT @PointValue = PointValue FROM dbo.Points  
    WHERE ID = 2;  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 La differenza tra l'utilizzo di SELECT e di SET per l'assegnazione delle variabili sta nel fatto che SELECT consente di assegnare più variabili in un'istruzione SELECT, mentre la sintassi di SET richiede che ogni assegnazione di variabile contenga la relativa istruzione SET.  
  
## <a name="comparing-data"></a>Confronto dei dati  
 È possibile usare gli operatori di confronto per confrontare i valori nel tipo definito dall'utente se la proprietà **IsByteOrdered** è stata impostata su **true** quando si definisce la classe. Per ulteriori informazioni, vedere [creazione di un tipo di User-Defined](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md).  
  
```sql  
SELECT ID, PointValue.ToString() AS Points   
FROM dbo.Points  
WHERE PointValue > CONVERT(Point, '2,2');  
```  
  
 È possibile confrontare i valori interni del tipo definito dall'utente indipendentemente dall'impostazione di **IsByteOrdered** se i valori stessi sono confrontabili. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente consente di selezionare le righe in cui X è maggiore di Y:  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points  
WHERE PointValue.X < PointValue.Y;  
```  
  
 È anche possibile utilizzare gli operatori di confronto con le variabili, come mostrato in questa query che esegue la ricerca di un valore PointValue corrispondente.  
  
```sql  
DECLARE @ComparePoint Point;  
SET @ComparePoint = CONVERT(Point, '3,4');  
SELECT ID, PointValue.ToString() AS MatchingPoint   
FROM dbo.Points  
WHERE PointValue = @ComparePoint;  
```  
  
## <a name="invoking-udt-methods"></a>Chiamata dei metodi UDT  
 È anche possibile richiamare i metodi definiti nel tipo definito dall'utente in [!INCLUDE[tsql](../../includes/tsql-md.md)]. La classe **Point** contiene tre metodi, **distance**, **DistanceFrom** e **DistanceFromXY**. Per gli elenchi di codice che definiscono questi tre metodi, vedere [codifica User-Defined tipi](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
 L' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione seguente chiama il metodo **PointValue. distance** :  
  
```sql  
SELECT ID, PointValue.X AS [Point.X],   
    PointValue.Y AS [Point.Y],  
    PointValue.Distance() AS DistanceFromZero   
FROM dbo.Points;  
```  
  
 I risultati vengono visualizzati nella colonna **distanza** :  
  
```  
ID X  Y  Distance  
-- -- -- ----------------  
 1  3  4                5  
 2  1  5 5.09901951359278  
 3  1 99 99.0050503762308  
```  
  
 Il metodo **DistanceFrom** accetta un argomento del tipo di dati **Point** e visualizza la distanza dal punto specificato a PointValue:  
  
```sql  
SELECT ID, PointValue.ToString() AS Pnt,  
   PointValue.DistanceFrom(CONVERT(Point, '1,99')) AS DistanceFromPoint  
FROM dbo.Points;  
```  
  
 Nei risultati vengono visualizzati i risultati del metodo **DistanceFrom** per ogni riga della tabella:  
  
```  
ID Pnt DistanceFromPoint  
-- --- -----------------  
 1 3,4  95.0210502993942  
 2 1,5                94  
 3 1,9                90  
```  
  
 Il metodo **DistanceFromXY** accetta i punti singolarmente come argomenti:  
  
```sql  
SELECT ID, PointValue.X as X, PointValue.Y as Y,   
PointValue.DistanceFromXY(1, 99) AS DistanceFromXY   
FROM dbo.Points  
```  
  
 Il set di risultati è lo stesso del metodo **DistanceFrom** .  
  
## <a name="updating-data-in-a-udt-column"></a>Aggiornamento dei dati in una colonna con tipo definito dall'utente  
 Per aggiornare i dati in una colonna con tipo definito dall'utente, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE. Per aggiornare lo stato dell'oggetto è anche possibile utilizzare un metodo del tipo definito dall'utente. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente consente di aggiornare una singola riga nella tabella:  
  
```sql  
UPDATE dbo.Points  
SET PointValue = CAST('1,88' AS Point)  
WHERE ID = 3  
```  
  
 È anche possibile aggiornare gli elementi UDT separatamente. L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente consente di aggiornare solo la coordinata Y:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Se il tipo definito dall'utente è stato definito con ordinamento dei byte impostato su **true**, [!INCLUDE[tsql](../../includes/tsql-md.md)] può valutare la colonna con tipo definito dall'utente in una clausola WHERE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = '4,5'  
WHERE PointValue = '3,4';  
```  
  
### <a name="updating-limitations"></a>Limitazioni relative all'aggiornamento  
 Non è possibile aggiornare più proprietà contemporaneamente utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ad esempio, l'istruzione UPDATE seguente ha esito negativo e restituisce un errore perché non è possibile utilizzare lo stesso nome di colonna due volte in un'unica istruzione UPDATE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue.X = 5, PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Per aggiornare ogni punto singolarmente, è necessario creare un metodo mutatore nell'assembly del tipo definito dall'utente Point. È quindi possibile richiamare il metodo mutatore per aggiornare l'oggetto in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE, come illustrato di seguito:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.SetXY(5, 99)  
WHERE ID = 3  
```  
  
## <a name="deleting-data-in-a-udt-column"></a>Eliminazione di dati in una colonna con tipo definito dall'utente  
 Per eliminare dati in un tipo definito dall'utente, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] DELETE. L'istruzione seguente consente di eliminare tutte le righe della tabella che corrispondono ai criteri specificati nella clausola WHERE. Se si omette la clausola WHERE in un'istruzione DELETE, verranno eliminate tutte le righe della tabella.  
  
```sql  
DELETE FROM dbo.Points  
WHERE PointValue = CAST('1,99' AS Point)  
```  
  
 Utilizzare l'istruzione UPDATE se si desidera rimuovere i valori in una colonna con tipo definito dall'utente, senza tuttavia modificare i valori di altre righe. In questo esempio PointValue viene impostato su Null.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = null  
WHERE ID = 2  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Uso di tipi definiti dall'utente in SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
  
  
