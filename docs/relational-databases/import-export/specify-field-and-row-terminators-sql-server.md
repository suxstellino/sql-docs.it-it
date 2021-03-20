---
title: Specificare i caratteri di terminazione del campo e della riga (SQL Server) | Microsoft Docs
description: I caratteri di terminazione del campo e della riga indicano ai programmi che leggono il file di dati dove termina un campo o una riga e dove inizia un altro campo o un'altra riga.
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], terminators
- field terminators [SQL Server]
- data formats [SQL Server], terminators
- row terminators [SQL Server]
- terminators [SQL Server]
ms.assetid: f68b6782-f386-4947-93c4-e89110800704
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3208393d25df19d980da6b66c78dc3711f428ecd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752281"
---
# <a name="specify-field-and-row-terminators-sql-server"></a>Impostazione dei caratteri di terminazione del campo e della riga (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Per i campi dati di tipo carattere è facoltativamente possibile contrassegnare la fine di ogni campo di un file di dati con un *carattere di terminazione del campo* a e la fine di ogni riga con un *carattere di terminazione della riga*. I caratteri di terminazione costituiscono un mezzo per indicare ai programmi che leggono il file di dati dove termina un campo o una riga e dove inizia un altro campo o un'altra riga.  
  
> [!IMPORTANT]
>  Per il formato nativo o nativo Unicode, utilizzare i prefissi di lunghezza anziché i caratteri di terminazione del campo. Possono verificarsi conflitti tra i dati in formato nativo e i caratteri di terminazione, perché un file di dati in formato nativo viene archiviato nel formato di dati binario interno di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="characters-supported-as-terminators"></a>Caratteri di terminazione supportati  
 Il comando **bcp** , l'istruzione BULK INSERT e il provider bulk per set di righe OPENROWSET supportano una vasta gamma di caratteri di terminazione del campo o della riga e cercano sempre la prima istanza di ogni carattere di terminazione. Nella tabella seguente sono inclusi i caratteri di terminazione supportati.  
  
|Carattere di terminazione|Indicato da|  
|---------------------------|------------------|  
|Scheda|\t<br /><br /> Questo è il carattere di terminazione del campo predefinito.|  
|Carattere di nuova riga|\n<br /><br /> Questo è il carattere di terminazione della riga predefinito.|  
|Ritorno a capo/avanzamento riga|\r|  
|Barra rovesciata*|\\\|  
|Carattere di terminazione Null (non visibile)**|\0|  
|Tutti i caratteri stampabili (i caratteri di controllo non sono stampabili, ad eccezione dei caratteri Null, di tabulazione, di nuova riga e di ritorno a capo)|(*, A, t, l e così via)|  
|Stringa costituita da un massimo di 10 caratteri stampabili, inclusi alcuni o tutti i caratteri di terminazione descritti sopra|(**\t\*\*, end, !!!!!!!!!!, \t-\n e così via)|  
  
 *Per generare un carattere di controllo, è possibile utilizzare solo i caratteri t, n, r, 0 e '\0' in associazione al carattere di escape barra rovesciata.  
  
 **Anche se il carattere di controllo Null (\0) non è visibile quando viene stampato, è un carattere distinto del file di dati. Pertanto, l'utilizzo di un carattere di controllo Null come carattere di terminazione è diverso dall'assenza di qualsiasi carattere di terminazione del campo o della riga.  
  
> [!IMPORTANT]  
>  Se i dati contengono un carattere di terminazione, questo viene interpretato come tale e non come un'informazione, mentre i dati che seguono tale carattere vengono interpretati come parte del campo o del record successivo. Di conseguenza, è necessario scegliere con attenzione i caratteri di terminazione per evitare che vengano visualizzati nei dati. Un carattere di terminazione del campo di surrogato basso, ad esempio, non costituisce una buona scelta per un carattere di terminazione del campo se i dati contengono tale surrogato basso.  
  
## <a name="using-row-terminators"></a>Utilizzo di caratteri di terminazione della riga  
 Il carattere di terminazione della riga può essere identico al carattere di terminazione dell'ultimo campo. In genere, tuttavia, è consigliabile utilizzare un carattere di terminazione della riga distinto. Per generare un output in formato tabulare, ad esempio, è possibile terminare l'ultimo campo di ogni riga con il carattere di nuova riga (\n) e tutti gli altri campi con il carattere di tabulazione (\t). Per posizionare ogni record di dati nella riga corrispondente del file di dati, specificare la combinazione \r\n come carattere di terminazione della riga.  
  
> [!NOTE]  
>  Se si usa l'utilità **bcp** in modalità interattiva e si specifica il carattere di nuova riga \n come carattere di terminazione della riga, il comando **bcp** aggiunge automaticamente il carattere di ritorno a capo \r come prefisso, generando il carattere di terminazione della riga \r\n.  
  
## <a name="specifying-terminators-for-bulk-export"></a>Impostazione dei caratteri di terminazione per l'esportazione bulk  
 Se si esegue l'esportazione bulk di dati **char** o **nchar** e si vuole usare un carattere di terminazione non predefinito, è necessario specificare tale carattere nel comando **bcp** . È possibile specificare i caratteri di terminazione in uno dei modi seguenti:  
  
-   Con un file di formato che specifica il carattere di terminazione per ogni singolo campo.  
  
    > [!NOTE]  
    >  Per informazioni su come usare i file di formato, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   Senza un file di formato, mediante le alternative seguenti:  
  
    -   Uso dell'opzione **-t** per specificare il carattere di terminazione del campo per tutti i campi, ad eccezione dell'ultimo campo della riga, e dell'opzione **-r** per specificare un carattere di terminazione della riga.  
  
    -   Uso di un'opzione per il formato carattere ( **-c** o **-w**) senza l'opzione **-t** , che imposta il carattere di terminazione del campo sul carattere di tabulazione, \t. Questa impostazione è equivalente all'uso di **-t**\t.  
  
        > [!NOTE]  
        >  Se si specifica l'opzione **-n** per i dati nativi o l'opzione **-N** per i dati nativi Unicode, i caratteri di terminazione non vengono inseriti.  
  
    -   Se un comando **bcp** interattivo include l'opzione **in** o **out** senza l'opzione per il file di formato ( **-f**) o per un formato dei dati ( **-n**, **-c**, **-w** o **-N**) e si è scelto di non specificare la lunghezza del prefisso e del campo, il comando richiede di specificare il carattere di terminazione di ogni campo. L'impostazione predefinita none non prevede alcun carattere di terminazione:  
  
         `Enter field terminator [none]:`  
  
         In genere, l'impostazione predefinita rappresenta una scelta appropriata. Per i campi che includono dati **char** o **nchar**, tuttavia, vedere la sottosezione seguente "Linee guida per l'utilizzo dei caratteri di terminazione". Per un esempio contestualizzato di tale richiesta, vedere [Impostazione dei formati di dati per la compatibilità mediante &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
        > [!NOTE]  
        >  Dopo l'impostazione interattiva di tutti i campi in un comando **bcp**, viene richiesto di salvare le risposte relative a ogni campo in un file di formato non XML. Per altre informazioni sui file di formato non XML, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="guidelines-for-using-terminators"></a>Linee guida per l'utilizzo dei caratteri di terminazione  
 In alcuni casi, un carattere di terminazione può essere utile per un campo dati **char** o **nchar** . Ad esempio:  
  
-   Per una colonna di dati che contiene un valore null in un file di dati da importare in un programma che non riconosce le informazioni sulla lunghezza del prefisso.  
  
     Qualsiasi colonna di dati che contiene un valore null è considerata a lunghezza variabile. In assenza di lunghezze dei prefissi, un carattere di terminazione è necessario per identificare la fine di un campo null, in modo da garantire la corretta interpretazione dei dati.  
  
-   Per una colonna a lunghezza fissa lunga il cui spazio è utilizzato solo parzialmente da molte righe.  
  
     In questa situazione, la definizione di un carattere di terminazione può ridurre lo spazio di archiviazione, facendo sì che il campo venga considerato un campo a lunghezza variabile.  

### <a name="specifying-n-as-a-row-terminator-for-bulk-export"></a>Specifica di `\n` come carattere di terminazione di riga per l'esportazione bulk

Quando si specifica `\n` come carattere di terminazione di riga per l'esportazione bulk o si usa in modo implicito il terminatore di riga predefinito, bcp restituisce una combinazione di ritorno a capo-avanzamento riga (CRLF) come carattere di terminazione di riga. Se si vuole usare solo un carattere di avanzamento riga (LF) come terminazione di riga, come avviene in genere nei computer Unix e Linux, usare la notazione esadecimale per specificare il carattere di terminazione di riga LF. Ad esempio:

```cmd
bcp -r '0x0A'
```

### <a name="examples"></a>Esempi  
 In questo esempio viene eseguita l'esportazione bulk dei dati dalla tabella `AdventureWorks.HumanResources.Department` al file di dati `Department-c-t.txt` usando il formato carattere con la virgola come carattere di terminazione del campo e il carattere di nuova riga (\n) come terminazione di riga.  
  
 Per il comando **bcp** sono disponibili le opzioni seguenti.  
  
|Opzione|Descrizione|  
|------------|-----------------|  
|**-c**|Specifica che i campi dati devono essere caricati come dati di tipo carattere.|  
|**-t** `,`|Specifica la virgola (,) come carattere di terminazione del campo.|  
|**-r** \n|Specifica il carattere di nuova riga come carattere di terminazione della riga. Questo è il carattere di terminazione della riga predefinito e, pertanto, la relativa impostazione è facoltativa.|  
|**-T**|Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.|  
  
 Per altre informazioni, vedere [bcp Utility](../../tools/bcp-utility.md).  
  
 Al prompt dei comandi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows digitare:  
  
```cmd
bcp AdventureWorks.HumanResources.Department out C:\myDepartment-c-t.txt -c -t, -r \n -T  
```  
  
 In questo modo viene creato il file di dati `Department-c-t.txt`contenente 16 record, ognuno con quattro campi. I campi sono separati da una virgola.  
  
## <a name="specifying-terminators-for-bulk-import"></a>Impostazione dei caratteri di terminazione per l'importazione bulk  
 Quando si esegue l'importazione bulk di dati **char** o **nchar** , il relativo comando deve riconoscere i caratteri di terminazione usati nel file di dati. L'impostazione dei caratteri di terminazione varia in base al comando per l'importazione bulk, come illustrato di seguito:  
  
-   **bcp**  
  
     Per impostare i caratteri di terminazione per un'operazione di importazione, è possibile utilizzare la stessa sintassi utilizzata per un'operazione di esportazione. Per ulteriori informazioni, vedere "Impostazione dei caratteri di terminazione per l'esportazione bulk", più indietro in questo argomento.  
  
-   BULK INSERT  
  
     È possibile specificare i caratteri di terminazione per i singoli campi di un file di formato o per l'intero file di dati utilizzando i qualificatori illustrati nella tabella seguente:  
  
    |Qualifier|Descrizione|  
    |---------------|-----------------|  
    |FIELDTERMINATOR **='** _field_terminator_*_'_*|Specifica il carattere di terminazione del campo da utilizzare per i file di dati di tipo carattere e carattere Unicode.<br /><br /> Il valore predefinito è il carattere di tabulazione (\t).|  
    |ROWTERMINATOR **='** _row_terminator_*_'_*|Specifica il carattere di terminazione della riga da utilizzare per i file di dati di tipo carattere e carattere Unicode.<br /><br /> Il valore predefinito è \n (carattere di nuova riga).|  
  
     Per altre informazioni, vedere [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
-   INSERT ... SELECT * FROM OPENROWSET(BULK...).  
  
     Per il provider di set di righe con lettura bulk OPENROWSET, è possibile specificare i caratteri di terminazione solo nel file di formato, ad eccezione dei tipi di dati contenenti oggetti di grandi dimensioni. Se un file di dati di tipo carattere utilizza un carattere di terminazione non predefinito, è necessario definire tale carattere nel file di formato. Per altre informazioni, vedere [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md) e [Utilizzo di un file di formato per l'importazione bulk dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  
  
     Per altre informazioni sulla clausola OPENROWSET BULK, vedere [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  

### <a name="specifying-n-as-a-row-terminator-for-bulk-import"></a>Specifica di `\n` come carattere di terminazione di riga per l'importazione bulk
Quando si specifica `\n` come carattere di terminazione di riga per l'importazione bulk o si usa in modo implicito il terminatore di riga predefinito, bcp e l'istruzione BULK INSERT si aspettano una combinazione di ritorno a capo-avanzamento riga (CRLF) come carattere di terminazione di riga. Se il file di origine usa solo un carattere di avanzamento riga (LF) come terminazione di riga, come avviene in genere nei file generati in computer Unix e Linux, usare la notazione esadecimale per specificare il carattere di terminazione di riga LF. Ad esempio, in un'istruzione BULK INSERT:

```sql
    ROWTERMINATOR = '0x0A'
```
 
### <a name="examples"></a>Esempi  
 Negli esempi di questa sezione viene eseguita l'importazione bulk di dati di tipo carattere dal file dei dati `Department-c-t.txt` creato nell'esempio precedente nella tabella `myDepartment` del database di esempio [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Prima di eseguire le procedure illustrate negli esempi, è necessario creare la tabella. Per crearla in base allo schema **dbo** , nell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eseguire il codice seguente:  
  
```sql  
USE AdventureWorks;  
GO  
DROP TABLE myDepartment;  
CREATE TABLE myDepartment   
(DepartmentID smallint,  
Name nvarchar(50),  
GroupName nvarchar(50) NULL,  
ModifiedDate datetime not NULL CONSTRAINT DF_AddressType_ModifiedDate DEFAULT (GETDATE())  
);  
GO 
```  
  
#### <a name="a-using-bcp-to-interactively-specify-terminators"></a>R. Utilizzo del comando bcp per l'impostazione interattiva dei caratteri di terminazione  
 Nell'esempio seguente viene eseguita l'importazione bulk del file di dati `Department-c-t.txt` utilizzando un comando `bcp` . Per questo comando sono disponibili le stesse opzioni del comando di esportazione bulk. Per ulteriori informazioni, vedere "Impostazione dei caratteri di terminazione per l'esportazione bulk", più indietro in questo argomento.  
  
 Al prompt dei comandi di Windows digitare:  
  
```cmd
bcp AdventureWorks..myDepartment in C:\myDepartment-c-t.txt -c -t , -r \n -T  
```  
  
#### <a name="b-using-bulk-insert-to-interactively-specify-terminators"></a>B. Utilizzo dell'istruzione BULK INSERT per l'impostazione interattiva dei caratteri di terminazione  
 Nell'esempio seguente viene eseguita l'importazione bulk del file di dati `Department-c-t.txt` utilizzando un'istruzione `BULK INSERT` con i qualificatori illustrati nella tabella seguente:  
  
|Opzione|Attributo|  
|------------|---------------|  
|DATAFILETYPE **='** char **'**|Specifica che i campi dati devono essere caricati come dati di tipo carattere.|  
|FIELDTERMINATOR **='** `,` **'**|Specifica la virgola (`,`) come carattere di terminazione del campo.|  
|ROWTERMINATOR **='** `\n` **'**|Specifica il carattere di nuova riga come carattere di terminazione della riga.|  
  
 Nell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eseguire il codice seguente:  
  
```sql  
USE AdventureWorks;  
GO  
BULK INSERT myDepartment FROM 'C:\myDepartment-c-t.txt'  
   WITH (  
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ',',  
      ROWTERMINATOR = '\n'  
);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Specificare la lunghezza del campo tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Specificare la lunghezza del prefisso nei file di dati tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)   
 [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  
