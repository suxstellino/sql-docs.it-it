---
description: Rimozione di SSMA per i componenti DB2 (DB2ToSQL)
title: Rimozione di SSMA per i componenti DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4ee0d698-6246-48eb-b963-d62be81cab6a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 63957996baa6f65b864a3fe74bdb6b5c90aab3a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072005"
---
# <a name="removing-ssma-for-db2-components-db2tosql"></a>Rimozione di SSMA per i componenti DB2 (DB2ToSQL)
Al termine della migrazione dei database da DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , potrebbe essere necessario disinstallare i componenti di SSMA. È possibile disinstallare i componenti client in qualsiasi momento. Tuttavia, non è consigliabile disinstallare il pacchetto di estensione da a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] meno che i database migrati non usino più funzioni nello schema **ssma_DB2** del database **sysdb** .  
  
## <a name="uninstalling-the-ssma-for-db2-client"></a>Disinstallazione di SSMA per il client DB2  
È possibile disinstallare SSMA utilizzando **Installazione applicazioni**.  
  
**Per disinstallare SSMA**  
  
1.  Nel Pannello di controllo aprire **Programmi e funzionalità**.  
  
2.  Selezionare **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per DB2**, quindi fare clic su **Rimuovi**.  
  
3.  Per confermare che si vuole disinstallare SSMA, fare clic su **Sì**.  
  
## <a name="uninstalling-the-extension-pack"></a>Disinstallazione del pacchetto di estensione  
Se si è certi che i database migrati non utilizzino oggetti nello schema di **sysdb.ssma_DB2** , è possibile rimuovere il pacchetto di estensione tramite l'eliminazione dallo schema, perché non è disponibile alcuna disinstallazione di Windows  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per il client DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-client-db2tosql.md)  
[Installazione dei componenti di SSMA in SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)  
  
