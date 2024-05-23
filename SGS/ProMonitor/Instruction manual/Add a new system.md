## SAP user autorisations

For ABAP monitoring, you have to give special permissions to one user.
In our case, it's `ITS-NIMSRV` for historical reasons. It's done in **client 000**.

You can download an authorization profile here:
Admin menu→Admin configuration→Upload/Download→Download SAP user profile. This contains `SAP role Z_AGENTIL_SOFTWARE_PROMONITOR`, which is only temporary (you can delete when you've finished). We need to import in DEV system, then copy its authorizations over to `ITS-NIMSRV`.

Extract the .zip file

Upload the file in `PFCG`
![[attachments/Pasted image 20230518104829.png]]

Enter into modification, go under `Authorizations` and generate to create the profile
![[attachments/Pasted image 20230518105350.png]]
Copy the profile name:
![[attachments/Pasted image 20230518111300.png]]
Check that `ITS-NIMSRV` has the role `ZA_U_XX_NIM_00`
![[attachments/Pasted image 20230518105510.png]]

Enter `ZA_U_XX_NIM_00` in modification mod, go inside the `Authorizations`, paste the profile name there and click OK.
![[attachments/Pasted image 20230518111532.png]]

Generate the authorizations and transport the role into next system.
### Set up organisations and groups
Go to Monitoring > Organisation and Groups. As their name indicate, it's for classification purposes.
Go to Settings > Collectors > Collector association and assign the new group to Agent 1
### Set up system
Here, it's pretty straightforward too: 
![[attachments/Pasted image 20230518115347.png]]
### Set up user in Cockpit
Go to Monitoring > users  > Add new user
Fill it up with necessary details. ITS-NIMSRV is common for all instances. Password is in Crypto
### Set up connector
On Systems screen
![[attachments/Pasted image 20230518115856.png]]

![[attachments/Pasted image 20230518120330.png]]
Don't test straight away, there may be a slight delay.
