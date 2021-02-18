---
title: Proprietà - SQL Server (scheda Accesso)
description: Informazioni sulla scheda Accesso della finestra di dialogo Proprietà di SQL Server Usare questa scheda per specificare l'account usato da SQL Server e per avviare o arrestare il servizio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 405073fc-eaa3-43c6-bf82-2cd58cacc1c3
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 63e71e0a0e4c53b77b3c1f11225f49abb250c585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345980"
---
# <a name="sql-server-properties-log-on-tab"></a>Proprietà - SQL Server (scheda Accesso)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilizzare la scheda **Accesso** della finestra di dialogo **Proprietà - SQL Server** per specificare l'account utilizzato dal servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per modificare la password di un account e per avviare e arrestare il servizio. La modifica della password di un account ha effetto immediato.  
  
> [!NOTE]  
>  Quando si modifica il nome account utilizzato da un servizio in un'istanza cluster, il nuovo account deve essere membro del gruppo di dominio specificato durante l’installazione per il servizio in corso di modifica oppure è necessario disporre dell’autorizzazione per aggiungere membri a tale gruppo. Se non si dispone dell’autorizzazione per modificare l'appartenenza al gruppo, contattare l’amministratore di dominio.  
>   
>  Per ulteriori informazioni sulla selezione di un account per l'esecuzione del servizio, vedere "Impostazione di account di servizio Windows" nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Opzioni  
 **Account predefinito**  
 **Sistema locale**  
 -   Specifica l'account di sistema locale. Per questo account non è necessaria una password. Tuttavia, a seconda dei privilegi concessi, l'account di sistema locale può impedire le interazioni tra il servizio e gli altri server.  
  
 **Servizio locale**  
 -   Specifica l'account di servizio locale. Per questo account non è necessaria una password. Tuttavia, a seconda dei privilegi concessi, l'account di servizio locale può impedire le interazioni tra il servizio e gli altri server.  
  
 **Servizio di rete**  
 -   È consigliabile non utilizzare l'account Servizio di rete per i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per questi servizi sono più adatti gli account utente locale o di dominio.  
  
 **Account seguente**  
 Specificare un account utente locale o di dominio che utilizza l’autenticazione di Windows. È consigliabile utilizzare un account utente di dominio con diritti minimi per i servizi.  
  
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
  
> [!IMPORTANT]  
>  Per impostazione predefinita, solo i membri del gruppo di amministratori locale possono avviare, arrestare, mettere in pausa, riprendere o riavviare un servizio. Per concedere a utenti non amministratori la possibilità di gestire servizi, vedere [Concedere agli utenti i privilegi per gestire i servizi in Windows Server 2003](https://support.microsoft.com/kb/325349). Il processo è analogo ad altre versioni di Windows.  
  
> [!NOTE]  
>  Quando si avvia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un errore WMI contenente la frase "non implementato [0x80004001]" può indicare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è installato nel computer di destinazione.  
  
  
