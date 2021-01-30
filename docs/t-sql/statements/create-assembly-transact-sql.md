---
description: CREATE ASSEMBLY (Transact-SQL)
title: CREATE ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASSEMBLY
- CREATE ASSEMBLY
- CREATE_ASSEMBLY_TSQL
- ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], validating
- validating assemblies
- CREATE ASSEMBLY statement
- assemblies [CLR integration], creating
ms.assetid: d8d1d245-c2c3-4325-be52-4fc1122c2079
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: c75155260afad6f4e7938aed1532e051b16e0a91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204966"
---
# <a name="create-assembly-transact-sql"></a>CREATE ASSEMBLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Crea un modulo di applicazione gestita contenente metadati di classe e codice gestito come un oggetto in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Facendo riferimento a questo modulo, è possibile creare nel database funzioni CLR (Common Language Runtime), stored procedure, trigger, funzioni di aggregazione definite dall'utente e tipi definiti dall'utente.  
  
> [!WARNING]
>  CLR usa la Sicurezza dall'accesso di codice (CAS, Code Access Security) in .NET Framework, non più supportata come limite di sicurezza. Un assembly CLR creato con `PERMISSION_SET = SAFE` potrebbe essere in grado di accedere alle risorse di sistema esterne, chiamare codice non gestito e acquisire privilegi sysadmin. A partire da [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], viene introdotta un'opzione `sp_configure` denominata `clr strict security` per migliorare la sicurezza degli assembly CLR. `clr strict security` è abilitata per impostazione predefinita e considera gli assembly CLR `SAFE` e `UNSAFE` come se fossero contrassegnati `EXTERNAL_ACCESS`. È possibile disabilitare l'opzione `clr strict security` per la compatibilità con le versioni precedenti, ma questa operazione è sconsigliata. Microsoft consiglia che tutti gli assembly siano firmati con un certificato o una chiave asimmetrica con un account di accesso corrispondente che disponga dell'autorizzazione `UNSAFE ASSEMBLY` nel database master. Per altre informazioni, vedere [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE ASSEMBLY assembly_name  
[ AUTHORIZATION owner_name ]  
FROM { <client_assembly_specifier> | <assembly_bits> [ ,...n ] }  
[ WITH PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE } ]  
[ ; ]  
<client_assembly_specifier> :: =  
        '[\\computer_name\]share_name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```  
  
## <a name="arguments"></a>Argomenti  
 *assembly_name*  
 Nome dell'assembly. Il nome deve essere univoco all'interno del database e un [identificatore](../../relational-databases/databases/database-identifiers.md) valido.  
  
 AUTHORIZATION *owner_name*  
 Viene specificato il nome di un utente o un ruolo come proprietario dell'assembly. *owner_name* deve essere il nome di un ruolo di cui l'utente corrente è membro oppure l'utente corrente deve avere l'autorizzazione IMPERSONATE per *owner_name*. Se viene omesso, la proprietà viene assegnata all'utente corrente.  
  
 \<client_assembly_specifier>  
Viene specificato il percorso locale o di rete in cui viene posizionato l'assembly caricato e il nome del file di manifesto che corrisponde all'assembly.  \<client_assembly_specifier> può essere espresso come stringa fissa o espressione tramite la quale viene restituita una stringa fissa, con variabili. CREATE ASSEMBLY non supporta il caricamento di assembly in più moduli. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono cercati anche tutti gli assembly dipendenti di questo assembly nello stesso percorso e tali assembly vengono caricati con lo stesso proprietario dell'assembly a livello di radice. Se tali assembly dipendenti non vengono trovati e non sono già caricati nel database corrente, CREATE ASSEMBLY ha esito negativo. Se gli assembly dipendenti sono già caricati nel database corrente, il proprietario degli assembly deve corrispondere al proprietario dell'assembly appena creato.

> [!IMPORTANT]
> Database SQL di Azure non supporta la creazione di un assembly da un file.
  
 \<client_assembly_specifier> non può essere specificato se l'utente connesso viene rappresentato.  
  
 \<assembly_bits>  
 Elenco dei valori binari che costituiscono l'assembly e i relativi assembly dipendenti. Il primo valore dell'elenco viene considerato l'assembly a livello di radice. I valori corrispondenti agli assembly dipendenti possono essere specificati in qualsiasi ordine. I valori non corrispondenti alle dipendenze dell'assembly radice vengono ignorati.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 *varbinary_literal*  
 Valore letterale **varbinary**.  
  
 *varbinary_expression*  
 Espressione di tipo **varbinary**.  
  
 PERMISSION_SET { **SAFE** | EXTERNAL_ACCESS | UNSAFE }  
> [!IMPORTANT]
>  L'opzione `PERMISSION_SET` è influenzata dall'opzione `clr strict security` descritta nell'avviso di apertura. Quando l'opzione `clr strict security` è abilitata, tutti gli assembly vengono considerati come `UNSAFE`.
 
 Specifica un set di autorizzazioni di accesso per il codice che vengono concesse all'assembly quando vi accede [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se è omesso, SAFE viene applicato come valore predefinito.  
  
 È consigliabile usare SAFE. SAFE è il set di autorizzazioni più restrittivo. Il codice eseguito da un assembly con autorizzazioni SAFE non può accedere alle risorse di sistema esterne, ad esempio i file, la rete, le variabili di ambiente o il Registro di sistema.  
  
 EXTERNAL_ACCESS consente agli assembly di accedere a determinate risorse di sistema esterne, ad esempio i file, la rete, le variabili di ambiente e il Registro di sistema.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 UNSAFE consente agli assembly privilegi di accesso senza limitazioni, sia all'interno che all'esterno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il codice eseguito all'interno di un assembly UNSAFE può chiamare il codice non gestito.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
> [!IMPORTANT]  
>  SAFE è l'impostazione di autorizzazioni consigliata per gli assembly che eseguono attività di calcolo e di gestione di dati senza accedere alle risorse all'esterno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
>   
>  È consigliabile usare EXTERNAL_ACCESS per gli assembly che accedono alle risorse all'esterno di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli assembly EXTERNAL_ACCESS includono l'affidabilità e la scalabilità degli assembly SAFE, ma dal punto di vista della sicurezza sono simili agli assembly UNSAFE. Ciò è dovuto al fatto che il codice negli assembly EXTERNAL_ACCESS viene eseguito con l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per impostazione predefinita e con tale account accede a risorse esterne, a meno che il codice non rappresenti esplicitamente il chiamante. Pertanto, l'autorizzazione per creare gli assembly EXTERNAL_ACCESS deve essere concessa solo agli account di accesso che si ritengono attendibili per l'esecuzione del codice con l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni sulla rappresentazione, vedere [Sicurezza per l'integrazione con CLR](../../relational-databases/clr-integration/security/clr-integration-security.md).  
>   
>  Specificando UNSAFE, al codice nell'assembly viene concessa l'assoluta libertà di eseguire operazioni nello spazio di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che potrebbero compromettere l'affidabilità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli assembly UNSAFE possono anche compromettere il sistema di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o del CLR. Le autorizzazioni UNSAFE devono essere concesso solo agli assembly altamente attendibili. Solo i membri del ruolo predefinito del server **sysadmin** possono creare e modificare gli assembly UNSAFE.  
  
 Per altre informazioni sui set di autorizzazioni per gli assembly, vedere [Progettazione di assembly](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="remarks"></a>Commenti  
 CREATE ASSEMBLY carica un assembly compilato precedentemente come file con estensione dll da codice gestito da usare all'interno di un'istanza di SQL Server.  
 
Quando abilitata, l'opzione `PERMISSION_SET` nelle istruzioni `CREATE ASSEMBLY` e `ALTER ASSEMBLY` viene ignorata durante l'esecuzione, ma le opzioni `PERMISSION_SET` vengono mantenute nei metadati. Ignorando l'opzione, si ridurranno al minimo le interruzioni nelle istruzioni di codice esistenti.
 
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non consente la registrazione di versioni diverse di un assembly con lo stesso nome, lingua e chiave pubblica.  
  
Durante il tentativo di accedere all'assembly specificato in \<client_assembly_specifier>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rappresenta il contesto di sicurezza dell'account di accesso di Windows corrente. Se \<client_assembly_specifier> specifica un percorso di rete (percorso UNC), la rappresentazione dell'account di accesso corrente non viene riportata nel percorso di rete a causa dei limiti di delega. In questo caso, l'accesso viene effettuato tramite il contesto di sicurezza dell'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md).
  
 Oltre all'assembly radice specificato da *assembly_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di caricare tutti gli assembly a cui fa riferimento l'assembly radice che viene caricato. Se un assembly con riferimenti è già caricato nel database a causa di un'istruzione CREATE ASSEMBLY precedente, questo assembly non viene caricato ma è disponibile per l'assembly radice. Se un assembly dipendente non è stato caricato precedentemente, ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di individuare il relativo file di manifesto nella directory di origine, CREATE ASSEMBLY restituisce un errore.  
  
 Se un assembly dipendente a cui viene fatto riferimento dall'assembly radice non è già nel database e viene caricato implicitamente con l'assembly radice, avrà lo stesso set di autorizzazioni dell'assembly a livello di radice. Se gli assembly dipendenti devono essere creati tramite un set di autorizzazioni diverso rispetto all'assembly a livello di radice, essi devono essere caricati esplicitamente prima dell'assembly a livello di radice con il set di autorizzazioni appropriato.  
  
## <a name="assembly-validation"></a>Convalida di assembly  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue controlli sui file binari degli assembly caricati tramite l'istruzione CREATE ASSEMBLY per garantire quanto segue:  
  
-   Il file binario di assembly è ben formato con metadati e segmenti di codice validi e i segmenti di codice contengono istruzioni MSIL (Microsoft Intermediate Language) valide.  
  
-   Il set di assembly di sistema cui fa riferimento è uno degli assembly seguenti supportati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: Microsoft.Visualbasic.dll, Mscorlib.dll, System.Data.dll, System.dll, System.Xml.dll, Microsoft.Visualc.dll, Custommarshallers.dll, System.Security.dll, System.Web.Services.dll , System.Data.SqlXml.dll, SystemCore.dll System.Data.SqlXml.dll. Può fare riferimento anche ad altri assembly di sistema, ma è necessario che questi siano registrati nel database in modo esplicito.  
  
-   Per gli assembly creati tramite i set di autorizzazioni SAFE o EXTERNAL ACCESS:  
  
    -   Il codice di assembly deve essere indipendente dai tipi. L'indipendenza dai tipi viene stabilita eseguendo CLR Verifier sull'assembly.  
  
    -   L'assembly non deve contenere membri dati statici all'interno delle relative classi a meno che non siano contrassegnati come di sola lettura.  
  
    -   Le classi nell'assembly non possono contenere metodi del finalizzatore.  
  
    -   Le classi o i metodi dell'assembly devono essere annotati solo con gli attributi del codice consentiti. Per altre informazioni, vedere [Attributi personalizzati per routine CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).  
  
 Oltre ai controlli precedenti eseguiti durante l'esecuzione di CREATE ASSEMBLY, vengono eseguiti controlli aggiuntivi in fase di esecuzione del codice dell'assembly:  
  
-   La chiamata ad alcune API [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] che richiedono un'autorizzazione di accesso al codice specifica può non riuscire se il set di autorizzazioni dell'assembly non include questa autorizzazione.  
  
-   Per gli assembly SAFE e EXTERNAL_ACCESS, ogni tentativo di chiamare API [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] annotate con determinati attributi HostProtectionAttributes non riuscirà.  
  
 Per altre informazioni, vedere [Progettazione di assembly](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE ASSEMBLY.  
  
 Se si specifica PERMISSION_SET = EXTERNAL_ACCESS, sarà necessaria l'autorizzazione **EXTERNAL ACCESS ASSEMBLY** nel server. Se si specifica PERMISSION_SET = UNSAFE, sarà necessaria l'autorizzazione **UNSAFE ASSEMBLY** nel server.  
  
 L'utente deve essere il proprietario degli assembly cui viene fatto riferimento dall'assembly che sta per essere caricato se gli assembly esistono già nel database. Per caricare un assembly usando un percorso di file, l'utente corrente deve avere un account di accesso autenticato in Windows o essere un membro del ruolo predefinito del server **sysadmin**. L'account di accesso di Windows dell'utente che esegue CREATE ASSEMBLY deve disporre dell'autorizzazione di lettura per la cartella condivisa e per i file che vengono caricati nell'istruzione.  

### <a name="permissions-with-clr-strict-security"></a>Autorizzazioni con CLR strict security    
Sono necessarie le autorizzazioni seguenti per creare un assembly CLR con `CLR strict security` abilitata:

- L'utente deve disporre dell'autorizzazione `CREATE ASSEMBLY`  
- Inoltre, una delle condizioni seguenti deve essere rispettata:  
  - L'assembly è firmato con un certificato o una chiave asimmetrica con un account di accesso corrispondente con l'autorizzazione `UNSAFE ASSEMBLY` nel server. È consigliabile firmare l'assembly.  
  - La proprietà `TRUSTWORTHY` del database è impostata su `ON` e il database è di proprietà di un accesso che dispone dell'autorizzazione `UNSAFE ASSEMBLY` nel server. Questa opzione non è consigliata.  
  
 Per altre informazioni sui set di autorizzazioni per gli assembly, vedere [Progettazione di assembly](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="example-a-creating-an-assembly-from-a-dll"></a>Esempio A: Creazione di un assembly da un file DLL  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Nell'esempio seguente viene presupposto che le applicazioni di esempio di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] siano installate nel percorso predefinito del computer locale e che l'applicazione di esempio HelloWorld.csproj sia compilata. Per altre informazioni, vedere [Esempio Hello World](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).  
  
```sql  
CREATE ASSEMBLY HelloWorld   
FROM '<system_drive>:\Program Files\Microsoft SQL Server\100\Samples\HelloWorld\CS\HelloWorld\bin\debug\HelloWorld.dll'  
WITH PERMISSION_SET = SAFE;  
```  

> [!IMPORTANT]
> Database SQL di Azure non supporta la creazione di un assembly da un file.
  
### <a name="example-b-creating-an-assembly-from-assembly-bits"></a>Esempio B: Creazione di un assembly dai bit dell'assembly  
  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Sostituire i bit di esempio, che non sono completi o validi, con i bit dell'assembly.  
  
```sql  
CREATE ASSEMBLY HelloWorld  
    FROM 0x4D5A900000000000  
WITH PERMISSION_SET = SAFE;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Scenari di utilizzo ed esempi per l'integrazione con CLR &#40;Common Language Runtime&#41;](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))  
  
