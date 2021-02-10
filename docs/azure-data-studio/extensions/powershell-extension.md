---
title: Estensione di PowerShell
description: Informazioni su come installare e usare l'estensione PowerShell di Azure Data Studio, che offre supporto avanzato per l'editor di PowerShell per la scrittura e il debug di script.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 04/19/2019
ms.openlocfilehash: 4de6c653503458ba839147b9a68b457330345755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048501"
---
# <a name="powershell-editor-support-for-azure-data-studio"></a>Supporto dell'editor PowerShell per Azure Data Studio

Questa estensione offre supporto avanzato per l'editor PowerShell in [Azure Data Studio](https://github.com/Microsoft/azuredatastudio).
È ora possibile scrivere ed eseguire il debug di script di PowerShell usando l'eccellente interfaccia di tipo IDE fornita da Azure Data Studio.

![Estensione di PowerShell](media/powershell-extension/powershell-extension.png)

## <a name="features"></a>Funzionalità

- Evidenziazione della sintassi
- Frammenti di codice
- IntelliSense per cmdlet e altro
- Analisi basata su regole fornita dall'[analizzatore script di PowerShell](http://github.com/PowerShell/PSScriptAnalyzer)
- Passaggio alla definizione di cmdlet e variabili
- Ricerca dei riferimenti di cmdlet e variabili
- Individuazione dei simboli di documenti e aree di lavoro
- Esecuzione della parte selezionata di codice PowerShell con <kbd>F8</kbd>
- Avvio della Guida per il simbolo sotto il cursore con <kbd>CTRL</kbd>+<kbd>F1</kbd>
- Supporto di base della console interattiva

## <a name="installing-the-extension"></a>Installazione dell'estensione

Per installare la versione ufficiale dell'estensione PowerShell, seguire i passaggi descritti nella [documentazione di Azure Data Studio](./add-extensions.md).
Nel riquadro Estensioni cercare l'estensione "PowerShell" e installarla.  Si riceverà una notifica automatica degli aggiornamenti futuri dell'estensione.

È anche possibile installare un pacchetto VSIX dalla [pagina delle versioni](https://github.com/PowerShell/vscode-powershell/releases) e installarlo tramite la riga di comando:

```powershell
azuredatastudio --install-extension PowerShell-<version>.vsix
```

## <a name="platform-support"></a>Piattaforme supportate

- **Da Windows 7 a Windows 10** con Windows PowerShell V3 e versioni successive e PowerShell Core
- **Linux** con PowerShell Core (tutte le distribuzioni con supporto di PowerShell)
- **macOS** con PowerShell Core

Leggere le [domande frequenti](https://github.com/PowerShell/vscode-powershell/wiki/FAQ) per trovare le risposte alle domande più comuni.

## <a name="installing-powershell-core"></a>Installazione di PowerShell Core

Se si esegue Azure Data Studio in macOS o Linux, può essere necessario installare anche PowerShell Core.

PowerShell Core è un progetto open source in [GitHub](https://github.com/powershell/powershell).
Per altre informazioni sull'installazione di PowerShell Core in piattaforme macOS o Linux, vedere gli articoli seguenti:

- [Installazione di PowerShell Core in Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Installazione di PowerShell Core in macOS](/powershell/scripting/install/installing-powershell-core-on-macos)

## <a name="example-scripts"></a>Script di esempio

Nella cartella `examples` dell'estensione sono disponibili alcuni script di esempio che è possibile usare per esplorare le funzionalità di modifica e debug di PowerShell.  Per altre informazioni sull'uso, vedere il file [README.md](https://github.com/PowerShell/vscode-powershell/blob/master/examples/README.md) incluso.

Questa cartella si trova nel percorso seguente:

```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.PowerShell-<version>/examples
```

oppure se si usa la versione di anteprima dell'estensione

 ```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.powershell-preview-<version>/examples
```

Per aprire/visualizzare gli esempi dell'estensione in Azure Data Studio, eseguire il codice seguente dal prompt dei comandi di PowerShell:

```powershell
azuredatastudio (Get-ChildItem $Home\.azuredatastudio\extensions\ms-vscode.PowerShell-*\examples)[-1]
```

### <a name="creating-and-opening-files"></a>Creazione e apertura di file

Per creare e aprire un nuovo file nell'editor, usare New-EditorFile dal terminale PowerShell integrato.

```powershell
PS C:\temp> New-EditorFile ExportData.ps1
```

Questo comando funziona per qualsiasi tipo di file, non solo per i file di PowerShell.

```powershell
PS C:\temp> New-EditorFile ImportData.py
```

Per aprire uno o più file in Azure Data Studio, usare il comando `Open-EditorFile`.

```powershell
Open-EditorFile ExportData.ps1, ImportData.py
```

### <a name="no-focus-on-console-when-executing"></a>Stato non attivo sulla console durante l'esecuzione

Gli utenti abituati per lavorare con SSMS ricorderanno la possibilità di eseguire una query e poi eseguirla di nuovo senza dover tornare al riquadro della query.  Per questi utenti, il comportamento predefinito dell'editor di codice potrebbe sembrare strano.  Per lasciare lo stato attivo nell'editor quando si esegue con <kbd>F8</kbd>, modificare l'impostazione seguente:

```json
"powershell.integratedConsole.focusConsoleOnExecute": false
```

Il valore predefinito è `true` a scopo di accessibilità.

Tenere presente che questa impostazione impedirà che lo stato attivo passi alla console, anche quando si usa un comando che richiede in modo esplicito un input, ad esempio `Get-Credential`.

## <a name="sql-powershell-examples"></a>Esempi di SQL PowerShell

Per usare gli esempi illustrati di seguito è necessario installare il modulo SqlServer da [PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer).

```powershell
Install-Module -Name SqlServer
```

> [!NOTE]
> Con la versione `21.1.18102` e successive, il modulo `SqlServer` supporta [PowerShell Core](https://github.com/PowerShell/PowerShell) 6.2 e versioni successive, oltre a Windows PowerShell.

In questo esempio viene usato il cmdlet `Get-SqlInstance` per ottenere gli oggetti Server SMO per ServerA e ServerB.  L'output predefinito per questo comando includerà il nome dell'istanza, la versione, il Service Pack e il livello di aggiornamento CU delle istanze.

```powershell
Get-SqlInstance -ServerInstance ServerA, ServerB
```

Ecco un esempio del possibile output:

```console
Instance Name             Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------             -------    ------------ -----------  ------------ ----------------
ServerA                   13.0.5233  SP2          CU4          Windows      Windows Server 2016 Datacenter
ServerB                   14.0.3045  RTM          CU12         Linux        Ubuntu
```

Il modulo `SqlServer` contiene un provider denominato `SQLRegistration`, che consente di accedere a livello di programmazione ai tipi seguenti di connessioni SQL Server salvate:

+ Server del motore di database (Server registrati)
+ Server di gestione centrale (CMS)
+ Analysis Services
+ Integration Services
+ Reporting Services

 Nell'esempio seguente si eseguirà un comando `dir` (alias di `Get-ChildItem`) per ottenere l'elenco di tutte le istanze di SQL Server elencate nel file dei server registrati.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse
```

Ecco un esempio del possibile output:

```powershell
Mode Name
---- ----
-    ServerA
-    ServerB
-    localhost\SQL2017
-    localhost\SQL2016Happy
-    localhost\SQL2017
```

Per molte operazioni che coinvolgono un database o gli oggetti all'interno di un database, è possibile usare il cmdlet `Get-SqlDatabase`.  Se si specificano i valori per entrambi i parametri `-ServerInstance` e `-Database`, verrà recuperato solo l'oggetto di database.  Se tuttavia si specifica solo il parametro `-ServerInstance`, verrà restituito un elenco completo di tutti i database di tale istanza.

Ecco un esempio del possibile output:

```console
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

Nell'esempio seguente viene usato il cmdlet `Get-SqlDatabase` per recuperare un elenco di tutti i database nell'istanza ServerB, quindi viene visualizzata una griglia/tabella (tramite il cmdlet `Out-GridView`) per selezionare i database di cui eseguire il backup.  Quando l'utente fa clic sul pulsante "OK", verrà eseguito il backup solo dei database evidenziati.

```powershell
Get-SqlDatabase -ServerInstance ServerB |
Out-GridView -PassThru |
Backup-SqlDatabase -CompressionOption On
```

Nell'esempio seguente viene di nuovo ottenuto l'elenco di tutte le istanze di SQL Server elencate nel file dei server registrati, quindi viene chiamato `Get-SqlAgentJobHistory`, che segnala ogni processo di SQL Agent non riuscito dalla mezzanotte in poi, per ogni istanza di SQL Server elencata.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE {$_.Mode -ne 'd' } |
FOREACH {
    Get-SqlAgentJobHistory -ServerInstance  $_.Name -Since Midnight -OutcomesType Failed
}
```

In questo esempio verrà eseguito un comando `dir` (alias di `Get-ChildItem`) per ottenere l'elenco di tutte le istanze di SQL Server elencate nel file dei server registrati e quindi verrà usato il cmdlet `Get-SqlDatabase` per ottenere un elenco dei database per ognuna di queste istanze.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE { $_.Mode -ne 'd' } |
FOREACH {
    Get-SqlDatabase -ServerInstance $_.Name
}
```

Ecco un esempio del possibile output:

```console
Name                 Status           Size  Space     Recovery Compat. Owner
                                            Available Model    Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

## <a name="reporting-problems"></a>Segnalazione di problemi

Per informazioni sulla diagnosi e la segnalazione dei problemi con l'estensione PowerShell, vedere la [ documentazione sulla risoluzione dei problemi](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md).

### <a name="security-note"></a>Nota sulla sicurezza

Per eventuali problemi di sicurezza, vedere [qui](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md#note-on-security).

## <a name="contributing-to-the-code"></a>Contribuire al codice

Per altre informazioni su come contribuire a questa estensione, consultare la [documentazione di sviluppo](https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md).

## <a name="maintainers"></a>Gestori

- [Keith Hill](https://github.com/rkeithhill) - [@r_keith_hill](http://twitter.com/r_keith_hill)
- Tyler Leonhardt - [@TylerLeonhardt](http://twitter.com/tylerleonhardt)
- [Rob Holt](https://github.com/rjmholt)

## <a name="license"></a>Licenza

Questa estensione è [concessa in licenza in base ai termini della licenza MIT](https://github.com/PowerShell/vscode-powershell/blob/master/LICENSE.txt). Per informazioni dettagliate sui file binari di terze parti inclusi nelle versioni di questo progetto, vedere il file delle [comunicazioni di terze parti](https://github.com/PowerShell/vscode-powershell/blob/master/Third%20Party%20Notices.txt).

## <a name="code-of-conduct"></a>Codice di comportamento

Questo progetto ha adottato il [Codice di comportamento di Microsoft per l'open source][conduct-code].
Per altre informazioni, vedere le [Domande frequenti sul codice di comportamento][conduct-FAQ] o scrivere a [opencode@microsoft.com][conduct-email] per domande aggiuntive o commenti.

[conduct-code]: https://opensource.microsoft.com/codeofconduct/
[conduct-FAQ]: https://opensource.microsoft.com/codeofconduct/faq/
[conduct-email]: mailto:opencode@microsoft.com