---
title: Matrice di supporto delle funzionalità dei driver
description: Informazioni sulle funzionalità più comuni supportate nei driver per SQL Server e su dove trovare le informazioni relative.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-daenge
ms.openlocfilehash: e93307a7f29f31aa078a7d705a904812e6aa3847
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166054"
---
# <a name="driver-feature-support-matrix-for-microsoft-sql-server"></a>Matrice di supporto delle funzionalità dei driver per Microsoft SQL Server

Se si prevede di usare una funzionalità in Microsoft SQL Server, tenere presente che non tutte le funzionalità sono disponibili in ogni driver. Tra i motivi per cui una funzionalità può non essere disponibile in un driver specifico sono inclusi i seguenti:

- La funzionalità non è applicabile alla tecnologia del driver.
- La funzionalità è nuova e non è stata ancora implementata in tutti i driver.
- La funzionalità non è richiesta in un driver specifico.
- Vengono implementate prima altre funzionalità.

Sarebbe utile se tutti i driver supportassero ogni funzionalità e Microsoft si impegna a garantire la parità delle funzionalità in tutti i driver. Tuttavia, non è sempre possibile. Per semplificare la scelta del driver appropriato in base alle esigenze, ecco un elenco delle funzionalità più comuni e dei driver in cui sono implementate.

- [.NET](#table1)
- [ODBC](#table2)
- [OLE DB](#table2)
- [JDBC](#table2)
- [PHP](#table3)
- [Node.js/JavaScript](#table3)
- [Python](#table3)

| <a id="table1"></a>Funzionalità | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Core)](ado-net/microsoft-ado-net-sql-server.md) | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Framework)](ado-net/microsoft-ado-net-sql-server.md) | System.<wbr>Data.<wbr>SqlClient (.NET Core) | [System.<wbr>Data.<wbr>SqlClient (.NET Framework)](/dotnet/framework/data/adonet/sql/) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md) | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md) | | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md) |
| [Always Encrypted con enclave sicuri](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | | [Sì](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) |
| [Autenticazione tramite token di accesso di Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Sì](/dotnet/api/system.data.sqlclient.sqlconnection.accesstoken) | [Sì](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [Sì](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [Sì](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) |
| [Autenticazione della password di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | | Sì |
| [Autenticazione integrata di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | | Sì |
| [Autenticazione interattiva (MFA) di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | | Sì |
| [Autenticazione tramite identità gestite di Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Autenticazione tramite entità servizio di Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | [Sì](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Autenticazione integrata di Windows](/windows-server/security/windows-authentication/windows-authentication-overview) | [Sì](ado-net/sql/authentication-sql-server.md) | [Sì](ado-net/sql/authentication-sql-server.md) | [Sì](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) | [Sì](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) |
| [Copia bulk](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [Sì](ado-net/sql/bulk-copy-operations-sql-server.md) | [Sì](ado-net/sql/bulk-copy-operations-sql-server.md) | [Sì](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) | [Sì](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) |
| [Metadati di sensibilità e classificazione dei dati](../relational-databases/security/sql-data-discovery-and-classification.md) | [Sì](ado-net/sql/data-classification.md) | [Sì](ado-net/sql/data-classification.md) | Sì | Sì |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Sì](ado-net/sql/multiple-active-result-sets-mars.md) | [Sì](ado-net/sql/multiple-active-result-sets-mars.md) | [Sì](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) | [Sì](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) |
| [Tipi di dati spaziali](../relational-databases/spatial/spatial-data-sql-server.md) | | sì | | Sì |
| [Parametri con valori di tabella](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [Sì](ado-net/sql/table-valued-parameters.md) | [Sì](ado-net/sql/table-valued-parameters.md) | [Sì](/dotnet/framework/data/adonet/sql/table-valued-parameters) | [Sì](/dotnet/framework/data/adonet/sql/table-valued-parameters) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netcore-1.0&preserve-view=true) | [Sì](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netframework-4.8&preserve-view=true) |
| [Risoluzione dell'IP di rete trasparente](odbc/using-transparent-network-ip-resolution.md) | | [Sì](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring?view=sqlclient-dotnet-1.1&preserve-view=true) | | [Sì](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring?view=netframework-4.8&preserve-view=true) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table2"></a>Funzionalità | [ODBC Driver per SQL Server in Windows](odbc/microsoft-odbc-driver-for-sql-server.md) | [ODBC Driver per SQL Server in Linux e macOS](odbc/microsoft-odbc-driver-for-sql-server.md) | [JDBC Driver per SQL Server](jdbc/microsoft-jdbc-driver-for-sql-server.md) | [Driver OLE DB per SQL Server](oledb/oledb-driver-for-sql-server.md) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Sì](odbc/using-always-encrypted-with-the-odbc-driver.md) | [Sì](odbc/using-always-encrypted-with-the-odbc-driver.md) | [Sì](jdbc/using-always-encrypted-with-the-jdbc-driver.md) |
| [Always Encrypted con enclave sicuri](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Sì](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [Sì](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [Sì](jdbc/using-always-encrypted-with-the-jdbc-driver.md) | |
| [Autenticazione tramite token di accesso di Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Sì](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [Sì](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md#connecting-using-access-token) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione della password di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) |  [Sì](odbc/using-azure-active-directory.md) | [Sì](odbc/using-azure-active-directory.md) | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione integrata di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](odbc/using-azure-active-directory.md) | [Sì](odbc/using-azure-active-directory.md) | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione interattiva (MFA) di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](odbc/using-azure-active-directory.md) | | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione tramite identità gestite di Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Sì](odbc/using-azure-active-directory.md) | [Sì](odbc/using-azure-active-directory.md) | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione tramite entità servizio di Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | [Sì](odbc/using-azure-active-directory.md) | [Sì](odbc/using-azure-active-directory.md) | [Sì](jdbc/connecting-using-azure-active-directory-authentication.md) | [Sì](oledb/features/using-azure-active-directory.md) |
| [Autenticazione integrata di Windows](/windows-server/security/windows-authentication/windows-authentication-overview) | Sì | [Sì](odbc/linux-mac/using-integrated-authentication.md) | [Sì](jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md) | Sì |
| [Copia bulk](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [Sì](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [Sì](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [Sì](jdbc/using-bulk-copy-with-the-jdbc-driver.md) | [Sì](oledb/features/performing-bulk-copy-operations.md) |
| [Metadati di individuazione e classificazione dei dati](../relational-databases/security/sql-data-discovery-and-classification.md) | [Sì](odbc/data-classification.md) | [Sì](odbc/data-classification.md) | [Sì](jdbc/data-discovery-classification-sample.md) | |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Sì](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Sì](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | | [Sì](oledb/features/using-multiple-active-result-sets-mars.md) |
| [Tipi di dati spaziali](../relational-databases/spatial/spatial-data-sql-server.md) | | | [Sì](jdbc/use-spatial-datatypes.md) | |
| [Parametri con valori di tabella](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [Sì](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [Sì](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [Sì](jdbc/using-table-valued-parameters.md) | [Sì](oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](jdbc/jdbc-driver-support-for-high-availability-disaster-recovery.md) | [Sì](oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [Risoluzione dell'IP di rete trasparente](odbc/using-transparent-network-ip-resolution.md) | [Sì](odbc/using-transparent-network-ip-resolution.md) | [Sì](odbc/using-transparent-network-ip-resolution.md) | [Sì](jdbc/setting-the-connection-properties.md) | [Sì](oledb/features/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table3"></a>Funzionalità | [Driver per PHP per SQL Server in Windows](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Driver per PHP per SQL Server in Linux e macOS](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Tedious (Node.js)](node-js/node-js-driver-for-sql-server.md) | [pyODBC (Python)](python/pyodbc/python-sql-driver-pyodbc.md)<sup>[1](#note1)</sup> |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Sì](php/using-always-encrypted-php-drivers.md) | [Sì](php/using-always-encrypted-php-drivers.md) | | Sì |
| [Always Encrypted con enclave sicuri](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Sì](php/always-encrypted-secure-enclaves.md) | [Sì](php/always-encrypted-secure-enclaves.md) | | Sì |
| [Autenticazione tramite token di accesso di Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Sì](php/azure-active-directory.md) | [Sì](php/azure-active-directory.md) | [Sì](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Sì |
| [Autenticazione della password di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](php/azure-active-directory.md) | [Sì](php/azure-active-directory.md) | [Sì](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Sì |
| [Autenticazione integrata di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Sì](php/azure-active-directory.md) | [Sì](php/azure-active-directory.md) | | Sì |
| [Autenticazione interattiva (MFA) di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | | | | Sì<sup>[2](#note2)</sup> |
| [Autenticazione tramite identità gestite di Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Sì](php/azure-active-directory.md) | [Sì](php/azure-active-directory.md) | [Sì](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Sì |
| [Autenticazione tramite entità servizio di Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | | | | |
| [Autenticazione integrata di Windows](/windows-server/security/windows-authentication/windows-authentication-overview) | [Sì](php/how-to-connect-using-windows-authentication.md) | [Sì](odbc/linux-mac/using-integrated-authentication.md) | | Sì |
| [Copia bulk](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | | | [Sì](https://tediousjs.github.io/tedious/bulk-load.html) | |
| [Metadati di individuazione e classificazione dei dati](../relational-databases/security/sql-data-discovery-and-classification.md) | sì | Sì | | |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Sì](php/how-to-disable-multiple-active-resultsets-mars.md) | [Sì](php/how-to-disable-multiple-active-resultsets-mars.md) | | Sì |
| [Tipi di dati spaziali](../relational-databases/spatial/spatial-data-sql-server.md) | | | | |
| [Parametri con valori di tabella](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | | | [Sì](https://tediousjs.github.io/tedious/parameters.html) | Sì |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Sì](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [Sì](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [Sì](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [Risoluzione dell'IP di rete trasparente](odbc/using-transparent-network-ip-resolution.md) | [Sì](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [Sì](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [Sì](odbc/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<a id="note1"></a><sup>1</sup> Poiché questi driver sono basati su Microsoft ODBC Driver for SQL Server, è anche necessario usare una versione del driver che supporta la funzionalità.

<a id="note2"></a><sup>2</sup> Solo in Windows.

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
