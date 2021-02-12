---
description: Mapping dei tipi di dati MySQL e SQL Server (MySQLToSQL)
title: Mapping di tipi di dati MySQL e SQL Server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, customize data type mapping
- Mapping, Type mapping
ms.assetid: 14f98054-13b4-4231-a6b0-2452f3b9941d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f7024f944030714e7aa2431815e326b1be5be701
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077202"
---
# <a name="mapping-mysql-and-sql-server-data-types-mysqltosql"></a>Mapping dei tipi di dati MySQL e SQL Server (MySQLToSQL)
I tipi di database MySQL sono diversi da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o dai tipi di database SQL di Azure. Quando si convertono oggetti di database MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti o SQL Azure, è necessario specificare come eseguire il mapping dei tipi di dati da MySQL a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. È possibile accettare i mapping dei tipi di dati predefiniti oppure personalizzare i mapping come illustrato nelle procedure seguenti.  
  
## <a name="default-mappings"></a>Mapping predefiniti  
SSMA dispone di un set predefinito di mapping dei tipi di dati. Per l'elenco dei mapping predefiniti, vedere [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
## <a name="type-mapping-inheritance"></a>Ereditarietà del mapping dei tipi  
È possibile personalizzare i mapping dei tipi a livello di progetto, di categoria dell'oggetto, ad esempio tutte le stored procedure, o a livello di oggetto. Le impostazioni vengono ereditate dal livello superiore a meno che non vengano sostituite a un livello inferiore. Se, ad esempio, si esegue il mapping di **smallint** a **int** a livello di progetto, tutti gli oggetti del progetto utilizzeranno questo mapping a meno che non si Personalizza il mapping a livello di oggetto o di categoria.  
  
Quando si visualizza la scheda **mapping dei tipi** in SSMA, lo sfondo è codificato a colori per visualizzare i mapping dei tipi ereditati. Lo sfondo di un mapping dei tipi è giallo per qualsiasi mapping di tipi ereditati e bianco per qualsiasi mapping specificato al livello corrente.  
  
## <a name="customizing-data-type-mappings"></a>Personalizzazione dei mapping dei tipi di dati  
  
-   **Per eseguire il mapping dei tipi di dati:**  
  
    Nelle procedure riportate di seguito viene illustrato come eseguire il mapping dei tipi di dati a livello di progetto, database o oggetto di database:  
  
    1.  Per personalizzare il mapping dei tipi di dati per l'intero progetto, aprire la finestra di dialogo **Impostazioni progetto** . Scegliere **Impostazioni progetto** dal menu strumenti.  
  
        Nel riquadro sinistro selezionare mapping dei **tipi**. Il grafico del mapping dei tipi e i pulsanti vengono visualizzati nel riquadro di destra.  
  
    2.  Per personalizzare i mapping dei tipi di dati a livello di database o di tabella, selezionare il database o la tabella in MySQL Metadata Explorer. In MySQL Metadata Explorer selezionare la cartella o l'oggetto da personalizzare.  
  
        Nel riquadro di destra fare clic su **mapping dei tipi**.  
  
-   **Per aggiungere un nuovo mapping, eseguire le operazioni seguenti:**  
  
    1.  Nel riquadro mapping dei tipi fare clic su **Aggiungi** .  
  
    2.  Nella finestra di dialogo nuovo mapping dei tipi, in **tipo di origine**, selezionare il tipo di dati MySQL da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare le lunghezze minime e massime dei dati per il mapping selezionando le caselle di controllo **da** e **a** e quindi immettendo i valori.  
  
    4.  In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati. In **tipo di destinazione** selezionare il tipo di dati SQL Server o SQL Azure di destinazione.  
  
        1.  Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
        2.  Alcuni tipi richiedono una **precisione** e una **scala** del tipo di dati di destinazione. Se necessario, immettere la nuova precisione e la scala nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
-   **Per modificare un mapping dei tipi, eseguire le operazioni seguenti:**  
  
    1.  Nel riquadro mapping dei tipi, fare clic su **modifica**.  
  
    2.  Nella finestra di dialogo elenco mapping dei tipi, in **tipo di origine**, selezionare il tipo di dati MySQL da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare le lunghezze minime e massime dei dati per il mapping selezionando le caselle di controllo **da** e **a** e quindi immettendo i valori.  
  
    In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati. In **tipo di destinazione** selezionare il tipo di dati SQL Server o SQL Azure di destinazione.  
  
    -  Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
    -  Alcuni tipi richiedono una **precisione** e una **scala** del tipo di dati di destinazione. Se necessario, immettere la nuova precisione e la scala nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
-   **Per rimuovere un mapping dei tipi di dati, eseguire le operazioni seguenti:**  
  
    1.  Nel riquadro mapping dei tipi selezionare la riga nell'elenco mapping dei tipi che contiene il mapping del tipo di dati che si desidera rimuovere.  
  
    2.  Fare clic su **Rimuovi**.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [creare un report di valutazione](assessing-mysql-databases-for-conversion-mysqltosql.md) o [convertire gli oggetti di database MySQL in SQL Server o SQL Azure sintassi](converting-mysql-databases-mysqltosql.md). Se si crea un report, gli oggetti MySQL vengono convertiti automaticamente durante la valutazione.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
