---
title: Configurazione di editor (SQL Server Management Studio)
description: Informazioni su come personalizzare il funzionamento degli editor di SQL Server Management Studio impostando le opzioni nella finestra di dialogo Opzioni.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 33ddc2599ae030e61059c23a462fb68f9f814966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354744"
---
# <a name="configure-editors-sql-server-management-studio"></a>Configurazione di editor (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile personalizzare il funzionamento degli editor di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] configurando le apposite opzioni.  
  
## <a name="setting-editor-options"></a>Impostazione delle opzioni degli editor  
 È possibile impostare la maggior parte delle opzioni degli editor scegliendo **Opzioni** dal menu **Strumenti** per visualizzare la finestra di dialogo **Opzioni**. Nella finestra di dialogo **Opzioni** , aprire il nodo **Editor di testo** nel riquadro sinistro per impostare le opzioni di modifica di codice e testo. I nodi in Editor di testo si applicano a editor specifici:  
  
1.  **Tutte le lingue**: le opzioni impostate in questo nodo si applicano a tutti gli editor di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. È possibile ignorare queste impostazioni utilizzando gli altri nodi che consentono di configurare opzioni diverse per un editor specifico.  
  
2.  **Testo normale**: le opzioni impostate in questo nodo si applicano agli editor di testo, MDX e DMX.  
  
3.  **Transact-SQL**: le opzioni impostate in questo nodo si applicano all'editor di query del motore di database.  
  
4.  **XML**: le opzioni impostate in questo nodo si applicano all'editor XML for Analysis.  
  
 Aprire i nodi **Esecuzione query** o **Risultati query** per personalizzare l'esecuzione di query e la modalità di visualizzazione dei risultati.  
  
## <a name="editor-configuration-tasks"></a>Attività di configurazione degli editor  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Descrive come specificare che un editor deve essere aperto facendo doppio clic su un file con un'estensione specificata in Esplora risorse.|[Associare estensioni di file a un editor di codice](./associate-file-extensions-to-a-code-editor.md)|  
|Descrive come personalizzare i tipi di carattere per rendere più leggibili codice e testo.|[Modificare lo stile, le dimensioni e il colore del carattere](./change-font-color-size-and-style.md)|  
|Descrive come visualizzare le proprietà.|[Usare la finestra Proprietà in Management Studio](./use-the-properties-window-in-management-studio.md)|  
|Percorso delle pagine della Guida per le finestre di dialogo delle opzioni degli editor.|[Guida sensibile al contesto relativa alle pagine di Opzioni query](../f1-help/f1-help-for-server-connections-sql-server-management-studio.md)|