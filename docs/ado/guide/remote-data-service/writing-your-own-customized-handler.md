---
description: Scrittura di un gestore personalizzato
title: Scrittura di un gestore personalizzato | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
- customized handler in RDS [ADO]
ms.assetid: d447712a-e123-47b5-a3a4-5d366cfe8d72
author: rothja
ms.author: jroth
ms.openlocfilehash: f2c62c8b12435ec70166b66416f343fa835f6158
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032609"
---
# <a name="writing-your-own-customized-handler"></a>Scrittura di un gestore personalizzato
È possibile scrivere un gestore personalizzato se si è un amministratore del server IIS che desidera il supporto per Servizi Desktop remoto predefinito, ma maggiore controllo sulle richieste e sui diritti di accesso degli utenti.  
  
 MSDFMAP. Il gestore implementa l'interfaccia **IDataFactoryHandler** .  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="idatafactoryhandler-interface"></a>Interfaccia IDataFactoryHandler  
 Questa interfaccia dispone di due metodi, **getRecordset** e **Reconnect**. Per entrambi i metodi è necessario che la proprietà [CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md) sia impostata su **adUseClient**.  
  
 Entrambi i metodi accettano argomenti che vengono visualizzati dopo la prima virgola nella parola chiave "**handler =**". Ad esempio, `"Handler=progid,arg1,arg2;"` passerà una stringa di argomento di `"arg1,arg2"` e `"Handler=progid"` passerà un argomento null.  
  
## <a name="getrecordset-method"></a>Metodo GetRecordSet  
 Questo metodo esegue una query sull'origine dati e crea un nuovo oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) usando gli argomenti forniti. Il **Recordset** deve essere aperto con **adLockBatchOptimistic** e non deve essere aperto in modo asincrono.  
  
### <a name="arguments"></a>Argomenti  
 ***conn***  Stringa di connessione.  
  
 ***argomenti***  Argomenti per il gestore.  
  
 ***query***  di  Testo del comando per eseguire una query.  
  
 ***PPRS** _ il puntatore in cui deve essere restituito _ *Recordset**.  
  
## <a name="reconnect-method"></a>Metodo di riconnessione  
 Questo metodo aggiorna l'origine dati. Viene creato un nuovo oggetto [Connection](../../reference/ado-api/connection-object-ado.md) e il **Recordset** specificato viene collegato.  
  
### <a name="arguments"></a>Argomenti  
 ***conn***  Stringa di connessione.  
  
 ***argomenti***  Argomenti per il gestore.  
  
 ***pRS** _ un oggetto _ *Recordset**.  
  
## <a name="msdfhdlidl"></a>msdfhdl. idl  
 Si tratta della definizione dell'interfaccia per **IDataFactoryHandler** che viene visualizzata nel file **msdfhdl. idl** .  
  
```cpp
[  
  uuid(D80DE8B3-0001-11d1-91E6-00C04FBBBFB3),  
  version(1.0)  
]  
library MSDFHDL  
{  
    importlib("stdole32.tlb");  
    importlib("stdole2.tlb");  
  
    // TLib : Microsoft ActiveX Data Objects 2.0 Library  
    // {00000200-0000-0010-8000-00AA006D2EA4}  
    #ifdef IMPLIB  
    importlib("implib\\x86\\release\\ado\\msado15.dll");  
    #else  
    importlib("msado20.dll");  
    #endif  
  
    [  
      odl,  
      uuid(D80DE8B5-0001-11d1-91E6-00C04FBBBFB3),  
      version(1.0)  
    ]  
    interface IDataFactoryHandler : IUnknown  
    {  
HRESULT _stdcall GetRecordset(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] BSTR query,  
      [out, retval] _Recordset **ppRS);  
  
// DataFactory will use the ActiveConnection property  
// on the Recordset after calling Reconnect.  
   HRESULT _stdcall Reconnect(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] _Recordset *pRS);  
    };  
};  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sezione connessione file di personalizzazione](./customization-file-connect-section.md)   
 [Sezione log file di personalizzazione](./customization-file-logs-section.md)   
 [Sezione SQL del file di personalizzazione](./customization-file-sql-section.md)   
 [Sezione utenti del file di personalizzazione](./customization-file-userlist-section.md)   
 [Personalizzazione di datafactory](./datafactory-customization.md)   
 [Impostazioni client obbligatorie](./required-client-settings.md)   
 [Informazioni sul file di personalizzazione](./understanding-the-customization-file.md)