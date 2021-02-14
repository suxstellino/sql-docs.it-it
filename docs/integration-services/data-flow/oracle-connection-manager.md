---
description: Gestione connessione Oracle
title: Gestione connessione Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 258cce05d25b02b046c9fc4e79bd031d6b27fc22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343251"
---
# <a name="oracle-connection-manager"></a>Gestione connessione Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Una gestione connessione Oracle viene usata per consentire a un pacchetto di estrarre i dati dai database Oracle e di caricare i dati nei database Oracle.

La proprietà **ConnectionManagerType** della gestione connessione Oracle viene impostata su **ORACLE**.

## <a name="configuring-the-oracle-connection-manager"></a>Configurazione della gestione connessione Oracle

Le modifiche di configurazione della gestione connessione Oracle verranno risolte da Integration Services in fase di esecuzione. Usare la finestra di dialogo **Editor gestione connessione Oracle** per aggiungere una connessione a un'origine dati Oracle.

![Gestione connessione](media/oracle-connection-manager.png)

### <a name="options"></a>Opzioni

#### <a name="connection-manager-information"></a>Informazioni gestione connessione

Immettere le informazioni sulla connessione Oracle.

**Nome**

Immettere un nome per la connessione Oracle. Il nome predefinito è Gestione connessione Oracle. 

**Descrizione** 

Immettere una descrizione della connessione. Questo input è facoltativo.

**Nome del servizio TNS**

Immettere il nome del database Oracle da usare. Il nome del servizio TNS può essere:

- Nome del descrittore di connessione definito nel file tnsnames.ora contenuto nella cartella admin del client Oracle.

- Formato EzConnect: [//]host[:port][/service_name]

Per ulteriori informazioni, vedere la documentazione di Oracle.

#### <a name="connection-manager-logging"></a>Registrazione della gestione connessione

Selezionare una delle opzioni seguenti:

- **Usa autenticazione di Windows**: selezionare questa opzione per usare l'autenticazione di Windows.

- **Usa autenticazione di Oracle**: selezionare questa opzione per usare l'autenticazione del database Oracle. Se si usa questa autenticazione, immettere le credenziali Oracle come indicato di seguito:  
    **Nome utente**: digitare il nome utente usato per la connessione al database Oracle.  
    **Password**: digitare la password del database Oracle per l'utente immesso nel campo del nome utente.

> [!NOTE]
>
>L'autenticazione di Windows non è supportata per Oracle Server 18c.

**Test connessione**

Fare clic su **Test connessione** per verificare se le informazioni specificate sono corrette. Se le informazioni immesse consentono la connessione al database Oracle, verrà visualizzato il messaggio **Test della connessione riuscito**.

### <a name="custom-properties"></a>Proprietà personalizzate

In Gestione connessione Oracle sono disponibili le proprietà personalizzate seguenti per la gestione connessione:

- **EnableDetailedTracing**: non usata.

- **OracleHome**: specificare il nome 32-bit Oracle Home o la cartella che verrà usata dal connettore. Facoltativa

- **OracleHome64**: specificare il nome 64-bit Oracle Home o la cartella che verrà usata dal connettore in esecuzione in modalità a 64 bit. Facoltativa

Le proprietà personalizzate non sono elencate in Editor gestione connessione Oracle. Per impostare le proprietà **OracleHome** e **OracleHome64**:

1. Dall'area Gestione connessione fare clic con il pulsante destro del mouse sulla gestione connessione Oracle in uso e scegliere **Proprietà**.

2. Nel riquadro **Proprietà** impostare la proprietà **OracleHome** o **OracleHome64** con il percorso completo della home directory Oracle.

## <a name="next-steps"></a>Passaggi successivi

- Configurare l'[origine Oracle](oracle-source.md).
- Configurare la [destinazione Oracle](oracle-destination.md).
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA5u35j).
