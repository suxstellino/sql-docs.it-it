---
description: Esempio della proprietà Clustered (VC++)
title: Esempio di proprietà Clustered (VC + +) | Microsoft Docs
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
- Clustered property [ADOX], VC++ example
ms.assetid: b993e357-3e2e-48a7-a627-76909160c97f
author: rothja
ms.author: jroth
ms.openlocfilehash: f0a28393a5a2bbe84088c774c45e58abd7cfc14d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050301"
---
# <a name="clustered-property-example-vc"></a>Esempio della proprietà Clustered (VC++)
In questo esempio viene illustrata la proprietà [Clustered](./clustered-property-adox.md) di un [Indice](./index-object-adox.md). Si noti che i database Microsoft Jet non supportano gli indici cluster, pertanto questo esempio restituirà **false** per la proprietà **Clustered** di tutti gli indici nel database *Northwind* .  
  
```  
// BeginClusteredCpp.cpp  
// compile with: /EHsc  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void ClusteredX();  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL) ) )  
      return -1;  
  
   ClusteredX();  
   ::CoUninitialize();  
}  
  
void ClusteredX() {  
   HRESULT hr = S_OK;  
  
   // Define ADOX object pointers, initialize pointers. These are in ADODB namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
   _IndexPtr m_pIndex = NULL;  
  
   // Define other variables here  
   _variant_t vIndex;  
   try {  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
  
      // Connect to the catalog.  
      m_pCatalog->PutActiveConnection("Provider='Microsoft.JET.OLEDB.4.0';data source='c:\\Northwind.mdb';");  
  
      // Enumerate Tables.  
      for (short iTable = 0 ; iTable < m_pCatalog->Tables->Count ; iTable++ ) {  
         vIndex = iTable;  
         m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
  
         // Enumerate Indexes.  
         for (short iIndex = 0 ; iIndex < m_pTable->Indexes->Count ; iIndex++) {  
            vIndex = iIndex;  
            m_pIndex = m_pTable->Indexes->GetItem(vIndex);  
            cout << m_pTable->Name << "   " ;  
            cout << m_pIndex->Name << "   " << (m_pIndex->GetClustered() ? "True" : "False") << endl;  
         }  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in ClusteredX...."<< endl;  
   }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà Clustered (ADOX)](./clustered-property-adox.md)   
 [Oggetto Index (ADOX)](./index-object-adox.md)