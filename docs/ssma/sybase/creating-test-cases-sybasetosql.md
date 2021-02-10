---
description: Creazione di test case (SybaseToSQL)
title: Creazione di test case (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester component,Test Case Wizard
ms.assetid: b52dfd93-95af-4299-bc8f-83f2a7a6a518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 460ea84af51b872841eb82cd405cc29ebfe59e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056686"
---
# <a name="creating-test-cases-sybasetosql"></a>Creazione di test case (SybaseToSQL)
Utilizzare la procedura guidata test case per creare un test. Questa procedura guidata consente di creare test case scegliendo oggetti testati e verificati e specificando i parametri di test.  
  
## <a name="starting-the-test-case-wizard"></a>Avvio della procedura guidata test case  
Per avviare la procedura guidata test case, fare clic su **nuovo test case...** dal menu **tester** .  
  
All'avvio, la procedura guidata cerca il database ssmatester2005db o ssmatester2008db (a seconda del tipo di progetto) nel Server Sybase di origine. Si tratta dello schema di estensione del tester utilizzato per archiviare gli oggetti ausiliari. Se la procedura guidata test case non riesce a trovare ssmatester2005db o ssmatester2008db, viene visualizzata una finestra di dialogo che propone di creare il database di estensione del tester. Questa situazione si verifica in genere durante la prima esecuzione di SSMA tester.  
  
Se si ottiene la finestra di dialogo, fare clic su **Sì** per creare il database del tester Sybase nel server di origine. Verrà visualizzata una nuova finestra di dialogo in cui è necessario aggiungere uno o più dispositivi in cui trovare il nuovo database tester. Fare clic su **Aggiungi** per aggiungere un dispositivo. Nella finestra di dialogo **spazio di allocazione per database tester** scegliere il dispositivo e specificare le dimensioni usate dal database tester. Inoltre, è possibile impostare il dispositivo separato per i log del database. Si noti che è necessario disporre dei privilegi di Sybase per la creazione di database.  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>Panoramica della creazione di test case tramite la procedura guidata  
Il processo di creazione di un test case prevede cinque passaggi:  
  
1.  [Inizializzazione di test case &#40;&#41;SybaseToSQL ](../../ssma/sybase/initializing-test-cases-sybasetosql.md).  
  
2.  [Selezione e configurazione degli oggetti per testare &#40;&#41;SybaseToSQL ](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md).  
  
3.  [Selezione e configurazione degli oggetti interessati &#40;&#41;SybaseToSQL ](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md).  
  
4.  [Personalizzazione dell'ordine delle chiamate &#40;&#41;SybaseToSQL ](../../ssma/sybase/customizing-calls-order-sybasetosql.md).  
  
5.  [Completamento della preparazione del test Case &#40;&#41;SybaseToSQL ](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Test di oggetti di database migrati &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
