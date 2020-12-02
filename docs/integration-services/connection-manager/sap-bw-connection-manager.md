---
description: Gestione connessione SAP BW
title: Gestione connessione SAP BW | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 06b166a1-a9df-48ea-a0e8-9b8d6979c0a1
f1_keywords:
- sql13.dts.designer.sapbwconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e09a6cdf6f8d863e57d2de0749e29b33c989eea
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "91719532"
---
# <a name="sap-bw-connection-manager"></a>Gestione connessione SAP BW

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La gestione connessione SAP BW è il componente per la gestione delle connessioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW. La gestione connessione SAP BW offre la connettività a un sistema SAP Netweaver BW versione 7 di cui hanno bisogno i componenti di origine e destinazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW. L'origine e la destinazione SAP BW che fanno parte del pacchetto di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW sono gli unici componenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che usano la gestione connessione SAP BW.  
  
> [!IMPORTANT]  
>  La documentazione per Microsoft Connector 1.1 for SAP BW presuppone la conoscenza dell'ambiente SAP Netweaver BW. Per ulteriori informazioni su SAP Netweaver BW o per informazioni su come configurare oggetti e processi di SAP Netweaver BW, vedere la documentazione SAP.  
  
 Quando si aggiunge una gestione connessione SAP BW a un pacchetto, la proprietà **ConnectionManagerType** della gestione connessione è impostata su **SAPBI**.  
  
## <a name="configuring-the-sap-bw-connection-manager"></a>Configurazione della gestione connessione SAP BW  
 Per configurare la gestione connessione SAP BW, procedere nel modo seguente:  
  
-   Specificare client, nome utente, password e lingua della connessione.  
  
-   Scegliere se stabilire la connessione a un server applicazioni singolo o a un gruppo di server con carico bilanciato.  
  
-   Specificare il numero di sistema e host per un server applicazioni oppure specificare server messaggi, gruppo e SID per un gruppo di server con carico bilanciato.  
  
-   Abilitare la registrazione personalizzata delle chiamate di funzione RFC per i componenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW. Tale registrazione è separata dalla registrazione facoltativa che è possibile abilitare nei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per abilitare la registrazione delle chiamate di funzione RFC, specificare una directory in cui archiviare i file di log creati prima e dopo ogni chiamata di funzione RFC. Questa funzionalità di registrazione crea molti file di log in formato XML. Poiché questi file di log contengono anche tutte le righe di dati trasferiti, è possibile che occupino una grande quantità di spazio su disco. Se non si seleziona una directory di log, la registrazione non è abilitata.  
  
    > [!IMPORTANT]  
    >  Se i dati trasferiti contengono informazioni riservate, anche i file di log conterranno tali informazioni riservate.  
  
-   Utilizzare i valori immessi per verificare la connessione.  
  
 Se non si conoscono tutti i valori richiesti per configurare la gestione connessione, può essere necessario consultare l'amministratore SAP.  
  
 Per una procedura dettagliata che illustra come configurare e utilizzare la gestione connessione, l'origine e la destinazione SAP BW, vedere il white paper [Utilizzo dei servizi di integrazione SQL Server 2008 con SAP BI 7.0](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100)). Nel white paper viene anche indicato come configurare gli oggetti necessari in SAP BW.  
  
### <a name="using-the-ssis-designer-to-configure-the-source"></a>Utilizzo di Progettazione SSIS per configurare l'origine  
 Per ulteriori informazioni sulle proprietà della gestione connessione SAP BW che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Editor gestione connessione SAP BW]()  
  
## <a name="sap-bw-connection-manager-editor"></a>Editor gestione connessione SAP BW
  Utilizzare l' **Editor gestione connessione SAP BW** per specificare le proprietà per la connessione a un sistema SAP Netweaver BW versione 7.  
  
 La gestione connessione SAP BW offre connettività a un sistema SAP Netweaver BW 7 per l'utilizzo da parte dell'origine o della destinazione SAP BW. Per sapere di più sulla gestione connessione SAP BW di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 per SAP BW, vedere [Gestione connessione SAP BW](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
> [!IMPORTANT]  
>  La documentazione per Microsoft Connector 1.1 for SAP BW presuppone la conoscenza dell'ambiente SAP Netweaver BW. Per ulteriori informazioni su SAP Netweaver BW o per informazioni su come configurare oggetti e processi di SAP Netweaver BW, vedere la documentazione SAP.  
  
 **Per aprire l'editor gestione connessione SAP BW**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il pacchetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene la gestione connessione SAP BW.  
  
2.  Nella scheda **Flusso di controllo** dell'area Gestioni connessioni, effettuare una delle operazioni seguenti:  
  
    -   Fare doppio clic sulla gestione connessione SAP BW.  
  
         -oppure-  
  
    -   Fare clic con il tasto destro del mouse sulla gestione connessione SAP BW, quindi scegliere **Modifica**.  
  
### <a name="options"></a>Opzioni  
  
> [!NOTE]  
>  Se non si conoscono tutti i valori richiesti per configurare la gestione connessione, può essere necessario consultare l'amministratore SAP.  
  
 **Client**  
 Specificare il numero client del sistema.  
  
 **Lingua**  
 Specificare la lingua utilizzata dal sistema. Ad esempio, specificare **IT** per l'italiano.  
  
 **Nome utente**  
 Specificare il nome utente che verrà utilizzato per connettersi al sistema.  
  
 **Password**  
 Specificare la password che verrà utilizzata con il nome utente.  
  
 **Usa server applicazioni singolo**  
 Connettersi a un server applicazioni singolo.  
  
 Per connettersi a un gruppo di server con carico bilanciato, usare in alternativa l'opzione **Usa bilanciamento del carico** .  
  
 **Host**  
 In caso di connessione a un server applicazioni singolo, specificare il nome host.  
  
> [!NOTE]  
>  Questa opzione è disponibile solo se è stata selezionata l'opzione **Usa server applicazioni singolo** .  
  
 **Numero di sistema**  
 In caso di connessione a un server applicazioni singolo, specificare il numero del sistema.  
  
> [!NOTE]  
>  Questa opzione è disponibile solo se è stata selezionata l'opzione **Usa server applicazioni singolo** .  
  
 **Usa bilanciamento del carico**  
 Connettersi a un gruppo di server con carico bilanciato.  
  
 Per connettersi a un server applicazioni singolo, usare in alternativa l'opzione **Usa server applicazioni singolo** .  
  
 **Server messaggi**  
 In caso di connessione a un gruppo di server con carico bilanciato, specificare il nome del server messaggi.  
  
> [!NOTE]  
>  Questa opzione è disponibile solo se è stata selezionata l'opzione **Usa bilanciamento del carico** .  
  
 **Gruppo**  
 In caso di connessione a un gruppo di server con carico bilanciato, specificare il nome del gruppo di server.  
  
> [!NOTE]  
>  Questa opzione è disponibile solo se è stata selezionata l'opzione **Usa bilanciamento del carico** .  
  
 **SID**  
 In caso di connessione a un gruppo di server con carico bilanciato, specificare l'ID sistema per la connessione.  
  
> [!NOTE]  
>  Questa opzione è disponibile solo se è stata selezionata l'opzione **Usa bilanciamento del carico** .  
  
 **Directory log**  
 Abilitare la registrazione per i componenti di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 per SAP BW.  
  
 Per abilitare la registrazione, specificare una directory per i file di log creati prima e dopo ogni chiamata di funzione RFC. Questa funzionalità di registrazione crea molti file di log in formato XML. Poiché questi file di log contengono anche tutte le righe di dati trasferiti, è possibile che occupino una grande quantità di spazio su disco.  
  
> [!IMPORTANT]  
>  Se i dati trasferiti contengono informazioni riservate, anche i file di log conterranno tali informazioni riservate.  
  
 Per specificare la directory di log, è possibile immettere il percorso della directory manualmente oppure fare clic su **Sfoglia** e passare alla directory di log.  
  
 Se non si seleziona una directory di log, la registrazione non è abilitata.  
  
 **Sfoglia**  
 Sfogliare per selezionare una cartella per la directory di log.  
  
 **Test connessione**  
 Verificare la connessione utilizzando i valori forniti. Dopo avere fatto clic su **Test connessione** viene visualizzata una finestra di messaggio che indica se la connessione ha avuto esito positivo o negativo.  
  
## <a name="see-also"></a>Vedere anche  
 [Componenti di Microsoft Connector for SAP BW](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
