---
title: Uso del controllo RSClientPrint in applicazioni personalizzate | Microsoft Docs
description: RSPrintClient consente la stampa sul lato client dei report visualizzati nel Visualizzatore HTML. Il server di report esegue il rendering del report nell'estensione per il rendering delle immagini (EMF).
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- RSPrintClient control
- print controls [Reporting Services]
- custom printing [Reporting Services]
- client-side printing
ms.assetid: 8c0bdd18-8905-4e22-9774-a240fc81a8a7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c977799cb20d3735b4cc77d2608c0412ce51769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022284"
---
# <a name="using-the-rsclientprint-control-in-custom-applications"></a>Utilizzo del controllo RSClientPrint in applicazioni personalizzate
  Il controllo [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ActiveX **RSPrintClient** consente la stampa sul lato client dei report visualizzati nel Visualizzatore HTML. Offre una finestra di dialogo **Stampa** per consentire all'utente di avviare un processo di stampa, visualizzare un'anteprima di un report, specificare le pagine da stampare e modificare i margini. Durante la stampa sul lato client, il server di report esegue il rendering del report con l'estensione per il rendering Immagine (EMF) e utilizza le funzionalità di stampa del sistema operativo per creare il processo di stampa e inviarlo a una stampante.  
  
 La funzionalità di stampa sul lato client consente di controllare e migliorare la qualità di stampa di un report HTML evitando l'uso delle impostazioni di stampa del browser del computer in uso e utilizzando invece le dimensioni di pagina, i margini, il testo dell'intestazione e del piè di pagina del report per creare l'output di stampa. Il controllo di stampa legge i valori delle proprietà del report per impostare le dimensioni e i margini delle pagine.  
  
 Gli sviluppatori che intendono attivare la funzionalità di stampa sul lato client in barre degli strumenti di terze parti possono accedere al controllo ActiveX tramite l'oggetto COM **RSClientPrint**. La distribuzione del controllo è consentita e gratuita. Se si desidera utilizzare il controllo, leggere i consigli seguenti:  
  
-   Utilizzare il controllo per migliorare la stampa di report per il Web. È possibile specificare l'oggetto in qualsiasi script o linguaggio di programmazione compatibile con [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Il controllo non è progettato per applicazioni [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Form.  
  
-   Copiare il file con estensione cab dai file di programma di [!INCLUDE[ssRSNoVersion](../../../includes/ssrsnoversion-md.md)] e aggiungerlo alla base di codice dell'applicazione personalizzata.  
 
-   Usare un \<OBJECT> tag per specificare il controllo.  
  
-   Specificare un URL relativo o completo per il file CAB nell'attributo OBJECT CODEBASE.  
  
-   Specificare le informazioni di versione dell'applicazione personalizzata per il file CAB per tenere traccia della versione utilizzata nell'applicazione.  
    
  
## <a name="rsprintclient-overview"></a>Cenni preliminari su RSPrintClient  
 Il controllo visualizza una finestra di dialogo di stampa personalizzata che supporta funzionalità comuni ad altre finestre di dialogo di stampa, inclusi l'anteprima di stampa, la selezione delle pagine per specificare pagine e intervalli, i margini delle pagine e l'orientamento. Il controllo è distribuito come file CAB. Il testo della finestra di dialogo **Stampa** è localizzato in tutte le lingue supportate da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per stampare il report, il controllo ActiveX **RSPrintClient** usa l'estensione per il rendering delle immagini (EMF). Vengono utilizzate le informazioni sul dispositivo EMF seguenti: StartPage, EndPage, MarginBottom, MarginLeft, MarginTop, MarginRight, PageHeight e PageWidth. Le altre impostazioni del dispositivo per il rendering in formato immagine non sono supportate.  
  
### <a name="language-support"></a>Supporto delle lingue  
 Il controllo include stringhe testo per l'interfaccia utente in diverse lingue e accetta valori di input in vari sistemi di misura. La lingua e il sistema di misura usati vengono determinati dalle proprietà **Culture** e **UICulture**. che accettano entrambe valori LCID. Se si specifica un identificatore LCID per una lingua che è una variante di una lingua supportata, verrà utilizzata la lingua più simile corrispondente. Se si specifica un LCID di una lingua non supportata e per la quale non esiste un LCID simile corrispondente, verrà utilizzato l'inglese (Stati Uniti).  
  
## <a name="using-rsclientprint-in-code"></a>Utilizzo di RSClientPrint nel codice  
 L'oggetto **RSClientPrint** viene usato per accedere al controllo ActiveX e ai relativi metodi e proprietà a livello di codice. Il controllo include una finestra di dialogo modale per l'anteprima di stampa.  
  
### <a name="specifying-default-values"></a>Specifica dei valori predefiniti  
 È possibile inizializzare la finestra di dialogo **Stampa** con i valori di margine e pagina del report. Per impostazione predefinita, la finestra di dialogo **Stampa** viene inizializzata con i valori della definizione del report. È possibile utilizzare i valori predefiniti oppure specificare valori diversi impostando le proprietà dell'oggetto.  
  
 Tutte le dimensioni sono specificate in millimetri. La conversione delle misure viene eseguita in fase di esecuzione se le proprietà **Culture** e **UICulture** specificano impostazioni locali che non usano il sistema metrico.  
  
 Per conoscere i valori usati per le dimensioni e i margini delle pagine, è possibile recuperare i valori predefiniti con il metodo **GetProperties**:  
  
-   **PageHeight** e **PageWidth** specificano l'altezza e la larghezza predefinite per la pagina. All'avvio del controllo di stampa, i valori di queste proprietà vengono utilizzati per selezionare le dimensioni del foglio più vicine a quelle del report per la stampante selezionata. Se **PageWidth** è maggiore di **PageHeight**, viene impostato l'orientamento orizzontale. In caso contrario, l'orientamento è verticale.  
  
-   L'impostazione predefinita per **LeftMargin**, **RightMargin**, **TopMargin** e **BottomMargin** è di 12,2 mm.  
  
 Queste proprietà sono archiviate nell'insieme di proprietà **Item** nel server di report. I valori vengono sovrascritti ogni volta che la definizione di un report viene aggiornata.  
  
### <a name="rsclientprint-properties"></a>Proprietà di RSClientPrint  
  
|Proprietà|Type|LS|Predefinito|Descrizione|  
|--------------|----------|--------|-------------|-----------------|  
|MarginLeft|Double|LS|Impostazione del report|Recupera o imposta il margine sinistro. Il valore predefinito è di 12,2 mm se non viene specificato un valore diverso dallo sviluppatore o nel report.|  
|MarginRight|Double|LS|Impostazione del report|Recupera o imposta il margine destro. Il valore predefinito è di 12,2 mm se non viene specificato un valore diverso dallo sviluppatore o nel report.|  
|MarginTop|Double|LS|Impostazione del report|Recupera o imposta il margine superiore. Il valore predefinito è di 12,2 mm se non viene specificato un valore diverso dallo sviluppatore o nel report.|  
|MarginBottom|Double|LS|Impostazione del report|Recupera o imposta il margine inferiore. Il valore predefinito è di 12,2 mm se non viene specificato un valore diverso dallo sviluppatore o nel report.|  
|PageWidth|Double|LS|Impostazione del report|Recupera o imposta la larghezza della pagina. Il valore predefinito è di 215,9 mm se non viene specificato un valore diverso dallo sviluppatore o nella definizione del report.|  
|PageHeight|Double|LS|Impostazione del report|Recupera o imposta l'altezza della pagina. Il valore predefinito è di 279,4 mm se non viene specificato un valore diverso dallo sviluppatore o nella definizione del report.|  
|Impostazioni cultura|Int32|LS|Impostazioni locali del browser|Specifica l'identificatore delle impostazioni locali (LCID). Questo valore determina l'unità di misura per l'input dell'utente. Se ad esempio l'utente digita **3**, il valore verrà misurato in millimetri se la lingua è il francese e in pollici se la lingua è l'inglese (Stati Uniti). I valori validi sono: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|UICulture|string|LS|Impostazioni internazionali del client|Specifica la lingua delle stringhe della finestra di dialogo. Per il testo della finestra di dialogo di stampa sono disponibili le lingue seguenti: cinese semplificato, cinese tradizionale, inglese, francese, tedesco, italiano, giapponese, coreano e spagnolo. I valori validi sono: 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|Authenticate|Boolean|LS|False|Specifica se il controllo genera un comando GET per il server di report per avviare una sessione per la stampa fuori sessione.|  
  
### <a name="when-to-set-the-authenticate-property"></a>Quando impostare la proprietà Authenticate  
 Quando si stampa nell'ambito di una sessione del browser, l'impostazione della proprietà **Authenticate** non è necessaria. Nel contesto di una sessione attiva, tutte le richieste dal controllo di stampa al server di report vengono gestite tramite il browser. Il browser imposta le variabili di sessione necessarie per la comunicazione al server di report.  
  
 Se si stampa fuori dall'ambito di una sessione, ad esempio inviando un report direttamente a una stampante senza prima aprirlo, il controllo di stampa deve generare una richiesta HTTP **GET** per impostare la connessione con il server di report. Per inviare la richiesta **GET**, impostare **Authenticate** su **True**.  
  
 La generazione della richiesta **GET** è necessaria solo se si usa la sicurezza integrata di Windows o l'autenticazione di base. Se si usa l'autenticazione basata su form, la proprietà **Authenticate** viene ignorata. Il codice dell'applicazione deve impostare la sessione e autenticare l'utente utilizzando l'estensione di sicurezza personalizzata fornita dall'utente. Se si utilizza l'autenticazione basata su form, assicurarsi di impostare la scadenza del cookie di autenticazione su un valore che mantenga le sessioni per un intervallo di tempo ragionevole. Se il valore è troppo basso, agli utenti verrà richiesto di immettere le credenziali di accesso ogni volta che il cookie scade.  
  
### <a name="clsids"></a>CLSID  
 Quando si esegue il report in locale, utilizzare i valori CLSID seguenti.  
  
-   41861299-EAB2-4DCC-986C-802AE12AC499  
  
-   5554DCB0-700B-498D-9B58-4E40E5814405  
  
-   60677965-AB8B-464f-9B04-4BA871A2F17F  
  
 Quando si esegue il report in Azure SQL Reporting, utilizzare i valori CLSID seguenti.  
  
-   3DD32426-554D-48C0-A200-65D3BF880E38  
  
-   05662494-ACF9-446A-BE4C-7D3F7EA7F62F  
  
### <a name="rsprintclient-support-for-the-print-method"></a>Supporto di RSPrintClient per il metodo di stampa  
 L'oggetto **RSClientPrint** supporta il metodo **Print** usato per avviare la finestra di dialogo di stampa. Il metodo **Print** include gli argomenti descritti di seguito.  
  
|Argomento|I/O|Type|Descrizione|  
|--------------|----------|----------|-----------------|  
|ServerPath|In ingresso|string|Specifica la directory virtuale del server di report, ad esempio `https://adventure-works/reportserver`.|  
|ReportPathParameters|In ingresso|string|Specifica il nome completo del report nello spazio dei nomi delle cartelle del server di report, inclusi i parametri. I report vengono recuperati mediante l'accesso a un URL, ad esempio "/AdventureWorks Sample Reports/Employee Sales Summary&EmpID=1234"|  
|ReportName|In ingresso|string|Nome breve del report (nell'esempio precedente è Employee Sales Summary) che viene visualizzato nella finestra di dialogo di stampa e nella coda di stampa.|  
  
### <a name="example"></a>Esempio  
 Nell'esempio HTML riportato di seguito viene illustrato come specificare il file con estensione cab, il metodo **Print** e le proprietà in JavaScript:  
  
 `<BODY onload="Print()">`  
  
 `<OBJECT ID="RSClientPrint" CLASSID="CLSID: 5554DCB0-700B-498D-9B58-4E40E5814405D3" CODEBASE="<URL to the .CAB file>#Version=<your application version information>" VIEWASTEXT></OBJECT>`  
  
 `<script language="javascript">`  
  
 `function Print()`  
  
 `{`  
  
 `RSClientPrint.MarginLeft = 12.7;`  
  
 `RSClientPrint.MarginTop = 12.7;`  
  
 `RSClientPrint.MarginRight = 12.7;`  
  
 `RSClientPrint.MarginBottom = 12.7;`  
  
 `RSClientPrint.Culture = 1033;`  
  
 `RSClientPrint.UICulture = 9;`  
  
 `RSClientPrint.Print('https://localhost/rtm', '%2fEmployee_Sales_Summary&ReportMonth=6&ReportYear=2004&EmpID=20', 'Employee_Sales_Summary')`  
  
 `}`  
  
 `</script>`  
  
 `</BODY>`  
  
## <a name="see-also"></a>Vedere anche  
 [Stampare i report da un browser con il controllo di stampa &#40;Generatore report e SSRS&#41;](../../../reporting-services/report-builder/print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [Stampa di report &#40;Generatore report e SSRS&#41;](../../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Impostazioni relative alle informazioni sul dispositivo di acquisizione immagini](../../../reporting-services/image-device-information-settings.md)  
  
  
