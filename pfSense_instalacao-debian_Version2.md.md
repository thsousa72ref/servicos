# Instalação do pfSense no Ambiente Debian

O **pfSense** é uma distribuição própria baseada em FreeBSD, projetada para funcionar como firewall/router, e **não pode ser instalada diretamente sobre o Debian**. Porém, é possível utilizar pfSense em ambientes onde Debian está presente, por exemplo, em uma máquina virtual (KVM, VirtualBox, VMware) hospedada no Debian.

Este guia detalha como instalar o pfSense em uma máquina virtual no Debian, usando o VirtualBox como exemplo.

---

## 1. Pré-requisitos

- Debian atualizado, com interface gráfica (opcional, mas facilita o uso do VirtualBox).
- Permissões de root/sudo.
- Conexão à internet.
- VirtualBox instalado.

---

## 2. Baixando o pfSense

1. Acesse o site oficial:  
   [https://www.pfsense.org/download/](https://www.pfsense.org/download/)

2. Selecione:
   - Arquitetura: AMD64 (x86-64)
   - Installer: ISO Installer
   - Mirror: Escolha o mais próximo de você

3. Baixe o arquivo `.iso`.

---

## 3. Instalando o VirtualBox (se necessário)

```bash
sudo apt update
sudo apt install virtualbox -y
```

---

## 4. Criando a Máquina Virtual para o pfSense

1. Abra o VirtualBox.
2. Clique em **Novo** e preencha:
   - Nome: pfSense
   - Tipo: BSD
   - Versão: FreeBSD (64-bit)
3. Defina a memória (recomenda-se pelo menos 1 GB).
4. Crie um disco rígido virtual (mínimo 10 GB).
5. Configure as placas de rede:
   - **Adapter 1:** “Bridged Adapter” (para que o pfSense tenha acesso direto à rede)
   - (Opcional) **Adapter 2:** “Internal Network” (para separar a LAN da WAN)

6. Aponte o drive óptico para o arquivo ISO do pfSense baixado.

---

## 5. Instalando o pfSense

1. Inicie a VM e siga o assistente de instalação do pfSense.
2. Aceite os termos e selecione as opções padrão.
3. Após a instalação, remova a mídia ISO e reinicie a VM.

---

## 6. Configurando o pfSense

- Na primeira inicialização, configure as interfaces de rede (WAN/LAN).
- Acesse o pfSense via navegador em um computador da rede LAN:
  ```
  https://IP_LAN_PFSENSE
  ```
  (Usuário padrão: `admin` / Senha: `pfsense`)

---

## 7. Dicas e Observações

- pfSense **NÃO** roda como um pacote ou serviço dentro do Debian, mas sim como um sistema independente, podendo ser virtualizado em um host Debian.
- Para ambientes de produção, prefira virtualização com KVM/QEMU ou instalação em hardware dedicado.
- Sempre mantenha o pfSense atualizado.

---

## 8. Referências

- [Documentação Oficial pfSense](https://docs.netgate.com/pfsense/en/latest/)
- [Guia Rápido de Instalação pfSense](https://www.pfsense.org/download/)
- [Instalação do VirtualBox no Debian](https://wiki.debian.org/VirtualBox)