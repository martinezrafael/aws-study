### Amazon Lightsail: Descomplicando a nuvem

- Entenda como provisionar de forma simples web apps e instâncias
- Crie um ambiente de alta disponibilidade de forma simples
- Aumente a capacidade de armazenamento de sua instância com discos adicionais
- Aprenda a utilizar snapshots e crie VMs a partir deles
- Aprenda sobre o gerenciamento de containers dentro do ambiente Lightsail

---

## Resumo do Curso: AWS Lightsail

* **Instrutor:** Ricardo Merces.
* **O que é o AWS Lightsail:** Uma tecnologia que simplifica o uso do console da AWS por meio de uma interface simples e amigável.
* **Recursos disponíveis:** Permite gerenciar máquinas virtuais, armazenamento (*storage*), balanceadores de carga (*load balancers*), deploy de aplicações com poucos cliques e gerenciamento de contêineres.
* **Foco prático do curso:** Criar máquinas virtuais, realizar o deploy de aplicações e explorar as principais funcionalidades da ferramenta.

---

## 📌 Pré-requisitos

* **Acesso à AWS:** É necessário ter uma conta ativa na AWS.
* **Atenção ao *Free Tier* (Nível Gratuito):** Fique atento às regras do nível gratuito para entender o que é cobrado e o que é isento de custos.

---

## 🚀 Como Acessar o AWS Lightsail

1. Faça login no **Console AWS**.
2. Na barra de pesquisa superior, digite **"Lightsail"** e selecione a primeira opção.
3. Você será direcionado para uma interface exclusiva, simples e objetiva, dividida em 6 abas principais:
* **Instances** (Instâncias/Máquinas Virtuais)
* **Containers** (Contêineres)
* **Databases** (Bancos de Dados)
* **Networking** (Redes)
* **Storage** (Armazenamento)
* **Snapshots** (Cópias de segurança/Backups)

---

## 💻 Criando sua Primeira Instância (WordPress)

O foco inicial é colocar uma aplicação no ar rapidamente para entender a simplicidade da ferramenta.

### Passo 1: Iniciar a criação

Na aba **Instances**, clique no botão **"Create instance"**.

### Passo 2: Escolher o Modelo (*Blueprint*)

No campo "Select a blueprint", existem duas opções:

* **OS Only:** Instala apenas o Sistema Operacional limpo (Linux/Windows).
* **Apps + OS:** Instala o Sistema Operacional já com uma aplicação configurada.

> **Ação:** Selecione **"Apps + OS"** e clique em **WordPress**.

### Passo 3: Plano e Precificação (*Instance Plan*)

* Deixe a chave SSH no padrão (*default*).
* Escolha o plano que melhor se adapta ao seu bolso ou necessidade.
* O plano inicial custa **$3,50/mês** (1 CPU e 512 MB de RAM) e oferece **3 meses gratuitos** para contas novas no *Free Tier*.

> **Ação:** Selecione o plano de **$3,50**.

### Passo 4: Finalizar

* Mantenha o nome padrão da máquina (**"WordPress-1"**).
* Clique no botão **"Create instance"** no final da página.

---

Aqui está um resumo claro, didático e focado nos pontos mais importantes desse aprendizado prático:

---

## 🔑 1. Acessando a Aplicação e Pegando as Credenciais

Assim que a instância do WordPress termina de ser criada, você já pode acessá-la:

* **Acesso público:** Basta copiar o **IP público** que aparece no cartão da instância e colá-lo no navegador.
* **Painel Administrativo:** Para gerenciar o WordPress, adicione `/wp-admin` ao final do IP no navegador.

### Como descobrir a senha do WordPress:

Em vez de configurar chaves e abrir terminais complexos, o Lightsail oferece um **cliente SSH Web** direto no navegador (um ícone de terminal no canto da instância).

1. Clique no ícone do **SSH Web** para abrir o terminal da máquina.
2. Digite o comando: `cat bitnami_credentials`
3. O terminal exibirá o usuário padrão (`user`) e a senha gerada automaticamente.

---

## 📌 2. Configurando um IP Estático (Indispensável para Produção)

Por padrão, se uma instância for reiniciada ou recriada, o IP público muda. Para evitar que seu site saia do ar, é necessário fixar um IP.

* **Como fazer:** Vá em `Manage` (gerenciar a instância) > aba `Networking` > clique em **"Create static IP"**. Dê um nome amigável (ex: `Staticip-wp`) e clique em **Create**.
* **Regra de ouro dos custos 💰:** O IP estático é **gratuito** desde que esteja **associado a uma instância ativa**. Se você criar um IP estático e deixá-lo solto (sem nenhuma máquina vinculada), a AWS cobrará por ele.

---

## 🌐 3. Vinculando um Domínio (DNS)

Para que os usuários não precisem digitar um número de IP para acessar o site, vinculamos um nome de domínio (ex: `seusite.com`).

1. **Comprar o domínio:** O instrutor utiliza o site *Namecheap* (uma opção externa e barata para testes).
2. **Configurar o DNS:** Dentro do painel do seu domínio (na aba de DNS Avançado), adicione um novo registro:
* **Type:** `A Record` (Aponta um nome para um IP)
* **Host:** `www` (ou `@` para o domínio principal)
* **Value:** O seu **IP Estático** do Lightsail.
* **TTL:** `1 min` (apenas para o teste propagar rápido; em produção o padrão é 60 min).



> ⏱️ **Nota:** Após salvar, o DNS leva alguns minutos para propagar. Você pode testar no terminal usando o comando `dig [www.seu-dominio.com](https://www.seu-dominio.com)` para checar se ele já responde com o IP correto.

---
Aqui está um resumo claro, didático e focado nos principais pontos da aula para ajudar nos seus estudos:

---

## 🧼 1. Como Eliminar uma Instância e Fazer a "Faxina"

Para evitar cobranças surpresas, é essencial limpar todos os recursos associados ao excluir um projeto.

1. **Excluir a Instância:** Na tela inicial do Lightsail, clique nos três pontos no cartão da máquina virtual e selecione **"Delete"**. Confirme a ação.
2. **Excluir o IP Estático:** Apagar a máquina **não** apaga o IP estático automaticamente. Como IPs estáticos sem uso geram custos, vá até a aba **"Networking"**, clique nos três pontos sobre o IP estático criado e selecione **"Delete"**.

---

## 🌍 2. Infraestrutura Global: Regiões e Zonas de Disponibilidade (AZs)

Antes de criar qualquer máquina virtual (VM), você deve definir onde ela ficará hospedada fisicamente:

* **Região (Region):** É a localização geográfica geral (ex: *Virginia - us-east-1*).
* **Zona de Disponibilidade (Availability Zone - AZ):** São os data centers físicos isolados dentro de uma mesma região (ex: *Zone A - us-east-1a*).
* **Conceito de Alta Disponibilidade:** Criar aplicações distribuídas em mais de uma zona garante que, se um data center cair devido a uma falha física, os outros mantêm seu site no ar.

---

## 🛠️ 3. Criando uma Máquina Virtual Limpa (Ubuntu Linux)

Em vez de usar uma aplicação pronta (como o WordPress), o instrutor cria uma máquina virtual pura:

* **Plataforma:** Linux/Unix.
* **Blueprint:** "OS Only" ➔ **Ubuntu 20.04 LTS**.
* **Localização:** Mantida no padrão (Virginia / Zone A).

---

## 🔑 4. Gerenciamento e Conexão via Chaves SSH

Para se conectar com segurança a um servidor Linux, a AWS utiliza pares de chaves criptográficas (`.pem`).

### Criando e baixando a chave:

1. No menu de criação, vá em *Change SSH key pair* > **"Create New"**.
2. Dê um nome (ex: `lightsail-rmerces`) e clique em **"Generate key pair"**.
3. **Atenção:** Faça o download da chave privada imediatamente. A AWS só permite o download **uma única vez**. Se perder, precisará criar outra.
4. Conclua a criação da instância (nomeada como `Ubuntu-1`).

### Conectando via Terminal Local (Passo a Passo):

Uma vez baixada a chave no seu computador, siga os comandos:

1. **Ajustar as permissões (Obrigatório no Linux/Mac):** O arquivo de chave não pode ser público. Defina permissão de leitura apenas para o dono:
```bash
chmod 400 lightsail-rmerces.pem

```


2. **Conectar ao servidor:** Use o comando `ssh`, apontando o arquivo da chave, o usuário padrão do sistema (`ubuntu`) e o IP da máquina:
```bash
ssh -i lightsail-rmerces.pem ubuntu@SEU_IP_AQUI

```



> 💡 **Dica Prática:** Se você não quiser lidar com chaves SSH no seu terminal local ou precisar passar o acesso rápido para outra pessoa, o **botão do terminal (SSH Web)** no painel do Lightsail continua funcionando perfeitamente a um clique de distância.

---


Aqui está um resumo claro, estruturado e didático do conteúdo para fixação ou guia de estudos:

---

## ☁️ Aula: Snapshots, Balanceadores de Carga e Alta Disponibilidade

Nesta etapa, o objetivo é evoluir a infraestrutura da aplicação utilizando dois serviços essenciais junto à máquina virtual (VM): **Snapshots** (para cópias de segurança e replicação) e **Load Balancers** (para distribuição de tráfego).

---

### 1. O que é e como criar um Snapshot?

Um **Snapshot** é uma cópia de segurança (backup) do estado da sua máquina virtual em um determinado momento.

> ⚠️ **Boa prática:** Sempre que possível, **pare a instância** antes de tirar um snapshot para evitar a corrupção de dados ativos.

#### Passo a Passo na AWS Lightsail:

1. Na tela inicial, clique nos três pontos da instância (`Ubuntu-1`) e selecione **Manage**.
2. Clique em **Stop** (e confirme) para desligar a máquina com segurança.
3. Quando o status mudar para **Stopped**, acesse a aba **Snapshots**.
4. Em *Manual snapshots*, clique em **Create snapshot**.
5. Altere o nome para `nginx-snapshot` e clique em **Create**.

**A Estratégia:** Esse snapshot servirá como um "molde" (template). Em vez de configurar uma nova VM do zero, criaremos uma segunda instância idêntica a partir dele, economizando tempo.

---

### 2. Alta Disponibilidade e Zonas de Disponibilidade

**Alta Disponibilidade (High Availability)** significa garantir que a sua aplicação continue funcionando e acessível para os usuários mesmo se uma das máquinas ou parte da infraestrutura sofrer uma falha crítica.

* **Regiões e Zonas:** Os provedores de nuvem (como a AWS) dividem sua infraestrutura em Regiões (ex: Virgínia) e cada região possui múltiplas **Zonas de Disponibilidade (Availability Zones - AZs)**, que são data centers isolados fisicamente.
* **Estratégia de Resiliência:** Para garantir a alta disponibilidade, não basta ter duas máquinas; elas devem ser distribuídas em **zonas diferentes** (ex: Zona A e Zona B). Se a Zona A cair por um problema elétrico ou climático, a máquina da Zona B assume o tráfego.

---

### 3. Criando a Segunda Instância em Outra Zona

Utilizando o molde criado no passo 1, vamos provisionar a segunda VM em uma zona distinta:

1. Na aba **Snapshots**, clique nos três pontos ao lado do `nginx-snapshot` criado.
2. Selecione **Create new instance**.
3. No campo **Select an Availability Zone**, altere a localização da nova máquina para a **Zone B** (garantindo a separação física da `Ubuntu-1`, que está na Zona A).
4. Defina o nome da nova instância como `Ubuntu-2`.
5. Mantenha a mesma chave SSH (`lightsail-rmerces`) e clique em **Create instance**.

---

### 🚀 Próximo Passo

Com as duas máquinas idênticas (`Ubuntu-1` na Zona A e `Ubuntu-2` na Zona B) rodando em paralelo, o próximo passo será configurar o **Load Balancer (Balanceador de Carga)** na aba *Networking*. Ele ficará à frente dessas instâncias, recebendo as requisições dos usuários e distribuindo o tráfego entre elas de forma inteligente.


