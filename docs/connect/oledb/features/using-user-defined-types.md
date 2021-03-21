---
title: Uso di tipi definiti dall'utente | Microsoft Docs
description: OLE DB Driver per SQL Server supporta i tipi definiti dall'utente come tipi binari con le informazioni sui metadati che consentono di gestirli come oggetti.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DBPROPSET_DATASOURCEINFO property set
- UDTs [OLE DB Driver for SQL Server]
- user-defined types [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, user-defined types
- DBPROPSET_SQLSERVERPARAMETER property set
- IColumnsRowset interface
- MSOLEDBSQL, user-defined types
- OLE DB Driver for SQL Server, user-defined types
- data access [OLE DB Driver for SQL Server], user-defined types
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 575d81916ee080a649ff27aa49dc32e802c739bd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751101"
---
# <a name="using-user-defined-types"></a>Utilizzo dei tipi definiti dall'utente (UDT)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] sono stati introdotti i tipi definiti dall'utente (UDT). I tipi definiti dall'utente estendono il sistema di tipi SQL consentendo di archiviare oggetti e strutture di dati personalizzate in un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. I tipi definiti dall'utente possono contenere più tipi di dati e possono assumere comportamenti, differenziandoli dai tipi di dati alias tradizionali costituiti da un singolo tipo di dati di sistema [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. I tipi definiti dall'utente vengono definiti utilizzando uno dei linguaggi supportati da .NET Common Language Runtime (CLR) che generano codice verificabile, ovvero Microsoft Visual C#<sup>®</sup> e Visual Basic<sup>®</sup> .NET. I dati vengono esposti come campi e proprietà di una classe o struttura .NET mentre i comportamenti vengono definiti dai metodi della classe o della struttura.  
  
 Un tipo definito dall'utente (UDT) può essere usato come definizione di colonna di una tabella, come variabile in un batch [!INCLUDE[tsql](../../../includes/tsql-md.md)] o come argomento di una funzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] o di una stored procedure.  
  
## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server  
 Il driver OLE DB per SQL Server supporta i tipi definiti dall'utente (UDT) come tipi binari con le informazioni sui metadati che consentono di gestire i tipi definiti dall'utente (UDT) come oggetti. Le colonne con tipo definito dall'utente vengono esposte come DBTYPE_UDT e i relativi metadati vengono esposti tramite l'interfaccia OLE DB principale **IColumnRowset** e la nuova interfaccia [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md).  
  
> [!NOTE]  
>  Il metodo **IRowsetFind::FindNextRow** non funziona con il tipo di dati UDT. Se il tipo definito dall'utente viene utilizzato come tipo di colonna di ricerca, viene restituito DB_E_BADCOMPAREOP.  
  
### <a name="data-bindings-and-coercions"></a>Associazione dati e coercizioni  
 Nella tabella seguente vengono descritte l'associazione e la coercizione che si verificano quando si utilizzano i tipi di dati elencati con un tipo definito dall'utente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le colonne con tipo definito dall'utente vengono esposte tramite OLE DB Driver per SQL Server come DBTYPE_UDT. È possibile ottenere i metadati tramite i set di righe dello schema appropriati in modo da potere gestire i tipi definiti personalizzati come oggetti.  
  
|Tipo di dati|Al server<br /><br /> **UDT**|Al server<br /><br /> **non UDT**|Dal server<br /><br /> **UDT**|Dal server<br /><br /> **non UDT**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_UDT|Supportato<sup>6</sup>|Errore<sup>1</sup>|Supportato<sup>6</sup>|Errore<sup>5</sup>|  
|DBTYPE_BYTES|Supportato<sup>6</sup>|N/D<sup>2</sup>|Supportato<sup>6</sup>|N/D<sup>2</sup>|  
|DBTYPE_WSTR|Supportato<sup>3,6</sup>|N/D<sup>2</sup>|Supportato<sup>4,6</sup>|N/D<sup>2</sup>|  
|DBTYPE_BSTR|Supportato<sup>3,6</sup>|N/D<sup>2</sup>|Supportato<sup>4</sup>|N/D<sup>2</sup>|  
|DBTYPE_STR|Supportato<sup>3,6</sup>|N/D<sup>2</sup>|Supportato<sup>4,6</sup>|N/D<sup>2</sup>|  
|DBTYPE_IUNKNOWN|Non supportate|N/D<sup>2</sup>|Non supportate|N/D<sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Supportato<sup>6</sup>|N/D<sup>2</sup>|Supportato<sup>4</sup>|N/D<sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|Supportato<sup>3,6</sup>|N/D<sup>2</sup>|N/D|N/D<sup>2</sup>|  
  
 <sup>1</sup>Se viene specificato un tipo di server diverso da DBTYPE_UDT con **ICommandWithParameters::SetParameterInfo** e il tipo di funzione di accesso è DBTYPE_UDT, si verifica un errore quando viene eseguita l'istruzione (DB_E_ERRORSOCCURRED; lo stato del parametro è DBSTATUS_E_BADACCESSOR). In caso contrario, i dati vengono inviati al server, ma il server restituisce un errore indicando che non è possibile eseguire una conversione implicita dal tipo definito dall'utente al tipo di dati del parametro.  
  
 <sup>2</sup>Esula dall'ambito di questo argomento.  
  
 <sup>3</sup>Si verifica la conversione dei dati da stringa esadecimale a dati binari.  
  
 <sup>4</sup>Si verifica la conversione dei dati da dati binari a stringa esadecimale.  
  
 <sup>5</sup>La convalida può verificarsi durante la creazione della funzione di accesso o durante il recupero, l'errore è DB_E_ERRORSOCCURRED, lo stato dell'associazione impostato su DBBINDSTATUS_UNSUPPORTEDCONVERSION.  
  
 <sup>6</sup>Può essere usato BY_REF.  
  
 DBTYPE_NULL e DBTYPE_EMPTY possono essere associati per i parametri di input ma non per i parametri di output o per i risultati. Se vengono associati per i parametri di input, lo stato deve essere impostato su DBSTATUS_S_ISNULL o DBSTATUS_S_DEFAULT.  
  
 DBTYPE_UDT può essere convertito anche in DBTYPE_EMPTY e DBTYPE_NULL, ma DBTYPE_NULL e DBTYPE_EMPTY non possono essere convertiti in DBTYPE_UDT. È coerente con DBTYPE_BYTES.  
  
> [!NOTE]  
>  Viene usata una nuova interfaccia per la gestione dei tipi definiti dall'utente come parametri, **ISSCommandWithParameters**, che eredita da **ICommandWithParameters**. Le applicazioni devono utilizzare questa interfaccia per impostare almeno SSPROP_PARAM_UDT_NAME del set di proprietà DBPROPSET_SQLSERVERPARAMETER per i parametri UDT. In caso contrario, **ICommand::Execute** restituirà DB_E_ERRORSOCCURRED. Questo set di proprietà e questa interfaccia sono descritti più avanti in questo articolo.  
  
 Se un tipo definito dall'utente viene inserito in una colonna che non consente di contenere tutti i relativi dati, **ICommand::Execute** restituirà S_OK con lo stato DB_E_ERRORSOCCURRED.  
  
 Le conversioni dei dati fornite dai servizi principali OLE DB (**IDataConvert**) non sono applicabili a DBTYPE_UDT. Non sono supportate altre associazioni.  
  
### <a name="ole-db-rowset-additions-and-changes"></a>Aggiunte e modifiche ai set di righe OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molti set di righe dello schema OLE DB principale.  
  
#### <a name="the-procedure_parameters-schema-rowset"></a>Set di righe dello schema PROCEDURE_PARAMETERS  
 Al set di righe dello schema PROCEDURE_PARAMETERS sono state effettuate le seguenti aggiunte.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Identificatore del nome in tre parti.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Identificatore del nome in tre parti.|  
|SS_UDT_NAME|DBTYPE_WSTR|Identificatore del nome in tre parti.|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Nome completo dell'assembly che include il nome del tipo e l'identificazione dell'assembly necessaria a cui deve fare riferimento CLR.|  
  
#### <a name="the-sql_assemblies-schema-rowset"></a>Set di righe dello schema SQL_ASSEMBLIES  
 Il driver OLE DB per SQL Server espone un nuovo set di righe dello schema specifico del provider che descrive i tipi definiti dall'utente registrati. Il server ASSEMBLY può essere specificato come DBTYPE_WSTR, ma non è presente nel set di righe. Se non viene specificato, il set di righe imposterà come valore predefinito il server corrente. Il set di righe dello schema SQL_ASSEMBLIES viene definito nella tabella seguente:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|Nome del catalogo dell'assembly contenente il tipo.|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|Nome dello schema o nome del proprietario dell'assembly contenente il tipo. Sebbene l'ambito degli assembly sia il database e non lo schema, gli assembly dispongono di un proprietario qui rappresentato.|  
|ASSEMBLY_NAME|DBTYPE_WSTR|Nome dell'assembly contenente il tipo.|  
|ASSEMBLY_ID|DBTYPE_UI4|ID oggetto dell'assembly contenente il tipo.|  
|PERMISSION_SET|DBTYPE_WSTR|Valore che indica l'ambito di accesso per l'assembly. I valori includono "SAFE", "EXTERNAL_ACCESS" e "UNSAFE".|  
|ASSEMBLY_BINARY|DBTYPE_BYTES|Rappresentazione binaria dell'assembly.|  
  
#### <a name="the-sql_assemblies_-dependencies-schema-rowset"></a>Set di righe dello schema SQL_ASSEMBLIES_ DEPENDENCIES  
 Il driver OLE DB per SQL Server espone un nuovo set di righe dello schema specifico del provider che descrive le dipendenze dell'assembly per un server specificato. ASSEMBLY_SERVER può essere specificato dal chiamante come DBTYPE_WSTR, ma non è presente nel set di righe. Se non viene specificato, il set di righe imposterà come valore predefinito il server corrente. Il set di righe dello schema SQL_ASSEMBLY_DEPENDENCIES viene definito nella tabella seguente:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|Nome del catalogo dell'assembly contenente il tipo.|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|Nome dello schema o nome del proprietario dell'assembly contenente il tipo. Sebbene l'ambito degli assembly sia il database e non lo schema, gli assembly dispongono di un proprietario qui rappresentato.|  
|ASSEMBLY_ID|DBTYPE_UI4|ID oggetto dell'assembly.|  
|REFERENCED_ASSEMBLY_ID|DBTYPE_UI4|ID oggetto dell'assembly a cui viene fatto riferimento.|  
  
#### <a name="the-sql_user_types-schema-rowset"></a>Set di righe dello schema SQL_USER_TYPES  
 Il driver OLE DB per SQL Server espone il nuovo set di righe dello schema, SQL_USER_TYPES, che descrive quando vengono aggiunti i tipi definiti dall'utente registrati per un server specificato. UDT_SERVER deve essere specificato dal chiamante come DBTYPE_WSTR, ma non è presente nel set di righe. Il set di righe dello schema SQL_USER_TYPES viene definito nella tabella seguente.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|UDT_CATALOGNAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente questa proprietà è una stringa che specifica il nome del catalogo in cui viene definito il tipo definito dall'utente.|  
|UDT_SCHEMANAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente questa proprietà è una stringa che specifica il nome dello schema in cui viene definito il tipo definito dall'utente.|  
|UDT_NAME|DBTYPE_WSTR|Nome dell'assembly contenente la classe UDT.|  
|UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Il nome completo del tipo (AQN) include il nome del tipo preceduto dallo spazio dei nomi se applicabile.|  
  
#### <a name="the-columns-schema-rowset"></a>Set di righe dello schema COLUMNS  
 Le aggiunte al set di righe dello schema COLUMNS includono le colonne seguenti:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente questa proprietà è una stringa che specifica il nome del catalogo in cui viene definito il tipo definito dall'utente.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente questa proprietà è una stringa che specifica il nome dello schema in cui viene definito il tipo definito dall'utente.|  
|SS_UDT_NAME|DBTYPE_WSTR|Nome del tipo definito dall'utente.|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Il nome completo del tipo (AQN) include il nome del tipo preceduto dallo spazio dei nomi se applicabile.|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>Aggiunte e modifiche ai set di proprietà OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molti set di proprietà dello schema OLE DB principale.  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>Set di proprietà DBPROPSET_SQLSERVERPARAMETER  
 Ai fini del supporto dei tipi definiti dall'utente (UDT) tramite OLE DB, nel driver OLE DB per SQL Server è stato implementato il nuovo set di proprietà DBPROPSET_SQLSERVERPARAMETER che contiene i valori seguenti:  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|SSPROP_PARAM_UDT_CATALOGNAME|DBTYPE_WSTR|Identificatore del nome in tre parti.<br /><br /> Per i parametri UDT questa proprietà è una stringa che specifica il nome del catalogo in cui viene definito il tipo definito dall'utente.|  
|SSPROP_PARAM_UDT_SCHEMANAME|DBTYPE_WSTR|Identificatore del nome in tre parti.<br /><br /> Per i parametri UDT questa proprietà è una stringa che specifica il nome dello schema in cui viene definito il tipo definito dall'utente.|  
|SSPROP_PARAM_UDT_NAME|DBTYPE_WSTR|Identificatore del nome in tre parti.<br /><br /> Per le colonne con tipo definito dall'utente questa proprietà è una stringa che specifica il nome costituito da una sola parte del tipo definito dall'utente.|  
  
 SSPROP_PARAM_UDT_NAME è obbligatorio. SSPROP_PARAM_UDT_CATALOGNAME e SSPROP_PARAM_UDT_SCHEMANAME sono facoltativi. Se una delle proprietà viene specificata in modo non corretto, verrà restituito DB_E_ERRORSINCOMMAND. Se non vengono specificati SSPROP_PARAM_UDT_CATALOGNAME e SSPROP_PARAM_UDT_SCHEMANAME, il tipo definito dall'utente dovrà essere definito nello stesso database e nello stesso schema della tabella. Se la definizione UDT non è presente nello stesso schema della tabella, ma nello stesso database, sarà necessario specificare SSPROP_PARAM_UDT_SCHEMANAME. Se la definizione UDT è presente in un database diverso, sarà necessario specificare SSPROP_PARAM_UDT_CATALOGNAME e SSPROP_PARAM_UDT_SCHEMANAME.  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>Set di proprietà DBPROPSET_SQLSERVERCOLUMN  
 Ai fini del supporto della creazione di tabelle nell'interfaccia **ITableDefinition**, nel driver OLE DB per SQL Server sono state aggiunte le tre nuove colonne seguenti al set di proprietà DBPROPSET_SQLSERVERCOLUMN.  
  
|Nome|Descrizione|Type|Descrizione|  
|----------|-----------------|----------|-----------------|  
|SSPROP_COL_UDT_CATALOGNAME|UDT_CATALOGNAME|VT_BSTR|Per le colonne di tipo DBTYPE_UDT questa proprietà è una stringa che specifica il nome del catalogo in cui viene definito il tipo definito dall'utente.|  
|SSPROP_COL_UDT_SCHEMANAME|UDT_SCHEMANAME|VT_BSTR|Per le colonne di tipo DBTYPE_UDT questa proprietà è una stringa che specifica il nome dello schema in cui viene definito il tipo definito dall'utente.|  
|SSPROP_COL_UDT_NAME|UDT_NAME|VT_BSTR|Per le colonne di tipo DBTYPE_UDT questa proprietà è una stringa che specifica il nome costituito da una singola parte del tipo definito dall'utente. Per gli altri tipi di colonna questa proprietà restituisce una stringa vuota.|  
  
> [!NOTE]  
>  I tipi definiti dall'utente non sono presenti nel set di righe dello schema PROVIDER_TYPES. Tutte le colonne dispongono dell'accesso in lettura e scrittura.  
  
 ADO farà riferimento a queste proprietà tramite la voce corrispondente nella colonna Descrizione.  
  
 SSPROP_COL_UDTNAME è obbligatorio. SSPROP_COL_UDT_CATALOGNAME e SSPROP_COL_UDT_SCHEMANAME sono facoltativi. Se una delle proprietà viene specificata in modo non corretto, verrà restituito **DB_E_ERRORSINCOMMAND**.  
  
 Se non vengono specificati SSPROP_COL_UDT_CATALOGNAME e SSPROP_COL_UDT_SCHEMANAME, il tipo definito dall'utente dovrà essere definito nello stesso database e nello stesso schema della tabella.  
  
 Se la definizione UDT non è presente nello stesso schema della tabella, ma nello stesso database, sarà necessario specificare SSPROP_COL_UDT_SCHEMANAME.  
  
 Se la definizione UDT è presente in un database diverso, sarà necessario specificare SSPROP_COL_UDT_CATALOGNAME e SSPROP_COL_UDT_SCHEMANAME.  
  
### <a name="ole-db-interface-additions-and-changes"></a>Aggiunte e modifiche alle interfacce OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molte interfacce dello schema OLE DB principale.  
  
#### <a name="the-isscommandwithparameters-interface"></a>Interfaccia ISSCommandWithParameters  
 Ai fini del supporto dei tipi definiti dall'utente (UDT) tramite OLE DB, nel driver OLE DB per SQL Server sono state implementate alcune modifiche, ad esempio l'aggiunta dell'interfaccia **ISSCommandWithParameters**. Questa nuova interfaccia eredita dall'interfaccia OLE DB principale **ICommandWithParameters**. Oltre ai tre metodi ereditati da **ICommandWithParameters**, **GetParameterInfo**, **MapParameterNames** e **SetParameterInfo**, **ISSCommandWithParameters** fornisce i metodi **GetParameterProperties** e **SetParameterProperties** usati per gestire i tipi di dati specifici del server.  
  
> [!NOTE]  
>  L'interfaccia **ISSCommandWithParameters** usa anche la nuova struttura SSPARAMPROPS.  
  
#### <a name="the-icolumnsrowset-interface"></a>Interfaccia IColumnsRowset  
 Oltre all'interfaccia **ISSCommandWithParameters**, nel driver OLE DB per SQL Server sono stati aggiunti nuovi valori al set di righe restituito dalla chiamata al metodo **IColumnsRowset::GetColumnRowset**, inclusi gli elementi seguenti.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_UDT_CATALOGNAME|DBTYPE_WSTR|Identificatore del nome di catalogo del tipo definito dall'utente.|  
|DBCOLUMN_SS_UDT_SCHEMANAME|DBTYPE_WSTR|Identificatore del nome dello schema del tipo definito dall'utente.|  
|DBCOLUMN_SS_UDT_NAME|DBTYPE_WSTR|Identificatore del nome del tipo definito dall'utente.|  
|DBCOLUMN_SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Nome completo dell'assembly che include il nome del tipo e l'identificazione dell'assembly necessaria a cui deve fare riferimento CLR.|  
  
 È possibile differenziare una colonna con tipo definito dall'utente del server dagli altri tipi binari quando DBCOLUMN_TYPE è impostato su DBTYPE_UDT analizzando i metadati UDT aggiunti specificati nella tabella precedente. Se tali dati sono parzialmente completi, il tipo di server sarà un tipo definito dall'utente. Per i tipi di server non UDT queste colonne vengono sempre restituite come NULL.  
 
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
