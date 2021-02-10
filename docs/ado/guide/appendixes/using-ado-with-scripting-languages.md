---
description: Uso di ADO con i linguaggi di scripting
title: Utilizzo di ADO con i linguaggi di scripting | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripting languages [ADO]
- ADO, scripting languages
ms.assetid: 76fc4d00-0c9f-422b-af5c-af6ed8fb29d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f1919f3524f05035a4376f30cf2e36ae731bdea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028851"
---
# <a name="using-ado-with-scripting-languages"></a>Uso di ADO con i linguaggi di scripting
All'interno di un ambiente di scripting, ADO consente di esporre i dati tramite scripting lato server. In questo scenario, ADO, il provider OLE DB sottostante utilizzato e tutti gli altri componenti necessari per fare riferimento a un determinato archivio dati vengono installati in un server che esegue Internet Information Services (IIS). Utilizzando Active Server Pages (ASP), ADO è un componente a cui viene fatto riferimento in uno script in grado di generare codice HTML, ad esempio. Questo contenuto HTML può essere passato tramite HTTP a un Web browser client. Utilizzando lo scripting, la pagina Web può inviare azioni allo script sul lato server, consentendo di aggiornare, attraversare o visualizzare dati specifici.  
  
 Prima di utilizzare un oggetto ActiveX in una pagina Web, è importante sapere se l'oggetto è sicuro per lo scripting. Quando un oggetto viene considerato sicuro per lo scripting, significa che il controllo non può eseguire alcuna azione dannosa sul computer dell'utente e pertanto può essere eseguito senza richiedere l'approvazione dell'utente. Nella tabella seguente sono elencati gli oggetti ADO e viene indicato se sono sicuri per lo scripting.  
  
|Oggetto|Sicuro per gli script?|  
|------------|-------------------------|  
|Connessione ADO|Sì|  
|Comando ADO|No|  
|ADO-parametro|No|  
|Recordset ADO|Sì|  
|Record ADO|Sì|  
|Flusso ADO|Sì|  
|Errore ADO|No|  
|Catalogo ADOX|No|  
|ADOX di celle|No|  
|Controllo Servizi Desktop remoto|Sì|  
|Spazio di servizi RDS|Sì|  
|Datafactory RDS|No|  
  
 Nella tabella seguente sono elencati i provider inclusi in Windows DAC/MDAC e viene indicato se sono sicuri per lo scripting.  
  
|Provider|Sicuro per gli script?|  
|--------------|-------------------------|  
|Con forme|Sì|  
|Persist|Sì|  
|Remoto|Sì|  
|Provider di OLE DB per SQL Server (SQLOLEDB)|No|  
|Provider di OLE DB per ODBC (MSDASQL)|No|  
  
## <a name="odbc-data-sources"></a>Origini dei dati ODBC  
 Una differenza rilevante tra lo scripting e il codice ADO non di scripting è l'origine dati ODBC, se utilizzata. Per le applicazioni non di scripting, è possibile creare un DSN utente in Amministrazione origine dati ODBC. Per gli script in esecuzione in IIS, è necessario creare un DSN di sistema. in caso contrario, gli script non rileveranno l'origine dati creata. Si applica a qualsiasi applicazione di scripting ADO che utilizza il provider Microsoft OLE DB per ODBC tramite Microsoft IIS.  
  
## <a name="referencing-the-ado-library"></a>Riferimento alla libreria ADO  
 Non applicabile ai linguaggi di scripting.  
  
## <a name="handling-events"></a>Gestione degli eventi  
 Non applicabile ai linguaggi di scripting.  
  
 Negli argomenti seguenti sono contenute informazioni più specifiche sull'utilizzo di ADO con i linguaggi di scripting:  
  
-   [Programmazione ADO VBScript](./vbscript-ado-programming.md)  
  
-   [Programmazione ADO JScript](./jscript-ado-programming.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Microsoft ActiveX Data Objects (ADO)](../../microsoft-activex-data-objects-ado.md)   
 [Utilizzo di ADO con Microsoft Visual Basic](./using-ado-with-microsoft-visual-basic.md)   
 [Uso di ADO con Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md)