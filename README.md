# SOC_HomeLab
Laboratório prático em desenvolvimento contínuo para validação de implementações, modificações e serviços em ambiente SOC

## Objetivo
> Laboratório defensivo para estudo e validação de:
- Detecção de intrusão (Suricata)
- Correlação de eventos (Wazuh)
- Monitoramento e alertas (Zabbix)

## Arquitetura
- 1 servidor Ubuntu (Wazuh + Zabbix)
- 1 host atacante/gerador de tráfego
- N máquinas agents para realizar o monitoramento
- Rede dedicada ponto-a-ponto

## Tecnologias
- Wazuh 4.x
- Zabbix 6.x
- Suricata
- Ubuntu Server 22.04

## Casos de uso testados
- Detecção de port scan
- Alertas de brute force
- Correlação de logs de firewall