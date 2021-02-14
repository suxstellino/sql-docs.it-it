---
title: Framework e librerie di connettività
description: Elenca i driver di connettività che le app client possono usare in diversi linguaggi per connettersi a Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure e in Azure Synapse Analytics.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 80efe5ff-09ba-48a0-ac93-a91d62cff47c
ms.openlocfilehash: bd3e6740bf460eda42a1448bcbe9745a41e1a387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273122"
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Framework e librerie di connettività per Microsoft SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vedere le [esercitazioni introduttive](https://aka.ms/sqldev) per iniziare rapidamente a usare linguaggi di programmazione come C#, Java, Node.js, PHP e Python e creare app con SQL Server in Linux o Windows o Docker in macOS.

La tabella seguente elenca le librerie di connettività o i *driver* che le applicazioni client possono usare da diversi linguaggi per connettersi a e usare Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure e in Azure Synapse Analytics. 

| Linguaggio | Piattaforma | Risorse aggiuntive | Download | Introduzione |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET per SQL Server](../connect/ado-net/microsoft-ado-net-sql-server.md) | [Scaricare](https://msdn.microsoft.com/vstudio/aa496123.aspx) | [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Driver Microsoft JDBC per SQL Server](../connect/jdbc/microsoft-jdbc-driver-for-sql-server.md) | [Scaricare](https://go.microsoft.com/fwlink/?LinkId=245496) |  [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Driver SQL PHP per SQL Server](../connect/php/microsoft-php-driver-for-sql-server.md) | Sistema operativo: <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Driver Node.js per SQL Server](../connect/node-js/node-js-driver-for-sql-server.md) |  [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Driver SQL per Python](../connect/python/python-driver-for-sql-server.md) <br/> \* [pyodbc](../connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md) |  [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Driver Ruby per SQL Server](../connect/ruby/ruby-driver-for-sql-server.md) | [Introduzione](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Driver Microsoft ODBC per SQL Server](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) | [Scaricare](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) |  

La tabella seguente elenca alcuni esempi di framework ORM (Object Relational Mapping) e framework Web che le applicazioni client possono usare con Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure e in Azure Synapse Analytics. 

| Linguaggio | Piattaforma | ORM |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](/ef)<br>[Entity Framework Core](/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](https://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://sequelize.org/) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |

## <a name="related-links"></a>Collegamenti correlati
- [Driver SQL Server](../connect/sql-connection-libraries.md) per la connessione da applicazioni client