---
title: Introduzione a SQL Server (in Linux) nel cloud
titleSuffix: SQL Server
description: Informazioni su come installare SQL Server in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) o Ubuntu nel cloud di propria scelta.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: ae7502fcba11d9892cbcf8ed2c94ef205e0abc53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273132"
---
# <a name="quickstart-run-sql-server-in-the-cloud"></a>Guida introduttiva: Eseguire SQL Server nel cloud
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In questa guida di avvio rapido verrà installato SQL Server in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) o Ubuntu nel cloud di propria scelta. Per eseguire SQL Server in Linux in Azure, vedere [Effettuare il provisioning di una macchina virtuale Linux di SQL Server nel portale di Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%252fsql%252ftoc%252ftoc.json).

> [!NOTE]
> Se si sceglie di eseguire un'edizione a pagamento di SQL Server, è necessario Bring Your Own License (BYOL).

## <a name="amazon-web-services"></a>Amazon Web Services
1.  Creare un AMI Linux con almeno 2 GB di memoria dal Marketplace 
    * [RHEL 7.3+](https://aws.amazon.com/marketplace/pp/B00KWBZVK6)
    * [SLES v12 SP2+](https://aws.amazon.com/marketplace/pp/B00PMM99PI)
    * [Ubuntu 16.04](https://aws.amazon.com/marketplace/pp/B01JBL2M0O)
1.  Connettersi all'AMI con ssh
1.  Seguire la guida di avvio rapido per la distribuzione di Linux scelta: 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Configurare per le connessioni remote: 
    * Aprire la [console di Amazon EC2]( https://console.aws.amazon.com/ec2/)
    * Nel riquadro di spostamento scegliere **Security Groups** (Gruppi di sicurezza). 
    * Scegliere **Inbound, Edit, Add Rule** (In ingresso, Modifica; Aggiungi regola)
    * Aggiungere una regola in ingresso per consentire il traffico sulla porta su cui è in ascolto SQL Server (porta TCP predefinita 1433)

    
## <a name="digital-ocean"></a>Digital Ocean
1. Accedere al [pannello di controllo](https://cloud.digitalocean.com/login) e fare clic su Create a droplet (Crea un droplet)
1. Scegliere un droplet di Ubuntu 16.04 con almeno 2 GB di memoria
1. Connettersi al droplet con ssh
1. Seguire la [guida di avvio rapido di Ubuntu](quickstart-install-connect-ubuntu.md)
1. Configurare per le connessioni remote:
    * Nella parte superiore del pannello di controllo seguire il collegamento **Networking** (Rete) e quindi selezionare **Firewalls**.
    * Aggiungere una regola in ingresso per consentire il traffico sulla porta su cui è in ascolto SQL Server (porta TCP predefinita 1433)
    
## <a name="google-cloud-platform"></a>Google Cloud Platform
1.  Creare un'immagine Linux con almeno 2 GB di memoria da Cloud Launcher 
    * [RHEL 7.3+](https://console.cloud.google.com/launcher/details/rhel-cloud/rhel-7)
    * [SLES v12 SP4](https://console.cloud.google.com/launcher/details/suse-cloud/sles-12)
    * [Ubuntu 16.04](https://console.cloud.google.com/launcher/details/ubuntu-os-cloud/ubuntu-xenial)
1.  Connettersi all'immagine con ssh
1.  Seguire la guida di avvio rapido per la distribuzione di Linux scelta: 
    * [RHEL](quickstart-install-connect-red-hat.md)
    * [SLES](quickstart-install-connect-suse.md)
    * [Ubuntu](quickstart-install-connect-ubuntu.md)
1.  Configurare per le connessioni remote: 
    * Passare a [Firewall Rules](https://console.cloud.google.com/networking/firewalls) (Regole del firewall)
    * Aggiungere una regola in ingresso per consentire il traffico sulla porta su cui è in ascolto SQL Server (porta TCP predefinita: 1433)