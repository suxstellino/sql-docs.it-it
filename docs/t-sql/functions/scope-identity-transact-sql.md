---
description: SCOPE_IDENTITY (Transact-SQL)
title: SCOPE_IDENTITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SCOPE_IDENTITY
- SCOPE_IDENTITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], SCOPE_IDENTITY function
- SCOPE_IDENTITY function
- last-inserted identity values
- identity values [SQL Server], last-inserted
ms.assetid: eef24670-059b-4f10-91d4-a67bc1ed12ab
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bb49f217e40b13b106743da61169eaf35a58ef1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183466"
---
# <a name="scope_identity-transact-sql"></a>SCOPE_IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce l'ultimo valore Identity inserito in una colonna Identity nello stesso ambito. Un ambito è un modulo, ovvero una stored procedure, un trigger, una funzione o un batch. Se due istruzioni sono incluse nella stessa stored procedure, nella stessa funzione o nello stesso batch, appartengono allo stesso ambito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
SCOPE_IDENTITY()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **numeric(38,0)**  
  
## <a name="remarks"></a>Osservazioni  
 Le funzioni SCOPE_IDENTITY, IDENT_CURRENT e @@IDENTITY sono simili, in quanto restituiscono valori inseriti in colonne Identity.  
  
 Per la funzione IDENT_CURRENT non esiste alcuna restrizione di ambito o di sessione. La funzione è limitata tuttavia a una tabella specifica. La funzione IDENT_CURRENT restituisce il valore generato per una tabella specifica in qualsiasi sessione e in qualsiasi ambito. Per altre informazioni, vedere [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 Le funzioni SCOPE_IDENTITY e @@IDENTITY restituiscono gli ultimi valori Identity generati in qualsiasi tabella durante la sessione corrente. SCOPE_IDENTITY, tuttavia, restituisce i valori inseriti solo nell'ambito corrente, mentre @@IDENTITY non è limitata a un ambito specifico.  
  
 Si supponga, ad esempio, che esistano due tabelle, T1 e T2, e che venga definito un trigger INSERT per T1. Quando viene inserita una riga in T1, viene attivato il trigger che inserisce una riga in T2. Questo scenario illustra due ambiti, ovvero l'inserimento in T1 e l'inserimento in T2 da parte del trigger.  
  
 Se entrambe le tabelle T1 e T2 contengono colonne Identity, @@IDENTITY e SCOPE_IDENTITY restituiscono valori diversi al termine dell'esecuzione di un'istruzione INSERT su T1. @@IDENTITY restituisce l'ultimo valore inserito in una colonna Identity in qualsiasi ambito durante la sessione corrente, ovvero il valore inserito in T2. SCOPE_IDENTITY() restituisce il valore IDENTITY inserito in T1, che rappresenta l'ultimo inserimento eseguito nello stesso ambito. Se viene chiamata prima che nell'ambito specifico venga eseguita un'istruzione INSERT in una colonna Identity, la funzione SCOPE_IDENTITY() restituisce il valore Null.  
  
 Le istruzioni e le transazioni con esito negativo sono in grado di modificare i dati Identity correnti di una tabella e creare gap nei valori della colonna Identity. Non viene mai eseguito il rollback del valore Identity, anche se non si esegue il commit della transazione che ha tentato l'inserimento del valore nella tabella. Se, ad esempio, un'istruzione INSERT ha esito negativo a causa di una violazione di IGNORE_DUP_KEY, il valore Identity corrente per la tabella viene comunque incrementato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-identity-and-scope_identity-with-triggers"></a>R. Uso di @@IDENTITY e SCOPE_IDENTITY con trigger  
 Nell'esempio seguente vengono create due tabelle, `TZ` e `TY`, e viene definito un trigger INSERT per `TZ`. Quando viene inserita una riga nella tabella `TZ`, viene attivato il trigger (`Ztrig`) che inserisce una riga in `TY`.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE TZ (  
   Z_id  INT IDENTITY(1,1)PRIMARY KEY,  
   Z_name VARCHAR(20) NOT NULL);  
  
INSERT TZ  
   VALUES ('Lisa'),('Mike'),('Carla');  
  
SELECT * FROM TZ;  
```     
Set di risultati: la tabella TZ ha il seguente aspetto.  
  
```  
Z_id   Z_name  
-------------  
1      Lisa  
2      Mike  
3      Carla  
```  
```sql 
CREATE TABLE TY (  
   Y_id  INT IDENTITY(100,5)PRIMARY KEY,  
   Y_name VARCHAR(20) NULL);  
  
INSERT TY (Y_name)  
   VALUES ('boathouse'), ('rocks'), ('elevator');  
  
SELECT * FROM TY;  
```   
Set di risultati: la tabella TY ha il seguente aspetto:  
```  
Y_id  Y_name  
---------------  
100   boathouse  
105   rocks  
110   elevator  
```  

Creare il trigger che inserisce una riga nella tabella TY quando viene inserita una riga nella tabella TZ.  
```sql  
CREATE TRIGGER Ztrig  
ON TZ  
FOR INSERT AS   
   BEGIN  
   INSERT TY VALUES ('')  
   END;  
```  
Attivare (FIRE) il trigger e determinare i valori Identity ottenuti con le funzioni @@IDENTITY e SCOPE_IDENTITY.   
```sql
INSERT TZ VALUES ('Rosalie');  
  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
/*SCOPE_IDENTITY returns the last identity value in the same scope. This was the insert on table TZ.*/`  
SCOPE_IDENTITY  
4  

/*@@IDENTITY returns the last identity value inserted to TY by the trigger. 
  This fired because of an earlier insert on TZ.*/
@@IDENTITY  
115  
```  
  
### <a name="b-using-identity-and-scope_identity-with-replication"></a>B. Uso di @@IDENTITY e SCOPE_IDENTITY() con la replica  
 Negli esempi seguenti viene illustrato come utilizzare`@@IDENTITY` e `SCOPE_IDENTITY()` per inserimenti in un database pubblicato per la replica di tipo merge. Entrambe le tabelle degli esempi sono incluse nel database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. La tabella `Person.ContactType` non viene pubblicata, mentre la tabella `Sales.Customer` viene pubblicata. Con la replica di tipo merge vengono aggiunti trigger alle tabelle pubblicate. `@@IDENTITY` può pertanto restituire il valore dell'inserimento in una tabella del sistema di replica anziché dell'inserimento in una tabella utente.  
  
 Il valore Identity massimo consentito per la tabella `Person.ContactType` è 20. Se si inserisce una riga nella tabella, `@@IDENTITY` e `SCOPE_IDENTITY()` restituiscono lo stesso valore.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Person.ContactType ([Name]) VALUES ('Assistant to the Manager');  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```  
SCOPE_IDENTITY  
21  
@@IDENTITY  
21
```  
  
 Il valore Identity massimo consentito per la tabella `Sales.Customer` è 29483. Se si inserisce una riga nella tabella, vengono restituiti valori diversi da `@@IDENTITY` e `SCOPE_IDENTITY()`. Tramite `SCOPE_IDENTITY()` viene restituito il valore dell'inserimento nella tabella utente, mentre tramite `@@IDENTITY` viene restituito il valore dell'inserimento nella tabella del sistema di replica. Utilizzare `SCOPE_IDENTITY()` per le applicazioni che necessitano dell'accesso al valore Identity inserito.  
  
```sql  
INSERT INTO Sales.Customer ([TerritoryID],[PersonID]) VALUES (8,NULL);  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 ```
 SCOPE_IDENTITY  
 29484  
 @@IDENTITY  
 89
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)  
  
  
