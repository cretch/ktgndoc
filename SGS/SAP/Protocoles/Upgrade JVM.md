Vérifier sur le solman la version de la JVM pour le SID cible [SolMan](http://gdc-sapp42a.swatchgroup.net:54200/webdynpro/dispatcher/sap.com/tc~smd~server~agent~admin/SMDAgentAdminApplication?sap-language=EN#](http://gdc-sapp42a.swatchgroup.net:54200/webdynpro/dispatcher/sap.com/tc~smd~server~agent~admin/SMDAgentAdminApplication?sap-language=EN)

_(ici pour Q06)_

![[attachments/Pasted image 20230329111233.png]]

Se connecter à gdc00620 en tant que itssapccbc

Pour trouver le nom de l’host pour un SID SAP

_s2h SID_

_gdcxxxx_

Se connecter au serveur en tant que daaadm

_ssh daaadm@gdcxxxx_

Créer le répertoire JVM 8 et lui donner les bonnes autorisations

_mkdir /usr/sap/DAA/SYS/exe/jvm/rs6000_64/sapjvm_8.1.091_

_chmod 775 /usr/sap/DAA/SYS/exe/jvm/rs6000_64/sapjvm_8.1.091_

Copier, décomprimer la nouvelle SAP JVM et supprimer le fichier

_cd /usr/sap/DAA/SYS/exe/jvm/rs6000_64/sapjvm_8.1.091_

_cp /share/sapdepot/JVM_8/SAPJVM8_91-80000208.SAR ._

_SAPCAR -xvf SAPJVM8_91-80000208.SAR_

_rm SAPJVM8_91-80000208.SAR_

Modifier les profils d’instances

_cd /usr/sap/DAA/SYS/profile_

_cp -p DAA_SMDAXX_gdcxxxxx DAA_SMDAXX_gdcxxxx_JVM_6_save_

Modifier les paramètres suivants de

__CPARG1 = list:$(DIR_CT_SAPJVM)/sapjvm_6.lst_

_SAPJVM_VERSION = 6.1.084_

_DIR_SAPJVM = $(DIR_EXECUTABLE)$(DIR_SEP)sapjvm_6_

en

__CPARG1 = list:$(DIR_CT_SAPJVM)/sapjvm_8.lst_

_SAPJVM_VERSION = 8.1.091_

_DIR_SAPJVM = $(DIR_EXECUTABLE)$(DIR_SEP)sapjvm_8_

Arret des agents SMD

stopsap SMDAXX

_ps -ef|grep daaadm_

![](file:///C:/Users/crettazg/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

kill les processes ratache au _SMDAXX_

_Kill -9 process_

![](file:///C:/Users/crettazg/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

Et kill -9 130420

Supprimer dans les kernels la JVM 6

```bash
rm – R /usr/sap/DAA/SMDAXX/exe/sapjvm_6
startsap SMDAXX
```

Vérifier quele file système ne soit pas plein

`df -k /usr/sap/DAA`

Faire de l’espace sur la file système _/usr/sap/DAA/SYS_

`rm -R /usr/sap/DAA/SYS/exe/jvm/rs6000_64/sapjvm_6.1.102`

ou bien

`rm -R /usr/sap/DAA/SYS/exe/jvm/rs6000_64/sapjvm_6.1.084`

Vérifier sur le solman que l’agent est bien vu avec la bonne version JVM url

[http://gdc-sapp42a.swatchgroup.net:54200/webdynpro/dispatcher/sap.com/tc~smd~server~agent~admin/SMDAgentAdminApplication?sap-language=EN#](http://gdc-sapp42a.swatchgroup.net:54200/webdynpro/dispatcher/sap.com/tc~smd~server~agent~admin/SMDAgentAdminApplication?sap-language=EN)

Exemple avec D04 gdc01228

![[attachments/Pasted image 20230329111253.png]]

Si répertoire “/usr/sap/DAA/SYS/exe/jvm/rs6000_64 » appartient à root essayer la procédure suivante

Séquence

s2h d46

gdc02892

ssh root@gdc02892

chown daaadm:sapsys /usr/sap/DAA/SYS/exe/jvm/rs6000_64

chmod 775 /usr/sap/DAA/SYS/exe/jvm/rs6000_64

exit