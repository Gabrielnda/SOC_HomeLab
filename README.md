# SOC_HomeLab
Este repositório tem como finalidade documentar a estrutura do meu Laboratório, criado como um ambiente para estudar infraestrutura, redes e segurança da informação. O objetivo é simular  situações parecidas com as de empresas reais, permitindo que eu pratique conceitos como segmentação de rede, controle de acesso, monitoramento, hardening e isolamento de serviços de forma segura.

A principio vai se seguir em uma arquitetura simples onde o ambiente de segurança foi implementado de forma centralizada, contendo os componentes de monitoramento, detecção e "engano" estão consolidados em um único servidor Ubuntu Server.

A documentação foi formulada tendo em mente que cada modificação seja documentada para que assim, seja possível acompanhar a evolução gradual do Laboratório

## Objetivo
> Laboratório defensivo para estudo e validação de:
- Detecção de intrusão (Suricata)
- Correlação de eventos (Wazuh)
- Monitoramento e alertas (Zabbix)

## Arquitetura

| Host | Função | Modo de Execução |
| --- | --- | --- |
| 🖼 Zabbix | Monitoramento da Infra | Monitoramento de recursos, serviços e disponibilidade |
| 📑 Wazuh	| SIEM / XDR (logs, FIM, alertas, agentes) | Manager + Indexer + Dashboard |
| 📶 Suricata |	IDS (análise de tráfego de rede) | Modo IDS (AF_PACKET ou PCAP) |
| 🍯 ... |	Engano e detecção de comportamento malicioso | Serviço local |
| 🗄 Ubuntu Server | Sistema operacional base | Host Único |

### Ideia de Funcionamento:

1. Zabbix
    - Monitora:
      - CPU, RAM, disco
      - Processos (Wazuh, Suricata, Honeypot)
      - Portas, serviços e disponibilidade
2. **Suricata** monitora o tráfego de rede local e gera eventos de segurança.
3. Os eventos do **Suricata** são enviados para o **Wazuh** via logs (eve.json).
4. O **Honeypot** expõe serviços controlados para atrair atividades maliciosas.
5. Tentativas de acesso ao **honeypot** geram logs e eventos.
6. O Wazuh correlaciona:
    - Logs do sistema
    - Eventos do Suricata
    - Atividades do honeypot
8. Alertas são visualizados no Dashboard do Wazuh.

## Versões
- Wazuh 4.12
- Zabbix 7.0
- Suricata 8.0
- Ubuntu Server 22.04

## Aplicações em Produção
<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/60ee7c72-5a9f-4bba-9f0e-a776314d3804" />

