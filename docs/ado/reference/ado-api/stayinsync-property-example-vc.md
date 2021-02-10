---
description: Esempio della proprietà StayInSync (VC++)
title: Esempio di proprietà StayInSync (VC + +) | Microsoft Docs
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
- StayInSync property [ADO], VC++ example
ms.assetid: 3a5db5f0-094b-46e1-939b-d9fa9417a406
author: rothja
ms.author: jroth
ms.openlocfilehash: cc3a77c9a4381a5493ea7219715ebad2aedc56f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040381"
---
# <a name="stayinsync-property-example-vc"></a>Esempio della proprietà StayInSync (VC++)
In questo esempio viene illustrato il modo in cui la proprietà [StayInSync](./stayinsync-property.md) facilita l'accesso alle righe in un [Recordset](./recordset-object-ado.md)gerarchico.  
  
 Il ciclo esterno Visualizza il nome, lo stato e l'identificazione di ogni autore. Il **Recordset** aggiunto per ogni riga viene recuperato dalla raccolta [Fields](./fields-collection-ado.md) e assegnato automaticamente a **rstTitleAuthor** dalla proprietà **StayInSync** ogni volta che il **Recordset** padre si sposta in una nuova riga. Il ciclo interno Visualizza quattro campi da ogni riga del Recordset accodato.  
  
```  
// BeginStayInSyncCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void StayInSyncX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1 ;  
  
   StayInSyncX();  
   ::CoUninitialize();  
}  
  
void StayInSyncX() {  
   HRESULT  hr = S_OK;  
  
   // Define string variables.  
   _bstr_t strCnn("Provider='MSDataShape'; Data Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   // Define ADO object pointers.  Initialize pointers on define.  
   // These are in the ADODB::  namespace.  
   _ConnectionPtr pConnection = NULL;  
   _RecordsetPtr pRst = NULL;  
   _RecordsetPtr pRstTitleAuthor = NULL;  
  
   try {  
      TESTHR(pRstTitleAuthor.CreateInstance(__uuidof(Recordset)));  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      TESTHR(pRst.CreateInstance(__uuidof(Recordset)));  
  
      // Open connection.  
      pConnection->Open (strCnn, "", "", adConnectUnspecified);  
      pRst->PutStayInSync(true);  
  
      // Open recordset with names from Author & titleauthor table.  
      pRst->Open("SHAPE  {select * from authors} "   
         "APPEND ({select * from titleauthor}"  
         "RELATE au_id TO au_id) AS chapTitleAuthor",  
         _variant_t((IDispatch*)pConnection, true), adOpenStatic, adLockReadOnly, adCmdText);  
  
      pRstTitleAuthor = pRst->GetFields()->GetItem("chapTitleAuthor")->Value;  
      while (!(pRst->EndOfFile)) {      
         printf("\n%s  %s  %s    %s\n", (LPCSTR)(_bstr_t)pRst->  
            Fields->GetItem("au_fname")->Value,  
            (LPCSTR)(_bstr_t)pRst->Fields->GetItem("au_lname")->Value,  
            (LPCSTR)(_bstr_t)pRst->Fields->GetItem("state")->Value,   
            (LPCSTR)(_bstr_t)pRst->Fields->GetItem("au_id")->Value);  
  
         _variant_t vIndex;  
         while ( !(pRstTitleAuthor->EndOfFile) ) {  
            vIndex = (short) 0;  
            printf("%s    ",(LPCSTR)(_bstr_t)pRstTitleAuthor->Fields->Item[&vIndex]->Value);  
            vIndex = (short) 1;  
            printf("%s    ",(LPCSTR)(_bstr_t)pRstTitleAuthor->Fields->Item[&vIndex]->Value);  
            vIndex = (short) 2;  
            printf("%s    ",(LPCSTR)(_bstr_t)pRstTitleAuthor->Fields->Item[&vIndex]->Value);  
            vIndex = (short) 3;  
            printf("%s\n",(LPCSTR)(_bstr_t)pRstTitleAuthor->Fields->Item[&vIndex]->Value);  
  
            pRstTitleAuthor->MoveNext();  
         }  
         pRst->MoveNext();  
      }  
   }  
   catch(_com_error &e) {  
      // Notify user of errors, if any.  Pass connection pointer accessed from the Recordset.  
      PrintProviderError(pConnection);  
      PrintComError(e);     
   }  
  
   // Clean up objects before exit.  
   if (pRst)  
      if (pRst->State == adStateOpen)  
         pRst->Close();  
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0) {  
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
 [Raccolta Fields (ADO)](./fields-collection-ado.md)   
 [Oggetto recordset (ADO)](./recordset-object-ado.md)   
 [Proprietà StayInSync](./stayinsync-property.md)