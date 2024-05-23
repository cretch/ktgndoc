Y:\Services\Security

check présence SPNEGO
<span style="background:#d3f8b6">COMMANDE</span>
connexion au GDC06040
aller chercher le nom de compte dans AD
Change password

--- Type (Windows) the command:  
   ktpass -princ SAPService/gdc-sap<sid>.swatchgroup.net@SWATCHGROUP.NET -mapuser ITS-SAP-SSnnn -crypto All -ptype KRB5_NT_PRINCIPAL -mapop set -desonly -pass $TronGp@s$W0rD -out <SID>.keytab
---Copy the keytab file to /share/sapdepot/divers/SSO
connexion sidadm
4) Type (UNIX AIX):  
/usr/bin/ksh /share/sapdepot/divers/SSO/update_keytab.ksh; ~/bin/kinitkb.csh; /usr/krb5/bin/klist -fea

