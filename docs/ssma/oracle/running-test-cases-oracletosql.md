---
description: Esecuzione di test case (OracleToSQL)
title: Esecuzione di test case (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: fc208cdb-7373-4f6b-8f6c-cdff9d3dcd02
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: bc811575dd845be43aef7a9398d8b4fb3458db1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067716"
---
# <a name="running-test-cases-oracletosql"></a>Esecuzione di test case (OracleToSQL)
Quando SSMA tester esegue un test case, esegue gli oggetti selezionati per il test e crea un report sui risultati della verifica. Se i risultati sono identici su entrambe le piattaforme, il test ha avuto esito positivo. La corrispondenza degli oggetti tra Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene determinata in base alle impostazioni di mapping dello schema per il progetto SSMA corrente.  
  
Un requisito necessario per un test riuscito è che tutti gli oggetti Oracle vengono convertiti e caricati nel database di destinazione. Inoltre, è necessario eseguire la migrazione dei dati della tabella in modo da sincronizzare il contenuto delle tabelle in entrambe le piattaforme.  
  
## <a name="run-test-case"></a>Esegui test case  
Per eseguire il test case preparato:  
  
1.  Fare clic sul pulsante **Esegui**.  
  
2.  Nella finestra di dialogo **Connetti a Oracle** immettere le informazioni di connessione e quindi fare clic su **Connetti**.  
  
Al termine del test, viene creato il report del test case. Fare clic sul pulsante **report** per visualizzare il [report del test case](viewing-test-case-reports-oracletosql.md). Il risultato del test (report test case) viene archiviato automaticamente nel [Repository risultati test](using-test-repositories-oracletosql.md) per un uso successivo.  
  
## <a name="test-case-execution-steps"></a>Passaggi di esecuzione del test case  
  
### <a name="prerequisites"></a>Prerequisiti  
SSMA tester verifica se tutti i prerequisiti sono soddisfatti per l'esecuzione dei test prima dell'avvio del test. Se alcune condizioni non vengono soddisfatte, viene visualizzato un messaggio di errore.  
  
### <a name="initialization"></a>Inizializzazione  
In questo passaggio SSMA tester crea oggetti ausiliari (tabelle, trigger e visualizzazioni) nello schema di SSMATESTER_ORACLE del server Oracle. Consentono la traccia delle modifiche apportate agli oggetti interessati scelti per la verifica.  
  
Si supponga che la tabella verificata sia denominata USER_TABLE. Per una tabella di questo tipo, vengono creati gli oggetti ausiliari seguenti in Oracle.  
  
|Nome|Tipo|Descrizione|  
|-|-|-|  
|USER_TABLE $ Trg|trigger|Attivare il controllo delle modifiche nella tabella verificata.|  
|USER_TABLE $ AUD|table|Tabella in cui vengono salvate le righe eliminate e sovrascritte.|  
|USER_TABLE $ AUDID|table|Tabella in cui vengono salvate le righe nuove e modificate.|  
|USER_TABLE|vista|Rappresentazione semplificata delle modifiche della tabella.|  
|USER_TABLE $ NEW|vista|Rappresentazione semplificata delle righe inserite e sovrascritte.|  
|USER_TABLE $ NEW_ID|vista|Identificazione delle righe inserite e modificate.|  
|USER_TABLE $ OLD|vista|Rappresentazione semplificata delle righe eliminate e sovrascritte.|  
  
L'oggetto seguente viene creato nello schema della tabella verificata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nome|Tipo|Descrizione|  
|-|-|-|  
|USER_TABLE $ Trg|trigger|Attivare il controllo delle modifiche nella tabella verificata.|  
  
E gli oggetti seguenti vengono creati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel database ssmatesterdb.  
  
|Nome|Tipo|Descrizione|  
|-|-|-|  
|USER_TABLE $ AUD|table|Tabella in cui vengono salvate le righe eliminate e sovrascritte.|  
|USER_TABLE $ AudId|table|Tabella in cui vengono salvate le righe nuove e modificate.|  
|USER_TABLE|vista|Rappresentazione semplificata delle modifiche della tabella.|  
|USER_TABLE $ New|vista|Rappresentazione semplificata delle righe inserite e sovrascritte.|  
|USER_TABLE $ new_id|vista|Identificazione delle righe inserite e modificate.|  
|USER_TABLE $ Old|vista|Rappresentazione semplificata delle righe eliminate e sovrascritte.|  
  
### <a name="test-object-calls"></a>Test delle chiamate all'oggetto  
In questo passaggio, SSMA tester richiama ogni oggetto selezionato per il test, confronta i risultati e visualizza il report.  
  
### <a name="finalization"></a>Finalizzazione  
Durante la finalizzazione, SSMA tester pulisce gli oggetti ausiliari creati al passaggio di **inizializzazione** .  
  
## <a name="next-step"></a>passaggio successivo  
[Visualizzazione dei report del test case &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Selezione e configurazione degli oggetti da testare &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql.md)  
[Selezione e configurazione degli oggetti interessati &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
[Test di oggetti di database migrati &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
