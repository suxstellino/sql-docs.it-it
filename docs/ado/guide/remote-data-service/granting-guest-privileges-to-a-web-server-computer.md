---
description: Concessione dei privilegi Guest a un computer server Web
title: Concessione dei privilegi guest a un computer server Web | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- guest privileges in RDS [ADO]
ms.assetid: e851a22d-01bc-4eb0-bc42-92b8f65d1c63
author: rothja
ms.author: jroth
ms.openlocfilehash: d6c810759ca8fa71e5b136d7be2c302fcf47adf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031953"
---
# <a name="granting-guest-privileges-to-a-web-server-computer"></a>Concessione dei privilegi Guest a un computer server Web
Per utilizzare Servizi Desktop remoto, è necessario aggiungere l'account del server Web anonimo (IUSR_ *ComputerName*) al gruppo locale Guest sul computer server Web.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
### <a name="to-grant-guest-privileges-to-a-web-server-computer"></a>Per concedere i privilegi guest a un computer server Web  
  
1.  Nel computer server Microsoft Windows 2000, fare clic sul pulsante **Start**, scegliere **programmi**, **strumenti di amministrazione**, quindi fare clic su **Gestione computer**.  
  
2.  Nell'albero della console, in **utenti e gruppi locali** fare clic su **gruppi**.  
  
3.  Selezionare il gruppo locale **Guest** . Scegliere **Proprietà** dal menu **azione** .  
  
4.  Nella finestra di dialogo **Proprietà Guest** fare clic su **Aggiungi**.  
  
5.  Se l'account del server Web anonimo non viene visualizzato nell'elenco della finestra di dialogo **Seleziona utenti o gruppi** , digitarne il nome (IUSR_ *nomecomputer*) nella casella vuota inferiore, quindi fare clic su **Aggiungi**.  
  
6.  Fare clic su **OK**.