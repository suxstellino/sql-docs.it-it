---
description: Impostazioni globali (tester) (SybaseToSQL)
title: Impostazioni globali (tester) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 6f0b9cea-5a24-4e42-8bbf-c4516b00da23
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8f4584f8b4cb4cd7c8117a33e2894ad3f6382335
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078732"
---
# <a name="global-settings-tester-sybasetosql"></a>Impostazioni globali (tester) (SybaseToSQL)
Utilizzare la pagina tester della finestra di dialogo **Impostazioni globali** per specificare le impostazioni per SSMA tester.  
  
Per accedere alle impostazioni del tester, scegliere **Impostazioni globali** dal menu **strumenti** e fare clic su **tester** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
**Analisi oggetto testabile**  
Questa impostazione specifica se eseguire l'analisi degli oggetti testabili. Selezionare **Sì** se si vuole che SSMA tester analizzi e controlli automaticamente gli oggetti dipendenti. Il set di opzioni predefinito è **Sì**.  
  
Per questa impostazione sono disponibili le opzioni seguenti:  
  
1.  Sì  
  
2.  No  
  
**Modalità di salvataggio delle tabelle ausiliarie**  
Questa impostazione specifica come salvare le tabelle ausiliarie interne create durante l'esecuzione del test case. Per questa particolare impostazione è possibile impostare le opzioni seguenti:  
  
1.  Elimina sempre  
  
2.  Salva sempre  
  
3.  Salva se il confronto tabella non è riuscito  
  
4.  Richiedi all'utente se il confronto tra tabelle non è riuscito  
  
Il set di opzioni predefinito è: **Always Delete**.  
  
**Eseguire il rollback dei dati**  
Questa impostazione specifica se eseguire un'operazione di rollback dopo l'esecuzione di ogni test case. Il set di opzioni predefinito è **No**.  
  
Per questa impostazione sono disponibili le opzioni seguenti:  
  
1.  Sì  
  
2.  No  
  
**Interrompi esecuzione del test dopo il primo errore**  
Questa impostazione specifica se arrestare la test case corrente in esecuzione, se si è verificato un errore durante l'esecuzione. Il set di opzioni predefinito è **Sì**.  
  
Per questa impostazione sono disponibili le opzioni seguenti:  
  
1.  Sì  
  
2.  No  
  
## <a name="see-also"></a>Vedere anche  
[Completamento della preparazione del test case &#40;SybaseToSQL&#41;](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md)  
  
