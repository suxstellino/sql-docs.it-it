---
title: Funzionalità di accesso non compatibili (AccessToSQL) | Microsoft Docs
description: Informazioni sui possibili problemi di migrazione con le funzionalità del database di Access non compatibili con SQL Server e su come risolverli.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, features incompatible with SQL Azure
- Access databases, features incompatible with SQL Server
- dates
- default expressions
- foreign keys
- hyperlink columns
- incompatible Access features
- indexes
- indexes, length of
- keywords
- primary keys
- reference, incompatible features
- replication columns
- special characters
- unique indexes
- validation rules
ms.assetid: 99d45b9c-e3b9-4d56-8c25-b594b887ace1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 492add4bd73d81c8c25256e35ff1b3de37fc13de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044531"
---
# <a name="incompatible-access-features-accesstosql"></a>Funzionalità di accesso non compatibili (AccessToSQL)
Non tutte le funzionalità del database di Access sono compatibili con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ad esempio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'accesso hanno set diversi di parole chiave riservate. Problemi come questi possono impedire una migrazione riuscita a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Usare la tabella seguente per informazioni sui possibili problemi di migrazione e sulle operazioni che è possibile eseguire su di essi.  
  
## <a name="database-settings-or-features-that-might-affect-migration"></a>Impostazioni o funzionalità del database che potrebbero influire sulla migrazione  
  
|Impostazione o funzionalità del database di Access|Problema di migrazione|  
|--------------------------------------|-------------------|  
|Le tabelle di accesso non hanno indici univoci.|Se viene eseguita la migrazione di una tabella che non dispone di un indice univoco a, non è [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possibile modificare la tabella dopo la migrazione. Questo può causare problemi di compatibilità delle applicazioni.<br /><br />Quando si convertono gli oggetti di database di Access, nella finestra di output vengono elencate le tabelle di accesso prive di indici univoci.<br /><br />È possibile configurare l'accesso per aggiungere una chiave primaria nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella durante la conversione. Per ulteriori informazioni, vedere [Impostazioni progetto (conversione)](./project-settings-conversion-accesstosql.md).|  
|Le tabelle di accesso contengono colonne di replica.|Se viene eseguita la migrazione di una tabella di accesso che include colonne di sistema di replica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le funzionalità di replica Jet verranno interrotte dopo la migrazione.<br /><br />Dopo la migrazione, è consigliabile utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la replica per mantenere copie sincronizzate dei database.|  
|Le tabelle di accesso con indici univoci contengono più valori null.|Le tabelle di accesso con indici univoci con più valori null non possono essere trasferite a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , perché in gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indici univoci non consentono più valori null. La migrazione non riuscirà per queste tabelle.<br /><br />SSMA contrassegna questo problema nei report di valutazione. Per creare un report di valutazione, vedere [valutazione degli oggetti di database di Access per la conversione](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />Se il problema persiste, è necessario assicurarsi che la chiave primaria non includa valori null duplicati. In alternativa, è necessario rimuovere la chiave primaria o gli indici univoci che contengono più valori null.|  
|Le tabelle di accesso contengono valori di data che non rientrano nell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intervallo.|Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo **DateTime** accetta solo le date nell'intervallo compreso tra 1 gennaio 1753 e 31 dicembre 9999. L'accesso accetta le date nell'intervallo compreso tra 1 gennaio 100 e 31 dicembre 9999.<br /><br />SSMA contrassegna questo problema nei report di valutazione. Per creare un report di valutazione, vedere [valutazione degli oggetti di database di Access per la conversione](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />È possibile configurare il modo in cui SSMA risolve le date che non rientrano nell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intervallo. Per ulteriori informazioni, vedere [Impostazioni progetto (migrazione)](./project-settings-migration-accesstosql.md).|  
|Le lunghezze degli indici in Access superano 900 byte.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli indici hanno un limite di 900 byte per le dimensioni totali delle colonne chiave dell'indice. Se le tabelle di accesso utilizzano indici di dimensioni maggiori, in SSMA verrà visualizzato un avviso.<br /><br />Se si continua con la migrazione dei dati, la migrazione potrebbe non riuscire.|  
|I nomi degli oggetti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di accesso sono parole chiave o contengono caratteri speciali.|Accedere a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] insiemi diversi di parole chiave riservate e caratteri speciali. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accetterà gli oggetti denominati usando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parole chiave o che contengono caratteri speciali se si usano identificatori racchiusi tra parentesi o tra virgolette, ad esempio "Select" o [Select]. p. Per ulteriori informazioni, vedere l'argomento relativo agli identificatori delimitati (motore di database) nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione in linea.<br /><br />**Nota:** Per usare le virgolette per delimitare gli identificatori, impostare QUOTED_IDENTIFIER deve essere ON.<br /><br />Ad esempio, `CREATE TABLE [schema](c1 [FOR])` è un'istruzione valida, anche se lo **schema** e **per** sono parole chiave riservate. Inoltre, `CREATE TABLE [xxx*yyy](c1 x&y)` è un'istruzione valida, anche se il nome della tabella e della colonna contiene i caratteri speciali **\& #42;** e **&amp;** .<br /><br />Tutte le query che fanno riferimento a tali oggetti devono utilizzare anche i nomi con parentesi quadre o virgolette. Ad esempio, la query `SELECT * FROM schema` avrà esito negativo. La query corretta è: `SELECT * FROM [schema]` .<br /><br />Quando si converte gli oggetti di database di Access, nel riquadro di output vengono elencate tutte le tabelle di accesso che utilizzano parole chiave o caratteri speciali. È possibile modificare le tabelle in Access, quindi rimuovere e aggiungere di nuovo il database. in alternativa, è possibile modificare le query che fanno riferimento a tali oggetti in modo che le query usino le parentesi quadre o le virgolette per delimitare gli identificatori. Se non si modificano le query, le applicazioni di Access potrebbero restituire errori o problemi.|  
|Le dimensioni dei campi sono diverse nelle relazioni di chiave primaria/chiave esterna.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta la funzionalità Jet del collegamento di colonne con tipi di dati o dimensioni differenti con vincoli di chiave esterna.<br /><br />Quando si convertono gli oggetti di database di Access, nella finestra di output verranno elencati tutti i vincoli di chiave primaria/chiave esterna che non verranno convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile modificare i tipi di dati e le dimensioni delle colonne di accesso in modo che corrispondano, quindi rimuovere e aggiungere nuovamente il database di Access. In alternativa, è possibile eseguire la migrazione dei dati, anche se questi vincoli non verranno creati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|Le tabelle a cui si fa riferimento nelle relazioni di accesso non hanno una chiave primaria né un indice univoco.|L'accesso accetta la relazione tra le tabelle in cui la tabella a cui si fa riferimento non dispone di una chiave primaria o di un indice univoco. Tuttavia, questo non è supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Quando si convertono gli oggetti di database di Access, nella finestra di output vengono elencate tutte le tabelle con relazioni ma senza chiave primaria o indice univoco. È possibile modificare le tabelle per aggiungere chiavi primarie o indici univoci, quindi rimuovere e aggiungere nuovamente il database di Access. In alternativa, è possibile eseguire la migrazione dei dati, anche se la relazione tra le tabelle verrà interruppe.|  
|Le tabelle di accesso contengono colonne collegamento ipertestuale.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta le colonne **collegamento ipertestuale** . Al contrario, le colonne vengono considerate come colonne Memo di accesso. Per impostazione predefinita, queste colonne verranno convertite in colonne **nvarchar (max)** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile personalizzare il mapping. Per ulteriori informazioni, vedere [mapping di tipi di dati di origine e di destinazione](mapping-source-and-target-data-types-accesstosql.md).|  
|Le espressioni delle regole di convalida o predefinite contengono funzioni di accesso che non possono essere convertite in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.|Le espressioni predefinite o le regole di convalida di Access possono includere funzioni di sistema o funzioni definite dall'utente che non eseguono il mapping a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. L'uso di funzioni che non eseguono [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il mapping a o SQL Azure impedirà di caricare le espressioni predefinite o le regole di convalida in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.|  
  
## <a name="see-also"></a>Vedere anche  
[Preparazione dei database di Access per la migrazione](preparing-access-databases-for-migration-accesstosql.md)  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
