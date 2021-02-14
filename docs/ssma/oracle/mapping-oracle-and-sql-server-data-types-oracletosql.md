---
title: Mapping di tipi di dati Oracle e SQL Server (OracleToSQL) | Microsoft Docs
description: Informazioni su come personalizzare SSMA per i mapping Oracle tra i tipi di dati Oracle e SQL Server o accettare il valore predefinito.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8058123020571ad43e3142fccb920aa22a4b5af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080782"
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>Mapping di tipi di dati Oracle e SQL Server (OracleToSQL)
I tipi di database Oracle sono diversi da quelli dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di database. Quando si convertono oggetti di database Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti, è necessario specificare come eseguire il mapping dei tipi di dati da Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile accettare i mapping dei tipi di dati predefiniti oppure personalizzare i mapping, come illustrato nelle sezioni seguenti.  
  
## <a name="default-mappings"></a>Mapping predefiniti  
SSMA dispone di un set predefinito di mapping dei tipi di dati. Per l'elenco dei mapping predefiniti, vedere [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
## <a name="type-mapping-inheritance"></a>Ereditarietà del mapping dei tipi  
È possibile personalizzare i mapping dei tipi a livello di progetto, di categoria dell'oggetto, ad esempio tutte le stored procedure, o a livello di oggetto. Le impostazioni vengono ereditate dal livello superiore a meno che non vengano sostituite a un livello inferiore. Se, ad esempio, si esegue il mapping di **smallmoney** a **Money** a livello di progetto, tutti gli oggetti del progetto utilizzeranno questo mapping a meno che non si Personalizza il mapping a livello di oggetto o di categoria.  
  
Quando si visualizza la scheda **mapping dei tipi** in SSMA, lo sfondo è codificato a colori per visualizzare i mapping dei tipi ereditati. Lo sfondo di un mapping dei tipi è giallo per qualsiasi mapping di tipi ereditati e bianco per qualsiasi mapping specificato al livello corrente.  
  
## <a name="customizing-data-type-mappings"></a>Personalizzazione dei mapping dei tipi di dati  
Nella procedura seguente viene illustrato come eseguire il mapping dei tipi di dati a livello di progetto, di database o di oggetto:  
  
**Per eseguire il mapping dei tipi di dati**  
  
1.  Per personalizzare il mapping dei tipi di dati per l'intero progetto, aprire la finestra di dialogo **Impostazioni progetto** :  
  
    1.  Scegliere **Impostazioni progetto** dal menu **strumenti** .  
  
    2.  Nel riquadro sinistro selezionare mapping dei **tipi**.  
  
        Il grafico del mapping dei tipi e i pulsanti vengono visualizzati nel riquadro di destra.  
  
    In alternativa, per personalizzare il mapping dei tipi di dati a livello di database, tabella, vista o stored procedure, selezionare il database, la categoria dell'oggetto o l'oggetto in Esplora metadati Oracle:  
  
    1.  In Oracle Metadata Explorer selezionare la cartella o l'oggetto da personalizzare.  
  
    2.  Nel riquadro di destra fare clic sulla scheda **mapping dei tipi** .  
  
2.  Per aggiungere un nuovo mapping, eseguire le operazioni seguenti:  
  
    1.  Fare clic su **Aggiungi**.  
  
    2.  In **tipo di origine** selezionare il tipo di dati Oracle da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare la lunghezza minima dei dati per il mapping nella casella **da** e la lunghezza massima dei dati nella casella **a** .  
  
        In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati.  
  
    4.  In **tipo di destinazione** selezionare il tipo di dati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** .  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Per modificare un mapping dei tipi di dati, eseguire le operazioni seguenti:  
  
    1.  Fare clic su **Modifica**.  
  
    2.  In **tipo di origine** selezionare il tipo di dati Oracle da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare la lunghezza minima dei dati per il mapping nella casella **da** e la lunghezza massima dei dati nella casella **a** .  
  
        In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati.  
  
    4.  In **tipo di destinazione** selezionare il tipo di dati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** , quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Per rimuovere un mapping del tipo di dati personalizzato, eseguire le operazioni seguenti:  
  
    1.  Selezionare la riga nell'elenco mapping dei tipi che contiene il mapping del tipo di dati che si desidera rimuovere.  
  
    2.  Fare clic su **Rimuovi**.  
  
        Non è possibile rimuovere i mapping ereditati. Tuttavia, i mapping ereditati vengono sottoposti a override da mapping personalizzati in un oggetto o una categoria di oggetti specifici.  
  
## <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo del processo di migrazione consiste nel [creare un report di valutazione](assessing-oracle-schemas-for-conversion-oracletosql.md) o [convertire gli oggetti di database Oracle nella sintassi SQL Server](converting-oracle-schemas-oracletosql.md). Se si crea un report di valutazione, gli oggetti Oracle vengono convertiti automaticamente durante la valutazione.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
