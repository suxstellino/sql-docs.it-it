---
title: Distribuire in modalità Active Directory - Prerequisiti
titleSuffix: SQL Server Big Data Cluster
description: Configurare Active Directory per i cluster Big Data di SQL Server
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3424863924338a0d3f02102609052d6c159e1043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048021"
---
# <a name="deploy-big-data-clusters-2019-in-active-directory-mode-prerequisites"></a>Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory: Prerequisiti

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo documento descrive le attività preliminari alla distribuzione di un cluster Big Data (BDC) di SQL Server in modalità di autenticazione di Active Directory. Il cluster usa un dominio di Active Directory esistente per l'autenticazione.

>[!Note]
>Prima di SQL Server 2019 CU5, i cluster Big Data prevedevano una restrizione per cui era possibile distribuire un solo cluster in un dominio di Active Directory. Questa restrizione è stata rimossa con la versione CU5. Per informazioni dettagliate sulle nuove funzionalità, vedere [Concetto: distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deployment-background.md). Gli esempi in questo articolo sono stati modificati per adattarsi a entrambi i casi d'uso di distribuzione.

## <a name="background"></a>Background

Per abilitare l'autenticazione di Active Directory, il cluster Big Data crea automaticamente gli utenti, i gruppi, gli account computer e i nomi dell'entità servizio (SPN) necessari per i vari servizi nel cluster. Per assicurare una certa indipendenza per questi account e consentire autorizzazioni di ambito, si consiglia di creare un'unità organizzativa prima della distribuzione del cluster. Tutti gli oggetti di Active Directory correlati al cluster Big Data verranno creati durante la distribuzione. 

## <a name="pre-requisites"></a>Prerequisiti

### <a name="organizational-unit-ou"></a>Unità organizzativa (OU)
Un'unità organizzativa è una suddivisione all'interno di Active Directory in cui posizionare utenti, gruppi e persino altre unità organizzative. Le unità organizzative di grandi dimensioni possono essere usate per eseguire il mirroring di una struttura funzionale o aziendale di un'organizzazione. In questo articolo verrà creata un'unità organizzativa denominata `bdc` come esempio. 

>[!NOTE]
>L'unità organizzativa rappresenta i confini amministrativi e consente ai clienti di controllare l'ambito di autorità degli amministratori dei dati. 

È possibile seguire i [principi di progettazione delle unità organizzative](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) per scegliere la struttura ottimale per l'utilizzo delle unità organizzative all'interno dell'organizzazione.

### <a name="ad-account-for-bdc-domain-service-account"></a>Account AD per l'account del servizio di dominio del cluster Big Data

Per poter creare automaticamente tutti gli oggetti necessari in Active Directory, il cluster Big Data necessita di un account AD con autorizzazioni specifiche per creare utenti, gruppi e account computer all'interno dell'unità organizzativa specificata. Questo articolo illustra come configurare l'autorizzazione dell'account AD. In questo articolo viene usata una chiamata all'account AD `bdcDSA` come esempio.

### <a name="auto-generated-active-directory-objects"></a>Oggetti di Active Directory generati automaticamente
La distribuzione di cluster Big Data genera automaticamente i nomi di account e gruppi. Ogni account rappresenta un servizio nel cluster Big Data e verrà gestito dal cluster Big Data per tutta la durata di utilizzo del cluster. Gli account sono proprietari dei nomi dell'entità servizio (SPN) richiesti da ogni servizio.  Per un elenco completo degli account e dei gruppi generati automaticamente di Active Directory e per il relativo servizio gestito, vedere [Oggetti di Active Directory generati automaticamente](active-directory-objects.md).

>[!IMPORTANT]
>In base ai criteri di scadenza delle password impostati nel controller di dominio, le password per questi account possono scadere. Il criterio predefinito per la scadenza prevede 42 giorni. Non è disponibile alcun meccanismo di rotazione delle credenziali per tutti gli account dei cluster Big Data, quindi il cluster diventa inutilizzabile quando viene raggiunta la scadenza. Per risolvere il problema, aggiornare i criteri di scadenza per gli account del servizio del cluster Big Data impostandoli su "La password non scade mai" nel controller di dominio. Questa azione può essere eseguita prima o dopo la scadenza. Nel secondo caso, Active Directory attiverà nuovamente le password scadute.
>
>L'immagine seguente illustra la posizione in cui impostare questa proprietà per utenti e computer di Active Directory.
>
>:::image type="content" source="media/deploy-active-directory/image25.png" alt-text="Impostare i criteri di scadenza delle password":::

La procedura seguente presuppone l'esistenza di un controller di dominio Active Directory. Se non è presente alcun controller di dominio, la [guida](https://social.technet.microsoft.com/wiki/contents/articles/37528.create-and-configure-active-directory-domain-controller-in-azure-windows-server.aspx) seguente include alcuni passaggi che possono essere utili.

## <a name="create-ad-objects"></a>Creare oggetti di Active Directory

Prima di distribuire un cluster Big Data con l'integrazione di Active Directory, eseguire le operazioni seguenti:

1. Creare un'unità organizzativa in cui verranno archiviati tutti gli oggetti di Active Directory correlati al cluster Big Data. In alternativa, è possibile scegliere un'unità organizzativa esistente in fase di distribuzione.
1. Creare un account AD per il cluster Big Data oppure usarne uno esistente e assegnare all'account le autorizzazioni appropriate all'interno dell'unità organizzativa specificata.

### <a name="create-a-user-in-ad-for-bdc-domain-service-account"></a>Creare un utente in Active Directory per l'account del servizio di dominio del cluster Big Data

Il cluster Big Data deve avere un account con autorizzazioni specifiche. Prima di continuare, assicurarsi che sia già presente un account Active Directory oppure crearne uno nuovo, che può essere usato dal cluster Big Data per configurare gli oggetti necessari.

Per creare un nuovo utente in Active Directory, è possibile fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa e scegliere **Nuovo** > **Utente**:

![Finestra di dialogo degli utenti di Active Directory](./media/deploy-active-directory/image12.png)

Questo utente verrà definito *account del servizio di dominio del cluster Big Data* in questo articolo.

### <a name="create-an-ou"></a>Creare un'unità organizzativa

Nel controller di dominio aprire **Utenti e computer di Active Directory**. Nel pannello di sinistra fare clic con il pulsante destro del mouse sulla directory in cui si vuole creare l'unità organizzativa e scegliere **Nuovo** \> **Unità organizzativa**, quindi seguire le istruzioni della procedura guidata per creare l'unità organizzativa. In alternativa, è possibile creare un'unità organizzativa con PowerShell:

```powershell
New-ADOrganizationalUnit -Name "<name>" -Path "<Distinguished name of the directory you wish to create the OU in>"
```

Gli esempi in questo articolo usano `bdc` per il nome dell'unità organizzativa.

![Unità organizzativa di Active Directory](./media/deploy-active-directory/image13.png)

![Nuovo oggetto - Unità organizzativa](./media/deploy-active-directory/image14.png)

### <a name="set-permissions-for-an-ad-account"></a>Impostare le autorizzazioni per un account di AD

Se è stato creato un nuovo utente di Active Directory o se ne usa uno esistente, l'utente deve avere determinate autorizzazioni. Questo account è l'account utente che verrà usato dal controller del cluster Big Data per l'aggiunta del cluster ad Active Directory.

L'account del servizio di dominio del cluster Big Data deve essere in grado di creare utenti, gruppi e account computer nell'unità organizzativa. Nei passaggi seguenti l'account del servizio di dominio del cluster Big Data è denominato `bdcDSA`. È possibile scegliere qualsiasi nome per l'account.

1. Nel controller di dominio aprire **Utenti e computer di Active Directory**

1. Nel pannello sinistro passare al dominio, quindi all'unità organizzativa che verrà usata da `bdc`

1. Fare clic con il pulsante destro del mouse sull'unità organizzativa e scegliere **Proprietà**.

1. Passare alla scheda Sicurezza, assicurandosi di aver selezionato **Funzionalità avanzate**, di avere fatto clic con il pulsante destro del mouse sull'unità organizzativa e quindi di avere scelto **Visualizza**

    ![Proprietà degli oggetti BDC](./media/deploy-active-directory/image15.png)

1. Fare clic su **Aggiungi** e aggiungere l'utente **bdcDSA**

    ![Aggiungere le proprietà degli oggetti BDC](./media/deploy-active-directory/image16.png)

    ![Selezionare un oggetto](./media/deploy-active-directory/image17.png)

1. Selezionare l'utente **bdcDSA** e deselezionare tutte le autorizzazioni, quindi fare clic su **Avanzate**

1. Fare clic su **Aggiungi**

    ![Fare clic su Aggiungi](./media/deploy-active-directory/image18.png)

    - Fare clic su **Selezionare un'entità**, immettere **bdcDSA** e fare clic su OK

    - Impostare **Tipo** su **Consenti**

    - Impostare **Si applica a** su **Questo oggetto e tutti i discendenti**

        ![Impostare Consenti per le proprietà](./media/deploy-active-directory/image19.png)

    - Scorrere fino alla fine e fare clic su **Cancella tutto**

    - Scorrere di nuovo verso l'alto e selezionare:
       - **Leggi tutte le proprietà**
       - **Scrivi tutte le proprietà**
       - **Crea oggetti computer**
       - **Delete Computer objects** (Elimina oggetti computer)
       - **Create Group objects** (Crea oggetti di gruppo)
       - **Delete Group objects** (Elimina oggetti di gruppo)
       - **Create User objects** (Crea oggetti utente)
       - **Elimina oggetti utente**

    - Fare clic su **OK**.

- Fare clic su **Aggiungi**

    - Fare clic su **Selezionare un'entità**, immettere **bdcDSA** e fare clic su OK

    - Impostare **Tipo** su **Consenti**

    - Impostare **Si applica a** su **Descendant Computer objects** (Oggetti computer discendenti)

    - Scorrere fino alla fine e fare clic su **Cancella tutto**

    - Tornare all'inizio e selezionare **Reimposta password**

    - Fare clic su **OK**.

- Fare clic su **Aggiungi**

    - Fare clic su **Selezionare un'entità**, immettere **bdcDSA** e fare clic su OK

    - Impostare **Tipo** su **Consenti**

    - Impostare **Si applica a** su **Descendant User objects** (Oggetti utente discendenti)

    - Scorrere fino alla fine e fare clic su **Cancella tutto**

    - Tornare all'inizio e selezionare **Reimposta password**

    - Fare clic su **OK**.

- Fare clic su **OK** altre due volte per chiudere le finestre di dialogo aperte

## <a name="next-steps"></a>Passaggi successivi

[Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deploy.md)

[Risolvere i problemi di integrazione di Active Directory di cluster Big Data di SQL Server](troubleshoot-active-directory.md)

[Concetto: distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deployment-background.md)
