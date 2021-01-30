---
description: ALTER ASSEMBLY (Transact-SQL)
title: ALTER ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASSEMBLY_TSQL
- ALTER ASSEMBLY
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], modifying
- refreshing assemblies
- assemblies [CLR integration], versioning
- assemblies [CLR integration], adding files
- modifying assemblies
- adding files
- ALTER ASSEMBLY statement
ms.assetid: 87bca678-4e79-40e1-bb8b-bd5ed8f34853
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 21f116d296a9145f514424677993fc9e1bd0536e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183017"
---
# <a name="alter-assembly-transact-sql"></a>ALTER ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica un assembly mediante la modifica delle proprietà di catalogo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di un assembly. ALTER ASSEMBLY esegue un aggiornamento dell'assembly in base alla copia più recente dei moduli [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] che includono la relativa implementazione e aggiunge o rimuove i file a esso associati. Gli assembly vengono creati tramite l'istruzione [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  

> [!WARNING]
>  CLR usa la Sicurezza dall'accesso di codice (CAS, Code Access Security) in .NET Framework, non più supportata come limite di sicurezza. Un assembly CLR creato con `PERMISSION_SET = SAFE` potrebbe essere in grado di accedere alle risorse di sistema esterne, chiamare codice non gestito e acquisire privilegi sysadmin. A partire da [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], viene introdotta un'opzione `sp_configure` denominata `clr strict security` per migliorare la sicurezza degli assembly CLR. `clr strict security` è abilitata per impostazione predefinita e considera gli assembly CLR `SAFE` e `UNSAFE` come se fossero contrassegnati `EXTERNAL_ACCESS`. È possibile disabilitare l'opzione `clr strict security` per la compatibilità con le versioni precedenti, ma questa operazione è sconsigliata. Microsoft consiglia che tutti gli assembly siano firmati con un certificato o una chiave asimmetrica con un account di accesso corrispondente che disponga dell'autorizzazione `UNSAFE ASSEMBLY` nel database master. Per altre informazioni, vedere [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md).  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER ASSEMBLY assembly_name  
    [ FROM <client_assembly_specifier> | <assembly_bits> ]  
    [ WITH <assembly_option> [ ,...n ] ]  
    [ DROP FILE { file_name [ ,...n ] | ALL } ]  
    [ ADD FILE FROM   
    {   
        client_file_specifier [ AS file_name ]   
      | file_bits AS file_name   
    } [,...n ]   
    ] [ ; ]  
<client_assembly_specifier> :: =  
    '\\computer_name\share-name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
    { varbinary_literal | varbinary_expression }  
  
<assembly_option> :: =  
    PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
  | VISIBILITY = { ON | OFF }  
  | UNCHECKED DATA  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *assembly_name*  
 Nome dell'assembly da modificare. *assembly_name* non deve essere già esistente nel database.  
  
 FROM \<client_assembly_specifier> | \<assembly_bits>  
 Aggiorna un assembly in base alla copia più recente dei moduli [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] contenenti la relativa implementazione. È possibile usare questa opzione solo se non esistono file associati all'assembly specificato.  
  
 \<client_assembly_specifier> specifica il percorso di rete o il percorso locale in cui si trova l'assembly in fase di aggiornamento. Il percorso di rete include il nome del computer e della condivisione, nonché un percorso all'interno di quest'ultima. *manifest_file_name* specifica il nome del file contenente il manifesto dell'assembly.  

> [!IMPORTANT]
> Il database SQL di Azure non supporta riferimenti a un file.
  
 \<assembly_bits> rappresenta il valore binario dell'assembly.  
  
 È necessario usare istruzioni ALTER ASSEMBLY separate per ogni assembly dipendente che inoltre richiede un aggiornamento.  
  
 PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
> [!IMPORTANT]
>  L'opzione `PERMISSION_SET` è influenzata dall'opzione `clr strict security` descritta nell'avviso di apertura. Quando l'opzione `clr strict security` è abilitata, tutti gli assembly vengono considerati come `UNSAFE`.  
>  Specifica la proprietà del set di autorizzazioni per l'accesso al codice di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per l'assembly. Per altre informazioni su questa proprietà, vedere [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md).  
> 
> [!NOTE]
>  Le opzioni EXTERNAL_ACCESS e UNSAFE non sono disponibili in un database indipendente.  
  
 VISIBILITY = { ON | OFF }  
 Specifica se l'assembly risulta visibile per la creazione in base a esso di funzioni CLR (Common Language Runtime), stored procedure, trigger, tipi definiti dall'utente e funzioni di aggregazione definite dall'utente. Se l'opzione è impostata su OFF, l'assembly verrà chiamato solo da altri assembly. Se esistono oggetti di database CLR precedentemente creati in base all'assembly, non è possibile modificare la visibilità dell'assembly. Per impostazione predefinita, gli eventuali assembly a cui *assembly_name* fa riferimento vengono caricati come non visibili.  
  
 UNCHECKED DATA  
 Per impostazione predefinita, ALTER ASSEMBLY ha esito negativo se deve verificare la consistenza delle singole righe di tabella. Questa opzione consente di posticipare le verifiche a un momento successivo tramite DBCC CHECKTABLE. Se viene specificata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue l'istruzione ALTER ASSEMBLY anche se nel database sono presenti tabelle contenenti quanto segue:  
  
-   Colonne calcolate persistenti che direttamente o indirettamente fanno riferimento a metodi nell'assembly tramite funzioni o metodi [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Vincoli CHECK che direttamente o indirettamente fanno riferimento a metodi nell'assembly.  
  
-   Se esistono colonne di un tipo CLR definito dall'utente che dipendono dall'assembly e il tipo implementa un formato di serializzazione **UserDefined** (non **Native**).  
  
-   Colonne di tipo CLR definito dall'utente che fanno riferimento a viste create tramite WITH SCHEMABINDING.  
  
 Se sono presenti vincoli CHECK, essi vengono disabilitati e contrassegnati come non attendibili. Le tabelle contenenti colonne che dipendono dall'assembly vengono contrassegnate come contenenti dati non controllati finché tali tabelle non vengono verificate in modo esplicito.  
  
 Solo i membri dei ruoli predefiniti del database **db_owner** e **db_ddlowner** possono specificare questa opzione.  
  
 Per specificare questa opzione, è necessaria l'autorizzazione **ALTER ANY SCHEMA**.  
  
 Per altre informazioni, vedere [Implementazione di assembly](../../relational-databases/clr-integration/assemblies-implementing.md).  
  
 [ DROP FILE { *file_name*[ **,** _...n_] | ALL } ]  
 Rimuove il nome file associato all'assembly oppure tutti i file associati all'assembly dal database. Se specificata assieme all'opzione ADD FILE (descritto di seguito), l'opzione DROP FILE viene eseguita per prima. Ciò consente di sostituire un file con lo stesso nome.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente o nel database SQL di Azure.  
  
 [ ADD FILE FROM { *client_file_specifier* [ AS *file_name*] | *file_bits* AS *file_name*}  
 Carica un file da associare all'assembly, ad esempio codice sorgente, file di debug o altre informazioni correlate, nel server e da visualizzare nella vista del catalogo **sys.assembly_files**. *client_file_specifier* specifica il percorso dal quale caricare il file. In alternativa è possibile usare *file_bits* per specificare l'elenco di valori binari che compongono il file. *file_name* specifica il nome in base al quale il file deve essere archiviato nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Specificare *file_name* se *file_bits* è specificato. È invece facoltativo se è specificato *client_file_specifier*. Se *file_name* non viene specificato, la parte file_name di *client_file_specifier* viene usata come *file_name*.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente o nel database SQL di Azure.  
  
## <a name="remarks"></a>Osservazioni  
 ALTER ASSEMBLY non interferisce con le sessioni in esecuzione che eseguono codice nell'assembly in fase di modifica. Le sessioni correnti completano l'esecuzione tramite l'utilizzo dei bit non modificati dell'assembly.  
  
 Se si specifica la clausola FROM, ALTER ASSEMBLY aggiorna l'assembly in base alle copie più recenti dei moduli specificati. Poiché nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero essere presenti funzioni CLR, stored procedure, trigger, tipi di dati e funzioni di aggregazione definite dall'utente già definiti in base all'assembly, l'istruzione ALTER ASSEMBLY riassocia questi elementi all'implementazione più recente dell'assembly. Per eseguire questa riassociazione, è necessario che i metodi che eseguono il mapping alle funzioni CLR, alle stored procedure e ai trigger esistano nell'assembly modificati con le stesse firme. Le classi che implementano i tipi CLR definiti dall'utente e le funzioni di aggregazione definite dall'utente devono continuare a soddisfare i requisiti richiesti per i tipi e le funzioni di aggregazione definiti dall'utente.  
  
> [!CAUTION]  
>  Se non è specificata la clausola WITH UNCHECKED DATA, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di impedire l'esecuzione dell'istruzione ALTER ASSEMBLY nel caso in cui la nuova versione dell'assembly modifichi i dati esistenti in tabelle, indici o altre posizioni persistenti. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non garantisce tuttavia la consistenza di colonne calcolate, indici, viste indicizzate o espressioni con i tipi e le routine sottostanti in caso di aggiornamento dell'assembly CLR. Quando si esegue ALTER ASSEMBLY è pertanto necessario verificare che non siano presenti discrepanze tra il risultato di una determinata espressione e i valori basati su tale espressione archiviati nell'assembly.  
  
 ALTER ASSEMBLY modifica la versione dell'assembly. La lingua e il token di chiave pubblica dell'assembly restano invariati.  
  
 L'istruzione ALTER ASSEMBLY non può essere usata per modificare:  
  
-   Le firme di funzioni CLR, funzioni di aggregazione, stored procedure e trigger in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che fa riferimento all'assembly. L'istruzione ALTER ASSEMBLY ha esito negativo se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di riassociare gli oggetti di database di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la nuova versione dell'assembly.  
  
-   Le firme dei metodi nell'assembly chiamati da altri assembly.  
  
-   L'elenco di assembly che dipendono dall'assembly come specificato nella proprietà **DependentList** dell'assembly.  
  
-   L'indicizzabilità di un metodo a meno che non esista alcun indice o colonna calcolata persistente che dipende da tale metodo, direttamente o indirettamente.  
  
-   L'attributo del nome di metodo **FillRow** per le funzioni CLR con valori di tabella.  
  
-   Le firme dei metodi **Accumulate** e **Terminate** per le aggregazioni definite dall'utente.  
  
-   Gli assembly di sistema.  
  
-   La proprietà degli assembly. In alternativa usare [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Per gli assembly che implementano tipi definiti dall'utente, inoltre, è possibile usare ALTER ASSEMBLY per apportare soltanto le modifiche seguenti:  
  
-   Modifica di metodi pubblici della classe di un tipo definito dall'utente, a condizione che non vengano modificati attributi o firme.  
  
-   Aggiunta di nuovi metodi pubblici.  
  
-   Modifica di metodi privati.  
  
 I campi contenuti in un tipo definito dall'utente con serializzazione nativa, inclusi i membri dei dati o le classi base, non possono essere modificati mediante ALTER ASSEMBLY. Tutte le altre modifiche non sono supportate.  
  
 Se si omette ADD FILE FROM, ALTER ASSEMBLY elimina i file associati all'assembly.  
  
 Se l'istruzione ALTER ASSEMBLY viene eseguita senza la clausola di dati UNCHECKED, vengono eseguite le verifiche per controllare che la nuova versione dell'assembly non abbia un impatto negativo sui dati delle tabelle. Le prestazioni potrebbero dipendere dalla quantità di dati che devono essere sottoposti a verifica.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione ALTER per l'assembly. È inoltre necessario disporre dei requisiti seguenti:  
  
-   Per modificare un assembly il cui set di autorizzazioni esistente è EXTERNAL_ACCESS, è necessaria l'autorizzazione **EXTERNAL ACCESS ASSEMBLY** nel server.  
  
-   Per modificare un assembly il cui set di autorizzazioni esistente è UNSAFE, è necessaria l'autorizzazione **UNSAFE ASSEMBLY** nel server.  
  
-   Per modificare il set di autorizzazioni di un assembly e impostarlo su EXTERNAL_ACCESS, è necessaria l'autorizzazione **EXTERNAL ACCESS ASSEMBLY** nel server.  
  
-   Per modificare il set di autorizzazioni di un assembly e impostarlo su UNSAFE, è necessaria l'autorizzazione **UNSAFE ASSEMBLY** nel server.  
  
-   Per specificare WITH UNCHECKED DATA, è necessaria l'autorizzazione **ALTER ANY SCHEMA**.  


### <a name="permissions-with-clr-strict-security"></a>Autorizzazioni con CLR strict security    
Sono necessarie le autorizzazioni seguenti per modificare un assembly CLR con `CLR strict security` abilitata:

- L'utente deve disporre dell'autorizzazione `ALTER ASSEMBLY`  
- Inoltre, una delle condizioni seguenti deve essere rispettata:  
  - L'assembly è firmato con un certificato o una chiave asimmetrica con un account di accesso corrispondente con l'autorizzazione `UNSAFE ASSEMBLY` nel server. È consigliabile firmare l'assembly.  
  - La proprietà `TRUSTWORTHY` del database è impostata su `ON` e il database è di proprietà di un accesso che dispone dell'autorizzazione `UNSAFE ASSEMBLY` nel server. Questa opzione non è consigliata.  
  
  
 Per altre informazioni sui set di autorizzazioni per gli assembly, vedere [Progettazione di assembly](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-refreshing-an-assembly"></a>R. Aggiornamento di un assembly  
 Nell'esempio seguente l'assembly `ComplexNumber` viene aggiornato in base alla copia più recente dei moduli [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] contenenti la relativa implementazione.  
  
> [!NOTE]  
>  Per creare l'assembly `ComplexNumber`, eseguire gli script di esempio UserDefinedDataType. Per altre informazioni, vedere [Tipo definito dall'utente](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130)).  
  
 ```sql
 ALTER ASSEMBLY ComplexNumber 
 FROM 'C:\Program Files\Microsoft SQL Server\130\Tools\Samples\1033\Engine\Programmability\CLR\UserDefinedDataType\CS\ComplexNumber\obj\Debug\ComplexNumber.dll' 
  ```

> [!IMPORTANT]
> Il database SQL di Azure non supporta riferimenti a un file.

### <a name="b-adding-a-file-to-associate-with-an-assembly"></a>B. Aggiunta di un file da associare a un assembly  
 Nell'esempio seguente viene caricato il file del codice sorgente `Class1.cs` da associare all'assembly `MyClass`. In questo esempio si presuppone che l'assembly `MyClass` sia già stato creato nel database.  
  
```sql  
ALTER ASSEMBLY MyClass   
ADD FILE FROM 'C:\MyClassProject\Class1.cs';  
```  

> [!IMPORTANT]
> Il database SQL di Azure non supporta riferimenti a un file.

### <a name="c-changing-the-permissions-of-an-assembly"></a>C. Modifica delle autorizzazioni di un assembly  
 Nell'esempio seguente il set di autorizzazioni dell'assembly `ComplexNumber` viene modificato da SAFE a `EXTERNAL ACCESS`.  
  
```sql  
ALTER ASSEMBLY ComplexNumber WITH PERMISSION_SET = EXTERNAL_ACCESS;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
