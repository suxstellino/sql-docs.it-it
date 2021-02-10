---
title: Aggiunta e rimozione di file di database di Access (AccessToSQL) | Microsoft Docs
description: Informazioni su come aggiungere o rimuovere database di Access da o verso il progetto SSMA per eseguire la migrazione dei dati di accesso a SQL Server o al database SQL di Azure.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- adding Access files
- adding and removing Access databases
- browsing Access metadata
- browsing metadata, Access databases
- connecting, Access databases
- databases
- files, adding and removing
- finding Access databases
- finding database files
- locating database files
- metadata
- metadata, browsing
- metadata, refreshing
- refreshing metadata
- removing Access files
- scanning for database files
- searching for database files
ms.assetid: e944c740-4c8a-4bc1-b0ed-be57bc06dced
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d8443eaceb150bd0cf2c936e44cb29fc2292f89c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042033"
---
# <a name="adding-and-removing-access-database-files-accesstosql"></a>Aggiunta e rimozione di file di database di Access (AccessToSQL)
Per eseguire la migrazione dei dati di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è necessario aggiungere uno o più database di Access al progetto SSMA. Questi database devono essere Access 97 o versioni successive. Se si dispone di database di una versione precedente di Access, è necessario convertire i database in una versione più recente. A tale scopo, aprire e salvare i database in Access 97 o versioni successive prima di aggiungerli a SSMA.  
  
## <a name="what-happens-when-you-add-access-database-files"></a>Cosa accade quando si aggiungono i file di database di Access?  
Quando si aggiunge un database di Access a un progetto SSMA, SSMA legge i metadati del database, quindi aggiunge i metadati al file di progetto. Questi metadati descrivono il database e i relativi oggetti. SSMA usa i metadati quando converte gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure la sintassi e quando esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. È possibile esplorare questi metadati in Esplora metadati di Access ed esaminare le proprietà dei singoli oggetti di database.  
  
> [!NOTE]  
> Un database di Access può essere suddiviso in più file: un database back-end che contiene tabelle e database front-end che contengono query, moduli, report, macro, moduli e collegamenti. Se si desidera eseguire la migrazione di un database suddiviso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, aggiungere il database front-end a SSMA.  
  
## <a name="permissions-that-are-required-by-ssma"></a>Autorizzazioni richieste da SSMA  
Per eseguire la migrazione di un database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, il gruppo Users e l'utente amministratore devono disporre delle autorizzazioni di amministrazione. Per informazioni su come eseguire la migrazione dei database con la protezione del gruppo di lavoro, vedere [preparazione dei database di Access per la migrazione](preparing-access-databases-for-migration-accesstosql.md).  
  
## <a name="selecting-databases-to-add"></a>Selezione dei database da aggiungere  
Se si desidera aggiungere uno o più database a un progetto SSMA e i file si trovano in un percorso noto, è possibile aggiungere i file utilizzando la procedura seguente.  
  
**Per aggiungere singoli file di database**  
  
1.  Scegliere **Aggiungi database** dal menu **file** .  
  
2.  Nella finestra di dialogo **Apri** individuare la cartella che contiene il file o i file di database.  
  
3.  Selezionare i file che si desidera aggiungere, quindi fare clic su **Apri**.  
  
## <a name="finding-databases-to-add"></a>Ricerca di database da aggiungere  
Se si desidera aggiungere più database di Access da cartelle diverse a un progetto SSMA o si desidera aggiungere un singolo file ma è necessario trovare il file, è possibile eseguire la procedura seguente per individuare uno o più file e aggiungerli al progetto.  
  
**Per trovare e aggiungere database**  
  
1.  Scegliere **trova database** dal menu **file** .  
  
2.  Nella procedura guidata trova database immettere il nome dell'unità, il percorso del file o il percorso UNC in cui si desidera eseguire la ricerca. In alternativa, fare clic su **Sfoglia** per individuare l'unità o la cartella di rete.  
  
3.  Fare clic su **Aggiungi** per aggiungere il percorso all'elenco.  
  
    Ripetere i due passaggi precedenti per aggiungere ulteriori percorsi di ricerca.  
  
4.  Facoltativamente, aggiungere i criteri di ricerca per ridefinire l'elenco dei database restituiti.  
  
    > [!IMPORTANT]  
    > La casella **di testo nome file o parte intera** non supporta caratteri jolly.  
  
5.  Fai clic su **Analisi**.  
  
    Viene visualizzata la pagina di analisi. Vengono visualizzati i database che sono stati trovati e lo stato di avanzamento della ricerca. Per arrestare la ricerca, fare clic su **Arresta**.  
  
6.  Nella pagina Seleziona file selezionare i database che si desidera aggiungere al progetto.  
  
    È possibile utilizzare i pulsanti **Seleziona tutto** e **Cancella tutto** nella parte superiore dell'elenco per selezionare o deselezionare tutti i database. È possibile tenere premuto il tasto CTRL per selezionare più database oppure tenere premuto MAIUSC per selezionare un intervallo di database.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina verifica fare clic su **fine**.  
  
## <a name="browsing-access-metadata"></a>Esplorazione dei metadati di accesso  
Dopo avere aggiunto un database di Access a un progetto, i metadati del progetto vengono visualizzati in Esplora metadati di Access. È possibile esplorare la gerarchia dei database e degli oggetti di database in Esplora.  
  
**Per esplorare i metadati**  
  
1.  In Esplora metadati di Access espandere **Access-metabase**, quindi espandere **database**.  
  
2.  Espandere il database che si desidera esaminare, quindi espandere **query**.  
  
    Si noti l'elenco delle query. Se si seleziona una query, nel riquadro destro vengono visualizzate una scheda **SQL** e una scheda **Proprietà** .  
  
3.  Espandere **tabelle** e quindi selezionare una tabella.  
  
    Si noti che vengono visualizzate quattro schede: **tabella**, **mapping dei tipi**, **proprietà** e **dati**.  
  
4.  Espandere una tabella, espandere **chiavi**, quindi selezionare una chiave.  
  
    Le proprietà chiave vengono visualizzate nel riquadro di destra.  
  
5.  Espandere **indici**, quindi selezionare un indice.  
  
    Le proprietà dell'indice vengono visualizzate nel riquadro di destra.  
  
## <a name="refreshing-databases"></a>Aggiornamento dei database  
Se un database di Access viene modificato dopo l'aggiunta del file, è possibile aggiornare i metadati dal database di Access.  
  
**Per aggiornare i metadati di accesso**  
  
-   In Accedi a Esplora metadati fare clic con il pulsante destro del mouse sul database, quindi scegliere **Aggiorna da database**.  
  
## <a name="removing-databases"></a>Rimozione di database  
È possibile rimuovere un database di Access da un progetto seguendo questa procedura.  
  
**Per rimuovere un database da un progetto**  
  
1.  In Esplora metadati di Access espandere **Access-metabase**, quindi espandere **database**.  
  
2.  Fare clic con il pulsante destro del mouse sul database, quindi scegliere **Rimuovi database**.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [connettersi a SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Creazione e gestione di progetti](creating-and-managing-projects-accesstosql.md)  
