
# Integração Suricata + Wazuh (PoC)

Doucumentação utilizada como guia: [Network IDS integration - Proof of Concept guide · Wazuh documentation](https://documentation.wazuh.com/current/proof-of-concept-guide/integrate-network-ids-suricata.html)


### [1] Instalação do Suricata
```bash
sudo su
add-apt-repository ppa:oisf/suricata-stable
apt-get update
apt-get install suricata
```
---

### [2] Alteração da Interface de Rede
 Editar o arquivo de configuração:

```bash
nano /etc/suricata/suricata.yaml
```

- Usar `CTRL+W` e procurar por `interface`
- Substituir:
        

```diff
- eth0
+ wlxc02a00000515
```
> **Atenção**: YAML é sensível a espaçamento. Um espaço ausente pode quebrar o parser.
---


### [3] Instalar componentes necessários (algumas versões podem vir sem o open sccl)
> Algumas distribuições não vêm com todos os pacotes necessários (ex: `jq`, `wget`).

```bash
yum -y install epel-release wget jq nano
```
---

### [4] Regras da comunidade que possuem uma melhor base de dados do que o padrão suricata. BAIXAR, DESCOMPACTAR E MOVER
Baixar: https://rules.emergingthreats.net/open/suricata-7.0.3/emerging.rules.tar.gz 
> observação: a partir de ``/open`` é onde se encontra a atual versão do documento, quando for realizar o download, LEMRAR DE VERIFICAR E ATUALIZAR PARA VERSÃO MAIS ATUAL )

```bash
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-7.0.3/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mkdir /etc/suricata/rules && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 777 /etc/suricata/rules/*.rules
```
---

### [5] Baixando o arquivo de configuração do wazuh para o Suricata
Baixar o ``suricata.yaml`` customizado pelo Wazuh:

```bash
wget -O /etc/suricata/suricata.yaml https://packages.wazuh.com/4.3/suricata.yml
```
> Essa etapa resolveu diversos erros de compatibilidade que tive inicialmente.

---

### [6] Teste de Configuração do Suricata
```bash
sudo suricata -T
```

> Se houver erro, o Suricata **não irá subir**.

---

### [7]. Reiniciando os serviços
```bash
sudo service suricata restart
```
---

### [8] Atualização das Regras
```bash
sudo suricata-update
```

## Suricata em produção

![alt text](../BK/suricata_running.png)