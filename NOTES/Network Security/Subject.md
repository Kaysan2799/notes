## Lec 13 - 14:

>[!TIP]- To check sid of all users type 
> CMD Command:
> **wmic useraccount get name, sid**
> ~
> PowerShell command  :
> Get-WmiObject win32_useraccount | Select name, sid 

> [!TIP]- Granting access to shared folder:
> ## Using cmd:
> net share SharedFolder-C:/SharedFiles /Grant:Bob,READ
> -
> ## Using Powershell:
> Grant-SmbSharedAccess name SharedFolder -Accountname Guest -AccessRight Read

> [!TIP]- Revoke shared folder access:
> Revoke-SmbShareAccess -Name SharedFolder -AccountName Everyone


