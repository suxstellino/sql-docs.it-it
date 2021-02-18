---
title: Proprietà - Analysis Server (scheda Accesso)
description: Informazioni sulla scheda Accesso della finestra di dialogo Proprietà computer Analysis Server e su come usarla per specificare l'account del servizio SSAS e avviare o arrestare il servizio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: a82e0c98-efaa-4b0b-9582-3c879ee42444
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 412fb42b6756d2a574b78f5695d34e5d8c445b0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349896"
---
# <a name="analysis-server-properties-log-on-tab"></a>Proprietà - Analysis Server (scheda Accesso)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilizzare la scheda **Accesso** della finestra di dialogo **Proprietà - Analysis Server** per specificare l'account utilizzato dal servizio [!INCLUDE[ssAS](../../includes/ssas-md.md)] e avviare e arrestare il servizio.  
  
> [!NOTE]  
>  Quando si modifica il **Nome account** utilizzato da un servizio in un'istanza cluster, il nuovo account deve essere membro del gruppo di dominio specificato durante l'installazione per il servizio in corso di modifica oppure è necessario disporre dell'autorizzazione per aggiungere membri a tale gruppo. Se non si dispone dell’autorizzazione per modificare l'appartenenza al gruppo, contattare l’amministratore di dominio.  
  
## <a name="options"></a>Opzioni  
 **Account di sistema locale**  
 Specificare un account di sistema locale che non richiede una password. Tuttavia, a seconda dei privilegi concessi, l'account di sistema locale può impedire le interazioni tra il servizio e gli altri server.  
  
 **Account seguente**  
 Specificare un account utente locale o di dominio che utilizza l'autenticazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di utilizzare un account utente di dominio con diritti minimi. Per ulteriori informazioni sulla selezione di un account, cercare l'argomento "Impostazione di account di servizio Windows" nella documentazione online.  
  
 **Account Name** (Nome account)  
 Specificare il nome dell'account utente locale o di dominio.  
  
 **Password**  
 Digitare la password dell'account.  
  
 **Conferma password**  
 Digitare nuovamente la password dell'account.  
  
 **Inizia**  
 Avviare il servizio.  
  
 **Stop**  
 Consente di arrestare il servizio.  
  
 **Sospendi**  
 Consente di sospendere il servizio.  
  
 **Riprendi**  
 Consente di riprendere un servizio sospeso.  
  
  
