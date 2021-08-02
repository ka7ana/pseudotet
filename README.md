# pseudotet
A PowerShell script used to create Word Documents containing an Emotet-like VBA/PowerShell payload.

The script performs the following steps:

1. Generates a PowerShell payload that calls out to a domain, saves the result to an .exe file and simulates execution of the downloaded file (`calc.exe` is run instead)
2. Encrypts the PowerShell payload using a randomly generated key
3. Creates a new Word Document, saving the encrypted PowerShell payload in the Document's "Comments" built-in property
4. Sets Custom Document Properties which, when re-assembled, contains the VBA `WScript.Run` command necessary to invoke PowerShell to decrypt and run the payload from steps 1 & 2
5. Generates a number of obfuscated auxillary functions to pad out the VBA

When the generated Document is open, the VBA `Sub AutoOpen` command is executed (provided macros are enabled, of course :)), which reassembles the command and decrypts and executes the PowerShell payload.

**NOTE:** the payload URL can be supplied or randomly generated. If the URL supplied/generated is valid, the response will be saved to a exe file in `C:\Temp`, however this is overwritten with a copy of `calc.exe` before being run. 

## Usage

To use, import the module in PS and call the Generate-Pseudotet function, passing in the required args:

```
PS> Import-Module .\pseudotet.psm1
PS> Generate-Pseudotet -PayloadDownloadURL "https://www.foobar.com/payload" -Debug $true
```
