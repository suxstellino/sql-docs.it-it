---
title: Distribuzione di un'estensione per il rendering | Microsoft Docs
description: Informazioni su come distribuire un'estensione per il rendering del report. Informazioni su quali voci del file di configurazione aggiungere in modo che il server di report e Progettazione report individuino l'estensione.
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- deploying [Reporting Services], extensions
- rendering extensions [Reporting Services], deploying
ms.assetid: 9fb8c887-5cb2-476e-895a-7b0e2dd11398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 56c22120fb08b538b845647f1a2300c22e17d017
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042212"
---
# <a name="deploying-a-rendering-extension"></a>Distribuzione di un'estensione per il rendering
  Dopo avere scritto e compilato l'estensione per il rendering del report [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] in una libreria di [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], è necessario renderla individuabile dal server di report e da Progettazione report. A tale scopo, copiare l'estensione nella directory appropriata e aggiungere voci ai file di configurazione di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] appropriati.  
  
## <a name="configuration-file-rendering-extension-element"></a>Elemento di estensione del rendering del file di configurazione  
 Dopo avere compilato un'estensione per il rendering in una DLL, è necessario aggiungere una voce al file rsreportserver.config. Per impostazione predefinita, il percorso è %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer. L'elemento padre è \<Render>. Sotto l'elemento Render è presente un elemento Extension per ogni estensione per il rendering. L'elemento **Extension** contiene due attributi, Name e Type.  
  
 Nella tabella riportata di seguito vengono descritti gli attributi relativi all'elemento **Extension** per le estensioni per il rendering:  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|**Nome**|Nome univoco dell'estensione. La lunghezza massima consentita per l'attributo **Name** è 255 caratteri. Il nome deve essere univoco all'interno di tutte le voci dell'elemento **Extension** di un file di configurazione. Se è presente un nome duplicato, il server di report restituirà un errore.|  
|**Tipo**|Elenco delimitato da virgole che include lo spazio dei nomi completo insieme al nome dell'assembly.|  
|**Visible**|Il valore **false** indica che l'estensione per il rendering non deve essere visibile nelle interfacce utente. Se l'attributo non è incluso, il valore predefinito è **true**.|  
|**LogAllExecutionRequests**|Il valore **false** indica che una voce viene registrata solo per la prima esecuzione del report in una sessione. Se l'attributo non è incluso, il valore predefinito è **true**.<br /><br /> Questa impostazione determina ad esempio se registrare una voce solo per la prima pagina di cui viene eseguito il rendering in un report (quando il valore è **false**) o una voce per ogni pagina sottoposta a rendering nel report (quando il valore è **true**).|  
  
 Per altre informazioni, vedere [File di configurazione RsReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
## <a name="deploying-the-extension-to-the-report-server"></a>Distribuzione dell'estensione nel server di report  
 Il server di report usano le estensioni per il rendering per esportare i report in altri formati. È necessario distribuire l'assembly dell'estensione per il rendering nel server di report come assembly privato. È inoltre necessario immettere una voce nel file di configurazione del server di report, rsreportserver.config.  
  
### <a name="to-deploy-the-assembly"></a>Per distribuire l'assembly  
  
1.  Copiare l'assembly dal percorso di gestione temporanea nella directory bin del server di report in cui si desidera usare l'estensione per il rendering. Il percorso predefinito della directory Bin del server di report è %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer\Bin.  
  
2.  Dopo aver copiato il file di assembly, aprire il file rsreportserver.config, situato nella directory bin del server di report. È necessario immettere una voce nel file di configurazione per il file di assembly dell'estensione. È possibile aprire il file con [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] o con un semplice editor di testo.  
  
     Per altre informazioni, vedere [File di configurazione RsReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
3.  Individuare l'elemento **Render** nel file Rsreportserver.config. È necessario immettere una voce per l'estensione appena creata nel percorso seguente:  
  
    ```  
    <Extensions>  
       <Render>  
          <extension configuration>  
       </Render>  
    </Extensions>  
    ```  
  
4.  Aggiungere una voce per l'estensione per il rendering. La voce deve includere un elemento con valori per **Name** e **Type** e deve essere simile a quanto riportato di seguito:  
  
    ```  
    <Extension Name="My Rendering Extension Name" Type="CompanyName.ExtensionName.MyRenderingProvider, AssemblyName" />  
    ```  
  
     Il valore per **Name** è il nome univoco dell'estensione per il rendering. Il valore per **Type** è un elenco con valori delimitati da virgole che include una voce per lo spazio dei nomi completo dell'implementazione di <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension>, seguito dal nome dell'assembly, senza l'estensione dll. Per impostazione predefinita, le estensioni per il rendering sono visibili. Per nascondere un'estensione dalle interfacce utente, come Gestione report, aggiungere un attributo **Visible** all'elemento **Extension** e impostarlo su **false**.  
  
## <a name="verifying-the-deployment"></a>Verifica della distribuzione  
 È inoltre possibile aprire Gestione report e verificare che l'estensione sia inclusa nell'elenco dei tipi di esportazione disponibili per un report.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione di un'estensione per il rendering](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Panoramica delle estensioni per il rendering](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)   
 [Implementazione dell'interfaccia IRenderingExtension](../../../reporting-services/extensions/rendering-extension/implementing-the-irenderingextension-interface.md)   
 [Considerazioni sulla sicurezza per le estensioni](../../../reporting-services/extensions/security-considerations-for-extensions.md)  
  
  
