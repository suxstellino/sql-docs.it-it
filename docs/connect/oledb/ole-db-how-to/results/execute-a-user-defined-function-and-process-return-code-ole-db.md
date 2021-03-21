---
title: Eseguire una funzione definita dall'utente ed elaborare il codice restituito (OLE DB) | Microsoft Docs
description: Procedura per eseguire una funzione definita dall'utente e stampare il codice restituito usando OLE DB Driver per SQL Server. In questo esempio viene utilizzato il database di esempio AdventureWorks.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- user-defined functions [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4d204a884fafb5112a0788c28b839f268c40fafe
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753861"
---
# <a name="execute-a-user-defined-function-and-process-return-code-ole-db"></a>Eseguire una funzione definita dall'utente ed elaborare il codice restituito (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../../includes/driver_oledb_download.md)]

  In questo esempio viene eseguita una funzione definita dall'utente e viene stampato il codice restituito. Questo esempio non è supportato in IA64.  
  
 Per l'esempio è necessario il database di esempio AdventureWorks, che è possibile scaricare dalla home page del sito relativo a [progetti della community ed esempi per Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=85384).  
  
> [!IMPORTANT]  
>  Se possibile, usare l'autenticazione di Windows. Se non è disponibile, agli utenti verrà richiesto di immettere le credenziali in fase di esecuzione. Evitare di archiviare le credenziali in un file. Se è necessario rendere persistenti le credenziali, è consigliabile crittografarle usando l'[API di crittografia Win32](/windows/win32/seccrypto/cryptography-reference).  
  
## <a name="example"></a>Esempio  
 Eseguire il primo listato di codice ( [!INCLUDE[tsql](../../../../includes/tsql-md.md)] ) per creare la stored procedure usata dall'applicazione.  
  
 Compilare il secondo listato di codice (C++) con ole32.lib oleaut32.lib ed eseguirlo. In questa applicazione viene eseguita la connessione all'istanza predefinita di [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)] nel computer in uso. In alcuni sistemi operativi Windows sarà necessario modificare (local) o (localhost) impostando il valore sul nome dell'istanza di [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)]. Per connettersi a un'istanza denominata, modificare la stringa di connessione da L"(local)" in L"(local)\\\nome", dove nome rappresenta l'istanza denominata. Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)] Express viene installato in un'istanza denominata. Verificare che nella variabile di ambiente INCLUDE sia presente la directory che contiene msoledbsql.h.  
  
 Eseguire il terzo listato di codice ( [!INCLUDE[tsql](../../../../includes/tsql-md.md)] ) per eliminare la stored procedure usata dall'applicazione.  
  
```  
USE AdventureWorks  
if exists (SELECT * FROM sys.objects WHERE object_id = OBJECT_ID(N'[fn_RectangleArea]'))  
   drop function fn_RectangleArea  
go  
  
CREATE FUNCTION fn_RectangleArea  
   (@Width int,   
@Height int )  
RETURNS int  
AS  
BEGIN  
  
   RETURN ( @Width * @Height )  
END  
GO  
```  
  
```  
// compile with: ole32.lib oleaut32.lib  
void InitializeAndEstablishConnection();  
#define UNICODE  
#define DBINITCONSTANTS  
#define INITGUID  
#define OLEDBVER 0x0250   // to include correct interfaces  
  
#include <windows.h>  
#include <stdio.h>  
#include <stddef.h>  
#include <iostream>  
#include <oledb.h>  
#include <oledberr.h>  
#include <msoledbsql.h>  
using namespace std;  
  
IDBInitialize* pIDBInitialize = NULL;      
IDBCreateSession* pIDBCreateSession = NULL;  
IDBCreateCommand* pIDBCreateCommand = NULL;  
ICommandText* pICommandText = NULL;  
IRowset* pIRowset = NULL;  
ICommandWithParameters* pICommandWithParams = NULL;  
IAccessor* pIAccessor = NULL;  
IDBProperties* pIDBProperties = NULL;  
WCHAR* pStringsBuffer;  
DBBINDING* pBindings;  
const ULONG nInitProps = 4;  
DBPROP InitProperties[nInitProps];      
const ULONG nPropSet = 1;  
DBPROPSET rgInitPropSet[nPropSet];    
HRESULT hr;  
HACCESSOR hAccessor;  
const ULONG nParams = 3;   // No. of parameters in the command  
DBPARAMBINDINFO ParamBindInfo[nParams];  
ULONG i;  
ULONG cbColOffset = 0;  
  
DB_UPARAMS ParamOrdinals[nParams];  
DBROWCOUNT cNumRows = 0;  
DBPARAMS Params;  
  
// Declare array of DBBINDING structures, one for each parameter in the command  
DBBINDING acDBBinding[nParams];  
DBBINDSTATUS acDBBindStatus[nParams];  
  
// The following buffer is used to store parameter values.  
typedef struct tagSPROCPARAMS {  
   long lReturnValue;  
   long inParam1;  
   long inParam2;  
} SPROCPARAMS;  
  
int main() {  
   // The command to execute.  
   WCHAR* wCmdString = L"{? = CALL fn_RectangleArea(?, ?) }";  
   // WCHAR* wCmdString = L"EXEC ? = fn_RectangleVolume(?, ?)";  
  
   SPROCPARAMS sprocparams = {0,5,10};  
  
   // All the initialization stuff in a separate function.  
   InitializeAndEstablishConnection();  
  
   // Let us create a new session from the data source object.  
   if (FAILED(pIDBInitialize->QueryInterface( IID_IDBCreateSession,  
                                              (void**) &pIDBCreateSession))) {  
      cout << "Failed to access IDBCreateSession interface\n";  
      goto EXIT;  
   }  
  
   if ( FAILED(pIDBCreateSession->CreateSession( NULL,   
                                                 IID_IDBCreateCommand,   
                                                 (IUnknown**) &pIDBCreateCommand))) {  
      cout << "pIDBCreateSession->CreateSession failed\n";  
      goto EXIT;  
   }  
  
   // Create a Command  
   if (FAILED(pIDBCreateCommand->CreateCommand( NULL,   
                                                IID_ICommandText,   
                                                (IUnknown**) &pICommandText))) {  
      cout << "Failed to access ICommand interface\n";  
      goto EXIT;  
   }  
  
   // Set the command text.  
   if (FAILED(pICommandText->SetCommandText(DBGUID_DBSQL, wCmdString))) {  
      cout << "failed to set command text\n";  
      goto EXIT;  
   }  
  
   // Describe the command parameters (parameter name, provider specific name   
   // of the parameter's data type etc.) in an array of DBPARAMBINDINFO   
   // structures.  This information is then used by SetParameterInfo().  
   ParamBindInfo[0].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[0].pwszName = NULL;  
   ParamBindInfo[0].ulParamSize = sizeof(long);  
   ParamBindInfo[0].dwFlags = DBPARAMFLAGS_ISOUTPUT;  
   ParamBindInfo[0].bPrecision = 11;  
   ParamBindInfo[0].bScale = 0;  
   ParamOrdinals[0] = 1;  
  
   ParamBindInfo[1].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[1].pwszName = NULL;   // L"@inparam1";                 
   ParamBindInfo[1].ulParamSize = sizeof(long);  
   ParamBindInfo[1].dwFlags = DBPARAMFLAGS_ISINPUT;  
   ParamBindInfo[1].bPrecision = 11;  
   ParamBindInfo[1].bScale = 0;  
   ParamOrdinals[1] = 2;  
  
   ParamBindInfo[2].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[2].pwszName = NULL;   // L"@inparam2";   
   ParamBindInfo[2].ulParamSize = sizeof(long);  
   ParamBindInfo[2].dwFlags = DBPARAMFLAGS_ISINPUT;  
   ParamBindInfo[2].bPrecision = 11;  
   ParamBindInfo[2].bScale = 0;  
   ParamOrdinals[2] = 3;  
  
   // Set the parameters information.  
   if (FAILED(pICommandText->QueryInterface( IID_ICommandWithParameters,  
                                             (void**)&pICommandWithParams))) {  
      cout << "failed to obtain ICommandWithParameters\n";  
      goto EXIT;  
   }  
   if (FAILED(pICommandWithParams->SetParameterInfo( nParams,   
                                                     ParamOrdinals,   
                                                     ParamBindInfo))) {  
      cout << "failed in setting parameter info.(SetParameterInfo)\n";  
      goto EXIT;  
   }  
  
   // Describe the consumer buffer; initialize the array of DBBINDING structures.    
   // Each binding associates a single parameter to the consumer's buffer.  
   for ( i = 0 ; i < nParams ; i++ ) {  
      acDBBinding[i].obLength = 0;  
      acDBBinding[i].obStatus = 0;  
      acDBBinding[i].pTypeInfo = NULL;  
      acDBBinding[i].pObject = NULL;  
      acDBBinding[i].pBindExt = NULL;  
      acDBBinding[i].dwPart = DBPART_VALUE;  
      acDBBinding[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      acDBBinding[i].dwFlags = 0;  
      acDBBinding[i].bScale = 0;  
   }   // for  
  
   acDBBinding[0].iOrdinal = 1;  
   acDBBinding[0].obValue = offsetof(SPROCPARAMS, lReturnValue);  
   acDBBinding[0].eParamIO = DBPARAMIO_OUTPUT;  
   acDBBinding[0].cbMaxLen = sizeof(long);  
   acDBBinding[0].wType = DBTYPE_I4;  
   acDBBinding[0].bPrecision = 11;  
  
   acDBBinding[1].iOrdinal = 2;  
   acDBBinding[1].obValue = offsetof(SPROCPARAMS, inParam1);  
   acDBBinding[1].eParamIO = DBPARAMIO_INPUT;  
   acDBBinding[1].cbMaxLen = sizeof(long);  
   acDBBinding[1].wType = DBTYPE_I4;  
   acDBBinding[1].bPrecision = 11;  
  
   acDBBinding[2].iOrdinal = 3;  
   acDBBinding[2].obValue = offsetof(SPROCPARAMS, inParam2);  
   acDBBinding[2].eParamIO = DBPARAMIO_INPUT;  
   acDBBinding[2].cbMaxLen = sizeof(long);  
   acDBBinding[2].wType = DBTYPE_I4;  
   acDBBinding[2].bPrecision = 11;  
  
   // Let us create an accessor from the above set of bindings.  
   hr = pICommandWithParams->QueryInterface( IID_IAccessor, (void**)&pIAccessor);  
   if (FAILED(hr))  
      cout << "Failed to get IAccessor interface\n";  
  
   hr = pIAccessor->CreateAccessor( DBACCESSOR_PARAMETERDATA,  
                                    nParams,   
                                    acDBBinding,   
                                    sizeof(SPROCPARAMS),   
                                    &hAccessor,  
                                    acDBBindStatus);  
   if (FAILED(hr))  
      cout << "failed to create accessor for the defined parameters\n";  
  
   // Initialize DBPARAMS structure for command execution. DBPARAMS specifies the  
   // parameter values in the command.  DBPARAMS is then passed to Execute.  
   Params.pData = &sprocparams;  
   Params.cParamSets = 1;  
   Params.hAccessor = hAccessor;  
  
   // Execute the command.  
   if (FAILED(hr = pICommandText->Execute( NULL,   
                                           IID_IRowset,   
                                           &Params,   
                                           &cNumRows,   
                                           (IUnknown **) &pIRowset))) {  
      cout << "failed to execute command\n";  
      goto EXIT;  
   }  
  
   printf("  Return value = %d\n", sprocparams.lReturnValue);  
  
   // Release result set without processing.  
   if (pIRowset != NULL)  
      pIRowset->Release();  
  
   // Release memory.  
   pIAccessor->ReleaseAccessor(hAccessor, NULL);  
   pIAccessor->Release();  
   pICommandWithParams->Release();  
   pICommandText->Release();  
   pIDBCreateCommand->Release();  
   pIDBCreateSession->Release();      
  
   if (FAILED(pIDBInitialize->Uninitialize()))  
      // Uninitialize is not required, but it fails if an interface  
      // has not been released.  This can be used for debugging.  
      cout << "Problem uninitializing\n";  
  
   pIDBInitialize->Release();  
  
   CoUninitialize();  
   return 0;  
  
EXIT:  
   if (pIAccessor != NULL)  
      pIAccessor->Release();  
   if (pICommandWithParams != NULL)  
      pICommandWithParams->Release();  
   if (pICommandText != NULL)  
      pICommandText->Release();  
   if (pIDBCreateCommand != NULL)  
      pIDBCreateCommand->Release();  
   if (pIDBCreateSession != NULL)  
      pIDBCreateSession->Release();  
   if (pIDBInitialize != NULL)  
      if (FAILED(pIDBInitialize->Uninitialize()))  
         // Uninitialize is not required, but it fails if an interface has   
         // not been released.  This can be used for debugging.  
         cout << "problem in uninitializing\n";  
      pIDBInitialize->Release();  
  
   CoUninitialize();  
}  
  
void InitializeAndEstablishConnection() {      
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the OLE DB Driver for SQL Server.      
   hr = CoCreateInstance( CLSID_MSOLEDBSQL,   
                          NULL,   
                          CLSCTX_INPROC_SERVER,  
                          IID_IDBInitialize,   
                          (void **) &pIDBInitialize);  
  
   if (FAILED(hr))  
      cout << "Failed in CoCreateInstance()\n";  
  
   // Initialize the property values needed to establish the connection.  
   for ( i = 0 ; i < nInitProps ; i++ )  
      VariantInit(&InitProperties[i].vValue);  
  
   // Specify server name.  
   InitProperties[0].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt = VT_BSTR;  
  
   // Replace "MySqlServer" with proper value.  
   InitProperties[0].vValue.bstrVal = SysAllocString(L"(local)");  
   InitProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid = DB_NULLID;  
  
   // Specify database name.  
   InitProperties[1].dwPropertyID = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt = VT_BSTR;  
   InitProperties[1].vValue.bstrVal = SysAllocString(L"AdventureWorks");  
   InitProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid = DB_NULLID;  
  
   InitProperties[2].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt = VT_BSTR;  
   InitProperties[2].vValue.bstrVal = SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid = DB_NULLID;  
  
   // Now that properties are set, construct the DBPROPSET structure  
   // (rgInitPropSet).  The DBPROPSET structure is used to pass an array  
   // of DBPROP structures (InitProperties) to SetProperties method.  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties = 4;  
   rgInitPropSet[0].rgProperties = InitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface( IID_IDBProperties, (void **)&pIDBProperties);  
   if (FAILED(hr))  
      cout << "Failed to obtain IDBProperties interface.\n";  
  
   hr = pIDBProperties->SetProperties( nPropSet, rgInitPropSet);  
   if (FAILED(hr))  
      cout << "Failed to set initialization properties\n";  
  
   pIDBProperties->Release();  
  
   // Now we establish connection to the data source.  
   if (FAILED(pIDBInitialize->Initialize()))  
      cout << "Problem in initializing\n";  
}  
```  
  
```  
USE AdventureWorks  
drop function fn_RectangleArea  
go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure relative all'elaborazione dei risultati &#40;OLE DB&#41;](../../../oledb/ole-db-how-to/results/processing-results-how-to-topics-ole-db.md)  
  
