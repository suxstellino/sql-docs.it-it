---
description: Log shipping - Impostazioni backup log delle transazioni
title: Log shipping - Impostazioni backup log delle transazioni | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.logshipping.settings.tlogback.f1
ms.assetid: 9a6e6c16-7f71-412b-bba6-7bffac001277
author: stevestein
ms.author: sstein
ms.openlocfilehash: 00516158d50c2ccc551e038647e38510636a85ed
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981097"
---
# <a name="log-shipping-transaction-log-backup-settings"></a>Log shipping - Impostazioni backup log delle transazioni
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa finestra di dialogo per configurare e modificare le impostazioni di backup del log delle transazioni di una configurazione per il log shipping.  
  
 Per approfondimenti sui concetti correlati al log shipping, vedere [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="options"></a>Opzioni  
 **Percorso di rete della cartella di backup**  
 In questa casella digitare la condivisione di rete per la cartella di backup. È necessario che la cartella locale in cui vengono salvati i backup del log della transazioni sia condivisa, in modo che i processi di copia per il log shipping siano in grado di copiare questi file nel server secondario. È necessario concedere le autorizzazioni di lettura per la condivisione di rete all'account proxy nel cui ambito verrà eseguito il processo di copia nell'istanza del server secondario. Per impostazione predefinita, l'account proxy è rappresentato dall'account del servizio SQLServerAgent dell'istanza del server secondario. L'amministratore può tuttavia scegliere un account proxy diverso per il processo.  
  
 **Se la cartella di backup si trova nel server primario, digitare il percorso locale della cartella**  
 Se la cartella di backup si trova nel server primario, digitare la lettera dell'unità locale e il percorso della cartella di backup. Se la cartella di backup non si trova nel server primario, è possibile omettere questa l'impostazione.  
  
 Se in questo punto si specifica un percorso locale, il comando BACKUP userà questo percorso per creare i backup del log delle transazioni. Se invece non si specifica alcun percorso locale, il comando BACKUP userà il percorso di rete specificato nella casella **Percorso di rete della cartella di backup** .  
  
> [!NOTE]  
>  Se l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione nell'ambito dell'account di sistema locale nel server primario, è necessario creare la cartella di backup nel server primario e specificare in questo punto il percorso locale della cartella. L'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dell'istanza del server primario deve disporre delle autorizzazioni di lettura e scrittura per questa cartella.  
  
 **Elimina i file più vecchi di**  
 Consente di specificare il periodo di tempo per il quale si desidera che i backup del log delle transazioni rimangano nella directory di backup prima di essere eliminati.  
  
 **Invia avviso se il backup non viene eseguito entro**  
 Consente di specificare il periodo di tempo di attesa prima che il log shipping invii un avviso relativo alla mancata esecuzione di backup del log delle transazioni.  
  
 **Nome processo**  
 Consente di visualizzare il nome del processo di SQL Server Agent utilizzato per creare i backup del log delle transazioni per il log shipping. Durante la creazione iniziale del processo, è possibile modificare il nome digitandolo nella casella.  
  
 **Pianificare**  
 Consente di visualizzare la pianificazione corrente per il backup dei log delle transazioni del database primario. Prima che il processo di backup venga creato, è possibile modificare la pianificazione facendo clic su **Pianificazione...** . Dopo la creazione del processo, è possibile modificare la pianificazione facendo clic su **Modifica processo...** .  
  
### <a name="backup-job"></a>Processo di backup  
 **Pianificazione**  
 Consente di modificare la pianificazione creata al momento della creazione del processo di SQL Server Agent.  
  
 **Modifica processo...**  
 Consente di modificare i parametri del processo di SQL Server Agent per il processo che esegue i backup del log delle transazioni nel database primario.  
  
 **Disabilita questo processo**  
 Consente di disabilitare il processo di SQL Server Agent per impedire la creazione di backup del log delle transazioni.  
  
### <a name="compression"></a>Compressione  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (o versioni successive) supporta la [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **Imposta compressione backup**  
 In [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] o versione successiva selezionare uno dei valori di compressione dei backup seguenti per i backup del log della configurazione per il log shipping corrente:  
  
|Valore|Descrizione|  
|-|-|  
|**Utilizza l'impostazione predefinita del server**|Fare clic su questa opzione per utilizzare l'impostazione predefinita a livello di server.<br /><br /> Questa impostazione predefinita è specificata dall'opzione di configurazione del server **Valore predefinito di compressione backup** . Per informazioni su come visualizzare l'impostazione corrente di questa opzione, vedere [Visualizzare o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
|**Comprimi backup**|Fare clic su questa opzione per comprimere il backup, indipendentemente dall'impostazione predefinita a livello di server.<br /><br /> **\*\* Importante \*\*** Per impostazione predefinita, la compressione aumenta significativamente l'uso della CPU e la CPU aggiuntiva usata dal processo di compressione può avere un impatto negativo sulle operazioni simultanee. Potrebbe quindi essere necessario creare backup compressi con priorità bassa in una sessione in cui l'utilizzo della CPU è limitato da [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Per ulteriori informazioni, vedere [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
|**Non comprimere il backup**|Fare clic su questa opzione per creare un backup non compresso, indipendentemente dall'impostazione predefinita a livello di server.|  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare un utente per la creazione e la gestione di processi di SQL Server Agent](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)   
 [Informazioni sul log shipping &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
  
