## Cenário
Ataque de port scan usando nmap.

## Geração do evento
nmap -sS -p- 192.168.50.10

## Detecção
- Suricata: alerta SID xxxx
- Wazuh: regra xxxx

## Evidência
(Log recortado, sem print inútil)

## Resultado
Alerta gerado corretamente.
