# 📘 Implementando Monitoramento no Azure – AZ-104

Este projeto demonstra um fluxo de **monitoramento no Azure** usando **Azure Monitor** e **Log Analytics**, dentro de um grupo de recursos chamado `az104-rg11`.

O objetivo é detectar a **exclusão de uma máquina virtual** (`az104-11-vm0`) e executar ações automáticas como o envio de um e-mail.

![imagem1](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/1.png)
---

## 🧩 Cenário

Queremos monitorar a exclusão da máquina virtual `az104-11-vm0` no Azure. Para isso, utilizamos os seguintes recursos:

- ✅ Azure Monitor  
- ✅ Log Analytics Workspace  
- ✅ Regras de Alerta (Alert Rules)  
- ✅ Grupo de Ações (Action Group)  
- ✅ Consulta KQL (Kusto Query Language)  

---

## ✅ Etapas do Laboratório

### 🔹 Etapa 1: Criar um Resource Group

1. No portal do Azure, vá para **"Resource groups"**
- ![imagem2](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/2.png)
2. Clique em **"Criar"**
- ![imagem3](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/3.png)
3. Preencha:
   - **Subscription**
   - **Resource Group Name**: `az104-rg11`
   - **Região**
4. Clique em **Review + Create > Create**
- ![imagem4](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/4.png)

---

### 🔹 Etapa 2: Fazer o Deploy do Template JSON

1. No portal, pesquise por **“Custom deployment”**
- ![imagem5](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/5.png)
2. Clique em **"Build your own template in the editor"**
- ![imagem6](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/6.png)
3. Clique em **upload file** e faça upload do arquivo `template.json` (pasta `az-104` do seu repositório)
- ![imagem7](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/7.png)
4. Clique em **Save**
- ![imagem8](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/8.png)
5. Preencha:
   - **Resource Group**: `az104-rg11`
   - **Admin Username e Password**
- ![imagem9](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/9.png)
6. Clique em **Review + Create > Create**

> 💡 Isso criará toda a infraestrutura necessária para o monitoramento.

---

### 🔹 Etapa 3: Habilitar Monitoramento na VM

1. Pesquise por **“Monitor”**
2. Acesse **Virtual Machines Insights**
- ![imagem10](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/10.png)
3. Localize a VM `az104-11-vm0` (deve aparecer como *não monitorada*)
4. Clique em **Enable**
- ![imagem11](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/11.png)
5. Clique em **Enable** novamente
- ![imagem12](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/12.png)
6. Clique em **Configure** (sem alterar nada)
- ![imagem13](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/13.png)
7. Aguarde a instalação do agente
- ![imagem14](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/14.png)
---

### 🔹 Etapa 4: Criar Regra de Alerta

1. No painel do **Monitor**, vá até **Alerts**
- ![imagem15](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/15.png)
2. Clique em **Create > Alert rule**
- ![imagem16](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/16.png)
3. Escolha o **Resource Group `az104-rg11`** como escopo
- ![imagem17](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/17.png)
4. Clique em **Aplicar**
- ![imagem18](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/18.png)
---

### 🔹 Etapa 5: Definir Condição

1. Em **Condition**, clique em **"See all signals"**
- ![imagem19](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/19.png)
2. Pesquise e selecione **"Delete Virtual Machine"**, ou use a seguinte consulta KQL:
- ![imagem20](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/20.png)
  ou
```kusto
AzureActivity 
| where OperationNameValue == "Microsoft.Compute/virtualMachines/delete"
| where Resource == "az104-11-vm0"
```

### 🔹 Etapa 6: Criar Grupo de Ação

1. Em Action, selecione Use existing ou Create Action Group
- ![imagem21](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/21.png)
2. Preencha:
   - Resource Group: az104-rg11
   -  Region
   -  Action Group Name e Display Name
- ![imagem22](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/22.png)
3. Em Notifications, selecione:
  - Email/SMS/Push/Voice
  - Nome da notificação
  - Clique no lápis ✏️ e insira seu e-mail
- ![imagem23](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/23.png)
4. Aba Actions: não precisa configurar nada agora
5. Tags: opcionais
6. Clique em Review + Create > Create
- ![imagem24](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/24.png)
### 🔹 Etapa 7: Testar o Alerta
1. Vá até a VM az104-11-vm0 no Resource Group
2. Clique em Delete
- ![imagem25](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/25.png)
3. A exclusão da VM acionará o alerta configurado
4. Verifique:
   - Seu e-mail para a notificação
   - O menu Monitor > Alerts
- ![imagem26](https://github.com/DurezahGeek/Implementando-Monitoramento-no-Azure-AZ-104/blob/main/src-Monitoramento/26.png)
