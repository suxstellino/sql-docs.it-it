---
title: Terminale integrato
description: Informazioni su come aprire un terminale integrato in Azure Data Studio. Un terminale integrato può essere più pratico di uno separato.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 054bfd71962a5775374e8faa567dc24242d846c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048341"
---
# <a name="integrated-terminal"></a>Terminale integrato

In Azure Data Studio è possibile aprire un terminale integrato, partendo inizialmente dalla radice dell'area di lavoro. Questo può essere utile perché non è necessario cambiare finestra o modificare lo stato di un terminale esistente per eseguire una rapida attività dalla riga di comando.

Per aprire il terminale:

* Usare la scelta rapida da tastiera **CTRL+`** con il carattere apice inverso.
* Usare il comando di menu **Visualizza** | **Terminale integrato**.
* Dal **riquadro comandi** (**CTRL+MAIUSC+P**) usare il comando **Visualizza:Attiva/Disattiva terminale integrato**.

![Terminale](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> È comunque possibile aprire una shell esterna con il comando **Apri nel prompt dei comandi** di Explorer (**Apri nel terminale** in Mac o Linux) se si preferisce lavorare all'esterno di Azure Data Studio.

## <a name="managing-multiple-terminals"></a>Gestione di più terminali

È possibile creare più terminali aperti in posizioni diverse e spostarsi facilmente tra di essi. Per aggiungere istanze del terminale, fare clic sull'icona con il segno più in alto a destra nel pannello **TERMINALE** o usare il comando **CTRL+MAIUSC+`** . Verrà creata un'altra voce nell'elenco a discesa, che può essere usata per spostarsi tra i terminali.

![Più terminali](media/integrated-terminal/terminal-multiple-instances.png)

Rimuovere istanze del terminale facendo clic sul pulsante del cestino.

> [!TIP]
> Se si usano frequentemente più terminali, è possibile aggiungere tasti di scelta rapida per i comandi `focusNext`, `focusPrevious` e `kill` descritti nella [sezione Tasti di scelta rapida](#key-bindings), per consentire gli spostamenti tra i terminali mediante la sola tastiera.

## <a name="configuration"></a>Configurazione

La shell usata è per impostazione predefinita `$SHELL` in Linux e MacOS, PowerShell in Windows 10 e cmd.exe nelle versioni precedenti di Windows. È possibile eseguire l'override di questi valori manualmente impostando `terminal.integrated.shell.*` nelle [impostazioni](settings.md). È possibile passare argomenti alla shell del terminale in Linux e MacOS usando le impostazioni `terminal.integrated.shellArgs.*`.

### <a name="windows"></a>Windows

Per configurare correttamente la shell in Windows occorre individuare l'eseguibile corretto e aggiornare l'impostazione. Di seguito è riportato un elenco di file eseguibili della shell comuni e delle rispettive posizioni predefinite:

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> Per poter essere usato come terminale integrato, il file eseguibile della shell deve essere un'applicazione console, in modo che sia possibile reindirizzare `stdin/stdout/stderr`.

> [!TIP]
> La shell del terminale integrato viene eseguita con le autorizzazioni di Azure Data Studio. Se è necessario eseguire un comando della shell con privilegi elevati (amministratore) o con autorizzazioni diverse, è possibile servirsi di utilità della piattaforma, ad esempio `runas.exe`, all'interno di un terminale.

### <a name="shell-arguments"></a>Argomenti della shell

È possibile passare argomenti alla shell quando viene avviata.

Ad esempio, per abilitare l'esecuzione di bash come shell di accesso (che esegue `.bash_profile`), passare l'argomento `-l` con virgolette doppie:

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>Impostazioni di visualizzazione del terminale

È possibile personalizzare il tipo di carattere e l'altezza della riga del terminale integrato con le impostazioni seguenti:

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a name="terminal-key-bindings"></a><a id="key-bindings"></a>Tasti di scelta rapida del terminale

Il comando **Visualizza: Attiva/Disattiva terminale integrato** è associato a **CTRL+`** per includere o escludere rapidamente il pannello terminale integrato dalla visualizzazione.

Di seguito sono riportate le scelte rapide da tastiera per spostarsi rapidamente all'interno del terminale integrato:

|Chiave|Comando|  
|---|---|  
|**CTRL+\`**|Mostra terminale integrato|  
|**CTRL+MAIUSC+\`**|Crea nuovo terminale|  
|**CTRL+freccia SU**|Scorri verso l'alto|  
|**CTRL+freccia GIÙ**|Scorri verso il basso|  
|**CTRL+PGSU**|Scorri pagina verso l'alto|  
|**CTRL+PGGIÙ**|Scorri pagina verso il basso|  
|**CTRL+HOME**|Scorri all'inizio|  
|**CTRL+FINE**|Scorri alla fine|  
|**CTRL+K**|Cancella il terminale|  

Sono disponibili altri comandi del terminale che è possibile associare alle scelte rapida da tastiera preferite.

ovvero:

* `workbench.action.terminal.focus`: Sposta stato attivo sul terminale. Questo comando è simile ad Attiva/Disattiva terminale integrato, ma se il terminale è visibile sposta lo stato attivo su di esso invece di nasconderlo.
* `workbench.action.terminal.focusNext`: Sposta stato attivo sull'istanza del terminale successiva.
* `workbench.action.terminal.focusPrevious`: Sposta stato attivo sull'istanza del terminale precedente.
* `workbench.action.terminal.kill`: Rimuovi l'istanza del terminale corrente.
* `workbench.action.terminal.runSelectedText`: Esegui il testo selezionato nell'istanza del terminale.
* `workbench.action.terminal.runActiveFile`: Esegui il file attivo nell'istanza del terminale.

### <a name="run-selected-text"></a>Eseguire il testo selezionato

Per usare il comando `runSelectedText`, selezionare testo in un editor ed eseguire il comando **Terminale: Esegui testo selezionato nel terminale attivo** tramite il **riquadro comandi** (**CTRL+MAIUSC+P**). Il terminale tenta di eseguire il testo selezionato:

![Eseguire il testo selezionato](media/integrated-terminal/terminal_run_selected.png)

Se nell'editor attivo non è selezionato alcun testo, nel terminale viene eseguita la riga in cui si trova il cursore.

### <a name="copy--paste"></a>Copiare e incollare

I tasti di scelta rapida per copiare e incollare seguono gli standard della piattaforma:

* Linux: **CTRL+MAIUSC+C** e **CTRL+MAIUSC+V**
* Mac: **CMD+C** e **CMD+V**
* Windows: **CTRL+C** e **CTRL+V**

### <a name="find"></a>Find

Il terminale integrato ha una funzionalità di ricerca di base che è possibile attivare con **CTRL+F**.

Se si vuole che **CTRL+F** passi alla shell anziché avviare il widget Trova in Linux e Windows, è necessario rimuovere il tasto di scelta rapida, come indicato di seguito:

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>Rinominare sessioni del terminale

È ora possibile rinominare le sessioni del terminale integrato usando il comando **Terminale: Rinomina** (`workbench.action.terminal.rename`). Il nuovo nome viene visualizzato nell'elenco a discesa di selezione del terminale.

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>Imporre ai tasti di scelta rapida l'attraversamento del terminale

Quando lo stato attivo è sul terminale integrato, molti tasti di scelta rapida non funzioneranno perché le pressioni di tasti vengono passate e utilizzate dal terminale stesso. Per aggirare questo problema, è possibile usare l'impostazione `terminal.integrated.commandsToSkipShell`. Contiene una matrice di nomi di comandi i cui tasti di scelta rapida ignorano l'elaborazione da parte della shell e vengono invece elaborati dal sistema dei tasti di scelta rapida di Azure Data Studio. Per impostazione predefinita sono inclusi tutti i tasti di scelta del terminale, oltre ad alcuni tasti di scelta rapida di uso frequente.

