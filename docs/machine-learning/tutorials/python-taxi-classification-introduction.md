---
title: 'Esercitazione su Python: Prevedere le tariffe dei taxi di NYC con la classificazione binaria'
titleSuffix: SQL machine learning
description: Informazioni su come incorporare il codice Python nelle stored procedure di SQL Server e nelle funzioni T-SQL con il Machine Learning di SQL per prevedere le tariffe dei taxi di NYC usando la classificazione binaria.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/28/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 103eaec3991e9c4f00f819ca2811f395bfbeb2b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339668"
---
# <a name="python-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Esercitazione su Python: Prevedere le tariffe dei taxi di NYC con la classificazione binaria
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In questa serie di esercitazioni in cinque parti per i programmatori SQL, verranno fornite informazioni sull'integrazione di Python in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o nei [cluster Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017"
In questa serie di esercitazioni in cinque parti per i programmatori SQL verranno fornite informazioni sull'integrazione di Python in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
In questa serie di esercitazioni in cinque parti per i programmatori SQL verranno fornite informazioni sull'integrazione di Python in [Machine Learning Services in Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Verrà creata e distribuita una soluzione di Machine Learning basata su Python usando un database di esempio in SQL Server. Verrà usato T-SQL, Azure Data Studio o SQL Server Management Studio e un'istanza del database con il Machine Learning di SQL e il supporto del linguaggio Python.

Questa serie di esercitazioni presenta le funzioni Python usate in un flusso di lavoro di modellazione dei dati. Le parti includono l'esplorazione dei dati, la creazione e il training di un modello di classificazione binaria e la distribuzione del modello. Si useranno dati di esempio di New York City Taxi e Limousine Commission. Il modello che verrà compilato consente di prevedere se è probabile che per una corsa venga lasciata una mancia, in base all'ora del giorno, alla distanza percorsa e al luogo di partenza della corsa.

Nella prima parte di questa serie verranno installati i prerequisiti e verrà ripristinato il database di esempio. Nelle seconda e nella terza parte verranno sviluppati alcuni script Python per preparare i dati ed eseguire il training di un modello di Machine Learning. Nella quarta e quinta parte verranno quindi eseguiti gli script Python all'interno del database usando stored procedure T-SQL.

Contenuto dell'articolo:

> [!div class="checklist"]
> + Installare i prerequisiti
> + Ripristinare il database di esempio

Nella [seconda parte](python-taxi-classification-explore-data.md) verranno esaminati i dati di esempio e verranno generati alcuni tracciati.

Nella [terza parte](python-taxi-classification-create-features.md) si apprenderà come creare funzionalità dai dati non elaborati tramite una funzione Transact-SQL. Tale funzione verrà quindi chiamata da una stored procedure per creare una tabella contenente i valori della funzionalità.

Nella [quarta parte](python-taxi-classification-train-model.md) verranno caricati i moduli e verranno chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server.

Nella [quinta parte](python-taxi-classification-deploy-model.md) si apprenderà come rendere operativi i modelli sottoposti a training e salvati nella quarta parte.

> [!NOTE]
> Questa esercitazione è disponibile sia in R che in Python. Per la versione R, vedere [Esercitazione su R: Prevedere le tariffe dei taxi di NYC con la classificazione binaria](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
+ Installare [SQL Server Machine Learning Services con Python](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ [Concedere le autorizzazioni per l'esecuzione di script Python](../security/user-permission.md)

+ Ripristinare il [database demo di NYC Taxi](demo-data-nyctaxi-in-sql.md)

Tutte le attività possono essere eseguite usando stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] in Azure Data Studio o [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

Questa serie di esercitazioni presuppone una certa familiarità con le operazioni di database di base, ad esempio la creazione di database e tabelle, l'importazione di dati e la scrittura di query SQL. Non si presuppone che l'utente abbia familiarità con il linguaggio Python. Viene fornito tutto il codice Python necessario.

## <a name="background-for-sql-developers"></a>Background per sviluppatori SQL

Il processo di creazione di una soluzione di Machine Learning è complesso e può richiedere l'uso di più strumenti e il coordinamento di esperti in materia in diverse fasi:

+ recupero e pulizia dei dati
+ esplorazione dei dati e creazione di caratteristiche utili per la modellazione
+ training e ottimizzazione del modello
+ distribuzione nell'ambiente di produzione

Per lo sviluppo e i test del codice effettivo è opportuno usare un ambiente di sviluppo dedicato. Dopo che lo script è stato testato, è tuttavia possibile distribuirlo facilmente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] nell'ambiente familiare di Azure Data Studio o [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Il wrapping del codice esterno nelle stored procedure è il meccanismo principale per rendere operativo il codice in SQL Server.

Dopo aver salvato il modello nel database, è possibile chiamarlo per eseguire stime da [!INCLUDE[tsql](../../includes/tsql-md.md)] usando le stored procedure.

Questa serie di esercitazioni in cinque parti presenta un flusso di lavoro tipico per l'esecuzione di analisi nel database con Python e SQL Server ed è rivolta a programmatori SQL che non hanno familiarità con Python o sviluppatori Python che non hanno familiarità con SQL.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Installare i prerequisiti
> + Ripristinare il database di esempio

> [!div class="nextstepaction"]
> [Esercitazione su Python: Esplorare e visualizzare i dati](python-taxi-classification-explore-data.md)