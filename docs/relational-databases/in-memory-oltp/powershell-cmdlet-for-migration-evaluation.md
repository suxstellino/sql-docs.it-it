---
title: Cmdlet di PowerShell per la valutazione della migrazione | Microsoft Docs
description: Informazioni su Save-SqlMigrationReport, che valuta l'idoneità per la migrazione di oggetti in un database di SQL Server per OLTP in memoria.
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78b4d84a98d7ea2c753a673a222a1a19e0b7e5e2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641281"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>Cmdlet di PowerShell per la valutazione della migrazione

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il cmdlet `Save-SqlMigrationReport` è uno strumento che valuta l'idoneità per la migrazione di più oggetti in un database di SQL Server.

Attualmente questo cmdlet è limitato alla valutazione dell'idoneità per la migrazione di OLTP in memoria. Il cmdlet può essere eseguito sia in un ambiente Windows PowerShell con privilegi elevati che in sqlps.

In alternativa all'esecuzione diretta di questo cmdlet di PowerShell, è possibile eseguire il cmdlet in modo implicito tramite SQL Server Management Studio (SSMS). In **Esplora oggetti** di SSMS è possibile fare clic con il pulsante destro del mouse su una tabella e quindi scegliere **Ottimizzazione guidata per la memoria**.

## <a name="syntax"></a>Sintassi

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>Parametri

I parametri sono descritti nella tabella seguente.

È necessario mettere in evidenza alcuni aspetti della sintassi. Se si specifica il parametro `-InputObject`, non è possibile specificare alcuno dei parametri seguenti:

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

Viceversa, se _non_ si specifica `-InputObject`, è necessario specificare `-Server` e `-Database`. Se si specifica `-Server`, è possibile limitare l'ambito specificando `-Schema` o `-Object` o entrambi.

| Nome parametro | Descrizione |
| :------------- | :---------- |
| Database | Nome del database di SQL Server di destinazione. Obbligatorio quando `-Server` è obbligatorio.<br/><br/> Facoltativo in SQLPS. |
| FolderPath | Cartella in cui il cmdlet deve archiviare i report generati.<br/><br/> Obbligatorio. |
| InputObject | Oggetto SMO a cui il cmdlet deve fare riferimento.<br/><br/> Obbligatorio nell'ambiente Windows Powershell se non si specifica `-Server`.<br/><br/> Facoltativo in SQLPS. |
| MigrationType | Tipo di scenario di migrazione specificato come destinazione dal cmdlet. Attualmente l'unico valore è il valore predefinito **'OLTP'** .<br/><br/> Facoltativa. |
| Oggetto | Nome dell'oggetto per cui creare il report. Può essere una tabella o una stored procedure. |
| Password | Obbligatorio quando `-Username` è obbligatorio. |
| SCHEMA | Nome dello schema a cui appartiene l'oggetto per cui creare il report.<br/><br/> Facoltativa. |
| Server | Nome dell'istanza di SQL Server di destinazione. Obbligatorio nell'ambiente Windows PowerShell se non viene specificato il parametro `-InputObject`.<br/><br/> Facoltativo in SQLPS. |
| Username | Obbligatorio quando si esegue la connessione tramite l'autenticazione di SQL Server, anziché l'autenticazione di Windows. In caso contrario, omettere. |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>Prerequisiti

Prima di poter eseguire questo cmdlet, è necessario installare il modulo denominato **SqlServer**:

- `Install-Module -Name SqlServer`

> [!NOTE]
> Il modulo `SQLPS` precedente non è più disponibile. Usare il modulo `SqlServer` più recente.

Per altre informazioni, vedere [Installare il modulo SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md).

## <a name="example-cmdlet-line"></a>Esempio di riga del cmdlet

Di seguito è riportata la riga del cmdlet effettiva eseguita per generare il report visualizzato più avanti in questo articolo.

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>Esempio di report di output

Nella cartella specificata per il parametro `-FolderPath` vengono creati i due percorsi di cartella seguenti eseguendo questo cmdlet. Entrambi i percorsi iniziano con il valore _nome\_server_:

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

Ogni file di report degli oggetti viene archiviato nella cartella appropriata.

I nomi dei file di report hanno estensione **html**. Ad esempio, un nome di file generato effettivo è **MigrationAdvisorChecklistReport_Table2_20190728.html**.

Il codice HTML è principalmente una tabella a due colonne con le intestazioni seguenti:

- Descrizione
- Risultato convalida

Di seguito è riportato un esempio effettivo del report HTML per una tabella.

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

E di seguito è riportata un'approssimazione dell'aspetto della tabella.

| Descrizione | Risultato convalida |
| :---------- | :---------------- |
| In questa tabella non sono definiti tipi di dati non supportati. | Operazione completata |
| In questa tabella non sono definite colonne di tipo sparse. | Operazione completata |
| In questa tabella non sono definite colonne Identity con incrementi e valori di inizializzazione non supportati. | Operazione completata |
| In questa tabella non sono definite relazioni di chiave esterna. | Operazione completata |
| In questa tabella non sono definiti vincoli non supportati. | Operazione completata |
| In questa tabella non sono definiti indici non supportati. | Operazione completata |
| In questa tabella non sono definiti trigger non supportati. | Operazione completata |
| Le dimensioni di riga in seguito alla migrazione non superano il limite delle tabelle ottimizzate per la memoria. | Operazione completata |
| Tabella non partizionata né replicata. | Operazione completata |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>Collegamenti correlati

- Documentazione di riferimento: [Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport)
