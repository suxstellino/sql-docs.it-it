---
title: Specificare la lunghezza del prefisso nei file di dati con bcp
description: Questo articolo descrive il campo prefisso, che codifica la lunghezza di un campo per fornire l'archiviazione di tipo file compatta per l'esportazione bulk in formato nativo in un file di dati.
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], prefix length
- prefix length [SQL Server]
- lengths [SQL Server], prefix characters
- data formats [SQL Server], prefix length
ms.assetid: ce32dd1a-26f1-4f61-b9fa-3f1feea9992e
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: f57d14262410c3b469cc4576e35f2eb0d6c625f6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745731"
---
# <a name="specify-prefix-length-in-data-files-using-bcp-sql-server"></a>Specificare la lunghezza del prefisso nei file di dati tramite bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Per fornire il tipo di archiviazione file con la massima compressione durante l'esportazione in blocco dei dati in formato nativo in un file di dati, il comando **bcp** inserisce davanti a ogni campo uno o più caratteri che ne indicano la lunghezza. Tali caratteri sono denominati *caratteri per il prefisso di lunghezza*.  
  
## <a name="the-bcp-prompt-for-prefix-length"></a>Richiesta della lunghezza del prefisso da parte di bcp  
 Se un comando interattivo **bcp** include l'opzione **in** o **out** senza l'opzione relativa al file di formato ( **-f**) o al formato dei dati ( **-n**, **-c**, **-w** o **-N**) viene richiesta la lunghezza del prefisso di ogni campo di dati, come illustrato di seguito:  
  
 `Enter prefix length of field <field_name> [<default>]:`  
  
 Se si specifica 0, **bcp** richiede la lunghezza del campo (per dati di tipo carattere) o il carattere di terminazione del campo (per dati di tipo nativo non carattere).  
  
> [!NOTE]  
>  Dopo l'impostazione interattiva di tutti i campi in un comando **bcp**, viene richiesto di salvare le risposte relative a ogni campo in un file di formato non XML. Per altre informazioni sui file di formato non XML, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
## <a name="overview-of-prefix-length"></a>Panoramica della lunghezza del prefisso  
 Per archiviare la lunghezza del prefisso di un campo, è necessario un numero di byte sufficiente per rappresentare la lunghezza massima del campo. Il numero di byte necessari dipende inoltre dal tipo di archiviazione di file, dall'impostazione relativa al supporto di valori Null della colonna e dal formato utilizzato per l'archiviazione nel file di dati: nativo o carattere. Ad esempio, il tipo di dati **text** o **image** richiede quattro caratteri di prefisso per archiviare la lunghezza del campo, mentre il tipo di dati **varchar** richiede due caratteri. I caratteri per il prefisso di lunghezza vengono archiviati nel file di dati utilizzando il formato binario interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Per i dati in formato nativo, utilizzare i prefissi di lunghezza anziché i caratteri di terminazione del campo. È possibile che si verifichino conflitti tra i caratteri di terminazione e i dati in formato nativo, in quanto per i file di dati in formato nativo viene utilizzato il formato binario interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="prefix-lengths-for-bulk-export"></a><a name="PrefixLengthsExport"></a> Lunghezze del prefisso per l'esportazione bulk  
  
> [!NOTE]  
>  Il valore predefinito visualizzato alla richiesta della lunghezza del prefisso quando si esegue l'esportazione di un campo rappresenta la lunghezza del prefisso ottimale per il campo.  
  
 I valori Null vengono rappresentati come campo vuoto. Per indicare che il campo è vuoto, ovvero che rappresenta un valore NULL, il prefisso contiene il valore -1, quindi è necessario almeno 1 byte. Si noti che per le colonne di tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che consentono valori Null è necessaria una lunghezza del prefisso maggiore o uguale a 1, a seconda del tipo di archiviazione di file.  
  
 Quando si esegue l'esportazione bulk di dati e li si archivia come tipi di dati nativi o in formato carattere, utilizzare le lunghezze di prefisso illustrate nella tabella seguente:  
  
|SQL Server<br /><br /> Tipo di dati|Formato nativo<br /><br /> NOT NULL|Formato nativo<br /><br /> NULL|Formato carattere<br /><br /> NOT NULL|Formato carattere<br /><br /> NULL|  
|------------------------------|--------------------------------|----------------------------|-----------------------------------|-------------------------------|  
|**char**|2|2|2|2|  
|**varchar**|2|2|2|2|  
|**nchar**|2|2|2|2|  
|**nvarchar**|2|2|2|2|  
|**text***|4|4|4|4|  
|**ntext***|4|4|4|4|  
|**binary**|2|2|2|2|  
|**varbinary**|2|2|2|2|  
|**image***|4|4|4|4|  
|**datetime**|0|1|0|1|  
|**smalldatetime**|0|1|0|1|  
|**decimal**|1|1|1|1|  
|**numeric**|1|1|1|1|  
|**float**|0|1|0|1|  
|**real**|0|1|0|1|  
|**int**|0|1|0|1|  
|**bigint**|0|1|0|1|  
|**smallint**|0|1|0|1|  
|**tinyint**|0|1|0|1|  
|**money**|0|1|0|1|  
|**smallmoney**|0|1|0|1|  
|**bit**|0|1|0|1|  
|**uniqueidentifier**|1|1|0|1|  
|**timestamp**|1|1|1|1|  
|**ntext**|8|8|8|8|  
|**varbinary(max)**|8|8|8|8|  
|**UDT** (tipo di dati definito dall'utente)|8|8|8|8|  
|**XML**|8|8|8|8|  
|**sql_variant**|8|8|8|8|  
  
 \*I tipi di dati **ntext**, **text** e **image** verranno rimossi in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di utilizzare questi tipi di dati in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente li utilizzano. Usare in alternativa **nvarchar(max)** , **varchar(max)** e **varbinary(max)** .  
  
##  <a name="prefix-lengths-for-bulk-import"></a><a name="PrefixLengthsImport"></a> Lunghezze del prefisso per il caricamento bulk  
 Quando si esegue l'importazione bulk di dati, la lunghezza del prefisso corrisponde al valore specificato al momento della creazione del file di dati. Se il file di dati non è stato creato da un comando **bcp** , i caratteri di prefisso di lunghezza probabilmente non esistono. In tal caso, specificare il valore 0 come lunghezza del prefisso.  
  
> [!NOTE]  
>  Per specificare la lunghezza del prefisso in un file di dati non creato usando il comando **bcp**, usare le lunghezze indicate in [Lunghezze del prefisso per l'esportazione bulk](#PrefixLengthsExport), più indietro in questo argomento.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Specificare la lunghezza del campo tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Specificare caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  
