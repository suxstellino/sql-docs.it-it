---
title: Recuperare colonne usando IRow::GetColumns (OLE DB Driver) | Microsoft Docs
description: Informazioni su come recuperare le colonne usando il metodo IRow::GetColumns in OLE DB Driver per SQL Server. In questo esempio si ottiene una colonna alla volta da una riga.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- IRow interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6e84293cb27027be572ff2e1f95e61cd0c9de17
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741511"
---
# <a name="fetch-columns-using-irowgetcolumns-ole-db"></a>Recuperare colonne tramite IRow::GetColumns (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L'interfaccia **IRow** consente di accedere direttamente alle colonne di una singola riga nel set di risultati. Di conseguenza, tramite **IRow** è possibile recuperare in modo efficace colonne da un set di risultati con una sola riga.  
  
 È disponibile un esempio di codice che mostra come recuperare una sola riga usando **IRow**. In questo esempio viene recuperata una colonna per volta dalla riga. Nell'esempio vengono illustrate le operazioni seguenti:  
  
-   Come recuperare un gruppo di colonne (in sequenza).  
  
-   Come accedere due volte a una colonna. La prima volta viene ottenuta la larghezza di colonna effettiva, mentre la seconda volta viene eseguito l'accesso ai dati effettivi. Nella struttura DBCOLUMNACCESS, se **pData** è NULL e **cbMaxLen** è 0, tramite la chiamata a **IRow**- **->GetColumns** viene restituita solo la lunghezza effettiva della colonna. In questo caso, è possibile chiamare nuovamente **IRow->GetColumns()** nella stessa colonna per recuperare i dati effettivi.  
  
> [!IMPORTANT]  
>  Se possibile, usare l'autenticazione di Windows. Se non è disponibile, agli utenti verrà richiesto di immettere le credenziali in fase di esecuzione. Evitare di archiviare le credenziali in un file. Se è necessario rendere persistenti le credenziali, è consigliabile crittografarle usando l'[API di crittografia Win32](/windows/win32/seccrypto/cryptography-reference).  
  
### <a name="to-fetch-columns-using-irowgetcolumns"></a>Per recuperare colonne tramite IRow::GetColumns  
  
1.  Stabilire una connessione all'origine dati.  
  
2.  Eseguire il comando (nell'esempio seguente viene chiamato ICommandExecute::Execute() con IID_IRow).  
  
3.  Eseguire IRow::GetColumns () per recuperare una o più colonne nella riga risultante. Se si desidera individuare le dimensioni di colonna effettive prima di recuperare i dati, impostare pData in DBCOLUMNACCESS su NULL. La chiamata a IRow::GetColumns () restituisce solo la larghezza di colonna. Un'altra chiamata a IRow::GetColumns () recupererà i dati.  
  
4.  Eseguire IRow::GetColumns () fino ad accedere a tutte le colonne necessarie. Alle colonne è necessario accedere in sequenza.  
  
## <a name="example"></a>Esempio  
 In questo esempio viene illustrato come utilizzare l'interfaccia di IRow per consentire accesso diretto alle colonne di un'unica riga nel set di risultati. In questo esempio vengono illustrati gli aspetti seguenti:  
  
-   Come recuperare un gruppo di colonne in sequenza.  
  
-   Come accedere a una colonna due volte. La prima volta viene ottenuta la larghezza di colonna effettiva, mentre la seconda volta viene eseguito l'accesso ai dati effettivi.  
  
 Nella struttura DBCOLUMNACCESS, se pData è NULL e cbMaxLen è 0, tramite la chiamata a IRow->GetColumns viene restituita solo la lunghezza effettiva della colonna. In questo caso è possibile chiamare nuovamente IRow->GetColumns sulla stessa colonna per recuperare i dati effettivi. Questo esempio non è supportato in IA64.  
  
 Per l'esempio è necessario il database di esempio AdventureWorks, che è possibile scaricare dalla home page del sito relativo a [progetti della community ed esempi per Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=85384).  
  
 Il primo listato di codice ([!INCLUDE[tsql](../../../includes/tsql-md.md)]) consente di creare una tabella utilizzata dall'esempio.  
  
 Compilare il secondo listato di codice (C++) con ole32.lib oleaut32.lib ed eseguirlo. In questa applicazione viene eseguita la connessione all'istanza predefinita di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nel computer in uso. In alcuni sistemi operativi Windows sarà necessario modificare (local) o (localhost) impostando il valore sul nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per connettersi a un'istanza denominata, modificare la stringa di connessione da L"(local)" a L"(local)\\\nome", dove nome è l'istanza denominata. Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Express viene installato in un'istanza denominata. Verificare che nella variabile di ambiente INCLUDE sia presente la directory che contiene msoledbsql.h.  
  
 Il terzo listato di codice ([!INCLUDE[tsql](../../../includes/tsql-md.md)]) consente di eliminare la tabella utilizzata dall'esempio.  
  
```  
use AdventureWorks  
go  
  
if exists (select name from sysobjects where name = 'MyTable')  
     drop table MyTable  
go  
  
create table MyTable  
(  
     col1  int,  
     col2  varchar(50),  
     col3  char(50),  
     col4  datetime,  
     col5  float,  
     col6  money,  
     col7  sql_variant,  
     col8  binary(50),  
     col9  text,  
     col10 image  
)  
go  
insert into MyTable  
values  
(  
     10,  
     'abcdefghijklmnopqrstuvwxyz',  
     'ABCDEFGHIJKLMNOPQRSTUVWXYZ',  
     '11/1/1999 11:52 AM',  
     3.14,  
     99.95,  
     convert(nchar(50), N'AbCdEfGhIjKlMnOpQrStUvWxYz'),  
     0x123456789,  
     replicate('AAAAABBBBB', 500),  
     replicate(0x123456789, 500)  
)  
Go  
```  
  
```  
// compile with: ole32.lib oleaut32.lib  
#define DBINITCONSTANTS  
#define OLEDBVER 0x0250   // to include correct interfaces  
  
#include <stdio.h>  
#include <windows.h>  
#include <iostream>  
#include <oledb.h>  
#include <msoledbsql.h>  
  
using namespace std;  
  
int InitializeAndEstablishConnection();  
HRESULT GetColumnSize(IRow* pUnkRow, ULONG iCol);  
ULONG PrintData(ULONG iCols, ULONG iStart, DBCOLUMNINFO* prgInfo, DBCOLUMNACCESS* prgColumns);  
HRESULT GetColumns(IRow* pUnkRow, ULONG iStart, ULONG iEnd);  
  
IDBInitialize*       pIDBInitialize     = NULL;  
IDBProperties*       pIDBProperties     = NULL;  
IDBCreateSession*    pIDBCreateSession  = NULL;  
IDBCreateCommand*    pIDBCreateCommand  = NULL;  
ICommandText*        pICommandText      = NULL;  
IRow   *             pIRow              = NULL;  
DBCOLUMNINFO*        pDBColumnInfo      = NULL;  
IAccessor*           pIAccessor         = NULL;  
DBPROP               InitProperties[4];  
DBPROPSET            rgInitPropSet[1];  
ULONG                i, j;  
HRESULT              hresult;  
DBROWCOUNT           cNumRows = 0;  
ULONG                lNumCols;  
WCHAR*               pStringsBuffer;  
DBBINDING*           pBindings;  
ULONG                ConsumerBufColOffset = 0;  
HACCESSOR            hAccessor;  
ULONG                lNumRowsRetrieved;  
HROW                 hRows[10];  
HROW*                pRows = &hRows[0];  
  
int main() {  
   ULONG iidx = 0;  
   WCHAR* wCmdString = OLESTR(" SELECT * FROM MyTable");  
  
   // Call a function to initialize and establish connection.   
   if (InitializeAndEstablishConnection() == -1) {  
      cout << "Failed to initialize and establish connection.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Create a session object.  
   if (FAILED(pIDBInitialize->QueryInterface (   
      IID_IDBCreateSession, (void**) &pIDBCreateSession))) {  
      cout << "Failed to obtain IDBCreateSession interface.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   if (FAILED(pIDBCreateSession->CreateSession( NULL, IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand))) {  
      cout << "pIDBCreateSession->CreateSession failed.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Access the ICommandText interface.  
   if (FAILED(pIDBCreateCommand->CreateCommand( NULL, IID_ICommandText, (IUnknown**) &pICommandText))) {  
      cout << "Failed to access ICommand interface.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Use SetCommandText() to specify the command text.  
   if (FAILED(pICommandText->SetCommandText(DBGUID_DBSQL, wCmdString))) {  
      cout << "Failed to set command text.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Fetch columns 1-5 and then 6-10 and display the contents  
   if (FAILED(hresult = pICommandText->Execute(NULL, IID_IRow, NULL, &cNumRows, (IUnknown **) &pIRow))) {  
      cout << "Failed to execute command.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   hresult = GetColumns(pIRow, 1, 5);  
   hresult = GetColumns(pIRow, 6, 10);  
  
   hresult = pIRow->Release();  
  
   // Execute the command.  
   if (FAILED(hresult = pICommandText->Execute(NULL, IID_IRow, NULL, &cNumRows, (IUnknown **) &pIRow))) {  
      cout << "Failed to execute command.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Get columns  
   for ( iidx = 1 ; iidx <= 10 ; iidx++ ) {  
      if (FAILED(hresult = GetColumnSize(pIRow, iidx))) {  
         cout << "Failed to get column size.\n";  
         // Insert your code for cleanup and error handling.     
         return -1;  
      }  
  
      hresult = GetColumns(pIRow, iidx, iidx);  
   }  
  
   pIRow->Release();  
  
   // Release memory.  
   pICommandText->Release();  
   pIDBCreateCommand->Release();  
   pIDBCreateSession->Release();  
  
   if (FAILED(pIDBInitialize->Uninitialize())) {  
      // Uninitialize not required, but fails if an interface has not been released.  Can be used for debugging.  
      cout << "Problem uninitializing.\n";  
   }  
  
   pIDBInitialize->Release();  
  
   // Release the COM library.  
   CoUninitialize();  
  
   return 0;  
};  
  
//--------------------------------------------------------------------  
BOOL InitColumn(DBCOLUMNACCESS* pCol, DBCOLUMNINFO* pInfo) {  
   // If text or image column is being read,in which case the max  possible length of a value is   
   // the column is hugh,we will limit that size to 512 bytes (for illustration purposes).  
   DBLENGTH ulSize = (pInfo->ulColumnSize < 0x7fffffff) ? pInfo->ulColumnSize : 512;  
  
   // Verify dta buffer is large enough.  
   if (pCol->cbMaxLen < (ulSize + 1)) {  
      if (pCol->pData) {  
         delete [] pCol->pData;  
         pCol->pData = NULL;  
      }  
  
      // Allocate data buffer  
      void * p = pCol->pData = new WCHAR[ulSize + 1];  
      if (!(p /* pCol->pData = new WCHAR[ulSize + 1] */ ))  
         return FALSE;  
  
      // set the max length of caller-initialized memory.  
      pCol->cbMaxLen = sizeof(WCHAR) * (ulSize + 1);  
  
      // In the above 2 steps, pData is pointing to memory (it is not NULL) and cbMaxLen has a value   
      // (not 0), so next call to IRow->GetData() will read the data from the column.  
   }  
  
   // Clear memory buffer  
   ZeroMemory((void*) pCol->pData, pCol->cbMaxLen);  
  
   // Set properties.  
   pCol->wType = DBTYPE_WSTR;  
   pCol->columnid = pInfo->columnid;  
   pCol->cbDataLen = 0;  
   pCol->dwStatus = 0;  
   pCol->dwReserved = 0;  
   pCol->bPrecision = 0;  
   pCol->bScale = 0;  
   return TRUE;  
}  
  
//--------------------------------------------------------------------  
HRESULT GetColumns(IRow* pUnkRow, ULONG iStart, ULONG iEnd) {  
// Start and end are same. Thus, get only one column.  
   HRESULT hr;  
   ULONG iidx;   // loop counter  
   DBORDINAL cColumns;   // Count of columns  
   ULONG cUserCols;   // Count of user columns  
  
   DBCOLUMNINFO* prgInfo;   // Column of info. array  
   OLECHAR* pColNames;   // Array of column names  
  
   DBCOLUMNACCESS* prgColumns;   // Ptr to column access structures array  
   DBCOLUMNINFO* pCurrInfo;  
   DBCOLUMNACCESS* pCurrCol;  
  
   IColumnsInfo* pIColumnsInfo = NULL;  
  
   // Initialize  
   cColumns = 0;  
   prgInfo = NULL;  
   pColNames = NULL;  
   prgColumns = NULL;  
  
   printf("Retrieving data\n");  
  
   // Get column info to build column access array  
   hr = pUnkRow->QueryInterface(IID_IColumnsInfo, (void**)&pIColumnsInfo);  
   if (FAILED(hr))  
      goto CLEANUP;  
   hr = pIColumnsInfo->GetColumnInfo(&cColumns, &prgInfo, &pColNames);  
   if (FAILED(hr))  
      goto CLEANUP;  
  
   printf("In GetColumns(), Columns= %d\n", cColumns);  
  
   // Determine no. of columns to retrieve. Since iEnd and iStart is same, this is redundent step.    
   // cUserCols will always be 1.  
   cUserCols = iEnd - iStart + 1;   
  
   // Walk list of columns and setup a DBCOLUMNACCESS structure  
   if (!(prgColumns= new DBCOLUMNACCESS[cUserCols])) {   // cUserCols is only 1  
      hr = E_FAIL;  
      goto CLEANUP;  
   }  
  
   ZeroMemory((void*) prgColumns, sizeof(DBCOLUMNACCESS) * cUserCols);  
  
   for ( iidx = 0 ; iidx < cUserCols ; iidx++ ) {  
      pCurrInfo = prgInfo + iidx + iStart - 1;  
      pCurrCol = prgColumns + iidx;  
  
      // Here the values of DBCOLUMNACCESS elements is set (pData and cbMaxLen)Thus IRow->GetColumns()   
      // will return actual data.  
      if (InitColumn(pCurrCol, pCurrInfo) == FALSE)  
         goto CLEANUP;  
   }  
  
   hr = pUnkRow->GetColumns(cUserCols, prgColumns);   // cUserCols = 1  
   if (FAILED(hr))  
      printf("Error occured\n");  
  
   // Show data.  
   PrintData(cUserCols, iStart, prgInfo, prgColumns);  
  
CLEANUP:  
   if (pIColumnsInfo)  
      pIColumnsInfo->Release();  
   if (prgColumns)  
      delete [] prgColumns;  
  
   return hr;  
}  
  
// This function returns the actual width of the data in the column (not the columnwidth in   
// DBCOLUMNFO structure which is the width of the column)  
HRESULT GetColumnSize(IRow* pUnkRow, ULONG iCol) {  
   HRESULT        hr = NOERROR;  
   DBORDINAL      cColumns = 0;   // Count the columns  
   DBCOLUMNINFO*  prgInfo;   // Column info array  
   OLECHAR*       pColNames;  
   DBCOLUMNACCESS column;  
   DBCOLUMNINFO*  pCurrInfo;  
   IColumnsInfo*  pIColumnsInfo = NULL;  
  
   // Initialize  
   prgInfo = NULL;  
   pColNames = NULL;  
  
   printf("Checking column size\n");  
  
   // Get column info to build column access array  
   hr = pUnkRow->QueryInterface(IID_IColumnsInfo, (void**) &pIColumnsInfo);  
   if (FAILED(hr))  
      goto CLEANUP;  
  
   hr = pIColumnsInfo->GetColumnInfo(&cColumns, &prgInfo, &pColNames);  
   if (FAILED(hr))  
      goto CLEANUP;  
   printf("Value of cColumns is %d\n", cColumns);  
  
   // Setup a DBCOLUMNACCESS structure: Here pData is set to NULL and cbMaxLen is set to 0. Thus   
   // IRow->GetColumns() returns only the actual column length in cbDataLen member of DBCOLUMNACCESS   
   // structure. In this case you can call IRow->GetColumns() again for the same  column to retrieve   
   // actual data in the second call.  
   ZeroMemory((void*) &column, sizeof(DBCOLUMNACCESS));  
   column.pData = NULL;  
  
   pCurrInfo = prgInfo + iCol - 1;  
  
   // Get the column id in DBCOLUMNACCESS structure. It is then used in GetColumn().  
   column.columnid = pCurrInfo->columnid;   
  
   printf("column.columnid value is %d\n", column.columnid);  
   // We know which column to get. The column.columnid gives the column no.  
   hr = pUnkRow->GetColumns(1, &column);   
   if (FAILED(hr))  
      printf("Errors occured\n");  
  
   // Show data  
   PrintData(1, iCol, prgInfo, &column);  
  
CLEANUP:  
   if (pIColumnsInfo)  
      pIColumnsInfo->Release();  
   return hr;  
}  
  
BOOL GetStatus(DWORD dwStatus, WCHAR* pwszStatus) {  
   switch (dwStatus) {  
   case DBSTATUS_S_OK:  
      wcscpy_s(pwszStatus, 255, L"DBSTATUS_S_OK");  
      break;  
   case DBSTATUS_E_UNAVAILABLE:  
      wcscpy_s(pwszStatus, 255, L"DBSTATUS_E_UNAVAILABLE");  
      break;  
   case DBSTATUS_S_TRUNCATED:  
      wcscpy_s(pwszStatus, 255, L"DBSTATUS_S_TRUNCATED");  
      break;  
   }  
   return TRUE;  
}  
  
ULONG PrintData(ULONG iCols, ULONG iStart, DBCOLUMNINFO* prgInfo, DBCOLUMNACCESS* prgColumns) {  
   WCHAR wszStatus[255];  
   DBCOLUMNINFO* pCurrInfo;  
   DBCOLUMNACCESS* pCurrCol;  
   ULONG iidx;  
  
   printf("No. Name       Status     Length  Max  Data\n");  
  
   for ( iidx = 0 ; iidx < iCols ; iidx++ ) {  
      pCurrInfo = prgInfo + iidx + iStart - 1;  
      pCurrCol = prgColumns + iidx;  
  
      GetStatus(pCurrCol->dwStatus, wszStatus);   
      // was the data successfully retrieved?  
      wprintf(L"%-3d %-*s %-20s %-3d %-3d %-20s\n", iStart+iidx,   
                                                    10,   
                                                    pCurrInfo->pwszName,   
                                                    wszStatus,  
                                                    pCurrCol->cbDataLen,  
                                                    pCurrCol->cbMaxLen,  
                                                    (WCHAR*) pCurrCol->pData);  
   }  
  
   wprintf(L"\n");  
   return iidx;  
}  
  
int InitializeAndEstablishConnection() {      
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the MSOLEDBSQL provider.  
   hresult = CoCreateInstance(CLSID_MSOLEDBSQL,  
                              NULL,  
                              CLSCTX_INPROC_SERVER,  
                              IID_IDBInitialize,  
                              (void **) &pIDBInitialize);  
  
   if (FAILED(hresult)) {  
      printf("Failed to get IDBInitialize interface.\n");  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   // Initialize the property values needed to establish the connection.  
   for ( i = 0 ; i < 4 ; i++ )   
      VariantInit(&InitProperties[i].vValue);  
  
   // Server name.  
   InitProperties[0].dwPropertyID  = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt     = VT_BSTR;  
  
   InitProperties[0].vValue.bstrVal= SysAllocString(L"(local)");  
   InitProperties[0].dwOptions     = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid         = DB_NULLID;  
  
   // Database.  
   InitProperties[1].dwPropertyID  = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt     = VT_BSTR;  
   InitProperties[1].vValue.bstrVal= SysAllocString(L"AdventureWorks");  
   InitProperties[1].dwOptions     = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid         = DB_NULLID;  
  
   InitProperties[2].dwPropertyID  = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt     = VT_BSTR;  
   InitProperties[2].vValue.bstrVal= SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions     = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid         = DB_NULLID;  
  
   // Now that the properties are set, construct the DBPROPSET structure (rgInitPropSet). The DBPROPSET   
   // structure is used to pass an array of DBPROP structures (InitProperties) to the SetProperties method.  
   rgInitPropSet[0].guidPropertySet= DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties    = 4;  
   rgInitPropSet[0].rgProperties   = InitProperties;  
  
   // Set initialization properties.  
   hresult = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);  
   if (FAILED(hresult)) {  
      cout << "Failed to get IDBProperties interface.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   hresult = pIDBProperties->SetProperties(1, rgInitPropSet);   
   if (FAILED(hresult)) {  
      cout << "Failed to set initialization properties.\n";  
      // Insert your code for cleanup and error handling.  
      return -1;  
   }  
  
   pIDBProperties->Release();  
  
   // Now establish the connection to the data source.  
   if (FAILED(pIDBInitialize->Initialize()))  
      cout << "Problem establishing connection to the data source.\n";  
  
   return 0;  
}  
```  
  
```  
use AdventureWorks  
go  
  
if exists (select name from sysobjects where name = 'MyTable')  
     drop table MyTable  
go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure relative a OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
