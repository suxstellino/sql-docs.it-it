---
description: Esempio della proprietà Source (VC++)
title: Esempio di proprietà Source (VC + +) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- C++
helpviewer_keywords:
- Source property [ADO], VC++ example
ms.assetid: e10d33da-ea30-4138-ae40-e9f6aa9d17d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bf3459db56449fb91ec3765d5812091c3f58a90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051352"
---
# <a name="source-property-example-vc"></a>Esempio della proprietà Source (VC++)
In questo esempio viene illustrata la proprietà [source](./source-property-ado-recordset.md) aprendo tre oggetti [Recordset](./recordset-object-ado.md) basati su origini dati diverse.  
  
```  
// Source_Property_Sample.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void SourceX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   SourceX();  
   ::CoUninitialize();  
}  
  
void SourceX() {  
   // Define string variables.  
   _bstr_t strCmdSQL("Select title ,type, pubdate FROM titles ORDER BY title");    
   _bstr_t strSQL("SELECT title_ID AS TitleID, title AS Title, "   
      "publishers.pub_id AS PubID, pub_name AS PubName "  
      "FROM publishers INNER JOIN titles "   
      "ON publishers.pub_id = titles.pub_id "   
      "ORDER BY Title");  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   // Define ADO object pointers.  Initialize pointers on define.  
   // These are in the ADODB:: namespace.  
   _ConnectionPtr pConnection = NULL;  
   _RecordsetPtr pRstTitles = NULL;  
   _RecordsetPtr pRstPublishers = NULL;  
   _RecordsetPtr pRstPublishersDirect = NULL;  
   _RecordsetPtr pRstTitlesPublishers = NULL;  
   _CommandPtr pCmdSQL = NULL;  
  
   try {  
      // Open a connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      pConnection->Open (strCnn, "", "", adConnectUnspecified);  
  
      // Open a recordset based on a command object.  
      TESTHR(pCmdSQL.CreateInstance(__uuidof(Command)));  
      pCmdSQL->ActiveConnection = pConnection;  
      pCmdSQL->CommandText = strCmdSQL;  
      pRstTitles = pCmdSQL->Execute(NULL, NULL, adCmdText);  
  
      // Open a recordset based on a table  
      TESTHR(pRstPublishers.CreateInstance(__uuidof(Recordset)));  
      pRstPublishers->Open ("publishers", _variant_t((IDispatch *) pConnection, true),  
         adOpenForwardOnly, adLockReadOnly, adCmdTable);  
  
      // Open a recordset based on a table  
      TESTHR(pRstPublishersDirect.CreateInstance(__uuidof(Recordset)));  
      pRstPublishersDirect->Open ("publishers", _variant_t((IDispatch *) pConnection, true),  
         adOpenForwardOnly, adLockReadOnly, adCmdTableDirect);  
  
      // Open a recordset based on a SQL string.  
      TESTHR(pRstTitlesPublishers.CreateInstance(__uuidof(Recordset)));  
      pRstTitlesPublishers->Open(strSQL, _variant_t((IDispatch *) pConnection, true),  
         adOpenForwardOnly, adLockReadOnly, adCmdText);  
  
      // Use the Source property to display the source of each recordset.  
      printf("rstTitles source: \n%s\n\n",  
         (LPCSTR)(_bstr_t) pRstTitles->GetSource().bstrVal);  
  
      printf("rstPublishers source: \n%s\n\n",  
         (LPCSTR)(_bstr_t) pRstPublishers->GetSource().bstrVal);  
  
      printf("rstPublishersDirect source: \n%s\n\n",  
         (LPCSTR)(_bstr_t) pRstPublishersDirect->GetSource().bstrVal);  
  
      printf("rstTitlesPublishers source: \n%s\n\n",  
         (LPCSTR)(_bstr_t) pRstTitlesPublishers->GetSource().bstrVal);  
   }  
   catch (_com_error &e) {  
      // Notify the user of errors if any.  
      PrintProviderError(pConnection);  
      PrintComError(e);  
   }  
  
   if (pRstTitles)  
      if (pRstTitles->State == adStateOpen)  
         pRstTitles->Close();  
   if (pRstPublishers)  
      if (pRstPublishers->State == adStateOpen)  
         pRstPublishers->Close();  
   if (pRstPublishersDirect)  
      if (pRstPublishersDirect->State == adStateOpen)  
         pRstPublishersDirect->Close();  
   if (pRstTitlesPublishers)  
      if (pRstTitlesPublishers->State == adStateOpen)  
         pRstTitlesPublishers->Close();  
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0 ) {  
      long nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("Error number: %x\t%s\n", pErr->Number, (LPCSTR) pErr->Description);  
      }  
   }  
}  
  
void PrintComError(_com_error &e) {  
   _bstr_t bstrSource(e.Source());  
   _bstr_t bstrDescription(e.Description());  
  
   // Print Com errors.    
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto recordset (ADO)](./recordset-object-ado.md)   
 [Proprietà Source (Recordset - ADO)](./source-property-ado-recordset.md)