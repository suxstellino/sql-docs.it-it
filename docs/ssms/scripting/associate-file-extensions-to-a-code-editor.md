---
title: Associazione di estensioni di file a un editor di codice
description: Informazioni su come associare un'estensione di file a un editor di codice specifico, in modo che quando si fa doppio clic su un file con tale estensione questo venga aperto dall'editor associato.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- file extensions [SQL Server]
- associating file extensions [SQL Server]
- Query Editor [SQL Server Management Studio], associating file extensions
ms.assetid: 193630f4-93de-4950-8f36-68702531f925
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4196519b2e743449864cd171d6da3c7863475157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354805"
---
# <a name="associate-file-extensions-to-a-code-editor"></a>Associazione di estensioni di file a un editor di codice

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'associazione di estensioni di file a un editor di codice specifico consente di aprire un file con l'editor di codice [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] appropriato quando si fa doppio clic su un file in Esplora risorse. L'associazione delle estensioni utilizzate solitamente da [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ad esempio sql e mdx, viene eseguita durante l'installazione. È inoltre necessario associare le nuove estensioni di file a [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] nel file system. Questa caratteristica consente di aprire file creati con altri editor oppure di aprire file rinominati, ad esempio copie di backup di file con estensione sql rinominati con estensione bak.  
  
 Il processo è suddiviso in due passaggi. È innanzitutto necessario associare l'estensione a [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi associare l'estensione a un editor di codice specifico.  
  
### <a name="to-associate-a-new-file-extension-with-sql-server-management-studio"></a>Per associare una nuova estensione di file a SQL Server Management Studio  
  
1.  Fare clic sul pulsante **Start** , scegliere **Tutti i programmi**, **Accessori** e quindi **Esplora risorse**.  
  
2.  In Esplora risorse scegliere **Opzioni cartella** dal menu **Strumenti**.  
  
3.  Nella finestra di dialogo **Opzioni cartella** fare clic su **Nuovo** nella scheda **Tipi di file**.  
  
4.  Nella finestra di dialogo **Creazione nuova estensione** immettere nella casella **Estensione file** la nuova estensione di file da associare e quindi fare clic su **OK**. Non inserire un punto davanti all'estensione.  
  
5.  Selezionare la nuova estensione nella casella **Tipi di file registrati** , quindi fare clic su **Cambia**.  
  
6.  Nella finestra di dialogo **Apri con** fare clic su **SSMS - SQL Server Management Studio** e quindi su **OK**.  
  
7.  Fare clic su **Chiudi** per chiudere la finestra di dialogo **Opzioni cartella** , quindi chiudere Esplora risorse.  
  
### <a name="to-associate-a-new-file-extension-with-a-code-editor-in-sql-server-management-studio"></a>Per associare una nuova estensione di file a un editor di codice in SQL Server Management Studio  
  
1.  In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]scegliere **Opzioni** dal menu **Strumenti**.  
  
2.  Nella finestra di dialogo **Opzioni** fare clic su **Editor di testo** e quindi su **Estensione file**.  
  
3.  Immettere la nuova estensione di file nella casella **Estensione** .  
  
4.  Nella casella **Editor** selezionare l'editor di codice da usare per aprire questo tipo di file, fare clic su **Aggiungi** e quindi su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità Ssms](../ssms-utility.md)  
  
