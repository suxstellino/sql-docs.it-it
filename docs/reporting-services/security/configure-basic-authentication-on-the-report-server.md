---
description: configurazione dell'autenticazione di base nel server di report
title: Configurare l'autenticazione di base nel server di report | Microsoft Docs
ms.date: 02/10/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- Basic authentication
ms.assetid: 8faf2938-b71b-4e61-a172-46da2209ff55
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d3b875eafb10e4234df2cde35bb61bbecfebad94
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489325"
---
# <a name="configure-basic-authentication-on-the-report-server"></a>configurazione dell'autenticazione di base nel server di report
  Per impostazione predefinita, tramite Reporting Services vengono accettate richieste che consentono di specificare l'autenticazione con negoziazione e NTLM. Se la distribuzione include applicazioni client o browser che utilizzano l'autenticazione di base, è necessario aggiungere questo tipo di autenticazione all'elenco di tipi supportati. Inoltre, se si desidera utilizzare Generatore report, è necessario abilitare l'accesso anonimo ai relativi file.  
  
 Per configurare l'autenticazione di base nel server di report, modificare gli elementi e i valori XML nel file RSReportServer.config. È possibile copiare e incollare gli esempi disponibili in questo argomento per sostituire i valori predefiniti.  
  
 Prima di abilitare l'autenticazione di base, verificare che l'infrastruttura di sicurezza la supporti. Con l'autenticazione di base, il servizio Web ReportServer passa le credenziali all'autorità di sicurezza locale. Se nelle credenziali è specificato un account utente locale, l'utente viene autenticato dall'autorità di sicurezza locale nel computer del server di report e ottiene un token di sicurezza valido per le risorse locali. Le credenziali per gli account utente di dominio vengono inoltrate e autenticate da un controller di dominio. Il ticket risultante è valido per le risorse di rete.  
  
 La crittografia del canale, ad esempio TLS (Transport Layer Security), protocollo precedentemente noto come SSL (Secure Sockets Layer), è necessaria se si vuole ridurre i rischi associati all'intercettazione delle credenziali durante il transito verso un controller di dominio nella rete. L'autenticazione di base trasmette il nome utente in testo non crittografato e la password con la codifica in base 64. Con l'aggiunta della crittografia del canale, il pacchetto diventa illeggibile. Per altre informazioni, vedere [Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
 Dopo aver abilitato l'autenticazione di base, tenere presente che gli utenti non possono selezionare l'opzione **sicurezza integrata di Windows** durante l'impostazione delle proprietà di connessione a un'origine dati esterna che fornisce i dati a un report. Nelle pagine di proprietà dell'origine dati l'opzione verrà visualizzata in grigio.  
  
> [!NOTE]  
>  Le istruzioni seguenti sono relative a un server di report in modalità nativa. Se il server di report è distribuito in modalità integrata SharePoint, è necessario utilizzare le impostazioni di autenticazione predefinite che specificano la sicurezza integrata di Windows. Per supportare server di report in modalità integrata SharePoint, il server di report utilizza caratteristiche interne nell'estensione di autenticazione di Windows predefinita.  
  
### <a name="to-configure-a-report-server-to-use-basic-authentication"></a>Per configurare un server di report per l'utilizzo dell'autenticazione di base  
  
1. Aprire RSReportServer.config in un editor di testo.  
  
     Per trovare il file di configurazione, vedere la sezione [percorso file](../report-server/rsreportserver-config-configuration-file.md#bkmk_file_location) nell'articolo "RsReportServer.config file di configurazione".
  
2. Individuare \<**Authentication**>.  
  
3. Tra le strutture XML seguenti, copiare quella che corrisponde meglio alle proprie esigenze. Nella prima struttura XML sono disponibili segnaposti per la specifica di tutti gli elementi, descritti nella sezione successiva:  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

    ```xml
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsBasic>  
                       <LogonMethod>3</LogonMethod>  
                       <Realm></Realm>  
                       <DefaultDomain></DefaultDomain>  
                 </RSWindowsBasic>  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
    Se si utilizzano i valori predefiniti, è possibile copiare la struttura di elementi minima:  
  
    ```xml
          <AuthenticationTypes>  
                 <RSWindowsBasic/>  
          </AuthenticationTypes>  
    ```  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

    ```xml
      <Authentication>
          <AuthenticationTypes>
                      <RSWindowsBasic/>
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
      <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
      <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>
      </Authentication>
    ```

4. Incollare la struttura sulle voci esistenti per \<**Authentication**>.  
  
     Se si usano più tipi di autenticazione, aggiungere solo l'elemento **RSWindowsBasic** , ma non eliminare le voci relative a **RSWindowsNegotiate**, **RSWindowsNTLM** o **RSWindowsKerberos**.  
  
     Si noti che non è possibile usare **Custom** con altri tipi di autenticazione.  
  
5. Sostituire i valori vuoti per \<**Realm**> o \<**DefaultDomain**> con valori validi per l'ambiente in uso.  
  
6. Salvare il file.  
  
7. Se è stata configurata una distribuzione con scalabilità orizzontale, ripetere questi passaggi per gli altri server di report presenti nella distribuzione.  
  
8. Riavviare il server di report per cancellare qualsiasi sessione attualmente aperta.  
  
## <a name="rswindowsbasic-reference"></a>Riferimento per RSWindowsBasic  
 Quando si configura l'autenticazione di base, è possibile specificare gli elementi seguenti.  
  
|Elemento|Obbligatoria|Valori validi|  
|-------------|--------------|------------------|  
|LogonMethod|Sì<br /><br /> Se non si specifica un valore, verrà utilizzato 3.|**2** = accesso alla rete, destinato ai server ad alte prestazioni per l'autenticazione di password in testo normale.<br /><br /> **3** = accesso non crittografato, che mantiene le credenziali di accesso nel pacchetto di autenticazione inviato con ogni richiesta HTTP, consentendo al server di rappresentare l'utente in caso di connessione ad altri server della rete. Valore predefinito.<br /><br /> Nota: I valori 0 (per accesso interattivo) e 1 (per accesso batch) **NON** sono supportati in [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|  
|Realm|Facoltativo|Specifica una partizione delle risorse che include le caratteristiche di autorizzazione e autenticazione utilizzate per controllare l'accesso alle risorse protette nell'organizzazione.|  
|DefaultDomain|Facoltativo|Specifica il dominio utilizzato dal server per autenticare l'utente. Questo valore è facoltativo, ma se non viene specificato il server di report utilizzerà il nome del computer come dominio. Se il computer è un membro di dominio, tale dominio rappresenta il dominio predefinito. Se il server di report è stato installato in un controller di dominio, il dominio utilizzato è quello controllato dal computer.|  
  
## <a name="see-also"></a>Vedere anche  
 [Domini applicazione per applicazioni del server di report](../../reporting-services/report-server/application-domains-for-report-server-applications.md)   
 [Sicurezza e protezione di Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
