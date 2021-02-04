---
description: Schermata 2 di Creazione guidata origine dati (driver ODBC per SQL Server)
title: Schermata 2 di Creazione guidata origine dati (driver ODBC per SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-jizho2
ms.openlocfilehash: 749ba8963234955e4893a2c5197bcfd4c26f2584
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159554"
---
# <a name="data-source-wizard-screen-2"></a>Creazione guidata origine dati - Schermata 2

Specificare il metodo di autenticazione e configurare le voci del client avanzato di Microsoft SQL Server, nonché l'account di accesso e la password usati dal driver ODBC per SQL Server per connettersi a SQL Server durante la configurazione dell'origine dati.

## <a name="options"></a>Opzioni

### <a name="with-integrated-windows-authentication"></a>Autenticazione Windows integrata

Indica che il driver richiede una connessione protetta (o trusted) a un SQL Server. Se si seleziona questa opzione, in SQL Server verrà utilizzata la sicurezza degli accessi integrata per stabilire connessioni con questa origine dati, indipendentemente dalla modalità di sicurezza degli accessi corrente del server. Qualsiasi password o ID di accesso specificato viene ignorato. L'amministratore di sistema di SQL Server deve associare l'account di accesso di Windows a un ID di accesso di SQL Server (ad esempio tramite SQL Server Management Studio).

È inoltre possibile specificare un nome SPN per il server.

### <a name="with-active-directory-integrated-authentication"></a>Con autenticazione integrata di Active Directory

Specifica che il driver esegue l'autenticazione SQL Server tramite Azure Active Directory. Se si seleziona questa opzione, SQL Server usa la sicurezza degli accessi integrata di Azure Active Directory per stabilire una connessione tramite questa origine dati, indipendentemente dalla modalità di protezione degli accessi corrente nel server.

### <a name="with-sql-server-authentication"></a>Autenticazione di SQL Server

Specifica che il driver esegue l'autenticazione SQL Server tramite un ID di accesso e una password.

### <a name="with-active-directory-password-authentication"></a>Con autenticazione della password di Active Directory

Specifica che il driver esegue l'autenticazione SQL Server tramite un ID di accesso di Azure Active Directory e una password.

### <a name="with-active-directory-interactive-authentication"></a>Con autenticazione interattiva di Active Directory

Specifica che il driver esegue l'autenticazione SQL Server tramite la modalità interattiva di Azure Active Directory fornendo un ID di accesso. Questa opzione attiverà la finestra di dialogo di richiesta di autenticazione di Azure.

### <a name="with-managed-identity-authentication"></a>Con autenticazione identità gestita

Specifica che il driver esegue l'autenticazione SQL Server tramite l'identità gestita.

### <a name="with-active-directory-service-principal-authentication"></a>Con Active Directory l'autenticazione basata su entità servizio

Specifica che il driver esegue l'autenticazione per SQL Server usando un'entità servizio Azure Active Directory.


### <a name="login-id"></a>ID accesso

Specifica l'ID di accesso usato dal driver per la connessione a SQL Server se viene selezionata l'opzione **Autenticazione SQL Server tramite ID e password di accesso immessi dall'utente** o **Con autenticazione della password di Active Directory mediante ID di accesso e password immessi dall'utente** o **Con autenticazione interattiva di Active Directory mediante un ID di accesso immesso dall'utente**. Se è selezionata l'opzione **Con autenticazione identità gestita**, specificare l'ID oggetto dell'identità gestita o lasciare vuoto per usare l'identità predefinita. Questo campo si applica solo alla connessione effettuata per determinare le impostazioni predefinite del server. Non si applica invece alle connessioni successive effettuate utilizzando l'origine dati dopo la relativa creazione, tranne nel caso in cui si utilizzi l'autenticazione identità gestita.

### <a name="password"></a>Password

Specifica la password usata dal driver per la connessione a SQL Server se viene selezionata l'opzione **Autenticazione SQL Server tramite ID e password di accesso immessi dall'utente** o **Con autenticazione della password di Active Directory mediante ID di accesso e password immessi dall'utente**. Questo campo si applica solo alla connessione effettuata per determinare le impostazioni predefinite del server. Non si applica invece alle connessioni successive effettuate utilizzando la nuova origine dati.

Le caselle **ID di accesso** e **Password** sono entrambe disabilitate se è selezionata l'opzione **Autenticazione Windows integrata** o **Autenticazione integrata di Active Directory**.

### <a name="next"></a>Prossima

Consente di passare alla schermata successiva della procedura guidata.

### <a name="back"></a>Indietro

Consente di tornare alla schermata precedente della procedura guidata.

## <a name="next-steps"></a>Passaggi successivi

[Creazione guidata origine dati - Schermata 1](../../../connect/odbc/windows/dsn-wizard-1.md)

[Creazione guidata origine dati - Schermata 3](../../../connect/odbc/windows/dsn-wizard-3.md)

