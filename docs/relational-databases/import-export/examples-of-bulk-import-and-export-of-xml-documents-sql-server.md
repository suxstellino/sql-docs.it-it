---
title: Importazione ed esportazione bulk di documenti XML
description: Esempi di importazione ed esportazione in blocco di documenti XML con SQL Server
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- field terminators [SQL Server]
- bulk importing [SQL Server], data formats
- row terminators [SQL Server]
- OPENROWSET function, XML bulk load
- terminators [SQL Server]
- bulk exporting [SQL Server], data formats
- XML bulk load [SQL Server]
ms.assetid: dff99404-a002-48ee-910e-f37f013d946d
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 5e2a52bcfa2e95aa364bbdf3079e2f8666f0e1e0
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978790"
---
# <a name="examples-of-bulk-import-and-export-of-xml-documents-sql-server"></a>Esempi di importazione ed esportazione bulk di documenti XML (SQL Server)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

## <a name="top"></a>

È possibile eseguire importazioni bulk di documenti XML in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o esportazioni bulk da un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo argomento vengono forniti esempi di entrambe le situazioni.

Per l'importazione bulk di dati da un file di dati a una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a una vista non partizionata, è possibile utilizzare quanto segue:  
  
- utilità **bcp**  
   È possibile usare l'utilità **bcp** anche per esportare dati da qualunque posizione in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su cui sia possibile usare l'istruzione SELECT, incluse le viste partizionate.  

- BULK INSERT  
  
- INSERT ... SELECT * FROM OPENROWSET(BULK...).  

Per ulteriori informazioni, vedere gli argomenti seguenti.
- [Importare ed esportare dati per operazioni bulk usando l'utilità bcp (SQL Server).](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [Importare dati per operazioni bulk usando BULK INSERT o OPENROWSET (BULK...)(SQL Server).](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md) 
- [Come importare XML in SQL Server con il componente di caricamento bulk XML.](https://mskb.pkisolutions.com/kb/316005)
- [Raccolte di XML Schema (SQL Server)](../xml/xml-schema-collections-sql-server.md)
  
## <a name="examples"></a>Esempi  
 Gli esempi sono i seguenti:  
  
-  [A. Importazione bulk di dati XML come flusso di byte binario](#binary_byte_stream)  
  
-  [B. Importazione bulk di dati XML in una riga esistente](#existing_row)  
  
-  [C. Importazione bulk di dati XML da un file contenente una definizione DTD](#file_contains_dtd)  
  
- [D. Definizione esplicita del carattere di terminazione del campo tramite un file di formato](#field_terminator_in_format_file)  
  
-  [E. Esportazione bulk di dati XML](#bulk_export_xml_data)  
  
## <a name="bulk-importing-xml-data-as-a-binary-byte-stream"></a><a name="binary_byte_stream"></a>Importazione bulk di dati XML come flusso di byte binario  
 Quando si esegue un'importazione bulk di dati XML da un file contenente una dichiarazione di codifica che si desidera applicare, specificare l'opzione SINGLE_BLOB nella clausola OPENROWSET(BULK...). L'opzione SINGLE_BLOB garantisce che il parser XML di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importi i dati in base allo schema di codifica specificato nella dichiarazione XML.  
  
#### <a name="sample-table"></a>Tabella di esempio  
 Per testare l'esempio A seguente, creare la tabella di esempio `T`.  
  
```sql
USE tempdb  
CREATE TABLE T (IntCol int, XmlCol xml);  
GO  
```  
  
#### <a name="sample-data-file"></a>File di dati di esempio  
 Prima di eseguire l'esempio A, è necessario creare un file con codifica UTF-8 (`C:\SampleFolder\SampleData3.txt`) contenente l'istanza di esempio seguente che specifica lo schema di codifica `UTF-8` .  
  
```xml  
<?xml version="1.0" encoding="UTF-8"?>  
<Root>  
          <ProductDescription ProductModelID="5">  
             <Summary>Some Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-a"></a>Esempio A  
 In questo esempio viene utilizzata l'opzione `SINGLE_BLOB` in un'istruzione `INSERT ... SELECT * FROM OPENROWSET(BULK...)` per importare dati da un file denominato `SampleData3.txt` e inserire un'istanza XML in una tabella a colonna singola, ovvero la tabella di esempio `T`.  
  
```sql
INSERT INTO T(XmlCol)  
SELECT * FROM OPENROWSET(  
   BULK 'c:\SampleFolder\SampleData3.txt',  
   SINGLE_BLOB) AS x;  
```  
  
#### <a name="remarks"></a>Osservazioni  
 L'utilizzo di SINGLE_BLOB in questo caso consente di evitare una mancata corrispondenza tra la codifica del documento XML (come specificata dalla dichiarazione di codifica XML) e la tabella codici della stringa implicita del server.  
  
 Se si utilizzano tipi di dati NCLOB o CLOB e si verifica un conflitto di tabella codici o di codifica, è necessario eseguire una delle operazioni seguenti:  
  
-   Rimuovere la dichiarazione XML per importare correttamente il contenuto del file di dati XML.  
  
-   Specificare una tabella codici nell'opzione CODEPAGE della query corrispondente allo schema di codifica utilizzato nella dichiarazione XML.  
  
-   Verificare la corrispondenza o risolvere le impostazioni delle regole di confronto del database con uno schema di codifica XML non Unicode.  
  
 [&#91;Torna all'inizio&#93;](#top)  
  
##  <a name="bulk-importing-xml-data-in-an-existing-row"></a><a name="existing_row"></a> Importazione bulk di dati XML in una riga esistente  
 In questo esempio viene utilizzato il provider di set di righe con lettura bulk `OPENROWSET` per aggiungere un'istanza XML a una o più righe esistenti nella tabella di esempio `T`.  
  
> [!NOTE]  
>  Per eseguire questo esempio è necessario innanzitutto completare lo script di prova fornito nell'esempio A, nel quale viene creata la tabella `tempdb.dbo.T` e viene eseguita l'importazione bulk di dati da `SampleData3.txt`.  
  
#### <a name="sample-data-file"></a>File di dati di esempio  
 Nell'esempio B viene utilizzata una versione modificata del file di dati di esempio `SampleData3.txt` dell'esempio precedente. Per eseguire questo esempio, modificare il contenuto del file nel modo seguente:  
  
```xml
<Root>  
          <ProductDescription ProductModelID="10">  
             <Summary>Some New Text</Summary>  
          </ProductDescription>  
</Root>  
```  
  
#### <a name="example-b"></a>Esempio B  
  
```sql  
-- Query before update shows initial state of XmlCol values.  
SELECT * FROM T  
UPDATE T  
SET XmlCol =(  
SELECT * FROM OPENROWSET(  
   BULK 'C:\SampleFolder\SampleData3.txt',  
           SINGLE_BLOB  
) AS x  
)  
WHERE IntCol = 1;  
GO  
```  
  
 [&#91;Torna all'inizio&#93;](#top)  
  
## <a name="bulk-importing-xml-data-from-a-file-that-contains-a-dtd"></a><a name="file_contains_dtd"></a> Importazione bulk di dati XML da un file contenente una definizione DTD  
  
> [!IMPORTANT]  
>  È consigliabile non abilitare il supporto per le definizioni DTD (Document Type Definition) se non è necessario nell'ambiente XML utilizzato. L'attivazione del supporto DTD aumenta la superficie di attacco del server esposta a rischi e può esporre quest'ultimo a un attacco Denial-of-Service. Se è necessario abilitare il supporto DTD, è possibile ridurre questo rischio per la sicurezza limitando l'elaborazione a documenti XML attendibili.  
  
 Quando si tenta di usare un comando [bcp](../../tools/bcp-utility.md) per importare dati XML da un file contenente una definizione DTD, è possibile che venga visualizzato un errore analogo al seguente:  
  
 "SQLState = 42000, NativeError = 6359"  
  
 "Error = [Microsoft][SQL Server Native Client][SQL Server]L'analisi di codice XML con DTD di subset interni non è consentita. Utilizzare l'istruzione CONVERT con l'opzione di stile 2 per abilitare il supporto limitato per i DTD di subset interni."  
  
 "Copia BCP %s non riuscita"  
  
 Per risolvere il problema, è possibile importare dati XML da un file di dati contenente una definizione DTD utilizzando la funzione `OPENROWSET(BULK...)` e specificando quindi l'opzione `CONVERT` nella clausola `SELECT` del comando. La sintassi di base per il comando è la seguente:  
  
 `INSERT ... SELECT CONVERT(...) FROM OPENROWSET(BULK...)`  
  
#### <a name="sample-data-file"></a>File di dati di esempio  
 Prima di testare questo esempio di importazione bulk, creare un file (`C:\temp\Dtdfile.xml`) contenente l'istanza di esempio seguente:  
  
```xml 
<!DOCTYPE DOC [<!ATTLIST elem1 attr1 CDATA "defVal1">]><elem1>January</elem1>  
```  
  
#### <a name="sample-table"></a>Tabella di esempio  
 Nell'esempio C viene utilizzata la tabella di esempio `T1` creata mediante l'istruzione `CREATE TABLE` seguente:  
  
```sql  
USE tempdb;  
CREATE TABLE T1(XmlCol xml);  
GO  
```  
  
#### <a name="example-c"></a>Esempio C  
 In questo esempio viene utilizzata la funzione `OPENROWSET(BULK...)` e viene specificata l'opzione `CONVERT` nella clausola `SELECT` per importare i dati XML da `Dtdfile.xml` nella tabella di esempio `T1`.  
  
```sql
INSERT T1  
  SELECT CONVERT(xml, BulkColumn, 2) FROM   
    OPENROWSET(Bulk 'c:\temp\Dtdfile.xml', SINGLE_BLOB) [rowsetresults];  
```  
  
 Dopo l'esecuzione dell'istruzione `INSERT` , la definizione DTD viene rimossa dai dati XML e archiviata nella tabella `T1` .  
  
 [&#91;Torna all'inizio&#93;](#top)  
  
## <a name="specifying-the-field-terminator-explicitly-using-a-format-file"></a><a name="field_terminator_in_format_file"></a> Definizione esplicita del carattere di terminazione del campo tramite un file di formato  
 Nell'esempio seguente viene descritta la procedura per l'importazione bulk del documento XML `Xmltable.dat`.  
  
#### <a name="sample-data-file"></a>File di dati di esempio  
 Il documento in `Xmltable.dat` contiene due valori XML, uno per ogni riga. Per il primo valore XML è stata utilizzata la codifica UTF-16, mentre per il secondo valore è stata utilizzata la codifica UTF-8.  
  
 Il contenuto di questo file di dati è illustrato nel dump Hex seguente:  
  
```  
FF FE 3C 00 3F 00 78 00-6D 00 6C 00 20 00 76 00  *..\<.?.x.m.l. .v.*  
65 00 72 00 73 00 69 00-6F 00 6E 00 3D 00 22 00  *e.r.s.i.o.n.=.".*  
31 00 2E 00 30 00 22 00-20 00 65 00 6E 00 63 00  *1...0.". .e.n.c.*  
6F 00 64 00 69 00 6E 00-67 00 3D 00 22 00 75 00  *o.d.i.n.g.=.".u.*  
74 00 66 00 2D 00 31 00-36 00 22 00 3F 00 3E 00  *t.f.-.1.6.".?.>.*  
3C 00 72 00 6F 00 6F 00-74 00 3E 00 A2 4F 9C 76  *\<.r.o.o.t.>..O.v*  
0C FA 77 E4 80 00 89 00-00 06 90 06 91 2E 9B 2E  *..w.............*  
99 34 A2 34 86 00 83 02-92 20 7F 02 4E C5 E4 A3  *.4.4..... ..N...*  
34 B2 B7 B3 B7 FE F8 FF-F8 00 3C 00 2F 00 72 00  *4.........\<./.r.*  
6F 00 6F 00 74 00 3E 00-00 00 00 00 7A EF BB BF  *o.o.t.>.....z...*  
3C 3F 78 6D 6C 20 76 65-72 73 69 6F 6E 3D 22 31  *\<?xml version="1*  
2E 30 22 20 65 6E 63 6F-64 69 6E 67 3D 22 75 74  *.0" encoding="ut*  
66 2D 38 22 3F 3E 3C 72-6F 6F 74 3E E4 BE A2 E7  *f-8"?><root>....*  
9A 9C EF A8 8C EE 91 B7-C2 80 C2 89 D8 80 DA 90  *................*  
E2 BA 91 E2 BA 9B E3 92-99 E3 92 A2 C2 86 CA 83  *................*  
E2 82 92 C9 BF EC 95 8E-EA 8F A4 EB 88 B4 EB 8E  *................*  
B7 EF BA B7 EF BF B8 C3-B8 3C 2F 72 6F 6F 74 3E  *.........</root>*  
00 00 00 00 7A                                   *....z*  
```  
  
#### <a name="sample-table"></a>Tabella di esempio  
 Per l'importazione o l'esportazione bulk di un documento XML, è consigliabile usare un [carattere di terminazione del campo](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md) che non sia contenuto in alcun documento, ad esempio una serie di quattro valori Null (`\0`) seguita dalla lettera `z`: `\0\0\0\0z`.  
  
 In questo esempio viene illustrato come utilizzare questo carattere di terminazione del campo per la tabella di esempio `xTable` . Per creare questa tabella di esempio, utilizzare l'istruzione `CREATE TABLE` seguente:  
  
```sql
USE tempdb;  
CREATE TABLE xTable (xCol xml);  
GO  
```  
  
#### <a name="sample-format-file"></a>File di formato di esempio  
 È necessario specificare il carattere di terminazione del campo nel file di formato. Nell'esempio D viene utilizzato un file di formato non XML denominato `Xmltable.fmt` che contiene quanto segue:  
  
```  
9.0  
1  
1       SQLBINARY     0       0       "\0\0\0\0z"    1     xCol         ""  
```  
  
 È possibile utilizzare tale file di formato per l'importazione bulk di documenti XML nella tabella `xTable` tramite un comando `bcp` o un'istruzione `BULK INSERT` o `INSERT ... SELECT * FROM OPENROWSET(BULK...)` .  
  
#### <a name="example-d"></a>Esempio D  
 In questo esempio viene utilizzato il file di formato `Xmltable.fmt` in un'istruzione `BULK INSERT` per importare il contenuto di un file di dati XML denominato `Xmltable.dat`.  
  
```sql
BULK INSERT xTable   
FROM 'C:\Xmltable.dat'  
WITH (FORMATFILE = 'C:\Xmltable.fmt');  
GO  
```  
  
 [&#91;Torna all'inizio&#93;](#top)  
  
## <a name="bulk-exporting-xml-data"></a><a name="bulk_export_xml_data"></a> Esportazione bulk di dati XML  
 Nell'esempio seguente viene usata l'utilità [bcp](../../tools/bcp-utility.md) per l'esportazione bulk di dati XML dalla tabella creata nell'esempio precedente usando lo stesso file di formato XML. Nel comando `bcp` seguente, `<server_name>` e `<instance_name>` rappresentano segnaposto da sostituire con valori appropriati:  
  
```cmd
bcp bulktest..xTable out a-wn.out -N -T -S<server_name>\<instance_name>  
```  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non salva la codifica XML in caso di persistenza di dati XML nel database. La codifica originale dei campi XML, pertanto, non è disponibile quando vengono esportati i dati XML. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa la codifica UTF-16 durante l'esportazione di dati XML.  
  

## <a name="see-also"></a>Vedere anche  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Clausola SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-clause-transact-sql.md)   
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Informazioni sull'importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)  
  
  
