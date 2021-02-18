---
title: Utilizzo di sqlcmd con variabili di scripting
description: Informazioni su come usare le variabili di scripting per creare uno script che può essere usato in più scenari.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799bc2eca98002343d8ff755782d8f3b71d2af77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342644"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>sqlcmd - Usare con variabili di scripting
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Per variabile di scripting si intende una variabile utilizzata negli script. Le variabili di scripting consentono di utilizzare uno script in più scenari. Se, ad esempio, si desidera eseguire uno script su più server, è possibile utilizzare una variabile di scripting per il nome del server anziché modificare lo script per ogni server. È infatti sufficiente modificare il nome del server fornito alla variabile di scripting per eseguire lo stesso script su server diversi.  
  
 Le variabili di scripting possono essere definite in modo esplicito con il comando **setvar** oppure in modo implicito con l'opzione **sqlcmd -v**.  
  
 Questo argomento presenta anche alcuni esempi in cui vengono definite variabili di ambiente al prompt dei comandi Cmd.exe con **SET**.  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>Impostazione delle variabili di scripting tramite il comando setvar  
 Il comando **setvar** consente di definire variabili di scripting. Le variabili definite con il comando **setvar** vengono archiviate internamente. Le variabili di scripting non devono essere confuse con le variabili di ambiente che vengono definite al prompt dei comandi con **SET**. Se uno script fa riferimento a una variabile che non corrisponde a una variabile di ambiente oppure che non è stata definita con il comando **setvar**, verrà restituito un messaggio di errore e l'esecuzione dello script verrà arrestata. Per altre informazioni, vedere l'opzione **-b** in [Utilità sqlcmd](../../tools/sqlcmd-utility.md).  
  
## <a name="variable-precedence-low-to-high"></a>Precedenza delle variabili (in ordine crescente)  
 Se è stato assegnato lo stesso nome a più tipi di variabile, verrà utilizzata la variabile con la precedenza più alta.  
  
1.  Variabili di ambiente a livello di sistema  
  
2.  Variabili di ambiente a livello di utente  
  
3.  Shell dei comandi (**SET X=Y**) impostata al prompt dei comandi prima dell'avvio di **sqlcmd**  
  
4.  **sqlcmd -v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Per visualizzare le variabili di ambiente, nel **Pannello di controllo** aprire **Sistema** quindi fare clic sulla scheda **Avanzate** .  
  
## <a name="implicitly-setting-scripting-variables"></a>Impostazione implicita delle variabili di scripting  
 Quando si avvia **sqlcmd** con un'opzione cui è correlata una variabile **sqlcmd** , la variabile **sqlcmd** viene impostata in modo implicito sul valore specificato con l'opzione. Nell'esempio seguente `sqlcmd` viene avviato con l'opzione `-l` . La variabile SQLLOGINTIMEOUT viene quindi impostata in modo implicito.  
  
```
c:\> sqlcmd -l 60
```
 
È anche possibile usare l'opzione **-v** per impostare una variabile di scripting presente in uno script. Nello script seguente (il nome di file è `testscript.sql`) `ColumnName` è una variabile di scripting.  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

È quindi possibile specificare il nome della colonna che si desidera restituire utilizzando l'opzione `-v` :  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

Per restituire una colonna diversa utilizzando lo stesso script, modificare il valore della variabile di scripting `ColumnName` .  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>Linee guida relative ai nomi e ai valori delle variabili di scripting  
 Quando si assegna un nome alle variabili di scripting, è necessario tenere presenti le linee guida seguenti:  
  
-   I nomi delle variabili non devono contenere spazi vuoti o virgolette.  
  
-   Il formato dei nomi delle variabili non deve essere identico a quello di un'espressione con variabili, ad esempio *$(var)* .  
  
-   Per le variabili di scripting non viene fatta distinzione tra maiuscole e minuscole.  
  
    > [!NOTE]  
    >  Se non viene assegnato alcun valore a una variabile di ambiente **sqlcmd** , la variabile viene rimossa. Se si usa **:setvar VarName** senza un valore, la variabile viene cancellata.  
  
 Quando si specifica un valore per le variabili di scripting, è necessario tenere presenti le linee guida seguenti:  
  
-   I valori delle variabili definiti con **setvar** o l'opzione **-v** devono essere racchiusi tra virgolette se il valore stringa contiene spazi.  
  
-   Se le virgolette fanno parte del valore della variabile, è necessario utilizzare i caratteri di escape. Ad esempio, :`setvar MyVar "spac""e"`.  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>Linee guida relative ai valori e ai nomi delle variabili definite tramite SET in Cmd.exe  
 Le variabili definite con SET fanno parte dell'ambiente Cmd.exe e **sqlcmd** può farvi riferimento. Considerare le linee guida seguenti:  
  
-   I nomi delle variabili non devono contenere spazi vuoti o virgolette.  
  
-   I valori delle variabili possono contenere spazi o virgolette.  
  
## <a name="sqlcmd-scripting-variables"></a>Variabili di scripting di sqlcmd  
 Le variabili definite con **sqlcmd** sono note come variabili di scripting. Nella tabella seguente sono elencate le variabili di scripting di **sqlcmd** .  
  
|        Variabile         | Opzione correlata | L/S |         Predefinito         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | -S             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | L/S | "8" (secondi)           |
| SQLCMDSTATTIMEOUT       | -T             | L/S | "0" = attesa illimitata |
| SQLCMDHEADERS           | -H             | L/S | "0"                     |
| SQLCMDCOLSEP            | -S             | L/S | " "                     |
| SQLCMDCOLWIDTH          | -w             | L/S | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | L/S | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | L/S | "256"                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | L/S | "0" = numero illimitato         |
| SQLCMDEDITOR            |                | L/S | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

Le variabili SQLCMDUSER, SQLCMDPASSWORD e SQLCMDSERVER vengono impostate quando viene usato **:Connect** .  

La lettera L indica che il valore può essere impostato una sola volta durante l'inizializzazione del programma.  
  
L/S indica che il valore può essere reimpostato con il comando **setvar** e i comandi successivi useranno il nuovo valore.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>R. Utilizzo del comando setvar in uno script  
 Molte opzioni di **sqlcmd** possono essere controllate in uno script usando il comando **setvar** . Nell'esempio seguente viene creato lo script `test.sql` , in cui la variabile `SQLCMDLOGINTIMEOUT` viene impostata su `60` secondi e un'altra variabile di scripting, `server`, viene impostata su `testserver`. Il codice seguente è incluso in `test.sql`.  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

Lo script viene chiamato usando sqlcmd:

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>B. Utilizzo interattivo del comando setvar  
 Nell'esempio seguente viene illustrato come impostare una variabile di scripting in modo interattivo tramite il comando `setvar` .  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>C. Utilizzo delle variabili di ambiente dal prompt del comandi all'interno di sqlcmd  
 Nell'esempio seguente vengono impostate quattro variabili di ambiente `are` , che vengono quindi chiamate da `sqlcmd`.  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>D. Utilizzo di variabili di ambiente a livello di utente all'interno di sqlcmd  
 Nell'esempio seguente viene impostata al prompt dei comandi la variabile di ambiente a livello di utente `%Temp%` , che viene quindi passata al file di input `sqlcmd` . Per ottenere la variabile di ambiente a livello di utente, nel **Pannello di controllo** fare doppio clic su **Sistema**. Fare clic sulla scheda **Avanzate** e quindi fare clic sul pulsante **Variabili d'ambiente**.  
  
 Il codice seguente è presente nel file di input `c:\testscript.txt`:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

Il codice seguente viene immesso al prompt dei comandi:

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 Il risultato seguente viene inviato al file di output C:\Documents and Settings\\<utente>\>\Local Settings\Temp\output.txt.  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>E. Utilizzo di uno script di avvio  
 Uno script di avvio di **sqlcmd** viene eseguito all'avvio di **sqlcmd** . Nell'esempio seguente viene impostata la variabile di ambiente `SQLCMDINI`. Di seguito è riportato il contenuto di `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 Di conseguenza, viene chiamato il file `init.sql` all'avvio di `sqlcmd` .  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 Di seguito è riportato l'output.  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  L'opzione **-X** disabilita la funzionalità degli script di avvio.  
  
### <a name="f-variable-expansion"></a>F. Espansione delle variabili  
 L'esempio seguente illustra l'uso dei dati sotto forma di variabile di **sqlcmd** .  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 Inserire una riga nella colonna `Col1` di `dbo.VariableTest` contenente il valore `$(tablename)`.  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 Nel prompt di `sqlcmd` , se non viene impostata alcuna variabile su `$(tablename)`, la riga viene restituita dalle istruzioni seguenti.  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 A condizione che la variabile `MyVar` sia impostata su `$(tablename)`.  

```
>6 :setvar MyVar $(tablename)
```

 Queste istruzioni restituiscono la riga e il messaggio "Variabile di scripting 'tablename' non definita".  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 La riga viene restituita dalle istruzioni seguenti.  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo dell'utilità sqlcmd](./sqlcmd-use-the-utility.md)   
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)   
 [Guida di riferimento alle utilità del prompt dei comandi &#40;motore di database&#41;](../../tools/command-prompt-utility-reference-database-engine.md)  
  
