We can generally disregard those  instances
![[attachments/Automatic SAP checks 29.06.2021 - 0700.msg]]

MEP p11 p15 p22 P43 P29 p40

We must send a mail to the SAP DL (or IT DL) with screenshoits of the errors + copy to BASIS DL

Then we must check in Nimsoft (gdc04019) for more errors

Db2diag errors

Connect to the server, then

Db2diag -l severe to list

Db2diag -A to archive

Db2XXXd1d/d2d

Or ssh db2p03@gdc-sapp03

-   1 aborted status > sm37 (doesn't come back)
-   1 PRIV work process > sm50 Time column
-   BW PC + Critical number: don't touch
-   The number of RFC > SM58
-   There are 1 update > SM13

when you need ROOT

use this

ssh -i /share/sapdepot/users/jfontes/scripts/db2upgrade/key root@hostname