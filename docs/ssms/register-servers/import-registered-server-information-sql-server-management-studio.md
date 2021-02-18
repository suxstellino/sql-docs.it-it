---
description: Importazione di informazioni relative a server registrati (SQL Server Management Studio)
title: Importare informazioni relative ai server registrati
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.importregisteredservers.f1
helpviewer_keywords:
- transferring registered server information
- Registered Servers [SQL Server], importing
- importing registered server information
ms.assetid: cc497a14-1360-4887-b70c-002f042823b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 23fd49ac140d4e8da98324725e42f2e8ea6e006b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350499"
---
# <a name="import-registered-server-information-sql-server-management-studio"></a>Importazione di informazioni relative a server registrati (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene illustrato come importare le informazioni sul server registrato salvate in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. L'esportazione e la successiva importazione dei file dei server registrati consente di configurare con facilità diversi computer con gli stessi server presenti in Server registrati. Ciò risulta utile quando si gestisce un numero elevato di server da computer distribuiti in diversi luoghi oppure quando si desidera configurare le impostazioni di connessione di base per un utente poco esperto.  
  
> [!NOTE]  
>  Non è possibile importare le informazioni sui server registrati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] da versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-import-registered-server-information"></a>Per importare informazioni relative ai server registrati  
  
1.  In Server registrati fare clic sul tipo di server sulla barra degli strumenti Server registrati. Il tipo di server deve corrispondere al tipo del file di esportazione dei server registrati. Se, ad esempio, sono state esportate informazioni sui server registrati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , fare clic su **SQL Server** sulla barra degli strumenti Server registrati.  
  
2.  Fare clic con il pulsante destro del mouse su un gruppo di server e quindi scegliere **Importa**.  
  
3.  Nella finestra di dialogo **Importa Server registrati** selezionare il file dei server registrati da importare e quindi fare clic su **OK**.  
  
     **File da importare**  
     Digitare il nome del file da importare nella casella di testo oppure fare clic sul pulsante Sfoglia (**...**) per individuare il file da importare nel computer client. Se si seleziona un file esistente, le informazioni relative ai server registrati verranno aggiunte al file. Il file da importare deve essere un file dei server registrati precedentemente esportato. I file dei server registrati hanno l'estensione regsrvr.  
  
     **Selezionare il gruppo di server in cui eseguire l'importazione**  
     Consente di selezionare il nodo radice o un gruppo di server particolare in cui verranno importate le voci dei server registrati presenti nel file. È possibile importare tutti i server registrati, i server registrati in un dato gruppo di server oppure un singolo server registrato. La funzionalità di importazione è ricorsiva. Se, ad esempio, il gruppo di server A contiene il gruppo di server B e il gruppo di server B contiene i gruppi di server C e D, l'importazione del gruppo di server A determinerà l'importazione di tutte le voci contenute nei gruppi A, B, C e D.  
  
     Il gruppo di server visualizza solo i gruppi di server dell'albero di server registrati corrente.  
  
     Se si sceglie di importare un gruppo di server o un server esistente, verrà visualizzato un messaggio che richiede di confermare la sovrascrittura del server o del gruppo di server esistente.  
  
 Le registrazioni dei server che utilizzano l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguono l'archiviazione delle password separatamente per ogni utente. Dopo avere importato le registrazioni dei server, è necessario che gli utenti immettano la password per ogni server al momento della prima connessione, in modo tale che le password vengano archiviate nei relativi elenchi dei server registrati. Questa operazione non è necessaria per i server registrati mediante l'autenticazione di Windows.  
  
## <a name="see-also"></a>Vedere anche  
 [Modifica della registrazione di un server &#40;SQL Server Management Studio&#41;](./change-a-server-s-registration-sql-server-management-studio.md)   
 [Esportare informazioni relative a server registrati &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md)   
 [Creare un nuovo server registrato &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md)  
  
