---
description: Test di oggetti di database migrati (SybaseToSQL)
title: Test di oggetti di database migrati (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4937f6b4-86bd-4070-88df-3d216306c33a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f889538e5232eedaf3bab8006c54d56e1649ea4c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014791"
---
# <a name="testing-migrated-database-objects-sybasetosql"></a>Test di oggetti di database migrati (SybaseToSQL)
Microsoft SQL Server Migration Assistant per Sybase tester (SSMA tester) verifica automaticamente la conversione degli oggetti di database e la migrazione dei dati eseguita da SSMA. Al termine di tutti i passaggi della migrazione di SSMA, utilizzare SSMA tester per verificare che gli oggetti convertiti funzionino allo stesso modo e che tutti i dati siano stati trasferiti correttamente.  
  
> [!NOTE]  
> Il componente tester è disabilitato nel caso di connettività di Azure.  
  
Con SSMA tester è possibile testare i tipi di oggetti seguenti:  
  
-   Tabelle  
  
-   Stored procedure  
  
-   Visualizzazioni.  
  
-   Istruzioni autonome.  
  
SSMA tester esegue gli oggetti selezionati per il test in Sybase e le relative controparti in SQL Server. In seguito, i risultati vengono confrontati in base ai criteri seguenti:  
  
-   Le modifiche ai dati della tabella sono identiche?  
  
-   I valori dei parametri di output per le procedure e le funzioni sono identici?  
  
-   Le funzioni restituiscono gli stessi risultati?  
  
-   I set di risultati sono identici?  
  
> [!NOTE]  
> Attenzione Non usare mai SSMA tester nei sistemi di produzione. Durante l'esecuzione del tester, lo schema di origine e i dati vengono modificati. Nel frattempo, il ripristino completo dello stato originale potrebbe non essere possibile per alcuni tipi di codice testato.  
  
## <a name="prerequisites"></a>Prerequisiti  
Se si vuole usare SSMA tester, installare SSMA Sybase Extension Pack con l'opzione **Installa database tester** attivata.  
  
Verificare inoltre quanto segue:  
  
-   Sybase OLE DB provider è installato nel computer in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito.  
  
-   Nell'motore di database è stata abilitata l'integrazione con CLR (Common Language Runtime) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Si noti che la versione corrente di SSMA tester non supporta l'esecuzione parallela da parte di utenti diversi nello stesso server di origine o di destinazione.  
  
## <a name="getting-started"></a>Introduzione  
[Creazione di test case &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-test-cases-sybasetosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Installazione dei componenti di SSMA in SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
[Impostazioni progetto &#40;conversione&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
