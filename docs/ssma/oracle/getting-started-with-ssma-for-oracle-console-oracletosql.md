---
description: Introduzione a SSMA per la console Oracle (OracleToSQL)
title: Introduzione con SSMA per la console Oracle (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Console, Console Output Conventions
- Oracle Console, Launching Console
ms.assetid: 667a5e4a-6848-4973-a72d-1287f64718ac
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: fd87502c1eda3dc8d4caf4c654804b164f24f36f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068046"
---
# <a name="getting-started-with-ssma--for-oracle-console-oracletosql"></a>Introduzione a SSMA per la console Oracle (OracleToSQL)
In questa sezione viene descritta la procedura per avviare e iniziare a usare l'applicazione console Oracle. Sono inoltre elencate le convenzioni usate in una tipica finestra di output della console di SSMA.  
  
## <a name="launching-ssma-console"></a>Avvio della console SSMA  
Per avviare l'applicazione console SSMA, attenersi alla procedura seguente:  
  
1.  Passare a **Start** e puntare a **tutti i programmi**.  
  
2.  Fare clic sul collegamento del **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prompt dei comandi Migration Assistant per Oracle** .  
  
    Viene visualizzato il menu di utilizzo della console SSMA e, che consente di iniziare a `(/? Help)` usare l'applicazione console.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procedura per l'uso della console di SSMA  
Dopo che la console è stata avviata correttamente nel sistema Windows, è possibile utilizzare la procedura seguente per lavorare su di essa:  
  
1.  Configurare la console SSMA tramite i file di script. Per ulteriori informazioni su questa sezione, vedere [creazione di file di Script &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md) .  
  
2.  [Creazione di file di valori di variabile &#40;OracleToSQL&#41;](../../ssma/oracle/creating-variable-value-files-oracletosql.md)  
  
3.  [Creazione dei file di connessione del server &#40;OracleToSQL&#41;](../../ssma/oracle/creating-the-server-connection-files-oracletosql.md)  
  
4.  [Esecuzione della console SSMA &#40;OracleToSQL&#41;](../../ssma/oracle/executing-the-ssma-console-oracletosql.md) in base alle esigenze del progetto  
  
Altre funzionalità:  
  
1.  [Specificare una password](managing-passwords-oracletosql.md) e esportarla/importarla in altri computer Windows  
  
2.  [Generare report](generating-reports-oracletosql.md) per visualizzare i report di output XML dettagliati per la valutazione/conversion e la migrazione dei dati. È anche possibile generare report di errore dettagliati per i comandi di aggiornamento e sincronizzazione.  
  
## <a name="ssma-console-output-conventions"></a>Convenzioni di output della console SSMA  
Quando si eseguono i comandi e le opzioni di script di SSMA, il programma console Visualizza i risultati e i messaggi (informazioni, errore e così via) per l'utente nella console o, se necessario, reindirizza a un file di output XML. Ogni tipo di messaggio nell'output è identificato da un colore univoco. Ad esempio, il messaggio di testo in colore bianco denota i comandi del file di script; quello in verde rappresenta una richiesta per l'input dell'utente e così via.  
  
![SSMAConsoleOutput_Oracle](../../ssma/db2/media/ssmaconsoleoutput_oracle.jpg "SSMAConsoleOutput_Oracle")  
  
Interpretazione dei colori dell'output della console nella tabella seguente:  
  
|Colore|Descrizione|  
|---------|---------------|  
|Red|Errore irreversibile durante l'esecuzione|  
|Grigio|Indicatore di data e ora, messaggio all'utente|  
|White|Comandi file script, tipo di messaggio|  
|Giallo|Avviso|  
|Green|Richiedi input utente|  
|azzurro|Inizio, fine e risultato di un'operazione|  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per Oracle](installing-ssma-for-oracle-oracletosql.md)  
  
