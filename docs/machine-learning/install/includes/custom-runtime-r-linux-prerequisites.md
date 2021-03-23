---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: f14691931190da9f72d3d54a3f1b4280b679c6e3
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833744"
---
## <a name="prerequisites"></a>Prerequisiti

Prima di installare un runtime personalizzato di R, installare quanto segue:

+ Installare SQL Server 2019 per Linux. È possibile installare SQL Server in Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) versione 12 e Ubuntu. Per altre informazioni, vedere le [Linee guida per l'installazione di SQL Server in Linux](../../../linux/sql-server-linux-setup.md).

+ Installare l'aggiornamento cumulativo (CU) 3 o versione successiva per SQL Server 2019. Seguire questa procedura:
    1. Configurare i repository per gli aggiornamenti cumulativi. Per altre informazioni, vedere [Configurare i repository per l'installazione e l'aggiornamento di SQL Server in Linux](../../../linux/sql-server-linux-change-repo.md).

    1. Aggiornare il pacchetto **mssql-server** all'aggiornamento cumulativo più recente. Per altre informazioni, vedere [la sezione sull'aggiornamento di SQL Server nelle linee guida per l'installazione di SQL Server in Linux](../../../linux/sql-server-linux-setup.md#upgrade).
