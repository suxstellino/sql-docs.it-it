---
title: Versioni in lingua locale di SQL Server | Microsoft Docs
description: Le versioni in lingua inglese dei sistemi operativi supportati supportano le versioni localizzate di SQL Server tramite l'uso delle impostazioni di Multilingual User Interface Pack di Windows.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 20b99363-0490-4aa3-9a3d-262f827d81e8
author: cawrites
ms.author: chadam
ms.openlocfilehash: cabe727dd72136f2b4848620bdee0a02d8c915e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336143"
---
# <a name="local-language-versions-in-sql-server"></a>Versioni in lingua locale di SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta tutte le lingue supportate dai sistemi operativi di Windows.  
  
## <a name="cross-language-support"></a>Supporto di lingue diverse  
  
-   La versione in lingua inglese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è supportata in tutte le versioni localizzate dei sistemi operativi.  
  
-   Le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportate nei sistemi operativi localizzati nella lingua corrispondente o nelle versioni in lingua inglese dei sistemi operativi supportati tramite l'utilizzo delle impostazioni di Windows Multilingual User Interface Pack (MUI). Per altre informazioni, vedere [Configure Operating System to Support Localized Versions](../../sql-server/install/local-language-versions-in-sql-server.md#BK_ConfigureOS).  
  
-   Solo le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere aggiornate alle versioni localizzate nella stessa lingua e non possono essere aggiornate alla versione in lingua inglese.  
  
-   Le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono anche essere installate side-by-side alle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]in lingua inglese.  
  
##  <a name="configure-operating-system-to-support-localized-versions"></a><a name="BK_ConfigureOS"></a> Configure Operating System to Support Localized Versions  
 Grazie all'utilizzo delle impostazioni MUI (Multilingual User Interface Pack) di Windows, le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportate nelle versioni in lingua inglese dei sistemi operativi supportati.  
  
 Tuttavia, prima di installare una versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un server in cui viene eseguito un sistema operativo in lingua inglese con un'impostazione MUI non in lingua inglese, è necessario verificare alcune impostazioni del sistema operativo. È necessario innanzitutto controllare che le impostazioni del sistema operativo riportate di seguito corrispondano alla lingua della versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da installare:  
  
-   Impostazione dell'interfaccia utente del sistema operativo  
  
-   Impostazioni locali dell'utente nel sistema operativo  
  
-   Impostazioni locali del sistema  
  
 Se le impostazioni non corrispondono alla lingua della versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da installare, sarà necessario utilizzare le procedure seguenti per configurare correttamente le impostazioni del sistema operativo.  
  
> [!CAUTION]  
>  NON sono supportate installazioni di versioni localizzate diverse delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nello stesso computer.  
  
#### <a name="to-change-the-operating-system-user-interface-setting"></a>Per modificare le impostazioni dell'interfaccia utente del sistema operativo  
  
1.  Se non è stato già fatto, installare l'interfaccia MUI del sistema operativo che corrisponde alla versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Nel Pannello di controllo aprire **Opzioni internazionali e della lingua**.  
  
3.  Nella scheda **Lingue** , in **Lingua utilizzata nei menu e nelle finestre di dialogo** selezionare un valore nell'elenco.  
  
     Questa impostazione avrà effetto sulla lingua dell'interfaccia utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pertanto deve corrispondere alla versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Fare clic su **Applica** per confermare la modifica e quindi su **OK** per chiudere la finestra.  
  
#### <a name="to-change-the-operating-system-user-locale-setting"></a>Per modificare le impostazioni locali dell'utente del sistema operativo  
  
1.  Se non è stato già fatto, installare l'interfaccia MUI del sistema operativo che corrisponde alla versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Nel Pannello di controllo aprire **Opzioni internazionali e della lingua**.  
  
3.  Nella scheda **Opzioni internazionali** , in **Selezionare l'elemento corrispondente alle preferenze** selezionare un valore nell'elenco.  
  
     Questa impostazione avrà effetto sulla formattazione dei dati specifici della lingua.  
  
4.  Fare clic su **Applica** per confermare la modifica e quindi su **OK** per chiudere la finestra.  
  
#### <a name="to-change-the-system-locale-setting"></a>Per modificare le impostazioni locali del sistema  
  
1.  Se non è stato già fatto, installare l'interfaccia MUI del sistema operativo che corrisponde alla versione localizzata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Nel Pannello di controllo aprire **Opzioni internazionali e della lingua**.  
  
3.  Nella scheda **Avanzate** , in **Selezionare una lingua per i programmi non Unicode da utilizzare** selezionare un valore nell'elenco.  
  
     Mediante questa impostazione, nel programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno selezionate le regole di confronto predefinite ottimali per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
4.  Fare clic su **Applica** per confermare la modifica e quindi su **OK** per chiudere la finestra.  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Installare SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  
  
