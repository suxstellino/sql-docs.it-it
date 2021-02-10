---
description: Nozioni fondamentali su ADO
title: Nozioni fondamentali su ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: d6a66928-e68f-4c38-b87a-838c5de50a28
author: rothja
ms.author: jroth
ms.openlocfilehash: fdf7580f3a04a15fd75fb6d2c7712a694dfd2175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033761"
---
# <a name="ado-fundamentals"></a>Nozioni fondamentali su ADO
ADO fornisce agli sviluppatori un modello a oggetti logico e potente per accedere a livello di codice, modificare e aggiornare i dati da un'ampia gamma di origini dati tramite OLE DB interfacce di sistema. L'utilizzo più comune di ADO consiste nell'eseguire una query su una tabella o tabelle in un database relazionale, recuperare e visualizzare i risultati in un'applicazione e, forse, consentire agli utenti di apportare e salvare le modifiche ai dati. Di seguito sono riportate altre attività:  
  
-   Esecuzione di query su un database tramite SQL e visualizzazione dei risultati.  
  
-   Accesso alle informazioni in un archivio di file su Internet.  
  
-   Manipolazione di messaggi e cartelle in un sistema di posta elettronica.  
  
-   Salvataggio di dati da un database in un file XML.  
  
-   Esecuzione di comandi descritti con XML e recupero di un flusso XML.  
  
-   Salvataggio di dati in un flusso binario o XML.  
  
-   Consentire a un utente di esaminare e modificare i dati nelle tabelle di database.  
  
-   Creazione e riutilizzo di comandi di database con parametri.  
  
-   Esecuzione di stored procedure.  
  
-   Creazione dinamica di una struttura flessibile, denominata **Recordset**, che consente di mantenere, esplorare e modificare i dati.  
  
-   Esecuzione di operazioni di database transazionali.  
  
-   Filtro e ordinamento delle copie locali delle informazioni sul database in base ai criteri di Runtime.  
  
-   Creazione e modifica di risultati gerarchici da database.  
  
-   Binding dei campi di database ai componenti in grado di riconoscere i dati.  
  
-   Creazione di **Recordset** disconnessi e remoti.  
  
 ADO espone un'ampia gamma di opzioni e impostazioni per fornire tale flessibilità. Pertanto, è importante adottare un approccio metodico per apprendere come usare ADO in un'applicazione, suddividendo ognuno degli obiettivi in parti gestibili.  
  
 Per la maggior parte delle applicazioni che utilizzano ADO: recupero di dati, analisi dei dati, modifica dei dati e aggiornamento dei dati, sono necessarie quattro operazioni primarie. Queste operazioni vengono esaminate in modo più dettagliato più avanti in questa sezione.  
  
 Tuttavia, prima di discutere questi dettagli, viene presentata una panoramica del modello a oggetti ADO e di una semplice applicazione ADO, scritta in Microsoft® Visual Basic® ed esegue ognuna delle quattro operazioni ADO principali:  
  
-   [Oggetti e raccolte di ADO](./ado-objects-and-collections.md)  
  
-   [HelloData: applicazione ADO semplice](./hellodata-a-simple-ado-application.md)  
  
-   [Provider di OLE DB](./ole-db-providers-ado.md)  
  
-   [Errori](./errors-ado.md)