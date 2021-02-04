---
description: Visualizzazione di avvisi relativi all'esecuzione di report personalizzati
title: Visualizzazione di avvisi relativi all'esecuzione di report personalizzati
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 0deed900-c910-4d12-aac0-6ab9e39eb068
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cf87ce7e04e50ea64a6f98db0b29dfc7e90f813d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250490"
---
# <a name="unsuppress-run-custom-report-warnings"></a>Visualizzazione di avvisi relativi all'esecuzione di report personalizzati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Per i report personalizzati sono disponibili due finestre di dialogo di avviso. In questo argomento vengono descritte le modalità per consentire la visualizzazione delle caselle in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Per impostazione predefinita, prima dell'esecuzione di un report personalizzato viene visualizzata la finestra di dialogo **Esegui report personalizzato** . Se si seleziona la casella di controllo **Non visualizzare più questo avviso** , la finestra di dialogo non verrà più visualizzata. Per impostazione predefinita, la finestra di dialogo **Esegui report personalizzato** viene visualizzata anche quando si apre un report personalizzato e quindi si fa clic su un collegamento a un altro report personalizzato. La finestra di dialogo contiene il percorso del file di report personalizzato drill-through. Se si seleziona la casella di controllo **Non visualizzare più questo avviso** , la finestra di dialogo non verrà più visualizzata.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-unsuppress-the-main-custom-report-warning-dialog-box"></a>Per consentire la visualizzazione della finestra di dialogo di avviso relativa al report personalizzato principale  
  
1.  Connettersi a \<*Server*>\\<*Condivisione*>|\<*Drive*>\Documents and Settings\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml.  
  
2.  Fare clic con il pulsante destro del mouse su **reports.xml** e quindi scegliere **Modifica**.  
  
3.  Modificare **<SuppressWarning>true\<\/SuppressWarning> in <SuppressWarning>false\<\/SuppressWarning>** .  
  
4.  Riavviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-unsuppress-the-drill-through-custom-report-warning-dialog-box"></a>Per consentire la visualizzazione della finestra di dialogo di avviso relativa al report personalizzato drill-through  
  
1.  Connettersi a \<*Server*>\\<*Condivisione*>|\<*Drive*>\Documents and Settings\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml.  
  
2.  Fare clic con il pulsante destro del mouse su **reports.xml**, quindi fare clic su **Modifica**.  
  
3.  Modificare **<SuppressDrillthroughWarning>true\<\/SuppressDrillthroughWarning> in <SuppressDrillthroughWarning>false\<\/SuppressDrillthroughWarning>** .  
  
4.  Riavviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
[Report personalizzati in Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
[Aggiunta di un report personalizzato a Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md)  
[Usare report personalizzati con proprietà dei nodi di Esplora oggetti](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
  
