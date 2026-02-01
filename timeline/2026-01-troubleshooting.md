# Erros Encontrados e Correções
> Surgiram alguns erros após a implementação do suricata devido a recomendações de configurações incondizentes com o permitido e algumas desatenções. 
## ❌ Erro – Linha 284 (YAML)

```text
Failed to parse configuration file at line 284: did not find expected '-' indicator
```

**Causa**: ausência de espaço após a alteração da interface de rede.  
✔️ Corrigido ajustando indentação.

---

## ❌ Erro – Linha 775

```text
Failed to parse configuration file at line 775: did not find expected key
```

**Causa**: configuração sugerida na documentação **não é aceita** pela versão do Suricata utilizada.

Configuração problemática:

```yaml
default-rule-path: /etc/suricata/rules
rule-files:
  - "*.rules"
```

✔️ Foi necessário remover/ajustar essa seção.

---

## ❌ Erro – Keyword não suportada

> A keyword **não é suportada pela sua versão do Suricata**

Isso ocorre quando:

- Regras **mais novas**
- Suricata **mais antigo**
- Parser (ex: **Modbus**) desativado
    

---

## ⚠️ Warning – Stats (modo legado)

```text
global stats config is missing. Stats enabled through legacy stats.log
```

**Motivo**:
- Seção `stats:` não definida explicitamente no `suricata.yaml`
- Suricata entrou em modo legado (`stats.log`)


✔️ Correção aplicada:

```yaml
- stats:
    enabled: no
    filename: stats.log
    interval: 8
```
