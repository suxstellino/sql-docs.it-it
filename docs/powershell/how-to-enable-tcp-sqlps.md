---
title: Come abilitare il protocollo TCP con SQLPS
description: Informazioni su come abilitare il protocollo TCP con SQLPS
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 08/06/2020
ms.openlocfilehash: 158b15f6d7e7471fccab43400def4609f55a13c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062352"
---
# <a name="how-to-enable-the-tcp-protocol"></a>Come abilitare il protocollo TCP

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-with-sqlps"></a>Come abilitare il protocollo TCP quando si è connessi alla console con SQLPS.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

1. Aprire un prompt dei comandi e digitare:

    ```console
    C:\> SQLPS.EXE
    ```

    > [!TIP]
    > Se SQLPS non viene trovato, potrebbe essere necessario aprire un nuovo prompt dei comandi o semplicemente disconnettersi ed eseguire nuovamente l'accesso.

2. Al prompt dei comandi, digitare:

    ```powershell
    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-not-using-sqlps"></a>Come abilitare il protocollo TCP quando si è connessi alla console senza SQLPS.

1. Aprire un prompt dei comandi e digitare:

    ```console
    C:\> PowerShell.exe
    ```

2. Al prompt dei comandi, digitare:

    ```powershell
    # Get access to SqlWmiManagement DLL on the machine with SQL
    # we are on, which is where SQL Server was installed.
    # Note: this is installed in the GAC by SQL Server Setup.

    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SqlWmiManagement')

    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="next-steps"></a>Passaggi successivi

- [Installare il modulo SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)
- [Abilitare o disabilitare un protocollo di rete del server](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
- [Configurare SQL Server in un'installazione Server Core](../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md)
