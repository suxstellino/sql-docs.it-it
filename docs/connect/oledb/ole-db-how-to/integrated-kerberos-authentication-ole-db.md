---
title: Autenticazione Kerberos integrata (OLE DB Driver) | Microsoft Docs
description: Informazioni ed esempi su come ottenere l'autenticazione Kerberos reciproca usando OLE DB in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3613746d14764192ad366af906654561320e7730
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739041"
---
# <a name="integrated-kerberos-authentication-ole-db"></a>Autenticazione Kerberos integrata (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questo esempio illustra come ottenere l'autenticazione Kerberos reciproca usando OLE DB Driver per SQL Server. Questo esempio può essere eseguito solo in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] o versione successiva.  
  
 Per altre informazioni sui nomi SPN e l'autenticazione Kerberos, vedere l'argomento relativo al [supporto dei nomi dell'entità servizio &#40;SPN&#41; nelle connessioni client](../../oledb/features/service-principal-name-spn-support-in-client-connections.md).  
  
## <a name="example"></a>Esempio  
 È necessario specificare un server. Nel file con estensione cpp impostare "MyServer" su un nome di computer in cui sia presente un'istanza di [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] o versione successiva.  
  
 È inoltre necessario specificare un nome SPN fornito dall'utente. Nel file con estensione cpp modificare "CPSPN" impostando un nome SPN fornito dall'utente.  
  
 Verificare che nella variabile di ambiente INCLUDE sia presente la directory che contiene msoledbsql.h. Eseguire la compilazione con oleaut32.lib ole32.lib.  
  
```cpp
// compile with: ole32.lib oleaut32.lib
#pragma once

#define WIN32_LEAN_AND_MEAN   // Exclude rarely-used stuff from Windows headers
#include <stdio.h>
#include <tchar.h>
#include <msoledbsql.h>

#define CHECKHR(stmt) \
do\
{\
    hr = (stmt);\
    if (FAILED(hr))\
    {\
       printf("CHECK_HR " #stmt " failed at (%hs, %d), hr=0x%08X\r\n", __FILE__, __LINE__, hr); \
       goto CleanUp; \
    } \
} while (0)

#define SAFERELEASE(p) \
do\
{\
    if ((p) != nullptr)\
    {\
       p->Release(); \
       p = nullptr; \
    } \
} while (0)


int _tmain(int argc, _TCHAR* argv[])
{
    HRESULT hr = S_OK;
    IDBInitialize* pInitialize = nullptr;
    IDBProperties* pProperties = nullptr;
    DBPROP rgDBProp[1] = {};
    LPCWSTR lpwszProviderString = L"Server=MyServer;"   // server with SQL Server 2008 (or later)
       L"Trusted_Connection=Yes;"
       L"Encrypt=yes;"
       L"ServerSPN=CP_SPN;";   // customer-provided SPN

    DBPROPSET* prgPropertySets = nullptr;
    ULONG cPropertySets = 0;

    CHECKHR(CoInitialize(nullptr));
    CHECKHR(CoCreateInstance(CLSID_MSOLEDBSQL, nullptr, CLSCTX_INPROC_SERVER, __uuidof(IDBProperties), reinterpret_cast<void**>(&pProperties)));

    // set provider string
    rgDBProp[0].dwPropertyID = DBPROP_INIT_PROVIDERSTRING;
    rgDBProp[0].dwOptions = DBPROPOPTIONS_REQUIRED;
    rgDBProp[0].colid = DB_NULLID;
    VariantInit(&(rgDBProp[0].vValue));
    V_VT(&(rgDBProp[0].vValue)) = VT_BSTR;
    V_BSTR(&(rgDBProp[0].vValue)) = SysAllocString(lpwszProviderString);

    { // set the property to the property set
        DBPROPSET PropertySet[1] = {};
        PropertySet[0].rgProperties = &rgDBProp[0];
        PropertySet[0].cProperties = 1;
        PropertySet[0].guidPropertySet = DBPROPSET_DBINIT;

        // set properties and connect to server
        CHECKHR(pProperties->SetProperties(sizeof(PropertySet)/sizeof(DBPROPSET), PropertySet));
    }

    CHECKHR(pProperties->QueryInterface<IDBInitialize>(&pInitialize));
    CHECKHR(pInitialize->Initialize());

    { // get properties
        DBPROPIDSET rgDBPropIDSet[1] = {};

        DBPROPID rgDBPropID[2] = {};
        rgDBPropID[0] = SSPROP_INTEGRATEDAUTHENTICATIONMETHOD;
        rgDBPropID[1] = SSPROP_MUTUALLYAUTHENTICATED;

        rgDBPropIDSet[0].rgPropertyIDs = &rgDBPropID[0];
        rgDBPropIDSet[0].cPropertyIDs = 2;
        rgDBPropIDSet[0].guidPropertySet = DBPROPSET_SQLSERVERDATASOURCEINFO;
        CHECKHR(pProperties->GetProperties(1, rgDBPropIDSet, &cPropertySets, &prgPropertySets));
    }
    wprintf(L"Authentication method: %s\r\n", V_BSTR(&(prgPropertySets[0].rgProperties[0].vValue)));
    wprintf(L"Mutually authenticated: %s\r\n",
        (V_BOOL(&(prgPropertySets[0].rgProperties[1].vValue)) == VARIANT_TRUE) ? L"yes" : L"no");

CleanUp:
    SAFERELEASE(pProperties);
    SAFERELEASE(pInitialize);

    if (prgPropertySets)
    {
        for (ULONG iPropSet = 0; iPropSet < cPropertySets; ++iPropSet)
        {
            for (ULONG iProp = 0; iProp < prgPropertySets[iPropSet].cProperties; ++iProp)
            {
                VariantClear(&prgPropertySets[iPropSet].rgProperties[iProp].vValue);
            }
        }
    }

    VariantClear(&(rgDBProp[0].vValue));
    CoUninitialize();
}
```