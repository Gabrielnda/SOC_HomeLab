# Script de instalação

## Nota
- Força a instalação do Wazuh como administrador;
- Informa se o wazuh já está instalado no sistema;
- Informa se a instalação foi finalizada.

---

## Script

```shell script
# Verifica se o script está sendo executado como administrador
$IsAdmin = [Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()
$IsAdmin = $IsAdmin.IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)

# Se não for, reinicia o script com privilégios de administrador
if (-not $IsAdmin) {
    Start-Process powershell -ArgumentList "-File `"$PSCommandPath`"" -Verb RunAs
    exit
}

# Verifica se o arquivo MSI já foi baixado
$FileToDetect = "C:\Program Files (x86)\ossec-agent"
if (Test-Path $FileToDetect) {
    Write-Output "O arquivo já existe"
    Write-Output "By Gabriel Assis"
} 
else {
    # Caso o arquivo não exista, faz o download (essa parte tem que ser alterada de acordo com a vigência da versão atual do wazuh)
    Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.0-1.msi -OutFile $env:tmp\wazuh-agent;
    msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='SEU.IP.AQUI' WAZUH_AGENT_GROUP='default' 
    Write-Output "O arquivo foi baixado com sucesso."
    Write-Output "By Gabriel Assis"
}
    Start-Sleep -Seconds 90

# Inicia o serviço Wazuh após a instalação
NET START WazuhSvc

# By Gabriel Assis
```