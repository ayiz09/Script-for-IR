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
