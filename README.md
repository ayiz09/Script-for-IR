# Script-for-IR
Ease IR cases

## PowerShell Decode Compress Base64
```
$input = Read-Host -Prompt 'base64'
$decoded = $(New-Object IO.StreamReader ($(New-Object IO.Compression.DeflateStream($(New-Object IO.MemoryStream(,$([Convert]::FromBase64String($input)))), [IO.Compression.CompressionMode]::Decompress)), [Text.Encoding]::ASCII)).ReadToEnd();

Write-Host "----- Decoded -----"
Write-Host $decoded
Write-Host "----- Decoded End -----"
```   
## Merge multiple CSV file and Export to One
```
Get-ChildItem -Recurse *.csv | Select-Object -ExpandPropertyFullName| Import-Csv | Export-Csv ..\merged.csv -NoTypeInformation -Append
```   
## Find String in File with newline
```
Get-ChildItem -Path "<path logs>" -Recurse | select-string -Pattern "getRuntime()" 
```

## Find files having MZ header
```
$filesWithMZ = Get-ChildItem -Recurse | Where-Object {
    $fileContent = Get-Content -LiteralPath $_.FullName -Encoding Byte -ErrorAction SilentlyContinue
    $fileContent -ne $null -and ($fileContent[0] -eq 77 -and $fileContent[1] -eq 90)
}
 
$filesWithMZ | ForEach-Object { Write-Output $_.FullName }
```
## Unzip .zip file recursively
```
$sourcePath = "C:\test"
 
 
Add-Type -AssemblyName System.IO.Compression.FileSystem
function Unzip
{
  param([string]$zipfile, [string]$outpath)
  [System.IO.Compression.ZipFile]::ExtractToDirectory($zipfile, $outpath)
}
 
$flag = $true
while($flag)
{
 $zipFiles = Get-ChildItem -Path $sourcePath -Recurse | Where-Object {$_.Name -like "*.zip"}
 
 if($zipFiles.count -eq 0)
 {
    $flag = $false
 }
 
 elseif($zipFiles.count -gt 0)
 {
    foreach($zipFile in $zipFiles)
    {
     #create the new name without .zip
     $newName = $zipFile.FullName.Replace(".zip", "")
 
     Unzip $zipFile.FullName $newName
 
     #remove zip file after unzipping so it doesn't repeat 
     Remove-Item $zipFile.FullName   
    }
 }
 Clear-Variable zipFiles
}
```
## Zip back all folder
```
$sourcePath = "C:\Source"
$destinationPath = "C:\test"
 
$source = Get-ChildItem -Path $sourcePath -Filter "*" -Directory
Add-Type -assembly "system.io.compression.filesystem"
Foreach ($s in $source)
{
  $destination = Join-path -path $destinationPath -ChildPath "$($s.name).zip"
  If(Test-path $destination) {Remove-item $destination}
  [io.compression.zipfile]::CreateFromDirectory($s.fullname, $destination)
}
```
## Unzip .gz file recursively
```
# Define the directory to search for .gz files
$directoryPath = "C:\Path\To\Your\Directory"

# Get all .gz files recursively
$gzFiles = Get-ChildItem -Path $directoryPath -Filter *.gz -Recurse

# Iterate through each .gz file
foreach ($gzFile in $gzFiles) {
    # Define the output file path by removing the .gz extension
    $outputFilePath = [System.IO.Path]::ChangeExtension($gzFile.FullName, $null)

    # Create a FileStream for the .gz file
    $fileStream = [System.IO.File]::OpenRead($gzFile.FullName)

    # Create a FileStream for the output file
    $outputStream = [System.IO.File]::OpenWrite($outputFilePath)

    # Use a GZipStream to decompress the .gz file
    $gzipStream = New-Object System.IO.Compression.GZipStream($fileStream, [System.IO.Compression.CompressionMode]::Decompress)

    try {
        # Copy the decompressed data to the output file
        $gzipStream.CopyTo($outputStream)
    } finally {
        # Close streams
        $gzipStream.Close()
        $outputStream.Close()
        $fileStream.Close()
    }

    # Delete the .gz file after decompression
    Remove-Item -Path $gzFile.FullName -Force

    Write-Host "Decompressed: $($gzFile.FullName) to $outputFilePath and deleted the .gz file."
}
```
## Check Drives
```
wmic logicaldisk get deviceid, volumename, description

```
```
wmic diskdrive get Caption, MediaType, Index, InterfaceType

```
## PowerShell and POwerShell ISE
```
C:\Users\user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline

c:\users\user\appdata\local\microsoft_corporation\powershell_ise.exe_strongname_lw2v2vm3wmtzzpebq33gybmeoxukb04w\3.0.0.0\autosavefiles\

```
## Task scheduler

```
Get-ScheduledTask | Where-Object {$_.TaskName -like '*strings_apa*'} | Format-List *

```
## Network @ Netstat ?
```
Get-NetTCPConnection | Select-Object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,OwningProcess,@{Name='ProcessName';Expression={(Get-Process -Id $_.OwningProcess -ErrorAction SilentlyContinue).Name}}
```
### Find File recursive
```
Get-PSDrive -PSProvider FileSystem | ForEach-Object { Get-ChildItem -Path $_.Root -Filter "strings_apa" -Recurse -ErrorAction SilentlyContinue }
```
