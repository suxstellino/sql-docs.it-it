---
title: Eseguire l'importazione in un progetto di database
description: Informazioni su come importare oggetti in un progetto di database da un database attivo, da un'applicazione livello dati e uno script. Informazioni sull'importazione di oggetti crittografati.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLPROJECTIMPORTSNAPSHOTSUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.SQLPROJECTIMPORTDATABASESUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.IMPORTSCRIPTWIZARD.SUMMARY
ms.assetid: d0a0a394-6cb6-416a-a25f-9babf8ba294a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0820f680382d7a126d2837c653ab7870defeb0ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017931"
---
# <a name="import-into-a-database-project"></a>Eseguire l'importazione in un progetto di database

È possibile utilizzare l'opzione Importa per popolare un progetto con nuovi oggetti da un database attivo o da un file con estensione dacpac o per aggiornare oggetti esistenti con una definizione da uno script. Nei tre percorsi sono presenti differenze di comportamento che vengono descritte di seguito.  
  
**Menu Importa**  
  
![Menu Importa di SSDT](../ssdt/media/ssdt-import.gif "Menu Importa di SSDT")  
  
**Sezioni dell'argomento**  
  
[Origine di importazione: database o applicazione livello dati (*.dacpac)](#bkmk_import_source_db)  
  
[Origine di importazione: script (*.sql)](#bkmk_import_source_script)  
  
[Importare oggetti crittografati](#bkmk_import_encrypted)  
  
## <a name="import-source-database-or-data-tier-application-dacpac"></a><a name="bkmk_import_source_db"></a>Origine di importazione: database o applicazione livello dati (*.dacpac)  
È possibile importare uno schema da un database o da un file con estensione dacpac solo se non sono presenti oggetti dello schema già definiti nel progetto. Questo non include file refactorlog o script di pre-distribuzione o post-distribuzione.  
  
Durante l'importazione le definizioni di oggetti verranno inserite in script in file di progetto tramite impostazioni predefinite organizzative di SSDT per i nuovi oggetti, ovvero nuovi file per oggetti di livello superiore, figli gerarchici definiti nello stesso file come il padre o vincoli di tabella o colonna definiti inline quando possibile. Per una visibilità e un controllo più mirati per ogni oggetto, utilizzare Confronto schema anziché Importa.  
  
Se contenuti nell'origine di importazione, script di pre-distribuzione o di post-distribuzione, file refactorlog o definizioni di variabile SQLCMD verranno importati nel progetto. Se il progetto contiene già uno di questi artefatti, i file importati verranno aggiunti alla cartella **Ignorato durante l'importazione** del progetto.  
  
**Cartella Ignorato durante l'importazione**  
  
![Cartella di elementi ignorati durante l'importazione di SSDT](../ssdt/media/ssdt-ignoredonimport.gif "Cartella di elementi ignorati durante l'importazione di SSDT")  
  
## <a name="import-source-script-sql"></a><a name="bkmk_import_source_script"></a>Origine di importazione: script (*.sql)  
Tutti gli oggetti dell'origine di importazione che *non* esistono già nel progetto verranno aggiunti, mentre tutti quelli che *esistono già* nel progetto verranno sovrascritti dalla definizione di oggetto nel progetto.  
  
> [!NOTE]  
> In questo percorso sono presenti due bug conosciuti che verranno risolti in una versione futura:  
>   
> -   Se i vincoli di tabella o di colonna vengono definiti all'esterno dell'istruzione CREATE TABLE nella definizione di tabella del progetto, in seguito all'importazione la definizione di tabella verrà sovrascritta in modo che il vincolo si trovi inline. Poiché il vincolo out-of-line non verrà eliminato, nel progetto saranno tuttavia presenti vincoli duplicati.  
> -   Durante l'importazione, tutte le chiavi master o le chiavi di crittografia del database derivanti dallo script di origine già presenti nel progetto verranno duplicate. Per compilare il progetto, rimuovere i duplicati.  
  
Il processo di importazione da script non includerà gli script di pre-distribuzione o di post-distribuzione, i file refactorlog e le variabili SQLCMD. Questi elementi e altri costrutti non supportati rilevati durante l'importazione verranno inseriti in un file **ScriptsIgnoredOnImport.sql** in una cartella **Script** del progetto.  
  
 
## <a name="import-encrypted-objects"></a><a name="bkmk_import_encrypted"></a>Importare oggetti crittografati  
Quando si importano oggetti crittografati in un progetto di database, non è sempre possibile recuperare l'intero corpo della definizione di oggetto dal server. Per questo motivo, quando viene applicata a questa classe di oggetti, il comportamento dell'importazione può essere diverso.  
  
Quando non è possibile recuperare la definizione dell'intero corpo, l'intestazione o il piè di pagina dell'oggetto verranno inseriti nello script con un corpo fittizio. Questa situazione può verificarsi quando si utilizza l'opzione Importa o Confronto schema in cui l'origine di importazione è un database attivo o un file con estensione dacpac estratto da un database.  
  
**Script con corpo fittizio**  
  
![Script con un corpo fittizio](../ssdt/media/ssdt-procwithencryption.gif "Script con un corpo fittizio")  
  
Nel caso in cui la definizione di oggetto completa sia disponibile e possa essere recuperata, le operazioni di importazione e di confronto schema la inseriranno completamente nel progetto. Questa situazione di verifica durante l'aggiornamento del progetto da uno script, da un file con estensione dacpac creato da un progetto di database o da un altro progetto di database.  
  
