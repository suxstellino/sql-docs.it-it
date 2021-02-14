---
description: Usare la gestione connessione Teradata
title: Usare la gestione connessione Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 85465e017126a116289c396787d7d8ccecba042f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339879"
---
# <a name="use-the-teradata-connection-manager"></a>Usare la gestione connessione Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Con la gestione connessione Teradata è possibile consentire a un pacchetto di estrarre dati da database Teradata e di caricare dati in database Teradata.

È necessario impostare la proprietà `ConnectionManagerType` della gestione connessione Teradata su *TERADATA*.

## <a name="configure-the-teradata-connection-manager"></a>Configurare la gestione connessione Teradata

Le modifiche di configurazione della gestione connessione Teradata vengono risolte da Integration Services in fase di esecuzione. Per aggiungere una connessione a un'origine dati Teradata, completare le informazioni nel riquadro **Editor gestione connessione Teradata**.

![Riquadro Editor gestione connessione Teradata](media/teradata-connection-manager.png)

1. Nella casella **Nome** immettere un nome per la connessione. Il nome predefinito è **Gestione connessione Teradata**.

1. Immettere una descrizione della connessione nella casella **Descrizione** (facoltativo).

1. Nella casella **Nome server** immettere il nome del server Teradata a cui connettersi.

1. In **Autenticazione** eseguire una delle operazioni seguenti:

   - Per usare l'autenticazione di Windows, selezionare **Usa autenticazione di Windows**.
   - Per usare l'autenticazione del database Teradata, selezionare **Usa autenticazione Teradata** e immettere le credenziali seguenti relative a questo tipo di autenticazione:
     - Nella casella **Meccanismo** immettere il meccanismo di controllo della sicurezza che si vuole usare. È possibile specificare uno dei seguenti valori di meccanismo: TD1, TD2, LDAP, KRB5, KRB5C, NTLM e NTLMC.
     - Nella casella **Parametro** immettere i tipi di parametri necessari per il meccanismo di controllo della sicurezza specificato.
     - Nella casella **Nome utente** immettere il nome utente usato per la connessione al database Teradata.  
     - Nella casella **Password** immettere la password di accesso al database Teradata dell'utente.

1. Nell'elenco a discesa **Database predefinito** selezionare il database Teradata a cui connettersi (facoltativo). Se questa autorizzazione di accesso al database non è corretta, viene visualizzato un errore ed è possibile immettere manualmente il nome del database.

1. Nella casella **Account** immettere il nome dell'account corrispondente al nome utente (facoltativo). Se questo valore è vuoto, viene usato l'account dell'immediato proprietario del database.
1. Selezionare **OK**.

## <a name="custom-property"></a>Proprietà personalizzata

La proprietà personalizzata `UseUTF8CharSet` specifica se viene usato il set di caratteri UTF-8. Il valore predefinito è *True*.

Per impostare la proprietà:

1. Aprire SQL Server Data Tools (SSDT).
1. Nell'area **Gestione connessioni** fare clic con il pulsante destro del mouse su **Gestione connessione Teradata** e selezionare **Proprietà**.
1. Nel riquadro **Proprietà**, per la proprietà `UseUTF8CharSet` selezionare *True* o *False*.

## <a name="troubleshoot-the-teradata-connection-manager"></a>Risolvere i problemi relativi alla gestione connessione Teradata

Per registrare le chiamate della gestione connessione Teradata al driver Open Database Connectivity (ODBC) di Teradata, abilitare la traccia ODBC in Amministrazione origine dati ODBC di Windows.

## <a name="next-steps"></a>Passaggi successivi

- Configurare l'[origine di Teradata](teradata-source.md).
- Configurare la [destinazione di Teradata](teradata-destination.md).
- Per eventuali domande, visitare la [TechCommunity](https://aka.ms/AA5u35j).
