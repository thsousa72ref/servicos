# Instalação e Configuração do Wazuh no Debian

O **Wazuh** é uma plataforma de monitoramento de segurança de código aberto, utilizada para detecção de ameaças, monitoramento de integridade, resposta a incidentes, entre outros. Abaixo, segue o passo a passo para instalar o servidor Wazuh (Manager, API e Dashboard) no Debian 11/12.

---

## 1. Pré-requisitos

- Debian 11 ou 12 atualizado.
- IP fixo.
- Usuário com privilégio de root (ou sudo).
- Recursos mínimos: 4 GB RAM, 2 CPUs, 20 GB de espaço (recomendado mais para ambientes reais).
- Sincronização de horário (NTP) recomendada.

---

## 2. Atualização do Sistema

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 3. Instalação do Wazuh (All-in-One)

A Wazuh fornece um instalador automatizado que instala o manager, o dashboard (interface web) e o indexador (OpenSearch).

### a) Baixe e execute o instalador

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
chmod +x wazuh-install.sh
sudo ./wazuh-install.sh --all-in-one
```

- O processo pode demorar (10-30 minutos) dependendo do hardware e conexão.

### b) Siga as orientações do script

- O script realiza todo o processo: instalação dos componentes, configuração de certificados e inicialização dos serviços.

---

## 4. Acesso ao Dashboard

- Acesse via navegador:  
  ```
  https://<IP_DO_SERVIDOR>
  ```
- Usuário padrão:  
  ```
  Username: admin
  Password: senha exibida ao final do script de instalação
  ```
- **Obs:** O acesso é feito via HTTPS (aceite o certificado autoassinado no navegador).

---

## 5. Adicionando Agentes ao Wazuh Manager

### a) Instale o agente (exemplo para Debian/Ubuntu):

No host a ser monitorado:

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-agent-4.7.4.deb
sudo dpkg -i wazuh-agent-4.7.4.deb
```

### b) Configure o agente

Edite `/var/ossec/etc/ossec.conf` e defina o IP do manager Wazuh:

```xml
<server>
  <address>IP_DO_MANAGER</address>
  <port>1514</port>
  <protocol>tcp</protocol>
</server>
```

### c) Inicie e habilite o agente

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## 6. Verificando o Funcionamento

- No dashboard web, vá em **Agents** e verifique se o novo agente aparece como "Active".
- Logs do manager: `/var/ossec/logs/ossec.log`
- Logs do agente: `/var/ossec/logs/ossec.log`

---

## 7. Dicas de Segurança e Manutenção

- Altere a senha padrão do dashboard.
- Mantenha o sistema e o Wazuh sempre atualizados.
- Utilize firewall para restringir o acesso ao dashboard e portas dos agentes.
- Consulte a [documentação oficial](https://documentation.wazuh.com/current/pt/) para integrações avançadas (SIEM, syslog, cloud, etc).

---

## 8. Referências

- [Documentação Oficial Wazuh](https://documentation.wazuh.com/current/installation-guide/open-distro/all-in-one-deployment.html)
- [Wazuh Instalador Automático GitHub](https://github.com/wazuh/wazuh-installation-guide)