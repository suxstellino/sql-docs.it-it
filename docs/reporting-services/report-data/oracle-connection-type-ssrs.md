---
title: Tipo di connessione Oracle (SSRS e server di report di Power BI)
description: Usare le informazioni di questo articolo sul tipo di connessione Oracle per apprendere come creare un'origine dati.
ms.date: 11/03/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 9db86dd2-beda-42d8-8af7-2629d58a8e3d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bc812447f38575375de10cea13d0376e879375c5
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978423"
---
# <a name="oracle-connection-type-ssrs--power-bi-report-server"></a>Tipo di connessione Oracle (SSRS e server di report di Power BI)

Per usare dati di un database Oracle nel report è necessario avere un set di dati basato su un'origine dati del report di tipo Oracle. Questo tipo di origine dati predefinita usa il provider di dati Oracle direttamente e richiede un componente software client Oracle. Questo articolo spiega come scaricare e installare i driver per Reporting Services, Server di report di Power BI, Generatore report e Power BI Desktop.


Usare le informazioni presenti in questo articolo per creare un'origine dati. Per istruzioni dettagliate, vedere [Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md). 

> [!IMPORTANT]
> I comandi seguenti che usano lo strumento OraProvCfg.exe di Oracle per registrare i driver ODP.NET gestiti e non gestiti di Oracle sono forniti come esempi per l'uso con i prodotti Microsoft precedenti. Per la configurazione dei driver ODP.NET specifici per l'ambiente in uso, potrebbe essere necessario contattare il supporto Oracle o consultare la documentazione di Oracle per [configurare il provider di dati Oracle per .NET](https://docs.oracle.com/en/database/oracle/oracle-database/19/odpnt/InstallConfig.html#GUID-1F689B90-2CC4-4907-B8FE-A5F4EE36F673).

## <a name="64-bit-drivers-for-the-report-servers"></a>Driver a 64 bit per i server di report

Dal sito di download di Oracle installare il programma di installazione [Oracle 64-bit ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html). Quando si usano i driver Oracle ODAC 12.2 e versioni successive, è necessario eseguire solo i passaggi seguenti. In caso contrario, vengono installati per impostazione predefinita in una configurazione non a livello di computer per una nuova installazione della Oracle home. Questi passaggi presuppongono che siano stati installati i file di ODAC 18.x nella cartella c:\oracle64.


### <a name="paginated-rdl-reports-use-managed-odpnet"></a>I report impaginati (RDL) usano ODP.NET gestito

Server di report di Power BI e SQL Server Reporting Services 2016 e versioni successive usano tutti **ODP.NET gestito** per i report impaginati (RDL). Per registrare ODP.NET gestito, seguire questa procedura:

1. Registrare il client ODP.NET gestito nella GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

2. Aggiungere le voci del client ODP.NET gestito a machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

### <a name="power-bi-reports-use-unmanaged-odpnet"></a>I report di Power BI usano ODP.NET non gestito

Server di report di Power BI usa **ODP.NET non gestito** per i report di Power BI. Per registrare ODP.NET non gestito, seguire questa procedura:

1. Registrare il client ODP.NET non gestito nella GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```
2. Aggiungere le voci del client ODP.NET non gestito a machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

## <a name="32-bit-drivers-for-report-builder"></a>Driver a 32 bit per Generatore report.

Generatore report usa **ODP.NET gestito** per la creazione di report impaginati (RDL). Quando si usano i driver Oracle ODAC 12.2 e versioni successive, è necessario eseguire solo i passaggi seguenti. In caso contrario, vengono installati per impostazione predefinita in una configurazione non a livello di computer per una nuova installazione della Oracle home. Questi passaggi presuppongono che siano stati installati i file ODAC 18.x nella cartella c:\oracle32 in cui è installato Generatore report. Per registrare ODP.NET gestito, seguire questa procedura:

1. Dal sito di download di Oracle installare il programma di installazione [Oracle 32-bit ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html).

2. Registrare il client ODP.NET gestito nella GAC:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

3. Aggiungere le voci del client ODP.NET gestito a machine.config:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

## <a name="64-bit-and-32-bit-drivers-for-power-bi-desktop"></a>Driver a 64 bit e 32 bit per Power BI Desktop

Power BI Desktop usa **ODP.NET non gestito** per la creazione di report di Power BI. Quando si usano i driver Oracle ODAC 12.2 e versioni successive, è necessario eseguire solo i passaggi seguenti. In caso contrario, vengono installati per impostazione predefinita in una configurazione non a livello di computer per una nuova installazione della Oracle home. Questi passaggi presuppongono che siano stati installati i file ODAC 18.x nella cartella c:\oracle64 per Power BI Desktop a 64 bit o nella cartella c:\oracle32 per Power BI Desktop a 32 bit. Per registrare ODP.NET non gestito, seguire questa procedura:

### <a name="64-bit-power-bi-desktop"></a>Power BI Desktop a 64 bit

1. Dal sito di download di Oracle installare il programma di installazione [Oracle 64-bit ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html).
2. Registrare il client ODP.NET non gestito nella GAC:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. Aggiungere le voci del client ODP.NET non gestito a machine.config:

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

### <a name="32-bit-power-bi-desktop"></a>Power BI Desktop a 32 bit

1. Dal sito di download di Oracle installare il programma di installazione [Oracle 32-bit ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html).

2. Registrare il client ODP.NET non gestito nella GAC:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. Aggiungere le voci del client ODP.NET non gestito a machine.config:

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

##  <a name="connection-string"></a><a name="Connection"></a> Stringa di connessione  

Contattare l'amministratore del database per ottenere le informazioni di connessione e le credenziali da utilizzare per connettersi all'origine dati. La stringa di connessione di esempio seguente specifica un database Oracle nel server "Oracle18" che usa Unicode. Il nome del server deve corrispondere a quello definito nel file di configurazione Tnsnames.ora come nome dell'istanza del server Oracle.  
  
```  
Data Source="Oracle"; Unicode="True"  
```  
  
Per altri esempi di stringhe di connessione, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenziali  
Le credenziali sono necessarie per eseguire query, nonché per visualizzare l'anteprima del report in locale e dal server di report.  
  
Dopo aver pubblicato il report, potrebbe essere necessario modificare le credenziali per l'origine dati affinché quando il report viene eseguito nel server di report, le autorizzazioni per il recupero dei dati risultino valide.  
  
Per altre informazioni, vedere [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Query  
Per creare un set di dati, è possibile selezionare una stored procedure in un elenco a discesa oppure creare una query SQL. Per compilare una query, è necessario utilizzare la finestra Progettazione query basata su testo. Per altre informazioni, vedere [Interfaccia utente di Progettazione query basata su testo &#40;Generatore report &#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
È possibile specificare le stored procedure che restituiscono solo un set di risultati. Le query basate su cursori non sono supportate.  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parametri  

Se la query include variabili di query, i parametri di report corrispondenti verranno generati automaticamente. I parametri denominati sono supportati da questa estensione. Per Oracle versione 9 o successive, i parametri multivalore sono supportati.  
  
 I parametri di report vengono creati con valori di proprietà predefiniti che all'occorrenza possono essere modificati. Ad esempio, i dati di ogni parametro di report sono di tipo **Text**. Dopo aver creato i parametri di report, potrebbe essere necessario modificare i valori predefiniti. Per ulteriori informazioni, vedere la pagina relativa al [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
##  <a name="remarks"></a><a name="Remarks"></a> Osservazioni  

Prima di poter connettere un'origine dati Oracle, l'amministratore di sistema deve installare la versione del provider di dati .NET per Oracle che supporta il recupero di dati dal database Oracle. Il provider di dati deve essere installato nello stesso computer di Generatore report e anche nel server di report.  
  
Per altre informazioni, vedere gli articoli seguenti:  
  
- [Come utilizzare Reporting Services per configurare e accedere a un'origine dati Oracle](/archive/blogs/dataaccesstechnologies/configure-oracle-data-source-for-sql-server-reporting-services-ssdt-and-report-server)  
- [Come aggiungere autorizzazioni per l'entità di sicurezza SERVIZIO DI RETE](https://mskb.pkisolutions.com/kb/870668)  
  
### <a name="alternate-data-extensions"></a>Estensioni per i dati alternative 

È inoltre possibile recuperare dati da un database Oracle tramite un tipo di origine dati OLE DB. Per altre informazioni, vedere [Tipo di connessione OLE DB &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  

::: moniker range=">=sql-server-2017"

### <a name="report-models"></a>Modelli di report

 È possibile creare anche modelli basati su un database Oracle.  
::: moniker-end

### <a name="platform-and-version-information"></a>Informazioni sulla piattaforma e sulla versione  

Per altre informazioni sulle piattaforme e le versioni supportate, vedere [Origini dati supportate da Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  

## <a name="see-also"></a>Vedere anche

[Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   

[Filtro, raggruppamento e ordinamento di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   

[Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
