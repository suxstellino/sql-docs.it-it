---
description: Mapping dei tipi di dati di origine e di destinazione (AccessToSQL)
title: Mapping di tipi di dati di origine e di destinazione (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- customizing data type mappings
- data types, mapping
- mapping, data types
- source data types
- target data types
ms.assetid: b362a075-16e7-423f-b63f-e1e9f02844a9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f5a725d0e2c6d9651884c399caabe2a8833826f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059050"
---
# <a name="mapping-source-and-target-data-types-accesstosql"></a>Mapping dei tipi di dati di origine e di destinazione (AccessToSQL)
I tipi di database di Access variano a seconda dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di database. Quando si converte gli oggetti di database di Access in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti, è necessario specificare come eseguire il mapping dei tipi di dati da Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile accettare i mapping dei tipi di dati predefiniti oppure personalizzare i mapping come illustrato nelle procedure seguenti.  
  
## <a name="default-mappings"></a>Mapping predefiniti  
SSMA dispone di un set predefinito di mapping dei tipi di dati. Per l'elenco dei mapping predefiniti, vedere [Impostazioni progetto (mapping dei tipi)](./project-settings-type-mapping-accesstosql.md).  
  
## <a name="customizing-data-type-mappings"></a>Personalizzazione dei mapping dei tipi di dati  
Utilizzando la finestra di dialogo **Impostazioni progetto** , è possibile personalizzare la modalità di mapping dei tipi per tutti i database e gli oggetti di database in un progetto. I mapping dei tipi per un progetto si applicano a tutti i database e gli oggetti di database che non dispongono di mapping di tipi personalizzati.  
  
È inoltre possibile personalizzare il mapping dei tipi di dati a livello di database o di tabella.  
  
Nella procedura riportata di seguito viene illustrato come eseguire il mapping dei tipi di dati a livello di progetto, database o oggetto di database.  
  
**Per eseguire il mapping dei tipi di dati**  
  
1.  Per personalizzare il mapping dei tipi di dati per l'intero progetto, aprire la finestra di dialogo **Impostazioni progetto** :  
  
    1.  Scegliere **Impostazioni progetto** dal menu **strumenti** .  
  
    2.  Nel riquadro sinistro selezionare mapping dei **tipi**.  
  
        Il grafico del mapping dei tipi e i pulsanti vengono visualizzati nel riquadro di destra.  
  
    In alternativa, per personalizzare il mapping dei tipi di dati a livello di database o di tabella, selezionare il database o la tabella nel riquadro Esplora metadati di Access:  
  
    1.  Nel riquadro accedi a Esplora Metadati espandere **Access-metabase**, quindi espandere **database**.  
  
    2.  Consente di selezionare il database o la tabella per cui si desidera personalizzare il mapping dei tipi di dati.  
  
    3.  Nel riquadro di destra fare clic su **mapping dei tipi**.  
  
2.  Per aggiungere un nuovo mapping, eseguire le operazioni seguenti:  
  
    1.  Nel riquadro mapping dei tipi fare clic su **Aggiungi**.  
  
    2.  Nella finestra di dialogo **nuovo mapping dei tipi** , in **tipo di origine**, selezionare il tipo di dati di accesso da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare le lunghezze minime e massime dei dati per il mapping selezionando le caselle di controllo **da** e **a** e quindi immettendo i valori.  
  
        In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati.  
  
    4.  In **tipo di destinazione** selezionare il tipo di dati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
3.  Per modificare un mapping dei tipi di dati, eseguire le operazioni seguenti:  
  
    1.  Nel riquadro mapping dei tipi, fare clic su **modifica**.  
  
    2.  Nella finestra di dialogo **elenco mapping dei tipi** , in **tipo di origine**, selezionare il tipo di dati di accesso da mappare.  
  
    3.  Se il tipo richiede una lunghezza, specificare le lunghezze minime e massime dei dati per il mapping selezionando le caselle di controllo **da** e **a** e quindi immettendo i valori.  
  
        In questo modo è possibile personalizzare il mapping dei dati per valori più piccoli e più grandi dello stesso tipo di dati.  
  
    4.  In **tipo di destinazione** selezionare il tipo di dati di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        Alcuni tipi richiedono una lunghezza del tipo di dati di destinazione. Se necessario, immettere la nuova lunghezza dei dati nella casella **Sostituisci con** , quindi fare clic su **OK**.  
  
4.  Per rimuovere un mapping dei tipi di dati, eseguire le operazioni seguenti:  
  
    1.  Nel riquadro mapping dei tipi selezionare la riga nell'elenco mapping dei tipi che contiene il mapping del tipo di dati che si desidera rimuovere.  
  
    2.  Fare clic su **Rimuovi**.  
  
## <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo del processo di migrazione consiste nel [convertire gli oggetti di database di Access in oggetti SQL Server](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
