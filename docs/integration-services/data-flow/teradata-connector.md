---
description: Connettore Microsoft per Teradata
title: Connettore Microsoft per Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5f82833ec5498c107155a5284d7a419ba6289239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346538"
---
# <a name="microsoft-connector-for-teradata"></a>Connettore Microsoft per Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Connettore Microsoft per Teradata consente di esportare e caricare i dati in database Teradata all'interno di un pacchetto SSIS.

Questo nuovo connettore supporta database con tabelle abilitate per 1 MB.

## <a name="version-support"></a>Supporto versione

I prodotti Microsoft SQL Server seguenti sono supportati da Connettore Microsoft per Teradata:

- Microsoft SQL Server 2019
- Microsoft SQL Server Data Tools (SSDT) 15.8.1 o versioni successive per Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) per Visual Studio 2019

Microsoft Connector per Teradata usa l'interfaccia del linguaggio di programmazione dell'applicazione Teradata Parallel Transporter per caricare ed esportare dati in e da database Teradata. Sono supportate le versioni seguenti:

- Teradata Parallel Transporter (Teradata PT) 16.10
- Teradata Parallel Transporter (Teradata PT) 16.20

Sono supportate le origini dati delle versioni seguenti di database Teradata:

- Database Teradata 16.20
- Database Teradata 16.10
- Database Teradata 15.10
- Database Teradata 15.00

Per informazioni dettagliate sulla guida per programmatori dell'interfaccia di programmazione dell'applicazione Teradata Parallel Transporter, vedere la [documentazione di Teradata](https://docs.teradata.com/).

## <a name="installation"></a>Installazione

Nei computer a 32 bit installare i seguenti driver da [Teradata Tools and Utilities - Pacchetto di installazione per Windows](https://downloads.teradata.com/download/tools/teradata-tools-and-utilities-windows-installation-package):

- Driver ODBC Teradata (32 bit)
- API Teradata PT (32 bit)

Nei computer a 64 bit installare i driver seguenti:

- Driver ODBC Teradata (64 bit)
- API Teradata PT (64 bit)

Per installare il connettore per database Teradata, scaricare ed eseguire il programma di installazione dalla [versione più recente di Connettore Microsoft per Teradata](https://www.microsoft.com/download/details.aspx?id=100599). Quindi, seguire le indicazioni nell'installazione guidata.

Dopo aver installato il connettore, è necessario riavviare il servizio SQL Server Integration Services per assicurarsi che l'origine e la destinazione Teradata funzionino correttamente.

## <a name="design-and-execute-ssis-packages"></a>Progettare ed eseguire pacchetti SSIS

Microsoft Connector per Teradata offre un'esperienza utente simile con il connettore Attunity Teradata. L'utente può progettare nuovi pacchetti in base all'esperienza precedente, usando SSDT per VS 2017 o VS 2019, con *SQL Server 2019 come destinazione*.

L'origine e la destinazione Teradata sono specificate in Categoria comune.

![Componente Teradata](media/teradata-component.png)

La gestione connessione Teradata è visualizzata come "TERADATA".

![Tipo di gestione connessione Teradata](media/teradata-connection-manager-type.png)

I pacchetti SSIS esistenti progettati con il connettore Attunity Teradata verranno aggiornati automaticamente per l'uso di Microsoft Connector per Teradata. Verranno modificate anche le icone.

Per eseguire il pacchetto SSIS *con destinazione SQL Server 2017 e versioni precedenti*, è necessario installare **Microsoft Connector per Teradata di Attunity** con la versione corrispondente dal collegamento seguente:

- [SQL Server 2017: Connettore Microsoft versione 5.0 per Teradata di Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016: Connettore Microsoft versione 4.0 per Teradata di Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014: Connettore Microsoft versione 3.0 per Teradata di Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012: Connettore Microsoft versione 2.0 per Teradata di Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

Per progettare il pacchetto SSIS *con destinazione SQL Server 2017 e versioni precedenti*, è necessario avere **Microsoft Connector per Teradata** e installare **Microsoft Connector per Teradata di Attunity** con la versione corrispondente.

## <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti

- Editor origine/destinazione Teradata, la proprietà **Database predefinito** non viene applicata.  Come soluzione alternativa, digitare il nome del database nella casella a discesa per filtrare la tabella o la vista.

- Editor origine/destinazione Teradata, il passaggio di mapping non funziona quando il tipo è \<database>.<table/view>. Come soluzione alternativa, digitare \<database>.<table/view> e quindi fare clic sul pulsante a discesa.

- Editor origine Teradata, la vista non viene visualizzata quando la modalità di accesso ai dati è "Nome tabella - Esportazione TPT". Come soluzione alternativa, usare l'Editor avanzato dell'origine Teradata.

- Destinazione Teradata, l'attributo 'PackMaximum' non può essere impostato su 'True'.  In caso contrario, si verificherà un errore.

## <a name="uninstallation"></a>Disinstallazione

È possibile eseguire la disinstallazione guidata per rimuovere **Connettore Microsoft per Teradata**.

## <a name="next-steps"></a>Passaggi successivi

- Configurare la [gestione connessione Teradata](teradata-connection-manager.md)
- Configurare l'[origine Teradata](teradata-source.md)
- Configurare la [destinazione Teradata](teradata-destination.md)
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA6iwdw).
