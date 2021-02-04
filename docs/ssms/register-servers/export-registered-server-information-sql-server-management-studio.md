---
description: Esportare informazioni relative a server registrati (SQL Server Management Studio)
title: Esportare informazioni relative ai server registrati
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.exportregisteredservers.f1
helpviewer_keywords:
- Registered Servers [SQL Server], exporting
- exporting registered server information
- transferring registered server information
ms.assetid: b65e168f-b6bf-489c-b8ad-3b8644acf0b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a71bebde2525a18e062832aba064a4b9aecf7f3a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250473"
---
# <a name="export-registered-server-information-sql-server-management-studio"></a>Esportare informazioni relative a server registrati (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene illustrato come salvare ed esportare le informazioni relative a server registrati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]e distribuirle ad altri dipendenti o server. È possibile utilizzare questa funzionalità di esportazione per ottenere un'interfaccia utente coerente su più computer.  
  
 L'esportazione e la successiva importazione dei file dei server registrati consente di configurare con facilità diversi computer con gli stessi server presenti in Server registrati. Ciò risulta utile quando si gestisce un numero elevato di server da computer distribuiti in diversi luoghi oppure quando si desidera configurare le impostazioni di connessione di base per un utente non esperto.  
  
> [!NOTE]  
>  Le registrazioni server che usano l'autenticazione di SQL Server archiviano le password separatamente per ogni utente. Dopo aver esportato e importato le registrazioni dei server, è necessario che gli utenti immettano la password per ogni server al momento della prima connessione, in modo tale che le password vengano archiviate nei relativi elenchi dei server registrati. Questa operazione non è necessaria per i server registrati mediante l'autenticazione di Windows.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-export-registered-server-information"></a>Per esportare informazioni relative a server registrati  
  
1.  Nella finestra Server registrati fare clic con il pulsante destro del mouse su un gruppo di server e quindi scegliere **Esporta**.  
  
    > [!NOTE]  
    >  È possibile esportare un singolo server, l'intero albero dei server registrati, oppure un subset dell'albero stesso.  
  
2.  Nella finestra di dialogo **Esporta server registrati** selezionare le selezioni seguenti.  
  
     **Gruppo server**  
     Specificare il gruppo di server che verrà esportato. Esportare nel file di esportazione tutti i server registrati, i server registrati in un particolare gruppo di server oppure un singolo server registrato. La funzionalità di esportazione è ricorsiva. Se, ad esempio, il gruppo di server A contiene il gruppo di server B e il gruppo di server B contiene i gruppi di server C e D, l'esportazione del gruppo A determinerà l'esportazione di tutte le voci contenute nei gruppi A, B, C e D.  
  
     Il gruppo di server visualizza solo i gruppi di server dell'albero di server registrati corrente.  
  
     **File di esportazione**  
     Digitare il nome del file da esportare nella casella di testo oppure usare il pulsante Sfoglia (**...**) per trovare il file di esportazione nel computer client. Se si seleziona un file esistente, le informazioni relative ai server registrati verranno aggiunte al file. Verrà utilizzata l'estensione regsrvr. Per rendere le informazioni relative ai server registrati disponibili ad altri utenti o a un altro computer, è possibile salvare il file sulla rete. Gli altri utenti possono accedere al file e importare le informazioni relative ai server registrati, anche parzialmente. Se si seleziona un file esistente come file di esportazione, il contenuto del file viene sovrascritto con le informazioni relative alla registrazione dei server.  
  
     **Non includere nomi utente e password nel file di esportazione**  
     Consente di escludere i nomi utente durante l'esportazione del file.  
  
    > [!IMPORTANT]  
    >  Sebbene i file di esportazione siano crittografati, se i nomi utente e le password di autenticazione di SQL Server vengono inclusi nel file, sarà opportuno controllare con attenzione l'accesso al file. Per questo motivo i nomi utente e le password vengono esclusi dal file di esportazione per impostazione predefinita.  
  
## <a name="see-also"></a>Vedere anche  
 [Importare informazioni relative a server registrati &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)   
 [Creare un nuovo server registrato &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md)  
  
