---
description: Completamento della preparazione del test case (SybaseToSQL)
title: Completamento della preparazione del test case (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Case Settings
ms.assetid: 8b2a49b0-4296-4f3f-9e56-323aa6a6fa8e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 0c1f34f19034042d542cba2d81a55bf32288058e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067519"
---
# <a name="finishing-test-case-preparation-sybasetosql"></a>Completamento della preparazione del test case (SybaseToSQL)
Nella pagina finale della procedura guidata vengono visualizzate la descrizione del test case e le informazioni sugli oggetti che coinvolgono il test. Inoltre, in questa pagina è possibile impostare le opzioni di esecuzione dei test.  
  
Nella sezione **informazioni sul test case** vengono visualizzati il nome e la descrizione del test case.  
  
La sezione **oggetti di test** contiene l'elenco denominato di oggetti testati raggruppati per tipo di oggetto.  
  
La sezione **oggetti interessati da analizzare** Visualizza l'elenco denominato di oggetti che devono essere confrontati con le modifiche dei dati dopo l'esecuzione di oggetti testati.  
  
## <a name="test-case-settings"></a>Impostazioni test case  
Nella sezione **impostazioni test case** è possibile impostare le opzioni di test di esecuzione seguenti:  
  
### <a name="stop-test-execution-after-first-failure"></a>Interrompi esecuzione del test dopo il primo errore  
Specifica di interrompere il test se si verifica un errore durante l'esecuzione del test.  
  
-   Se si sceglie **Sì**, l'esecuzione del test si interrompe se si verifica un errore.  
  
-   Se si sceglie **No**, l'esecuzione del test continuerà dopo un errore.  
  
### <a name="perform-data-rollback"></a>Eseguire il rollback dei dati  
Abilitare il rollback automatico dei dati dopo l'esecuzione del test.  
  
-   Se si sceglie **Sì**, le modifiche ai dati andranno perse dopo l'esecuzione del test.  
  
-   Se si sceglie **No**, verranno salvate tutte le modifiche apportate ai dati di esecuzione dei test.  
  
### <a name="auxiliary-tables-saving-mode"></a>Modalità di salvataggio delle tabelle ausiliarie  
Definisce la modalità di salvataggio per le tabelle ausiliarie create durante l'esecuzione del test. Vedere la descrizione delle tabelle ausiliarie nell'argomento [esecuzione di test case &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md) .  
  
-   Se si seleziona **Always save**, i dati della tabella ausiliaria verranno sempre archiviati per un uso successivo.  
  
-   Se si seleziona **Salva se il confronto tra tabelle non è riuscito**, i dati della tabella ausiliaria verranno archiviati solo se si verifica un errore.  
  
-   Se si seleziona **Always Delete**, le tabelle ausiliarie verranno sempre eliminate dopo l'esecuzione del test.  
  
-   Se si seleziona **Richiedi utente se il confronto tra tabelle non è riuscito**, l'utente può selezionare l'azione necessaria se si verifica un errore.  
  
Fare clic sul pulsante **fine** per salvare il test case preparato in [utilizzando i repository di test &#40;SybaseToSQL&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Uso di repository di test &#40;SybaseToSQL&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md)  
[Esecuzione di test case &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Test di oggetti di database migrati &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
