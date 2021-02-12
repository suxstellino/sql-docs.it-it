---
title: Azure Active Directory in SSDT
description: Informazioni sui metodi di autenticazione di Azure Active Directory forniti da SQL Server Data Tools (SSDT) per il database SQL di Azure e Azure Synapse Analytics.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
author: markingmyname
ms.author: maghan
reviewer: ''
ms.custom: seo-lt-2019
ms.date: 10/28/2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 84e1c427d01eef2bc7df4bdf7af0e28e0c469844
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079727"
---
# <a name="azure-active-directory-support-in-sql-server-data-tools-ssdt"></a>Supporto di Azure Active Directory in SQL Server Data Tools (SSDT)

[!INCLUDE[appliesto-xx-asdb-asdb-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

SQL Server Data Tools (SSDT) supporta vari metodi di autenticazione di [Azure Active Directory (Azure AD)](/azure/active-directory/active-directory-whatis).

In Visual Studio aprire **Esplora oggetti di SQL Server** nel menu **Visualizza** e selezionare **Aggiungi SQL Server**:

![Finestra di dialogo di connessione di SSDT](media/azure-active-directory/interactive.png)

#### <a name="which-azure-sql-products"></a>Prodotti Azure SQL

Questo articolo illustra Azure AD per l'elenco seguente di *prodotti Azure SQL* nel [cloud di Azure](https://azure.microsoft.com/):

- database SQL di Azure
- Azure Synapse Analytics

## <a name="active-directory-password-authentication"></a>Autenticazione della password Active Directory

*Autenticazione della password Active Directory* è un meccanismo per la connessione ai prodotti Azure SQL elencati sopra. Questo meccanismo usa le identità in Azure Active Directory (Azure AD). Usare questo metodo per la connessione quando:

- Si è connessi a Windows con credenziali di un dominio non federato con Azure, oppure
- Si usa l'autenticazione di Azure AD con Azure AD e tale autenticazione è basata sul dominio iniziale o sul dominio client.

Per altre informazioni, vedere [Connessione al database SQL tramite l'autenticazione di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication).  

## <a name="active-directory-integrated-authentication"></a>Autenticazione integrata di Active Directory

L'*autenticazione integrata di Active Directory* è un meccanismo di connessione ai prodotti Azure SQL elencati tramite identità in Azure Active Directory (Azure AD). Usare questo metodo per la connessione se si è connessi a Windows con le credenziali di Azure Active Directory da un dominio federato. Per altre informazioni, vedere [Connessione al database SQL tramite l'autenticazione di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication).

## <a name="active-directory-interactive-authentication"></a>Autenticazione interattiva di Active Directory

*Autenticazione interattiva di Active Directory* è disponibile per la connessione ai prodotti SQL Azure elencati con SSDT, ma solo con [.NET Framework 4.7.2](/dotnet/api/?view=netframework-4.7.2&preserve-view=true) o versione successiva.

- [Scaricare e installare qualsiasi versione di .NET Framework](https://www.microsoft.com/net/download/all).
- [Visual Studio 2017 versione 15.6](/visualstudio/releasenotes/vs2017-relnotes) o successiva.

#### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA).

Autenticazione interattiva di Active Directory supporta un'autenticazione interattiva che consente di usare la funzionalità Multi-Factor Authentication (MFA) di Azure Active Directory (AD) per l'autenticazione con i prodotti Azure SQL elencati. Questo metodo supporta gli utenti di Azure AD nativi e federati e gli utenti guest da altri account. Gli altri tipi di account includono:

- Utenti Business to Business (Azure AD B2B).
- Account Microsoft, ad esempio @outlook.com, @hotmail.com o @live.com.
- Account non Microsoft, ad esempio @gmail.com.

Se si specifica il metodo MFA è necessario specificare il **Nome utente**, mentre il campo **Password** è disabilitato. 

#### <a name="password-entry"></a>Immissione della password

Quando si esegue l'autenticazione con l'*autenticazione interattiva di Active Directory* viene visualizzata una finestra un'autenticazione che richiede agli utenti di immettere manualmente una password.

![Finestra di dialogo di accesso](media/azure-active-directory/sign-in.png)

L'autenticazione MFA viene imposta da Azure AD tramite questa finestra popup MFA aggiuntiva.

> [!NOTE]
> I flussi di lavoro automatizzati vengono bloccati dall'uso di *Autenticazione interattiva di Active Directory*. Deve essere presente un utente che interagisca con il processo di autenticazione, immettendo manualmente una password.

## <a name="known-issues-and-limitations"></a>Problemi noti e limitazioni

- *Autenticazione interattiva di Active Directory* è supportato solo per la connessione ai prodotti Azure SQL elencati all'inizio di questo articolo. Non è supportata per SQL Server (in locale o in una macchina virtuale).
- *Autenticazione interattiva di Active Directory* non è supportata nella finestra di dialogo di connessione di *Esplora server*. È necessario connettersi usando SSDT con *Esplora oggetti di SQL Server*.
- L'integrazione di Single Sign-On con l'account attualmente connesso in Visual Studio non è supportata per SSDT.
- Il file SQLPackage.exe installato nella directory Extensions durante l'installazione di Visual Studio non è progettato per l'uso da tale percorso. Per usare SqlPackage.exe con Azure AD, passare a [Framework applicazione livello dati](https://www.microsoft.com/download/details.aspx?id=55088) 
- La funzionalità Confronto dati di SSDT non è supportata per l'autenticazione di Azure AD.  


## <a name="see-also"></a>Vedere anche  

[Autenticazione a più fattori](/azure/sql-database/sql-database-ssms-mfa-authentication)  
[Autenticazione di Azure Active Directory con il database SQL](/azure/sql-database/sql-database-aad-authentication-configure)  
[Forum MSDN di SSDT](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt)  
[Blog del Team di SSDT](/archive/blogs/ssdt/)  
[Scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
