---
title: Creare le credenziali - Eseguire l'autenticazione nel servizio di archiviazione Azure | Microsoft Docs
description: In SQL Server usare la pagina Crea credenziali della finestra di dialogo Backup database per fornire un certificato di gestione di Azure per convalidare la connessione.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.backuptourl.createcred.f1
ms.assetid: 0622619d-27c5-4ff0-83e5-cde31648c27a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 331d973e3290c5528ba811f7d6306f7505b55041
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129263"
---
# <a name="create-credential---authenticate-to-azure-storage"></a>Creare le credenziali - Eseguire l'autenticazione nel servizio di archiviazione Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Usare la finestra di dialogo **Backup su URL - Creazione credenziali** per creare nuove credenziali SQL.  
  
 Quando si usa questa finestra di dialogo per creare le credenziali, è necessario specificare un certificato di gestione di Azure aggiunto all'archivio certificati locale o un profilo di pubblicazione scaricato nel computer per convalidare la sottoscrizione e le informazioni sull'account di archiviazione.  
  
 **Credenziali SQL**  
 Specificare il nome delle credenziali SQL che si desidera creare.  
  
## <a name="azure-credentials"></a>Credenziali Azure  
 **Certificato di gestione**  
 Usare questa opzione per specificare un certificato dall'archivio certificati locale corrispondente al certificato di gestione di Azure. Per altre informazioni sul certificato di gestione di Azure, vedere [Creare e caricare un certificato di gestione per Azure](/previous-versions/azure/gg551722(v=azure.100)).  
  
 **Sottoscrizione**  
 Selezionare, digitare o incollare l'ID sottoscrizione di Azure corrispondente al certificato di gestione dall'archivio certificati locale.  
  
 **Profilo di pubblicazione**  
 Utilizzare questa opzione se si dispone di un profilo di pubblicazione scaricato nel computer. Se si utilizza questa opzione, l'ID sottoscrizione e il certificato vengono inseriti automaticamente.  
  
> [!CAUTION]  
>  SQL Server supporta attualmente la versione del profilo di pubblicazione 2.0. Per scaricare la versione supportata del profilo di pubblicazione, vedere [Download del profilo di pubblicazione 2.0](https://go.microsoft.com/fwlink/?LinkId=396421).  
  
## <a name="storage-account"></a>Account di archiviazione  
 Selezionare l'account di archiviazione da utilizzare per archiviare i file di backup.  
  
