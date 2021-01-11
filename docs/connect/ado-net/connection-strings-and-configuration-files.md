---
title: Stringhe di connessione e file di configurazione
description: Informazioni su come archiviare le stringhe di connessione per le applicazioni ADO.NET in un file di configurazione dell'applicazione, come procedura consigliata per la sicurezza e la manutenzione.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 37df2641-661e-407a-a3fb-7bf9540f01e8
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8f1e0ac5fc52576f0e47ccb8be4b8e90899c6e68
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771459"
---
# <a name="connection-strings-and-configuration-files"></a>Stringhe di connessione e file di configurazione

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Se le stringhe di connessione vengono incorporate nel codice dell'applicazione, è possibile che si verifichino vulnerabilità della sicurezza e problemi di manutenzione. Le stringhe di connessione non crittografate compilate nel codice sorgente di un'applicazione possono essere visualizzate tramite lo strumento [Ildasm.exe (Disassembler IL)](/dotnet/framework/tools/ildasm-exe-il-disassembler). Inoltre, se la stringa di connessione viene modificata, è necessario ricompilare l'applicazione. Per questi motivi, si consiglia di archiviare le stringhe di connessione in un file di configurazione dell'applicazione.

## <a name="work-with-application-configuration-files"></a>Usare i file di configurazione dell'applicazione

I file di configurazione delle applicazioni contengono impostazioni specifiche di una determinata applicazione. Ad esempio, un'applicazione ASP.NET può includere uno o più file **web.config**, mentre un'applicazione Windows può includere un file **app.config** facoltativo. I file di configurazione condividono elementi comuni, anche se il nome e il percorso variano a seconda dell'host dell'applicazione.

### <a name="the-connectionstrings-section"></a>Sezione connectionStrings

Le stringhe di connessione possono essere archiviate come coppie chiave/valore nella sezione **connectionStrings** dell'elemento **configuration** di un file di configurazione dell'applicazione. Gli elementi figlio includono **add**, **clear** e **remove**.

Nel frammento di file di configurazione seguente sono illustrati lo schema e la sintassi per l'archiviazione di una stringa di connessione. L'attributo **name** corrisponde al nome specificato per identificare in modo univoco una connessione, in modo che possa essere recuperato in fase di esecuzione. Il **providerName** è **Microsoft.Data.SqlClient** con il provider di dati Microsoft SqlClient per SQL Server.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
  <configuration>  
    <connectionStrings>  
      <clear />  
      <add name="Name"
       providerName="Microsoft.Data.SqlClient"
       connectionString="Valid Connection String;" />  
    </connectionStrings>  
  </configuration>  
```  

> [!NOTE]
> È possibile salvare parte di una stringa di connessione in un file di configurazione e usare la classe <xref:System.Data.Common.DbConnectionStringBuilder> per completarlo in fase di esecuzione. Questa funzione è utile nelle situazioni in cui non si conoscono in anticipo gli elementi della stringa di connessione o quando non si desidera salvare informazioni sensibili in un file di configurazione. Per altre informazioni, vedere [Compilatori di stringhe di connessione](connection-string-builders.md).

### <a name="use-external-configuration-files"></a>Usare i file di configurazione esterni

I file di configurazione esterni sono file distinti che contengono un frammento di un file di configurazione costituito da un'unica sezione. Al file di configurazione esterno viene quindi fatto riferimento dal file di configurazione principale. L'archiviazione della sezione **connectionStrings** in un file fisicamente distinto risulta utile nelle situazioni in cui le stringhe di connessione potrebbero essere modificate dopo la distribuzione dell'applicazione. Ad esempio, il comportamento ASP.NET standard prevede il riavvio di un dominio di applicazione quando i file di configurazione vengono modificati, con la conseguenza che le informazioni sullo stato vanno perse. Tuttavia, la modifica di un file di configurazione esterno non provoca un riavvio dell'applicazione. Oltre che in ASP.NET, i file di configurazione esterni possono essere usati anche nelle applicazioni Windows. È possibile inoltre usare la sicurezza e le autorizzazioni di accesso ai file per limitare l'accesso ai file di configurazione esterni. L'uso di file di configurazione esterni in fase di esecuzione è trasparente e non richiede codice speciale.

Per archiviare le stringhe di connessione in un file di configurazione esterno, creare un file distinto che contenga solo la sezione **connectionStrings**. Non includere altri elementi, sezioni o attributi. Questo esempio illustra la sintassi di un file di configurazione esterno.

```xml  
<connectionStrings>  
  <add name="Name"
   providerName="Microsoft.Data.SqlClient"
   connectionString="Valid Connection String;" />  
</connectionStrings>  
```  

 Nel file di configurazione principale dell'applicazione usare l'attributo **configSource** per specificare il nome completo e il percorso del file esterno. In questo esempio si fa riferimento a un file di configurazione esterno denominato `connections.config`.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
<configuration>  
    <connectionStrings configSource="connections.config"/>  
</configuration>  
```  

## <a name="retrieving-connection-strings-at-run-time"></a>Recupero delle stringhe di connessione in fase di esecuzione

In .NET Framework 2.0 sono state introdotte nuove classi nello spazio dei nomi <xref:System.Configuration> per semplificare il recupero delle stringhe di connessione dai file di configurazione in fase di esecuzione. È possibile recuperare una stringa di connessione a livello di codice in base al nome o al nome del provider.

> [!NOTE]
> Il file **machine.config** include anche una sezione **connectionStrings** contenente le stringhe di connessione usate da Visual Studio. Se le stringhe di connessione vengono recuperate in base al nome del provider dal file **app.config** in un'applicazione Windows, vengono caricate dapprima le stringhe di connessione presenti in **machine.config** e quindi le voci di **app.config**. L'aggiunta di **clear** immediatamente dopo l'elemento **connectionStrings** consente di rimuovere tutti i riferimenti ereditati dalla struttura dati in memoria, in modo che vengano considerate solo le stringhe di connessione definite nel file **app.config** locale.

### <a name="work-with-the-configuration-files"></a>Usare i file di configurazione

A partire da .NET Framework 2.0, quando si usano i file di configurazione nel computer locale, viene usato <xref:System.Configuration.ConfigurationManager> al posto dell'oggetto <xref:System.Configuration.ConfigurationSettings> deprecato. Per i file di configurazione di ASP.NET, viene usato <xref:System.Web.Configuration.WebConfigurationManager>, progettato per essere usato con i file di configurazione in un server Web, che consente l'accesso a livello di codice alle sezioni del file di configurazione come **system.web**.

> [!NOTE]
> L'accesso ai file di configurazione in fase di esecuzione richiede la concessione di autorizzazioni al chiamante. Le autorizzazioni necessarie dipendono dal tipo di applicazione, dal file di configurazione e dal percorso. Per altre informazioni, vedere [Utilizzo delle classi di configurazione](/previous-versions/aspnet/ms228063(v=vs.100)) e <xref:System.Web.Configuration.WebConfigurationManager> per le applicazioni ASP.NET e <xref:System.Configuration.ConfigurationManager> per le applicazioni Windows.

È possibile usare <xref:System.Configuration.ConnectionStringSettingsCollection> per recuperare le stringhe di connessione dai file di configurazione dell'applicazione. Contiene una raccolta di oggetti <xref:System.Configuration.ConnectionStringSettings>, ognuno dei quali rappresenta una singola voce della sezione **connectionStrings**. Le proprietà sono mappate ad attributi di stringa di connessione, consentendo di recuperare una stringa di connessione in base al nome o al nome del provider.

|Proprietà|Descrizione|  
|--------------|-----------------|  
|<xref:System.Configuration.ConnectionStringSettings.Name%2A>|Nome della stringa di connessione. È mappata all'attributo **name**.|  
|<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>|Nome completo del provider. È mappata all'attributo **providerName**.|  
|<xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A>|Stringa di connessione. È mappata all'attributo **connectionString**.|  

### <a name="example-listing-all-connection-strings"></a>Esempio: elenco di tutte le stringhe di connessione

In questo esempio viene eseguita l'iterazione di <xref:System.Configuration.ConnectionStringSettingsCollection> e vengono visualizzate le proprietà <xref:System.Configuration.ConnectionStringSettings.Name%2A?displayProperty=nameWithType>, <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A?displayProperty=nameWithType> e <xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A?displayProperty=nameWithType> nella finestra della console.

> [!NOTE]
> System.Configuration.dll non è incluso in tutti i tipi di progetto e potrebbe essere necessario impostare un riferimento a questo file per usare le classi di configurazione. Il nome e il percorso di un determinato file di configurazione dell'applicazione variano in base al tipo di applicazione e al processo di hosting.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfig#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfig.cs#1)]

### <a name="example-retrieving-a-connection-string-by-name"></a>Esempio: recupero di una stringa di connessione in base al nome

In questo esempio viene illustrato come recuperare una stringa di connessione da un file di configurazione specificandone il nome. Nel codice viene creato un oggetto <xref:System.Configuration.ConnectionStringSettings> e il parametro di input specificato viene associato al nome <xref:System.Configuration.ConfigurationManager.ConnectionStrings%2A>. Se non viene trovato alcun nome corrispondente, la funzione restituisce `null` (`Nothing` in Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByName#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByName.cs#1)]

### <a name="example-retrieving-a-connection-string-by-provider-name"></a>Esempio: recupero di una stringa di connessione in base al nome del provider

In questo esempio viene illustrato come recuperare una stringa di connessione specificando il nome del provider nel formato *Microsoft.Data.SqlClient*. Il codice consente di scorrere <xref:System.Configuration.ConnectionStringSettingsCollection> e restituisce la stringa di connessione per il primo <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A> trovato. Se il nome del provider non viene trovato, la funzione restituisce `null` (`Nothing` in Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByProvider.cs#1)]

## <a name="encrypt-configuration-file-sections-using-protected-configuration"></a>Crittografare le sezioni del file di configurazione tramite la configurazione protetta

In ASP.NET 2.0 è stata introdotta una nuova funzionalità, denominata *configurazione protetta*, che consente di crittografare le informazioni riservate in un file di configurazione. Sebbene sia stata progettata principalmente per ASP.NET, questa funzionalità può essere usata anche per crittografare sezioni dei file di configurazione nelle applicazioni Windows. Per una descrizione dettagliata delle funzionalità della configurazione protetta, vedere [Crittografia delle informazioni di configurazione utilizzando la configurazione protetta](/previous-versions/aspnet/53tyfkaw(v=vs.100)).

Nel frammento di file di configurazione seguente è illustrata la sezione **connectionStrings** dopo la crittografia. Nella sezione **configProtectionProvider** è specificato il provider di configurazione protetta usato per crittografare e decrittografare le stringhe di connessione. La sezione **EncryptedData** contiene il testo crittografato.

```xml  
<connectionStrings configProtectionProvider="DataProtectionConfigurationProvider">  
  <EncryptedData>  
    <CipherData>  
      <CipherValue>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAH2... </CipherValue>  
    </CipherData>  
  </EncryptedData>  
</connectionStrings>  
```  

Quando la stringa di connessione crittografata viene recuperata in fase di esecuzione, in .NET Framework viene usato il provider specificato per decrittografare **CipherValue** e renderlo disponibile per l'applicazione. Non è necessario scrivere codice aggiuntivo per gestire il processo di decrittografia.

### <a name="protected-configuration-providers"></a>Provider di configurazione protetta

I provider di configurazione protetta sono registrati nella sezione **configProtectedData** del file **machine.config** nel computer locale, come illustrato nel frammento seguente, in cui sono indicati i due provider di configurazione protetta disponibili in .NET Framework. I valori mostrati sono stati troncati per facilitarne la lettura.

```xml  
<configProtectedData defaultProvider="RsaProtectedConfigurationProvider">  
  <providers>  
    <add name="RsaProtectedConfigurationProvider"
      type="System.Configuration.RsaProtectedConfigurationProvider" />  
    <add name="DataProtectionConfigurationProvider"
      type="System.Configuration.DpapiProtectedConfigurationProvider" />  
  </providers>  
</configProtectedData>  
```  

È possibile configurare altri provider di configurazione protetta aggiungendoli al file **machine.config**. È inoltre possibile creare un proprio provider di configurazione protetta ereditando dalla classe base astratta <xref:System.Configuration.ProtectedConfigurationProvider>. La tabella seguente descrive i due file di configurazione inclusi in .NET Framework.

|Provider|Descrizione|  
|--------------|-----------------|  
|<xref:System.Configuration.RsaProtectedConfigurationProvider>|Usa l'algoritmo di crittografia RSA per crittografare e decrittografare i dati. L'algoritmo RSA può essere usato per la crittografia a chiave pubblica e per le firme digitali. È anche noto come crittografia a "chiave pubblica" o asimmetrica perché impiega due chiavi diverse. È possibile usare lo [strumento di registrazione di IIS per ASP.NET (Aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)) per crittografare le sezioni di un file Web.config e gestire le chiavi di crittografia. In ASP.NET il file di configurazione viene decrittografato al momento dell'elaborazione. L'identità dell'applicazione ASP.NET deve disporre di accesso in lettura alla chiave di crittografia usata per crittografare e decrittografare le sezioni crittografate.|  
|<xref:System.Configuration.DpapiProtectedConfigurationProvider>|Usa DPAPI (Data Protection API) di Windows per crittografare le sezioni di configurazione. Usa i servizi di crittografia incorporati di Windows e può essere configurato per la protezione specifica del computer o specifica dell'account utente. La protezione specifica del computer risulta utile quando più applicazioni sullo stesso server devono condividere informazioni. La protezione specifica dell'account utente può essere usata con i servizi eseguiti con un'identità utente specifica, ad esempio un ambiente di hosting condiviso. Ogni applicazione viene eseguita con un'identità separata, che limita l'accesso a risorse quali file e database.|

Entrambi i provider offrono una crittografia avanzata per i dati. Se tuttavia si prevede di usare lo stesso file di configurazione crittografato in più server, ad esempio una Web farm, solo <xref:System.Configuration.RsaProtectedConfigurationProvider> consente di esportare le chiavi di crittografia usate per crittografare i dati e importarle in un altro server. Per altre informazioni, vedere [Importazione ed esportazione di contenitori di chiavi RSA della configurazione protetta](/previous-versions/aspnet/yxw286t2(v=vs.100)).

### <a name="use-the-configuration-classes"></a>Usare le classi di configurazione

Lo spazio dei nomi <xref:System.Configuration> fornisce classi per specificare le impostazioni di configurazione a livello di codice. La classe <xref:System.Configuration.ConfigurationManager> fornisce accesso a file di configurazione del computer, dell'applicazione e dell'utente. Se si crea un'applicazione ASP.NET, è possibile usare la classe <xref:System.Web.Configuration.WebConfigurationManager>, che fornisce la stessa funzionalità consentendo anche di accedere a impostazioni che sono univoche per le applicazioni ASP.NET, ad esempio quelle disponibili in **\<system.web>** .

> [!NOTE]
> Lo spazio dei nomi <xref:System.Security.Cryptography> contiene classi che forniscono opzioni aggiuntive per la crittografia e la decrittografia dei dati. Usare queste classi se si richiedono servizi di crittografia che non sono disponibili tramite configurazione protetta. In alcuni casi si tratta di wrapper presenti nelle CryptoAPI di Microsoft non gestite, in altri semplicemente di implementazioni gestite. Per altre informazioni, vedere [Servizi di crittografia](/previous-versions/visualstudio/visual-studio-2008/93bskf9z(v=vs.90)).

### <a name="appconfig-example"></a>Esempio di app.config

In questo esempio viene illustrato come attivare e disattivare la crittografia della sezione **connectionStrings** in un file **app.config** per un'applicazione Windows. La procedura assume il nome dell'applicazione come argomento, ad esempio "MyApplication.exe". Il file **app.config** verrà quindi crittografato e copiato nella cartella che contiene il file eseguibile con il nome "MyApplication.exe.config".

> [!NOTE]
> La stringa di connessione può essere decrittografata solo nel computer in cui è stata crittografata.

Nel codice viene usato il metodo <xref:System.Configuration.ConfigurationManager.OpenExeConfiguration%2A> per aprire il file **app.config** per la modifica, mentre il metodo <xref:System.Configuration.ConfigurationManager.GetSection%2A> restituisce la sezione **connectionStrings**. Viene quindi controllata la proprietà <xref:System.Configuration.SectionInformation.IsProtected%2A>, con una chiamata a <xref:System.Configuration.SectionInformation.ProtectSection%2A> per crittografare la sezione, se non è crittografata. Il metodo <xref:System.Configuration.SectionInformation.UnprotectSection%2A> viene richiamato per decrittografare la sezione. Il metodo <xref:System.Configuration.Configuration.Save%2A> completa l'operazione e salva le modifiche.

> [!NOTE]
> È necessario impostare un riferimento a `System.Configuration.dll` nel progetto affinché il codice venga eseguito.

[!code-csharp[DataWorks ConnectionStrings.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStrings_Encrypt.cs#1)]

### <a name="webconfig-example"></a>Esempio di Web.config

In questo esempio viene usato il metodo <xref:System.Web.Configuration.WebConfigurationManager.OpenWebConfiguration%2A> di `WebConfigurationManager`. Si noti che in questo caso è possibile specificare il percorso relativo del file **Web.config** usando una tilde. Il codice richiede un riferimento alla classe `System.Web.Configuration`.  
  
[!code-csharp[DataWorks ConnectionStringsWeb.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStringsWeb_Encrypt.cs#1)]

Per altre informazioni sulla protezione di applicazioni ASP.NET, vedere [Securing ASP.NET web sites](/previous-versions/aspnet/91f66yxt(v=vs.100)) (Protezione di siti Web ASP.NET).

## <a name="see-also"></a>Vedere anche

- [Generatori di stringhe di connessione](connection-string-builders.md)
- [Protezione delle informazioni di connessione](protecting-connection-information.md)
- [Uso delle classi di configurazione](/previous-versions/visualstudio/visual-studio-2008/ms228063(v=vs.90))
- [Configurazione di app](/dotnet/framework/configure-apps/index)
- [Amministrazione sito Web ASP.NET](/previous-versions/aspnet/6hy1xzbw(v=vs.100))
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
