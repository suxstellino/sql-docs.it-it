---
title: Proprietà - SQL Server Integration Services (scheda Accesso)
description: Informazioni sulla scheda Accesso della finestra di dialogo Proprietà di SQL Server Integration Services e su come specificare un account e avviare o arrestare il servizio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: c0eb1b87-6bb0-475e-8492-0fd3c3f910ea
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: dc1a5d6d5710d202d1bdc25a8e21cca550acf15f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481522"
---
# <a name="sql-server-integration-services-properties-log-on-tab"></a>Proprietà - SQL Server Integration Services (scheda Accesso)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Usare la scheda **Accesso** della finestra di dialogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] **Proprietà** di  per specificare l'account usato dal servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e avviare e arrestare il servizio.  
  
## <a name="options"></a>Opzioni  
 **Account di sistema locale**  
 Specificare un account di sistema locale che non richiede una password. Tuttavia, a seconda dei privilegi concessi, l'account di sistema locale può impedire le interazioni tra il servizio e gli altri server.  
  
 **Account seguente**  
 Specificare un account utente locale o di dominio che utilizza l'autenticazione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. [!INCLUDE[msCoName](../../includes/msconame-md.md)] consiglia di utilizzare un account utente di dominio con diritti minimi. Per ulteriori informazioni sulla selezione di un account, cercare l'argomento "Impostazione di account di Windows per i servizi" nella documentazione online.  
  
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
  
  
