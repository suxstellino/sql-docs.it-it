---
description: Rimozione di SSMA per componenti di Sybase (SybaseToSQL)
title: Rimozione di SSMA per i componenti Sybase (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 02a782e583463e5a986e3c7c1a42152fc27b14cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069946"
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Rimozione di SSMA per componenti di Sybase (SybaseToSQL)
Al termine della migrazione dei database da Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , potrebbe essere necessario disinstallare i componenti di SSMA. È possibile disinstallare i componenti client in qualsiasi momento, ma non è necessario disinstallare il pacchetto di estensione da a meno che non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si sia certi che i database migrati non usino più funzioni nello schema **ssma_syb** del database **sysdb** .  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Disinstallazione di SSMA per il client Sybase  
È possibile disinstallare SSMA utilizzando **Installazione applicazioni**.  
  
**Per disinstallare SSMA**  
  
1.  Nel Pannello di controllo aprire **Programmi e funzionalità**.  
  
2.  Selezionare **Microsoft SQL Server Migration Assistant per Sybase**, quindi fare clic su **Rimuovi**.  
  
3.  Per confermare che si vuole disinstallare SSMA, fare clic su **Sì**.  
  
## <a name="uninstalling-the-extension-pack"></a>Disinstallazione del pacchetto di estensione  
Se si è certi che i database migrati non utilizzino oggetti nello schema di **sysdb.ssma_syb** , è possibile rimuovere il pacchetto di estensione utilizzando **Installazione applicazioni**.  
  
Per disinstallare il pacchetto di estensione  
  
1.  Nel Pannello di controllo aprire **Programmi e funzionalità**.  
  
2.  Selezionare **Microsoft SQL Server Migration Assistant per Sybase Extension Pack**, quindi fare clic su **Rimuovi**.  
  
3.  Per confermare che si vuole disinstallare il pacchetto di estensione, fare clic su **Sì**.  
  
4.  Nella pagina istanze con script database utilità fare clic su **Avanti**.  
  
5.  Nella pagina parametri connessione selezionare il metodo di autenticazione e quindi fare clic su **Avanti**.  
  
    L'autenticazione di Windows utilizzerà le credenziali di Windows per tentare di accedere all'istanza di SQL Server. Se si seleziona SQL Server autenticazione, è necessario immettere un nome di account di accesso SQL Server e una password.  
  
6.  Nella pagina operazione completata fare clic su **OK**.  
  
7.  Nella pagina fine fare clic su **Esci**.  
  
Dopo la disinstallazione di, è possibile verificare che lo schema di **sysdb.ssma_syb** e possibilmente l'intero database di **sysdb** sia stato rimosso utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Tuttavia, se si usano altri prodotti SSMA, viene usato anche il database **sysdb** . Se il database esiste e si è certi che nessun altro database faccia riferimento a oggetti in questo database, è possibile scollegare il database.  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per Sybase client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Installazione dei componenti di SSMA in SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  
