# Arquivos de certificados renomeados após atualização

## Contexto
> Após atualização ao tentar acessar painel web wazuh, apareceu a seguinte mensagem: "Hum... Não consigo chegar a esta página"
- O arquivo ``/etc/wazuh-dashboard/certs/dashboard-key.pem`` teve seu nome alterado para ``/etc/wazuh-dashboard/certs/wazuh-dashboard-key.pem`` após atualização.

## ERROR
"srv-homelab opensearch-dashboards[96897]: Error: ENOENT: no such file or directory, open '/etc/wazuh-dashboard/certs/dashboard-key.pem"



## Timeline

#### [1] Sintoma
- Acesso web apresentou erro
    > "Hum... Não consigo chegar a esta página"

#### [2] Verificação de serviços
- ``systemctl status wazuh-dashboard``: **running**
- ``systemctl status wazuh-indexer``: **running**
- ``systemctl status wazuh-manager``: **running**
- ``systemctl status filebeat``: **running**

#### [3] Análise de logs
- Comando executado:
```bash
journalctl -u wazuh-dashboard | grep -iE "error|warn|crit|fatal"
```
- Erro retornado:
```text
"srv-homelab opensearch-dashboards[96897]: Error: ENOENT: no such file or directory, open '/etc/wazuh-dashboard/certs/dashboard-key.pem"
```

#### [4] Verificação de arquivos de certificado
- Diretório analisado:
 ```bash
ls -l /etc/wazuh-dashboard/certs/
```
> Identificado que os nomes dos arquivos não correspondiam aos referenciados no erro.

#### [5] Ajuste de Configuração
- Arquivo editado:
```bash
nano /etc/wazuh-dashboard/opensearch_dashboards.yml
```

- Alterações realizadas:
```diff
- server.ssl.key: "/etc/wazuh-dashboard/certs/dashboard-key.pem"
+ server.ssl.key: "/etc/wazuh-dashboard/certs/wazuh-dashboard-key.pem"

- server.ssl.certificate: "/etc/wazuh-dashboard/certs/dashboard.pem" 
+ server.ssl.certificate: "/etc/wazuh-dashboard/certs/wazuh-dashboard.pem" 
```
#### [6] Ação final
- Serviço reiniciado:
```bash
    sudo systemctl restart wazuh-dashboard
```

### Resultado
Acesso ao painel web restaurado.

