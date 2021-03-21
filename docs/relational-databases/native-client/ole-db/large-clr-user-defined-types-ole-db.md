---
description: Tipi di User-Defined CLR di grandi dimensioni in SQL Server Native Client (OLE DB)
title: Tipi CLR definiti dall'utente di grandi dimensioni (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- large CLR user-defined types [OLE DB]
ms.assetid: 4bf12058-0534-42ca-a5ba-b1c23b24d90f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d4d7c3b854f49a426dc5e788e1fc1ab44eb28b35
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753231"
---
# <a name="large-clr-user-defined-types-in-sql-server-native-client-ole-db"></a>Tipi di User-Defined CLR di grandi dimensioni in SQL Server Native Client (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono illustrate le modifiche apportate a OLE DB in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client per supportare i tipi CLR (Common Language Runtime) definiti dall'utente di grandi dimensioni.  
  
 Per ulteriori informazioni sul supporto di tipi definiti dall'utente CLR di grandi dimensioni in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client, vedere [tipi di User-Defined CLR di grandi dimensioni](../../../relational-databases/native-client/features/large-clr-user-defined-types.md). Per un esempio, vedere [Usare tipi definiti dall'utente CLR di grandi dimensioni &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/use-large-clr-udts-ole-db.md).  
  
## <a name="data-format"></a>Formato dati  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client viene utilizzato ~0 per rappresentare la lunghezza dei valori con dimensioni illimitate nel caso di tipi di oggetti LOB. ~ 0 rappresenta anche le dimensioni dei tipi CLR definiti dall'utente che superano 8.000 byte.  
  
 Nella tabella seguente viene illustrato il mapping dei tipi di dati nei parametri e nei set di righe:  
  
|Tipo di dati di SQL Server|Tipo di dati OLE DB|Layout in memoria|valore|  
|--------------------------|----------------------|-------------------|-----------|  
|tipo CLR definito dall'utente|DBTYPE_UDT|BYTE[](matrice di byte\)|132 (oledb.h)|  
  
 I valori dei tipi definiti dall'utente vengono rappresentati come matrici di byte. Le conversioni da e verso le stringhe esadecimali sono supportate. I valori letterali vengono rappresentati come stringhe esadecimali con il prefisso "0x". Una stringa esadecimale è la rappresentazione testuale di dati binari in base 16. Un esempio al riguardo è dato da una conversione dal tipo di dati del server **varbinary(10)** a DBTYPE_STR, che determina una rappresentazione esadecimale di 20 caratteri in cui ogni coppia di caratteri rappresenta un solo byte.  
  
## <a name="parameter-properties"></a>Proprietà dei parametri  
 Il set di proprietà DBPROPSET_SQLSERVERPARAMETER supporta i tipi definiti dall'utente tramite OLE DB. Per altre informazioni, vedere [Uso dei tipi definiti dall'utente](~/relational-databases/native-client/features/using-user-defined-types.md).  
  
## <a name="column-properties"></a>Proprietà delle colonne  
 Il set di proprietà DBPROPSET_SQLSERVERCOLUMN supporta la creazione di tabelle tramite OLE DB. Per altre informazioni, vedere [Uso dei tipi definiti dall'utente](~/relational-databases/native-client/features/using-user-defined-types.md).  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>Mapping dei tipi di dati in ITableDefinition::CreateTable  
 Le informazioni seguenti vengono usate nelle strutture **DBCOLUMNDESC** impiegate da ITableDefinition::CreateTable quando sono necessarie colonne con tipo definito dall'utente:  
  
|Tipo di dati OLE DB (*wType*)|*pwszTypeName*|Tipo di dati di SQL Server|*rgPropertySets*|  
|----------------------------------|--------------------|--------------------------|----------------------|  
|DBTYPE_UDT|Ignorato|UDT|Deve includere un set di proprietà DBPROPSET_SQLSERVERCOLUMN.|  
  
## <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 Di seguito sono riportate le informazioni restituite nella struttura DBPARAMINFO tramite **prgParamInfo**.  
  
|Tipo di parametro|*wType*|*ulParamSize*|*bPrecision*|*bScale*|*dwFlags* DBPARAMFLAGS_ISLONG|  
|--------------------|-------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|"DBTYPE_UDT"|*n*|Non definito|Non definito|deselezionare|  
|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|"DBTYPE_UDT"|~0|Non definito|Non definito|set|  
  
## <a name="icommandwithparameterssetparameterinfo"></a>ICommandWithParameters::SetParameterInfo  
 Le informazioni specificate nella struttura DBPARAMBINDINFO devono essere conformi agli elementi seguenti:  
  
|Tipo di parametro|*pwszDataSourceType*|*ulParamSize*|*bPrecision*|*bScale*|*dwFlags* DBPARAMFLAGS_ISLONG|  
|--------------------|--------------------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|DBTYPE_UDT|*n*|Ignorato|Ignorato|Deve essere impostato se è necessario passare il parametro utilizzando DBTYPE_IUNKNOWN.|  
|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|DBTYPE_UDT|~0|Ignorato|Ignorato|Ignorato|  
  
## <a name="isscommandwithparameters"></a>ISSCommandWithParameters  
 Le applicazioni usano **ISSCommandWithParameters** per ottenere e impostare le proprietà dei parametri definite nella sezione Proprietà dei parametri.  
  
## <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 Le colonne restituite sono le seguenti:  
  
|Tipo di colonna|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE|DBCOLUMN_FLAGS_ISLONG|DBCOLUMNS_ISSEARCHABLE|DBCOLUMN_OCTETLENGTH|  
|-----------------|--------------------|--------------------------|-------------------------|---------------------|-----------------------------|-----------------------------|---------------------------|  
|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|DBTYPE_UDT|*n*|NULL|NULL|Clear|DB_ALL_EXCEPT_LIKE|n|  
|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|DBTYPE_UDT|~0|NULL|NULL|Configurazione|DB_ALL_EXCEPT_LIKE|0|  
  
 Per i tipi definiti dall'utente vengono definite anche le colonne seguenti:  
  
|Identificatore di colonna|Type|Descrizione|  
|-----------------------|----------|-----------------|  
|DBCOLUMN_UDT_CATALOGNAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome del catalogo in cui è indicato il tipo definito dall'utente.|  
|DBCOLUMN_UDT_SCHEMANAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome dello schema in cui è indicato il tipo definito dall'utente.|  
|DBCOLUMN_UDT_NAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome composto da una parte del tipo definito dall'utente.|  
|DBCOLUMN_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome completo del tipo definito dall'utente. Il nome completo del tipo di assembly consente di creare un'istanza di un oggetto del tipo in questione utilizzando il metodo Type.GetType.|  
  
## <a name="icolumnsinfogetcolumninfo"></a>IColumnsInfo::GetColumnInfo  
 Di seguito sono riportate le informazioni restituite nella struttura DBCOLUMNINFO.  
  
|Tipo di parametro|*wType*|*ulColumnSize*|*bPrecision*|*bScale*|*dwFlags*<br /><br /> DBCOLUMNFLAGS_ISLONG|  
|--------------------|-------------|--------------------|------------------|--------------|-----------------------------------------|  
|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|DBTYPE_UDT|*n*|~0|~0|Clear|  
|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|DBTYPE_UDT|~0|~0|~0|Configurazione|  
  
## <a name="columns-rowset-schema-rowsets"></a>Set di righe COLUMNS (set di righe dello schema)  
 Per i tipi definiti dall'utente vengono restituiti i valori di colonna seguenti:  
  
|Tipo di colonna|DATA_TYPE|COLUMN_FLAGS, DBCOLUMFLAGS_ISLONG|CHARACTER_OCTET_LENGTH|  
|-----------------|----------------|-----------------------------------------|------------------------------|  
|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|DBTYPE_UDT|Clear|*n*|  
|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|DBTYPE_UDT|Configurazione|0|  
  
 Per i tipi definiti dall'utente vengono definite le colonne aggiuntive seguenti:  
  
|Identificatore di colonna|Type|Descrizione|  
|-----------------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome del catalogo in cui è indicato il tipo definito dall'utente.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome dello schema in cui è indicato il tipo definito dall'utente.|  
|SS_UDT_NAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome composto da una parte del tipo definito dall'utente.|  
|SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Per le colonne con tipo definito dall'utente, il nome completo del tipo definito dall'utente. Il nome completo del tipo di assembly consente di creare un'istanza di un oggetto del tipo in questione utilizzando il metodo Type.GetType.|  
  
 Se riferito al set di righe PROCEDURE_PARAMETERS, DATA_TYPE contiene gli stessi valori del set di righe dello schema COLUMNS e TYPE_NAME contiene il tipo definito dall'utente. Vengono definite anche le stesse colonne aggiuntive.  
  
 I tipi definiti dall'utente non sono presenti nel set di righe dello schema PROVIDER_TYPES.  
  
## <a name="bindings-and-conversions"></a>Associazioni e conversioni  
  
|Tipo di associazione dati|Tipo definito dall'utente al server|Tipo non definito dall'utente al server|Tipo definito dall'utente dal server|Tipo non definito dall'utente dal server|  
|----------------------|-------------------|------------------------|---------------------|--------------------------|  
|DBTYPE_UDT|Supportato (5)|Errore (1)|Supportato (5)|Errore (4)|  
|DBTYPE_BYTES|Supportato (5)|N/D|Supportato (5)|N/D|  
|DBTYPE_WSTR|Supportati (2), (5)|N/D|Supportato (3), (5), (6)|N/D|  
|DBTYPE_BSTR|Supportati (2), (5)|N/D|Supportato (3), (5)|N/D|  
|DBTYPE_STR|Supportati (2), (5)|N/D|Supportato (3), (5)|N/D|  
|DBTYPE_IUNKNOWN|Supportato (6)|N/D|Supportato (6)|N/D|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Supportato (5)|N/D|Supportato (3), (5)|N/D|  
|DBTYPE_VARIANT (VT_BSTR)|Supportati (2), (5)|N/D|N/D|N/D|  
  
### <a name="key-to-symbols"></a>Descrizione dei simboli  
  
|Simbolo|Significato|  
|------------|-------------|  
|1|Se viene specificato un tipo di dati del server diverso da DBTYPE_UDT con **ICommandWithParameters::SetParameterInfo** e il tipo di funzione di accesso è DBTYPE_UDT, si verifica un errore quando viene eseguita l'istruzione.  L'errore e lo stato del parametro saranno rispettivamente DB_E_ERRORSOCCURRED e DBSTATUS_E_BADACCESSOR.<br /><br /> È un errore specificare un parametro con tipo definito dall'utente per un parametro del server che non presenta un tipo definito dall'utente.|  
|2|I dati vengono convertiti dal formato di stringa esadecimale nel formato binario.|  
|3|I dati vengono convertiti dal formato binario nel formato di stringa esadecimale.|  
|4|È possibile che venga eseguita la convalida quando si usa **CreateAccessor** o **GetNextRows**. L'errore corrispondente è DB_E_ERRORSOCCURRED. Lo stato di associazione viene impostato su DBBINDSTATUS_UNSUPPORTEDCONVERSION.|  
|5|È possibile utilizzare BY_REF.|  
|6|I parametri con tipo definito dall'utente possono essere associati come DBTYPE_IUNKNOWN in DBBINDING. L'associazione a DBTYPE_IUNKNOWN indica che l'applicazione richiede l'elaborazione dei dati come flusso usando l'interfaccia ISequentialStream. Quando un consumer specifica *wType* in un'associazione come tipo DBTYPE_IUNKNOWN e la colonna o il parametro di output corrispondente della stored procedure è un tipo definito dall'utente, SQL Server Native Client restituirà ISequentialStream. Per un parametro di input, SQL Server Native Client eseguirà una query per per l'interfaccia ISequentialStream.<br /><br /> È possibile scegliere di non associare la lunghezza del tipo definito dall'utente quando si utilizza l'associazione DBTYPE_IUNKNOWN nel caso di tipi definiti dall'utente di grandi dimensioni. La lunghezza deve essere invece associata nel caso di tipi definiti dall'utente di piccole dimensioni. Un parametro DBTYPE_UDT può essere specificato come tipo definito dall'utente di grandi dimensioni se si verificano una o più delle condizioni seguenti:<br />*ulParamParamSize* è ~0.<br />DBPARAMFLAGS_ISLONG è impostato nella struttura DBPARAMBINDINFO.<br /><br /> Per i dati di riga, l'associazione DBTYPE_IUNKNOWN è consentita solo per i tipi definiti dall'utente di grandi dimensioni. È possibile verificare se una colonna è un tipo definito dall'utente di grandi dimensioni usando il metodo IColumnsInfo::GetColumnInfo su un set di righe o sull'interfaccia IColumnsInfo di un oggetto Command. Una colonna DBTYPE_UDT è una colonna con tipo definito dall'utente di grandi dimensioni se si verificano una o più delle condizioni seguenti:<br />Il flag DBCOLUMNFLAGS_ISLONG è impostato sul membro *dwFlags* della struttura DBCOLUMNINFO. <br />Il membro *ulColumnSize* di DBCOLUMNINFO è ~0.|  
  
 DBTYPE_NULL e DBTYPE_EMPTY possono essere associati per i parametri di input ma non per i parametri di output o per i risultati. Se vengono associati per i parametri di input, lo stato deve essere impostato su DBSTATUS_S_ISNULL per DBTYPE_NULL o su DBSTATUS_S_DEFAULT per DBTYPE_EMPTY. Non è possibile utilizzare DBTYPE_BYREF con DBTYPE_NULL o DBTYPE_EMPTY.  
  
 DBTYPE_UDT può anche essere convertito in DBTYPE_EMPTY e DBTYPE_NULL. DBTYPE_NULL e DBTYPE_EMPTY non possono invece essere convertiti in DBTYPE_UDT. È coerente con DBTYPE_BYTES. **ISSCommandWithParameters** viene usato per elaborare i tipi definiti dall'utente come parametri.  
  
 Le conversioni dei dati fornite dai servizi principali OLE DB (**IDataConvert**) non sono applicabili a DBTYPE_UDT.  
  
 Non sono supportate altre associazioni.  
  
## <a name="comparability-for-irowsetfind"></a>Possibilità di confronto per IRowsetFind  
 Per i tipi definiti dall'utente sono supportati solo i confronti seguenti:  
  
-   EQ  
  
-   NE  
  
-   IGNORE  
  
 Se viene tentato qualsiasi altro confronto, viene restituito DB_E_BADCOMPAREOP.  
  
## <a name="bcp-support-for-udts"></a>Supporto di BCP per i tipi definiti dall'utente  
 I valori dei tipi definiti dall'utente possono essere importati ed esportati solo come caratteri o valori binari.  
  
## <a name="down-level-client-behavior-for-udts"></a>Comportamento dei client legacy per i tipi definiti dall'utente  
 I tipi definiti dall'utente sono soggetti al mapping dei tipi con i client legacy nel modo seguente:  
  
|Versione client|DBTYPE_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|DBTYPE_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|  
|--------------------|------------------------------------------------------------------|---------------------------------------------------------|  
|SQL Server 2005|UDT|varbinary(max)|  
|SQL Server 2008 e versioni successive|UDT|UDT|  
  
 Quando **DataTypeCompatibility** (SSPROP_INIT_DATATYPECOMPATIBILITY) è impostato su "80", i tipi definiti dall'utente di grandi dimensioni vengono visualizzati ai client nello stesso modo in cui vengono visualizzati ai client legacy.  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi CLR definiti dall'utente di grandi dimensioni](~/relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  

