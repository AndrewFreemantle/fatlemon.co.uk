---
title: Increasing Processes, Sessions and Transactions in Oracle XE
permalink: /2009/06/increasing-processes-sessions-and-transactions-in-oracle-xe/
tags:
  - Oracle XE
---
<div class="alert alert-warning">
<strong>Disclaimer:</strong> following my technical advice has been known to scratch cars, void lottery tickets and confuse guide dogs - <em>proceed at your own risk..</em>
</div>

![Oracle Express Edition (XE)]({{ site.imageurl }}2009/OracleXE.png)

Out of the box, Oracle Datatbase 10g Express Edition RDBMS is fast and powerful. The stated limitations of 2GB of maximum RAM usage and 2GB of total datafile management are plentiful for it to easily run as the back-end for a small to medium-sized office application.

However, we soon hit a connection limit as characterised by the following Oracle Errors:

``` console
ORA-12516: TNS:listener could not find available handler with matching protocol stack

ORA-00020: maximum number of processes (%s) exceeded
```

![ORA-00020: maximum number of processes (%s) exceeded]({{ site.imageurl }}2009/OracleXE-ORA-00020.png)

We can get this second message because Oracle creates Operating System processes to handle Connections (or Sessions) - which means Processes, Sessions, and (as we'll soon see..) Transactions are all related.

The default values in Oracle XE for these parameters are:

  * Processes = 40
  * Sessions = 49
  * Transactions = 53

I was able to generate the above error message (`ORA-00020`) from about ~30 connections on a vanilla Oracle XE installation (on Windows 7).

So, let's increase these limits to allow more connections to our Oracle Server..

### 1. Log in as SYSDBA

From the menu 'Oracle Database 10g Express Edition', find and select 'Run SQL Command Line', then type:

``` sql
connect sys as sysdba
```

and enter your SYS, or SYSTEM password at the prompt

![Oracle XE - Connected as SYSDBA, showing default values for processes, sessions and transactions]({{ site.imageurl }}2009/OracleXE-Default-Processes-Sessions-Transactions.png)

### 2. ALTER SYSTEM commands

<p class="notice--info">
  <i class="fa-solid fa-fw fa-link"></i> <strong>Update:</strong> The Oracle XE 10g documentation links below were broken, so they now point to the Oracle 19c documentation instead.
</p>

The Oracle Documentation states that [TRANSACTIONS](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/TRANSACTIONS.html#GUID-5B403FA1-5B23-4BCC-8086-4B3DBB2B7A96 "TRANSACTIONS - Oracle 19c Documentation") is derived from [SESSIONS](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/SESSIONS.html#GUID-52804B5A-164F-44F3-8980-F2593B58D807 "SESSIONS - Oracle 19c Documentation"), which in turn is derived from [PROCESSES](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/PROCESSES.html#GUID-B757AF80-DA38-4167-A914-FE376A3AD4FE "PROCESSES - Oracle 19c Documentation"), thus:

PROCESSES = 40 to Operating System Dependant

SESSIONS = (1.1 * PROCESSES) + 5

TRANSACTIONS = 1.1 * SESSIONS

So, what value to start with for PROCESSES?  Trebling it is as good a start as any, then I'd add a few more for good measure..  Here are the values I recommend:

  * PROCESSES = 150
  * SESSIONS = 300
  * TRANSACTIONS = 330

type the following commands:

``` sql
alter system set processes = 150 scope = spfile;

alter system set sessions = 300 scope = spfile;

alter system set transactions = 330 scope = spfile;
```

then to make the settings take effect, we need to bounce the database..

``` sql
shutdown immediate;

startup;
```

![OracleXE - alter system commands and restarting the database]({{ site.imageurl }}2009/OracleXE-AlterSystemCommands.png)

### 3. Verify the new parameters

with this simple select statement..
``` sql
select name, value
from v$parameter
where name in ('processes', 'sessions', 'transactions');
```

![OracleXE - showing updated processes, sessions and transactions]({{ site.imageurl }}2009/OracleXE-ShowingUpdated-Processes-Sessions-Transactions.png)

**And we're done** - a free, light and powerful Oracle RDBMS that's able to serve more connections

