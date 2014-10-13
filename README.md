iis-shortname-scanner
=====================
The latest version of scanner for IIS short file name (8.3) disclosure vulnerability by using the tilde (~) character.

Description
-------------
Microsoft IIS contains a flaw that may lead to an unauthorized information disclosure. The issue is triggered during the parsing of a request that contains a tilde character (~). This may allow a remote attacker to gain access to file and folder name information.

This scanner was moved from https://code.google.com/p/iis-shortname-scanner-poc/ to GitHub for better support.

Original research file: http://soroush.secproject.com/downloadable/microsoft_iis_tilde_character_vulnerability_feature.pdf

It is possible to detect short names of files and directories which have an 8.3 equivalent in Windows by using some vectors in several versions of Microsoft IIS. For instance, it is possible to detect all short-names of “.aspx” files as they have 4 letters in their extensions.

Note: new techniques have been introduced to the latest versions of this scanner and it can now scan IIS8.5 when it is vulnerable. 

It is not easy to find the original file or folder names based on the short names. However, the following methods are recommended as examples:
- If you can guess the full extension (for instance .ASPX when the 8.3 extension is .ASP), always try the short name with the full extension.
- Sometimes short names are listed in Google which can be used to find the actual names
- Using text dictionary files is also recommended. If a name starts with another word, the second part should be guessed based on a dictionary file separately. For instance, ADDACC~1.ASP can be AddAccount.aspx, AddAccounts.aspx, AddAccurateMargine.aspx, etc
- Searching in the website contents and resources can also be useful to find the full name. This can be achieved for example by searching Site Map in the Burp Suite tool.

Installation
--------------
You only need to download the following files if you do not want to build this yourself:
- IIS_shortname_scanner.jar
- config.xml

If you need to compile this scanner, Java JDK 6 or 7 can be used: http://www.oracle.com/technetwork/java/javase/downloads/index.html
It should be also straight forward to open this project in Eclipse.

Usage
-------

### Command line options

USAGE 1 (To verify if the target is vulnerable with the default config file):
 java -jar IIS_shortname_scanner.jar [URL]


USAGE 2 (To find 8.3 file names with the default config file):
 java -jar IIS_shortname_scanner.jar [ShowProgress] [ThreadNumbers] [URL]


USAGE 3 (To verify if the target is vulnerable with a new config file):
 java -jar IIS_shortname_scanner.jar [URL] [configFile]


USAGE 4 (To find 8.3 file names with a new config file):
 java -jar IIS_shortname_scanner.jar [ShowProgress] [ThreadNumbers] [URL] [configFile]

DETAILS:
 [ShowProgress]: 0= Show final results only - 1= Show final results step by step  - 2= Show Progress
 [ThreadNumbers]: 0= No thread - Integer Number = Number of concurrent threads [be careful about IIS Denial of Service]
 [URL]: A complete URL - starts with http/https protocol
 [configFile]: path to a new config file which is based on config.xml


- Example 0 (to see if the target is vulnerable):
 java -jar IIS_shortname_scanner.jar http://example.com/folder/

- Example 1 (uses no thread - very slow):
 java -jar IIS_shortname_scanner.jar 2 0 http://example.com/folder/new%20folder/

- Example 2 (uses 20 threads - recommended):
 java -jar IIS_shortname_scanner.jar 2 20 http://example.com/folder/new%20folder/

- Example 3 (saves output in a text file):
 java -jar IIS_shortname_scanner.jar 0 20 http://example.com/folder/new%20folder/ > c:\results.txt

- Example 4 (bypasses IIS basic authentication):
 java -jar IIS_shortname_scanner.jar 2 20 http://example.com/folder/AuthNeeded:$I30:$Index_Allocation/

- Example 5 (using a new config file):
 java -jar IIS_shortname_scanner.jar 2 20 http://example.com/folder/ newconfig.xml 

Note 1: Edit config.xml file to change the scanner settings and add additional headers.
Note 2: Sometimes it does not work for the first time and you need to try again.

References
------------
One of the new methods: https://soroush.secproject.com/blog/2014/08/iis-short-file-name-disclosure-is-back-is-your-server-vulnerable/

Original research file: http://soroush.secproject.com/downloadable/microsoft_iis_tilde_character_vulnerability_feature.pdf

Website Reference: http://soroush.secproject.com/blog/2012/06/microsoft-iis-tilde-character-vulnerabilityfeature-short-filefolder-name-disclosure/

Video Link: http://www.youtube.com/watch?v=XOd90yCXOP4

http://www.osvdb.org/83771

http://www.exploit-db.com/exploits/19525/

http://securitytracker.com/id?1027223


