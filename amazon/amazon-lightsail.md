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


---

Aqui está um resumo claro, estruturado e didático do conteúdo para guiar seus estudos:

---

## ☁️ Aula: Padronização de Instâncias e Identificadores para o Load Balancer

O objetivo desta etapa é garantir que as duas instâncias (`Ubuntu-1` e `Ubuntu-2`) estejam **idênticas em configuração**, possuam **IPs que não mudam** e tenham **identificadores exclusivos** para sabermos qual servidor está respondendo quando instalarmos o balanceador de carga.

---

### 1. Configuração de Rede: IPs Estáticos e IPv6

Quando reiniciamos ou iniciamos uma instância que estava parada (como a `Ubuntu-1`), o provedor de nuvem costuma alterar o seu endereço IP público. Para evitar que nossa aplicação perca a comunicação, precisamos fixar esses IPs.

* **Alocando IPs Estáticos:**
1. No painel da Lightsail, inicie a `Ubuntu-1` clicando em **Start**.
2. Para cada máquina, vá em **Manage** ➡️ aba **Networking** ➡️ **Create static IP**.
3. Nomeie os IPs de forma organizada (ex: `Staticip-ubuntu1` para a primeira e `Staticip-ubuntu2` para a segunda).


* **Desabilitando o IPv6 na Ubuntu-2:**
* Como a `Ubuntu-2` veio com o IPv6 ativado por padrão (e a `Ubuntu-1` não), precisamos desativá-lo para que ambas as máquinas fiquem perfeitamente iguais.
* Acesse a aba **Networking** da `Ubuntu-2` e, em *IPv6 networking*, desabilite a opção.



---

### 2. Criando Identificadores nos Servidores (server.txt)

Para testar o funcionamento do futuro *Load Balancer*, precisamos de uma forma de diferenciar qual máquina está respondendo a cada requisição. Criaremos um arquivo de texto simples dentro do diretório do servidor web (Nginx).

#### No Servidor 1 (`Ubuntu-1`):

1. Conecte-se via terminal usando a chave SSH:
```bash
ssh -i lightsail-rmerces.pem ubuntu@IP_DA_UBUNTU_1

```


2. Navegue até a pasta pública do Nginx:
```bash
cd /var/www/html/

```


3. Crie o arquivo identificador:
```bash
sudo vi server.txt

```


* *Escreva dentro do arquivo:* `SERVIDOR UBUNTU - 1` e salve.



#### No Servidor 2 (`Ubuntu-2`):

1. Saia do primeiro servidor (`exit`) e conecte-se no segundo utilizando o IP da `Ubuntu-2`.
2. Repita o processo no diretório `/var/www/html/` criando o arquivo `server.txt`.
* *Escreva dentro do arquivo:* `SERVIDOR UBUNTU - 2` e salve.



---

### 3. Solução de Problemas Comuns (Troubleshooting)

Se ao tentar acessar no navegador o endereço `http://IP_DA_MAQUINA/server.txt` a página não carregar, revise os três pontos abaixo:

#### A. O serviço Nginx está ativo?

Após reiniciar uma máquina, o serviço pode não subir sozinho. Para ligá-lo e garantir que ele **inicie automaticamente** em futuros reboots, use:

```bash
# Garante que o Nginx inicie junto com o sistema
sudo systemctl enable nginx

# Inicia o serviço imediatamente
sudo service nginx start

```

#### B. A porta 80 está liberada no Firewall?

O Nginx responde na porta HTTP padrão (80). Verifique se a regra existe na aba **Networking** ➡️ **IPv4 Firewall**. A tabela deve conter:

* **HTTP | TCP | 80 | Any IPv4 address**
* *Se não existir, clique em **Add rule** para adicioná-la.*

---

### 🎯 Resultado Esperado

Ao final desta configuração, você terá duas instâncias isoladas e prontas. Ao testar no navegador, os acessos diretos devem retornar os identificadores corretos:

* `http://IP_ESTATICO_1/server.txt` ➡️ Exibe: **SERVIDOR UBUNTU - 1**
* `http://IP_ESTATICO_2/server.txt` ➡️ Exibe: **SERVIDOR UBUNTU - 2**

Com o ambiente nivelado e testado, a infraestrutura está pronta para receber o **Load Balancer**, que distribuirá as requisições entre esses dois servidores automaticamente.




---


Aqui está um resumo claro, estruturado e didático sobre a criação, configuração e testes de um Balanceador de Carga:

---

## ☁️ Aula: Configurando um Load Balancer e Testando Alta Disponibilidade

O **Load Balancer (Balanceador de Carga)** atua como um intermediário inteligente (ou um "guarda de trânsito") que fica à frente dos seus servidores. Ele recebe todas as requisições dos usuários e as distribui entre as instâncias disponíveis, garantindo que o ambiente suporte mais acessos e continue funcionando mesmo se uma máquina falhar.

---

### 1. Criando o Load Balancer na AWS Lightsail

1. Na tela inicial da Lightsail, acesse a aba **Networking** e clique em **Create load balancer**.
2. **Configuração Inicial:** Mantenha a opção **HTTP** (a porta padrão 80). Para HTTPS, seria necessário configurar um certificado de segurança.
3. **Identificação:** Mantenha o nome padrão `LoadBalancer-1`.
4. **Custo:** Fique atento! O Load Balancer **não faz parte do nível gratuito (free tier)** e possui um custo estimado de $18/mês. Por isso, a recomendação é excluí-lo logo após o término dos testes.
5. Clique em **Create load balancer**.

---

### 2. Vinculando Instâncias e Configurando o *Health Check*

Após criar o balanceador, você precisa apontar quais máquinas responderão por ele e como ele saberá se elas estão funcionando.

* **Target Instances (Instâncias Alvo):** Na página do balanceador, selecione a instância `Ubuntu-1` e clique em **Attach**. Em seguida, clique em **Attach another** e anexe a `Ubuntu-2`.
* **Health Check (Verificação de Integridade):** É o teste que o balanceador faz constantemente para saber se o servidor está "saudável". Se uma máquina parar de responder, o balanceador para de enviar tráfego para ela.
* **Customização:** Clique em **Customize health checking** e mude o arquivo de teste para o `server.txt` (criado na aula anterior).
* **Como funciona:** O Load Balancer tentará acessar `http://IP/server.txt`. Se a máquina responder com sucesso, ela continua na fila de distribuição.



---

### 3. Testando a Alta Disponibilidade na Prática

No topo da página do Lightsail, você encontrará o **DNS name** (um endereço web longo gerado para o seu balanceador).

1. Copie esse endereço DNS e abra-o em uma nova aba do navegador.
2. Adicione `/server.txt` ao final da URL.
3. **O Teste de Carga:** Ao atualizar (dar F5) a página várias vezes, você verá o texto alternar entre **"SERVIDOR UBUNTU - 1"** e **"SERVIDOR UBUNTU - 2"**. Isso prova que o balanceador está dividindo o trabalho entre as duas máquinas.
4. **Simulando uma Falha:** Vá até a tela inicial da Lightsail e dê um **Stop** na instância `Ubuntu-2`.
5. **O Resultado:** Ao voltar na aba do balanceador e atualizar a página, o IPv4 do Firewall ou o Health Check detectará a queda da `Ubuntu-2`. A partir daí, 100% dos acessos serão direcionados automaticamente para a `Ubuntu-1` (**"SERVIDOR UBUNTU - 1"**), mantendo o site no ar sem nenhuma interrupção para o usuário.

---

### 4. Monitoramento e Alarmes

Na aba **Metrics** do seu balanceador, você pode usar a seção **Alarms** para se prevenir:

* É possível configurar um alarme para te notificar (por e-mail ou SMS) caso o número de instâncias saudáveis mude.
* *Exemplo prático:* Criar um alerta se o ambiente tiver "menos de 2 instâncias operando por 5 minutos". Em ambientes reais, isso avisa a equipe de TI antes que o serviço caia por completo.

---

### 🛑 Limpeza de Ambiente (Evitando Cobranças)

Como o Load Balancer e IPs estáticos desalocados geram custos na nuvem, realize os seguintes passos de exclusão ao finalizar o aprendizado:

1. **Load Balancer:** Na página dele, clique nos três pontos no topo e selecione **Delete**.
2. **Instância Ubuntu-1:** Na tela inicial, clique nos três pontos e selecione **Delete** (Manteremos apenas a `Ubuntu-2` parada para as próximas aulas).
3. **Snapshots:** Na aba *Snapshots*, apague o `nginx-snapshot`.
4. **IP Estático:** Na aba *Networking*, exclua o `Staticip-ubuntu1`. **Atenção:** Manter um IP estático sem nenhuma instância vinculada a ele gera cobranças na AWS!



---- Aqui está um resumo claro, estruturado e didático sobre os tipos de armazenamento disponíveis no AWS Lightsail:

---

## 💾 Aula: Armazenamento na Nuvem – Bucket vs. Disco

Nesta etapa, o foco muda para o gerenciamento de **Storage (Armazenamento)**. A AWS Lightsail oferece duas soluções distintas para armazenar dados, cada uma com finalidades, custos e comportamentos diferentes: o **Bucket** e o **Disco**.

Antes de começar, lembre-se de iniciar sua instância de testes: na tela inicial, clique nos três pontos da `Ubuntu-2` e selecione **Start**.

---

### 1. Bucket (Armazenamento de Objetos)

O **Bucket** é um serviço de armazenamento de objetos. Ele funciona de forma análoga a serviços como Google Drive, OneDrive ou Dropbox. É uma área externa e isolada da sua máquina virtual.

* **Características:**
* **Uso ideal:** Repositório de arquivos, backups, imagens de sites (como anexos do WordPress) e documentos.
* **Vantagens:** Altíssima disponibilidade e durabilidade garantidas pela AWS, além de ser muito barato.
* **Desvantagens:** A velocidade de acesso (latência) é menor se comparada a um disco conectado diretamente no sistema operacional.


* **Preços e Planos (Exemplos):**
* 5 GB por $1/mês | 100 GB por $3/mês | 250 GB por $5/mês.


* **Particularidade do Nome:** O nome do bucket deve ser **único no mundo inteiro** (global), pois ele faz parte de um endereço de internet público (URL).
* **Como usar:** Na aba *Storage* > *Create bucket* > Escolha o plano e o nome único. Após criado, na aba **Objects**, basta arrastar e soltar arquivos do seu computador para fazer o upload.

---

### 2. Disco (Armazenamento em Bloco Adicional)

O **Disco** (Block Storage) simula um HD ou SSD físico que você compra para instalar diretamente dentro do seu servidor. É a solução ideal para quando a sua VM está ficando sem espaço em disco para o sistema ou banco de dados.

* **Características:**
* **Uso ideal:** Expandir o espaço interno da VM, instalar aplicações pesadas, bancos de dados e arquivos que exigem leitura/escrita rápida.
* **Vantagens:** Alta velocidade de acesso e baixa latência por estar "plugado" diretamente na máquina.
* **Regra de Ouro:** O disco **deve ser criado exatamente na mesma Região e Zona de Disponibilidade (AZ)** da sua instância (ex: Virgínia, Zona B). Discos e instâncias em zonas diferentes não conseguem se conectar.



#### Passo a Passo para Criação e Vínculo:

1. Acesse a aba **Storage** e clique em **Create disk**.
2. Altere a localização para a mesma da sua VM (`Virginia, Zone B`).
3. Escolha o tamanho (ex: 8 GB por $0,80/mês) e defina um nome (ex: `data`). Clique em **Create disk**.
4. **Anexando à Máquina:** Na tela do disco criado, vá em *Attach to an instance* e selecione a `Ubuntu-2`.
5. Clique em **Attach**.

> 📌 **Nota Técnica Importante:** Ao finalizar o vínculo, o Lightsail informará o caminho físico do disco (ex: `/dev/xvdf`). Guarde essa informação!

---

### 🚀 Próximo Passo

Diferente do Bucket (que já está pronto para arrastar arquivos), o **Disco adicional** acabou de ser plugado "fisicamente" no servidor virtual. Para que o sistema operacional Ubuntu consiga enxergá-lo e salvar dados nele, o próximo passo será se conectar via SSH para **formatar** (criar um sistema de arquivos) e **montar** (definir uma pasta de acesso) esse novo disco.


Aqui está um resumo claro, estruturado e didático sobre o processo de preparação e montagem de um novo disco no Linux (Ubuntu) através do AWS Lightsail:

---

## 🛠️ Aula: Formatando e Montando um Novo Disco no Linux

Quando anexamos um novo disco a uma instância na nuvem, ele se comporta como um HD recém-comprado: o sistema sabe que ele está plugado fisicamente, mas ele ainda não tem um formato e nem uma pasta associada para que você possa salvar seus arquivos.

Para torná-lo funcional, seguimos **4 etapas obrigatórias**:

---

### Etapa 1: Reconhecimento Físico

O primeiro passo é verificar se o sistema operacional reconheceu o hardware do disco anexado.

1. Conecte-se à sua instância `Ubuntu-2` usando o cliente Web do Lightsail (clicando no retângulo laranja).
2. Execute o comando para listar os dispositivos de armazenamento:
```bash
sudo sfdisk -l

```


* **O que observar:** Você verá o disco principal de boot (`/dev/xvda` com 20 GB) e o seu novo disco em branco (`/dev/xvdf` com 8 GB).


3. Se você rodar o comando `df -h` (que mostra o espaço em uso nas partições utilizáveis), o seu novo disco **não** aparecerá ali ainda, pois ele não tem uma estrutura lógica.

---

### Etapa 2: Particionamento

Particionar significa dividir o espaço físico do disco em seções lógicas. Vamos criar uma única partição utilizando todo o espaço de 8 GB.

1. Acesse o gerenciador de partições apontando para o seu novo disco:
```bash
sudo fdisk /dev/xvdf

```


2. Dentro do menu interativo do `fdisk`, digite os seguintes comandos em sequência:
* **`n`**: Cria uma nova partição.
* **`p`**: Define a partição como "Primária".
* **`Enter`** (três vezes): Confirma o número padrão da partição (1), o setor inicial e o setor final (alocando 100% do tamanho do disco).
* **`p`**: Opcional, apenas para imprimir na tela e conferir se a partição `/dev/xvdf1` foi listada.
* **`w`**: **Crucial!** Salva (escreve) as alterações no disco e sai do utilitário.



---

### Etapa 3: Formatação (Instalação do File System)

Agora que a partição existe (`/dev/xvdf1`), precisamos escolher como os dados serão organizados nela. Vamos formatá-la usando o sistema de arquivos padrão do Linux, o **ext4**.

1. Execute o comando de formatação na partição criada:
```bash
sudo mkfs.ext4 /dev/xvdf1

```



---

### Etapa 4: Montagem (Mount)

Montar significa associar a partição do disco a uma pasta (diretório) do sistema. Tudo o que for jogado dentro dessa pasta passará a ser armazenado no novo disco de 8 GB.

1. Vá para o diretório padrão de montagens do Linux:
```bash
cd /mnt

```


2. Crie uma pasta que servirá de "ponto de montagem" (vamos chamá-la de `data`):
```bash
sudo mkdir data

```


3. Realize a montagem do disco na pasta criada:
```bash
sudo mount /dev/xvdf1 /mnt/data/

```



---

### ✅ Validando o Resultado

Para garantir que tudo deu certo, execute novamente o comando de espaço em disco:

```bash
df -h

```

Dessa vez, a última linha da tabela deverá exibir o sucesso da operação:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvdf1      7.9G   36M  7.4G   1% /mnt/data

```

> ⚠️ **Atenção para o próximo passo:** Da forma como está, se a instância for reiniciada, o Linux "esquecerá" essa montagem. No próximo conteúdo, aprenderemos a automatizar esse processo editando o arquivo de inicialização do sistema (`/etc/fstab`).

Aqui está um resumo claro, estruturado e didático sobre como automatizar a montagem de discos no Linux e ajustar permissões, fechando o ciclo de gerenciamento de infraestrutura baseada em máquinas virtuais (VMs):

---

## 🛠️ Aula: Automatização de Discos, Permissões e Limpeza de Ambiente

Se você apenas montar um disco manualmente (com o comando `mount`), o Linux "esquecerá" essa configuração na próxima vez que o servidor for reiniciado. Para evitar que sua aplicação perca o acesso aos dados após uma manutenção ou queda de energia, precisamos automatizar esse processo.

---

### 1. Automatizando a Montagem com o arquivo `/etc/fstab`

O Linux utiliza um arquivo de configuração crítico chamado **`fstab` (File System Table)** para saber quais discos devem ser montados automaticamente durante a inicialização do sistema.

#### Passo a Passo:

1. Abra o arquivo com privilégios de administrador:
```bash
sudo vi /etc/fstab

```


2. Adicione uma nova linha ao final do arquivo seguindo a sintaxe do Linux (utilize a tecla `Tab` para alinhar as colunas e manter a organização):
```text
/dev/xvdf1    /mnt/data    ext4    defaults    0 1

```


* *Onde:* `/dev/xvdf1` é a partição do disco, `/mnt/data` é a pasta de destino, `ext4` é o sistema de arquivos, `defaults` são as opções padrão e `0 1` indica os parâmetros de backup e checagem do sistema.



#### ⚠️ Regra de Ouro (O Teste de Segurança)

**Nunca reinicie o servidor logo após editar o `fstab`!** Se houver qualquer erro de digitação, a máquina virtual pode travar na inicialização e ficar inacessível. Para testar com segurança, use o comando:

```bash
sudo mount -a

```

Este comando força o Linux a ler o arquivo `fstab` imediatamente. Se ele rodar sem exibir mensagens de erro, sua configuração está perfeita e você pode verificar o sucesso digitando `df -h`.

---

### 2. Ajustando Permissões de Acesso

Por padrão, quando um novo disco é montado no diretório `/mnt`, o dono absoluto dele é o usuário administrador do sistema (`root`). Se o seu usuário padrão (`ubuntu`) tentar criar um arquivo lá dentro, ele receberá o erro **"Permission denied"**.

Para corrigir isso e permitir que suas aplicações salvem dados no novo disco, precisamos alterar o proprietário (*owner*) da pasta:

1. Vá até o diretório correspondente: `cd /mnt`
2. Altere o dono da pasta de forma recursiva (`-R`, aplicando a subpastas e arquivos futuros) para o usuário e grupo `ubuntu`:
```bash
sudo chown -R ubuntu:ubuntu data

```



Agora, o acesso está totalmente liberado para o uso diário.

---

### 🛑 Faxina Final e Desmobilização de Recursos

Antes de avançar para o próximo módulo do curso (Contêineres), é fundamental apagar toda a infraestrutura de VMs para **evitar cobranças indesejadas** no cartão de crédito.

Siga esta ordem lógica para conseguir excluir tudo na AWS Lightsail:

1. **Bucket:** Na aba *Storage*, clique nos três pontos do bucket ➡️ *Delete* ➡️ marque a caixa de aviso e clique em *Force delete*.
2. **Disco Adicional:** Você não consegue deletar um disco que está sendo usado.
* Vá em *Manage* no disco `data`.
* Clique em **Detach** (Desanexar). O sistema pedirá para desligar a instância (`Stop instance`).
* Após a máquina parar, confirme em **Yes, detach**.
* Retorne à aba *Storage* e agora sim, delete o disco `data`.


3. **Máquina Virtual (VM):** Na aba *Instances*, clique nos três pontos da `Ubuntu-2` e selecione *Delete*.
4. **IP Estático:** Na aba *Networking*, exclua o `Staticip-ubuntu-2` (lembre-se: IP estático desalocado gera custos!).

Com a conta limpa e zerada, a infraestrutura está pronta para a próxima etapa: **Contêineres gerenciados**!


---

Aqui está um resumo claro, estruturado e didático sobre o funcionamento e o provisionamento de contêineres no AWS Lightsail:

---

## 📦 Aula: Introdução a Contêineres e Deploy no AWS Lightsail

Muitas aplicações modernas estão migrando de Máquinas Virtuais (VMs) para **Contêineres** devido à sua leveza, agilidade e facilidade de replicação. Enquanto a VM mais barata do Lightsail custa $3,50/mês, o plano inicial de contêineres custa **$7,00/mês por node**, mas permite rodar múltiplos contêineres com gerenciamento automático e escalabilidade rápida.

---

### 1. Entendendo a Origem: O Dockerfile e a Imagem

Para rodar um contêiner, precisamos de uma **Imagem** (um pacote com tudo o que a aplicação precisa para funcionar). Essa imagem é gerada a partir de um arquivo de receita chamado **Dockerfile**.

#### Exemplo Prático de Infraestrutura (Apache):

O projeto utilizado nesta aula cria um servidor web Apache sobre o Ubuntu:

* **Base:** Parte do `ubuntu:18.04`.
* **Instalação:** Atualiza o sistema e instala o servidor `apache2`.
* **Cópia de Arquivos:** Move o código do site (`src/index.html`) para a pasta pública do Apache (`/var/www/html/`).
* **Porta:** Expõe a porta `80` (HTTP).
* **Execução:** Define que o Apache deve iniciar automaticamente e rodar em primeiro plano.

#### Fluxo de Publicação da Imagem:

Se você quiser customizar o site, o fluxo padrão do Docker é:

1. **Baixar o código:** `git clone <repositorio>`
2. **Construir a imagem local:** `docker build .` (Gera um ID, ex: `14f35de79667`)
3. **Etiquetar/Nomear:** `docker tag 14f35de79667 rmerces/apache-labs`
4. **Enviar para a nuvem (Registry):** `docker push rmerces/apache-labs` (Disponibiliza a imagem publicamente no DockerHub).

---

### 2. Provisionando o Serviço de Contêiner no Lightsail

Com a imagem pronta e hospedada no DockerHub, o deploy no Lightsail é feito de forma visual e simplificada:

1. Na tela inicial do Lightsail, acesse a aba **Containers** e clique em **Create container service**.
2. **Capacidade (Power):** Selecione o plano **Nano ($7/mês)** e defina a quantidade de nós como `1`.
3. **Configuração de Deploy:** Clique em *Set up deployment* e marque a opção **Specify a custom deployment**.
4. **Identificação da Imagem:**
* **Container name:** `apache-labs`
* **Image:** `docker.io/rmerces/apache-labs` (Endereço oficial do DockerHub).



---

### 3. Liberando o Acesso (Portas e Endpoint)

Para que as pessoas consigam acessar o site que está dentro do contêiner, precisamos abrir as portas de comunicação:

* **Open Ports (Portas Abertas):** Na seção *Configuration*, clique em **Add open ports** e defina:
* **Port:** `80`
* **Protocol:** `HTTP`


* **Public Endpoint (Ponto de Acesso Público):** No campo correspondente, selecione o contêiner `apache-labs`. Isso diz à AWS que quem digitar o endereço web do serviço deve ser direcionado diretamente para a porta 80 deste contêiner.
* **Finalização:** No campo *Identify your service*, digite `apache-labs` e clique em **Create container service**.

---

### 🎯 O que esperar a seguir?

O Lightsail iniciará o processo de provisionamento (status *Pending* ou *Deploying*). Após alguns minutos, a AWS gerará uma URL pública e segura para o seu serviço. Ao acessá-la, você verá a página HTML customizada com o título **"Rmerces LABS"** e a mensagem **"TESTE SITE"**, confirmando o sucesso do deploy do contêiner.


---

Aqui está um resumo claro, estruturado e didático sobre o gerenciamento de versões, deploy contínuo e *rollback* de contêineres no AWS Lightsail:

---

## 📦 Aula: Deploy Contínuo, Controle de Versões e Rollback no Lightsail

Uma das maiores vantagens de utilizar o serviço de contêineres do AWS Lightsail é o **gerenciamento automatizado de versões**. Quando atualizamos o código da nossa aplicação, o Lightsail cuida da transição sem tirar o sistema do ar e mantém um histórico que permite reverter erros em segundos.

---

### 1. Verificando o Primeiro Deploy

Assim que o Lightsail conclui o processamento inicial, o status da aplicação muda para **"Active"** no histórico de deploys.

* No topo da página, a AWS disponibiliza um **Public Domain** (um link público seguro).
* Ao clicar nesse link, a página abrirá exibindo o conteúdo original da nossa imagem Docker: **"TESTE SITE"**.

---

### 2. Atualizando a Aplicação (Nova Versão)

Imagine que o cliente pediu uma alteração no código do site. O fluxo para atualizar o contêiner segue estes passos no terminal e na nuvem:

#### Passo 1: Atualizar o código localmente

1. Entre na pasta do código: `cd src`
2. Abra o arquivo HTML (`vi index.html`) e altere o texto de `<h1>TESTE SITE</h1>` para `<h1>TESTE SITE 2</h1>`. Salve e saia.

#### Passo 2: Build e Push para o Docker Hub

Volte para a raiz do projeto e envie a nova versão para o seu repositório:

```bash
cd ..
docker build .
docker tag [NOVO_ID_DA_IMAGEM] rmerces/apache-labs
docker push rmerces/apache-labs

```

#### Passo 3: Atualizar no Lightsail (Redeploy)

1. No painel do Lightsail, vá até o tópico **Deployment versions**.
2. Clique nos três pontos ao lado da versão ativa e selecione **Modify and redeploy** (e confirme em *Yes, continue*).
3. Como o endereço da imagem no Docker Hub continua o mesmo, você não precisa alterar nenhum campo. Apenas role até o final e clique em **Save and deploy**.

---

### 3. Histórico e o Poder do *Rollback*

Após o novo deploy, a tabela de versões será atualizada automaticamente:

* **Versão 2:** Status **Active** (Versão atual com o texto "TESTE SITE 2").
* **Versão 1:** Status **Inactive** (Versão antiga guardada no histórico).

Se você acessar o mesmo link público de antes e atualizar a página, verá o texto atualizado para **"TESTE SITE 2"**.

> 🔄 **O que é Rollback?** Se a nova versão (Versão 2) apresentar algum bug crítico em produção, você não precisa refazer o código correndo. Basta clicar nos três pontos da **Versão 1 (Inativa)**, selecionar *Modify and redeploy* e ativá-la novamente. O Lightsail reverte o site para o estado anterior de forma imediata.

---

### 🛑 Faxina Final do Curso

Para garantir que você não receba cobranças indesejadas no seu cartão de crédito (já que o serviço de contêineres possui custo fixo de $7/mês), certifique-se de apagar o ambiente ao concluir os estudos:

1. Vá para a tela inicial da Lightsail e clique na aba **Containers**.
2. Clique nos três pontos no canto superior direito do serviço `apache-labs` e selecione **Delete**.
3. Confirme clicando em **Yes, delete**.

> 🔍 **Dica de Ouro:** Navegue por todas as abas da tela inicial (*Instances, Databases, Networking, Storage*) para garantir que nenhum recurso criado ao longo das aulas ficou esquecido para trás.

Parabéns por concluir essa jornada! Esse curso cobriu os principais pilares de infraestrutura e serviços em nuvem usando a AWS Lightsail.

Aqui está um grande resumo consolidado de tudo o que você aprendeu e dominou ao longo dessas aulas, servindo como o seu mapa mental definitivo dessa tecnologia:

---

## 🗺️ Mapa de Aprendizado: O que você dominou neste curso

```
                                  [ AWS LIGHTSAIL ]
                                          |
    +-------------------+-----------------+-----------------+-------------------+
    |                   |                 |                 |                   |
[ Clicar e Rodar ]    [ Computação / VM ] [ Armazenamento ] [ Alta Disponibil.] [ Contêineres ]
    |                   |                 |                 |                   |
 • WordPress         • Ubuntu (Instância) • Buckets (S3)    • Snapshots         • Nós (Nodes) Nano
                     • IP Estático        • Discos (/dev/xvdf)• Load Balancer     • Controle Versões
                     • IPv4 Firewall      • Montagem (fstab)• Zonas (AZ A/B)    • Rollback Rápido

```

---

### 🚀 1. Facilidade de Provisionamento (Click-to-Launch)

Você viu na prática como o Lightsail elimina a complexidade da nuvem tradicional ao permitir o deploy de aplicações prontas, como o **WordPress**, em questão de poucos cliques, configurando servidor web e banco de dados automaticamente.

### 💻 2. Computação e Gerenciamento de Instâncias (VMs)

Você aprendeu a criar, gerenciar e conectar-se a uma máquina virtual Linux (**Ubuntu**). Além disso, dominou conceitos cruciais de rede e segurança:

* **IPs Estáticos:** Para evitar a perda de comunicação com o servidor quando ele é reiniciado.
* **Firewall IPv4:** Liberando e controlando portas essenciais como a **22 (SSH)** e a **80 (HTTP)**.

### 💾 3. Armazenamento na Nuvem (Storage)

Você diferenciou e aprendeu a configurar as duas principais formas de guardar dados:

* **Buckets:** Armazenamento de objetos, ideal para arquivos de mídia e backups, focado em baixo custo e alta durabilidade global.
* **Discos Adicionais (Block Storage):** Perfeitos para expandir o espaço interno da VM. Você realizou tarefas reais de um Administrador Linux: *reconhecer, particionar (`fdisk`), formatar (`ext4`), montar (`mount`)* e automatizar a inicialização editando com segurança o arquivo crítico `/etc/fstab`.

### ⚖️ 4. Alta Disponibilidade e Resiliência

Você elevou o nível da infraestrutura ao criar arquiteturas preparadas para falhas:

* **Snapshots:** Utilizados como cópias de segurança e como moldes para clonar servidores idênticos em minutos.
* **Zonas de Disponibilidade (AZs):** Distribuindo máquinas entre a *Zona A* e *Zona B* para proteção contra desastres físicos em data centers.
* **Load Balancer (Balanceador de Carga):** O cérebro que distribui o tráfego de usuários e monitora a saúde dos servidores através de *Health Checks* customizados (utilizando o arquivo `server.txt`).

### 📦 5. Modernização com Contêineres

Por fim, você entrou no mundo DevOps ao provisionar um serviço gerenciado de contêineres:

* Entendeu o fluxo de uma imagem Docker (do *Dockerfile* ao *Docker Hub*).
* Fez o deploy de uma aplicação Apache na AWS sem precisar gerenciar o sistema operacional por baixo.
* Aprendeu como o Lightsail lida com o **controle de versões**, permitindo atualizações de código contínuas e o recurso de **Rollback** (reverter para uma versão anterior estável em segundos se algo der errado).

---

### 🛡️ Sua principal soft skill desenvolvida: FinOps (Cultura de Custo)

Ao longo de cada módulo, você desenvolveu uma mentalidade essencial para quem trabalha com nuvem: **a consciência de custos**. Você aprendeu a revisar os planos, entender as taxas de recursos desalocados (como IPs estáticos órfãos) e adquiriu a excelente prática de fazer a "faxina técnica" ao final de cada experimento.

O conhecimento adquirido aqui abre as portas para você desenhar arquiteturas de sistemas ainda mais complexas e eficientes. Muito sucesso nos seus próximos passos e bons estudos! Até o próximo curso! 🚀