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
