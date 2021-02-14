---
description: Identificazione dell'origine dei pacchetti con firme digitali
title: Identificare l'origine dei pacchetti con firme digitali | Microsoft Docs
ms.custom: security
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.digitalsigning.f1
helpviewer_keywords:
- signing packages [Integration Services]
- certificates [Integration Services]
- packages [Integration Services], security
- security [Integration Services], certificates
- signing policies [Integration Services]
ms.assetid: a433fbef-1853-4740-9d5e-8a32bc4ffbb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f4607ffe77d7fe93aff86d8dc707bb86e515d67d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340795"
---
# <a name="identify-the-source-of-packages-with-digital-signatures"></a>Identificazione dell'origine dei pacchetti con firme digitali

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] può essere firmato con un certificato digitale per identificarne l'origine. Dopo la firma di un pacchetto con un certificato digitale, è possibile configurare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per controllare o verificare la firma digitale prima del caricamento del pacchetto. Per fare in modo che [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] controlli la firma, impostare un'opzione in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o nell'utilità **dtexec** (dtexec.exe) oppure impostare un valore facoltativo del Registro di sistema.  
  
## <a name="sign-a-package-with-a-digital-certificate"></a>Firma di un pacchetto con un certificato digitale  
 Prima di poter firmare un pacchetto con un certificato digitale, è necessario ottenere o creare il certificato. Dopo aver ottenuto il certificato, è possibile utilizzarlo per la firma del pacchetto. Per altre informazioni su come ottenere un certificato e usarlo per firmare un pacchetto, vedere [Firmare un pacchetto con un certificato digitale](#cert).  
  
## <a name="set-an-option-to-check-the-package-signature"></a>Impostazione di un'opzione per la verifica della firma del pacchetto  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] e l'utilità **dtexec** includono un'opzione per configurare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per la verifica della firma digitale dei pacchetti firmati. È possibile usare [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o l'utilità **dtexec** a seconda che si voglia controllare tutti i pacchetti o solo alcuni pacchetti specifici:  
  
-   Per controllare la firma digitale di tutti i pacchetti prima del caricamento in fase di progettazione, impostare l'opzione **Controlla firma digitale al caricamento di un pacchetto** in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Questa opzione è un'impostazione globale per tutti i pacchetti di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].
  
-   Per controllare la firma digitale di un singolo pacchetto, specificare l'opzione **/VerifyS[igned]** quando si usa l'utilità **dtexec** per eseguire il pacchetto. Per altre informazioni, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="set-a-registry-value-to-check-package-signature"></a>Impostazione di un valore del Registro di sistema per la verifica della firma del pacchetto  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] supporta anche un valore facoltativo del Registro di sistema, **BlockedSignatureStates**, che può essere usato per gestire i criteri di un'organizzazione per il caricamento di pacchetti firmati e non firmati. Il valore del Registro di sistema consente di impedire il caricamento di pacchetti non firmati o con firme non valide o non attendibili. Per altre informazioni su come impostare questo valore del Registro di sistema, vedere [Implementare criteri per le firme tramite l'impostazione di un valore del Registro di sistema](#registry).  
  
> **NOTA** Il valore facoltativo **BlockedSignatureStates** del registro di sistema può specificare un'impostazione più restrittiva rispetto all'opzione per la firma digitale impostata in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o alla riga di comando **dtexec** . In questo caso, l'impostazione del Registro di sistema più restrittiva ha la precedenza rispetto ad altre impostazioni.  

## <a name="implement-a-signing-policy-by-setting-a-registry-value"></a><a name="registry"></a> Implementare criteri per le firme tramite l'impostazione di un valore del Registro di sistema
  È possibile utilizzare un valore facoltativo del Registro di sistema per gestire i criteri dell'organizzazione per il caricamento dei pacchetti firmati o non firmati. Se si utilizza questo valore del Registro di sistema, è necessario crearlo in ogni computer in cui verranno eseguiti i pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e in cui si desidera applicare i criteri. Dopo l'impostazione del valore del Registro di sistema, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] controllerà o verificherà le firme prima di caricare i pacchetti.  
  
 La procedura in questo argomento descrive come aggiungere il valore facoltativo DWORD **BlockedSignatureStates** alla chiave del Registro di sistema HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS. Il valore dei dati in **BlockedSignatureStates** determina se un pacchetto debba essere bloccato se contiene una firma non attendibile o non valida oppure se non è firmato. In relazione allo stato delle firme usate per firmare i pacchetti, il valore del Registro di sistema **BlockedSignatureStates** usa le definizioni seguenti:  
  
-   Per *firma valida* si intende una firma che può essere letta.  
  
-   Per *firma non valida* si intende una firma il cui checksum decrittografato, ovvero l'hash unidirezionale del codice del pacchetto crittografato mediante una chiave privata, non corrisponde al checksum decrittografato calcolato nell'ambito del processo di caricamento dei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Per *firma attendibile* si intende una firma creata tramite un certificato digitale firmato da un'autorità di certificazione radice attendibile. Con questa impostazione non è necessario che il firmatario sia contenuto nell'elenco degli autori attendibili.  
  
-   Per *firma non attendibile* si intende una firma che non può essere verificata in riferimento al rilascio da parte di un'autorità di certificazione radice attendibile o una firma non corrente.  
  
 Nella tabella seguente sono elencati i valori validi dei dati DWORD e i criteri associati.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|0|Nessuna restrizione amministrativa.|  
|1|Blocco delle firme non valide.<br /><br /> Con questa impostazione non vengono bloccati i pacchetti non firmati.|  
|2|Blocco delle firme non valide e non attendibili.<br /><br /> Con questa impostazione non vengono bloccati i pacchetti non firmati, ma vengono bloccate le firme a generazione automatica.|  
|3|Blocco delle firme non valide e non attendibili e dei pacchetti non firmati<br /><br /> Con questa impostazione vengono bloccate anche le firme a generazione automatica.|  
  
> [!NOTE]  
>  L'impostazione consigliata per **BlockedSignatureStates** è 3. Questa impostazione garantisce la massima protezione da pacchetti non firmati o firme non valide o non attendibili, ma potrebbe non essere appropriata per tutte le circostanze. Per altre informazioni su come firmare elementi digitali, vedere l'argomento "[Introduzione alla firma di codice](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))" in MSDN Library.  
  
### <a name="to-implement-a-signing-policy-for-packages"></a>Per implementare criteri per le firme per i pacchetti  
  
1.  Fare clic sul menu **Start** e scegliere **Esegui**.  
  
2.  Nella finestra di dialogo Esegui digitare **regedit** e quindi fare clic su **OK**.  
  
3.  Individuare la chiave del Registro di sistema: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS  
  
4.  Fare clic con il pulsante destro del mouse su **MSDTS**, scegliere **Nuovo** e quindi **Valore DWORD**.  
  
5.  Aggiornare il nome del nuovo valore impostandolo su **BlockedSignatureStates**.  
  
6.  Fare clic con il pulsante destro del mouse su **BlockedSignatureStates** e quindi scegliere **Modifica**.  
  
7.  Nella finestra di dialogo **Modifica valore DWORD** digitare il valore 0, 1, 2 o 3.  
  
8.  Fare clic su **OK**.  
  
9. Scegliere **Esci** dal menu **File**.    

## <a name="sign-a-package-by-using-a-digital-certificate"></a><a name="cert"></a> Firmare un pacchetto con un certificato digitale
  Questo argomento illustra come firmare un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con un certificato digitale. È possibile utilizzare una firma digitale, insieme ad altre impostazioni, per evitare il caricamento e l'esecuzione di pacchetti non validi.  
  
 Prima di poter firmare un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è necessario effettuare le attività seguenti:  
  
-   Creare o ottenere una chiave privata da associare al certificato e archiviarla nel computer locale.  
  
-   Ottenere un certificato a scopo di firma del codice da un'autorità di certificazione attendibile. Per ottenere o creare un certificato, è possibile utilizzare uno dei metodi seguenti:  
  
    -   Ottenere un certificato da un'autorità di certificazione commerciale pubblica che emette certificati.  
  
    -   Ottenere un certificato da un server dei certificati che consente alle organizzazioni di emettere certificati internamente. È necessario aggiungere il certificato radice usato per firmare il certificato nell'archivio **Autorità di certificazione radice disponibili nell'elenco locale** . Per aggiungere il certificato radice, è possibile utilizzare lo snap-in Certificati per [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC). Per altre informazioni, vedere l'argomento "[Certificate Services](/windows/win32/seccrypto/certificate-services)" (Servizi certificati) in MSDN Library.  
  
    -   Creare un certificato solo a scopo di testing. Lo strumento di creazione certificati (Makecert.exe) genera certificati X.509 solo a scopo di testing. Per altre informazioni, vedere l'argomento "[Strumento di creazione certificati (Makecert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))" in MSDN Library.  
  
     Per ulteriori informazioni sui certificati, vedere la Guida relativa allo snap-in Certificati. Per altre informazioni sulla firma di risorse digitali, vedere l'argomento "[Signing and Checking Code with Authenticode](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537364(v=vs.85))" (Firma e verifica del codice con Authenticode) in MSDN Library.  
  
-   Verificare che il certificato sia stato abilitato per la firma di codice. Per determinare se un certificato è abilitato per la firma di codice, controllare le proprietà del certificato nello snap-in Certificati.  
  
-   Archiviare il certificato nell'archivio personale.  
  
 Dopo avere completato le attività precedenti, è possibile utilizzare la procedura descritta di seguito per firmare un pacchetto.  
  
### <a name="to-sign-a-package"></a>Per firmare un pacchetto  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto da firmare.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  In Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] scegliere **Firma digitale** dl menu **SSIS**.  
  
4.  Nella finestra di dialogo **Firma digitale** fare clic su **Firma**.  
  
5.  Nella finestra di dialogo **Seleziona certificato** selezionare un certificato.  
  
6.  (Facoltativo) Fare clic su **Visualizza certificato** per visualizzare informazioni sul certificato.  
  
7.  Fare clic su **OK** per chiudere la finestra di dialogo **Seleziona certificato** .  
  
8.  Fare clic su **OK** per chiudere la finestra di dialogo **Firma digitale** .  
  
9. Per salvare il pacchetto aggiornato, scegliere **Salva elementi selezionati** dal menu **File** .  
  
     Anche se il pacchetto è stato firmato, è necessario configurare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per controllare o verificare la firma digitale prima del caricamento del pacchetto.  

## <a name="digital-signing-dialog-box-ui-reference"></a><a name="signing_dialog"></a> Riferimento all'interfaccia utente della finestra di dialogo Firma digitale
  Utilizzare la finestra di dialogo **Firma digitale** per apporre una firma digitale a un pacchetto o rimuovere quella esistente. Per accedere alla finestra di dialogo **Firma digitale** , scegliere l'opzione **Firma digitale** dal menu **SSIS** in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Per altre informazioni, vedere [Firmare un pacchetto con un certificato digitale](#cert).  
  
### <a name="options"></a>Opzioni  
 **Firma**  
 Fare clic per aprire la finestra di dialogo **Seleziona certificato** e selezionare il certificato da usare.  
  
 **Rimuovi**  
 Fare clic su questo pulsante per rimuovere la firma digitale.  

## <a name="see-also"></a>Vedere anche  
 [Pacchetti di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Panoramica sulla sicurezza &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
  
