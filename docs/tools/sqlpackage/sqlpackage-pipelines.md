---
title: SqlPackage nelle pipeline di sviluppo
description: Informazioni su come risolvere i problemi relativi alle pipeline di sviluppo del database con SqlPackage.exe controllando il numero della build installata.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 5bc3b1591dd05722f1b0d89a0113df4723f26d67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060996"
---
# <a name="sqlpackage-in-development-pipelines"></a>SqlPackage nelle pipeline di sviluppo

**SqlPackage.exe** è un'utilità della riga di comando che automatizza diverse attività di sviluppo di database e può essere incorporata nelle pipeline CI/CD.

## <a name="managed-virtual-environments"></a>Ambienti virtuali gestiti

Gli ambienti virtuali usati per gli strumenti di esecuzione ospitati di GitHub Actions e le immagini delle macchine virtuali di Azure Pipelines sono gestiti nel repository GitHub degli [ambienti virtuali](https://github.com/actions/virtual-environments).  SqlPackage è incluso nell'ambiente `windows-latest` e gli aggiornamenti delle immagini vengono eseguiti entro poche settimane da ogni rilascio di SqlPackage.

## <a name="checking-the-sqlpackage-version"></a>Controllo della versione di SqlPackage

Durante la risoluzione dei problemi, è importante conoscere la versione di SqlPackage in uso.  L'acquisizione di queste informazioni può essere eseguita aggiungendo un passaggio alla pipeline per eseguire SqlPackage con il parametro `/version`.  Di seguito sono riportati alcuni esempi basati sugli ambienti gestiti Microsoft e GitHub. Gli ambienti self-hosted possono avere percorsi di installazione diversi per la directory di lavoro.

### <a name="azure-pipelines"></a>Azure Pipelines

Usando la parola chiave [script](/azure/devops/pipelines/yaml-schema#script) in una pipeline di Azure, è possibile aggiungere un passaggio a una pipeline di Azure che restituisce il numero di versione di SqlPackage.

```yaml
- script: sqlpackage.exe /version
  workingDirectory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  displayName: 'get sqlpackage version'
```

### <a name="github-actions"></a>Azioni di GitHub

Usando la parola chiave [run](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) in un flusso di lavoro di GitHub Actions, è possibile aggiungere un passaggio a un'azione di GitHub che restituisce il numero di versione di SqlPackage.

```yaml
- name: get sqlpackage version
  working-directory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  run: ./sqlpackage.exe /version
```

:::image type="content" source="media/sqlpackage-pipelines-github-action.png" alt-text="Output dell'azione di GitHub che visualizza il numero di build 15.0.4897.1":::

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)