# 📖 GUIA DE CAMPO: IMPLEMENTAÇÃO DE INFRAESTRUTURA NA AWS LIGHTSAIL

**Subtítulo:** Da Instância Monolítica à Arquitetura Resiliente em Contêineres com Foco em FinOps

**Autor:** Engenharia de Sistemas & Cloud

**Ano:** 2026

---

## 🧭 INTRODUÇÃO

Este e-book é um manual prático de implementação projetado para engenheiros de sistemas, administradores de nuvem e desenvolvedores. O objetivo é guiar você através do provisionamento, proteção, escalabilidade e modernização de cargas de trabalho utilizando o **AWS Lightsail**, mantendo o foco rigoroso na alta disponibilidade e no controle de custos (*FinOps*).

---

## 🛠️ CAPÍTULO 1: DEPLOY ACELERADO E EXTRAÇÃO DE CREDENCIAIS

O primeiro nível de implementação consiste em colocar uma aplicação estruturada no ar rapidamente utilizando os modelos prontos da AWS (*Blueprints*).

### Checklist de Implementação:

1. Faça login no **Console AWS** e pesquise por **Lightsail**.


2. Na aba **Instances**, clique em **Create instance**.


3. Em *Select a blueprint*, selecione **Apps + OS** e escolha **WordPress**.


4. Selecione o plano de **$3.50/mês** (elegível para o nível gratuito).


5. Nomeie a instância como `WordPress-Prod-01` e clique em **Create instance**.



### Script de Captura de Credenciais (SSH Web):

Assim que o status mudar para *Running*, clique no ícone do **Terminal SSH Web** integrado e execute o comando abaixo para extrair a senha administrativa gerada pela Bitnami:

```bash
# Executar na raiz do terminal para ler as credenciais nativas
cat bitnami_credentials

```

*Copie o usuário (`user`) e a senha exibidos para acessar o painel em `http://IP_PUBLICO/wp-admin*`.

---

## 🌐 CAPÍTULO 2: IMPLEMENTAÇÃO DE REDE, IP ESTÁTICO E DNS

Instâncias em nuvem nascem com IPs públicos dinâmicos que mudam a cada reboot. Para ambientes de produção, é obrigatório fixar esse endereço e acoplar um domínio.

### Passo a Passo Técnico:

1. Acesse a instância criada, vá na aba **Networking** e clique em **Create static IP**.


2. Vincule-o à sua instância e dê o nome de `StaticIP-WordPress`.



### 💰 Alerta de FinOps (IPs Órfãos):

> **REGRA DE OURO:** O IP Estático é gratuito enquanto estiver anexado a uma máquina ativa. Se você deletar a máquina virtual e esquecer o IP estático alocado na sua conta, a AWS cobrará por ele por hora. Sempre limpe seus recursos de rede ao desmobilizar um projeto.
> 
> 

### Mapeamento de DNS Externo:

Acesse o painel do seu registrador de domínio (ex: *Namecheap*) e adicione uma nova entrada na tabela de DNS Avançado:

* **Type:** `A Record`

* **Host:** `@` (ou `www`)


* **Value:** *Coloque o IP Estático alocado no Lightsail*

* **TTL:** `1 min` (apenas para testes rápidos de propagação)



---

## 💻 CAPÍTULO 3: PROVISIONAMENTO LIMPO (UBUNTU) E CONEXÃO SSH LOCAL

Para cenários onde você precisa de controle total do servidor sem pacotes pré-instalados, adotamos o modelo *OS Only*.

### Passo a Passo de Criação:

1. Clique em **Create instance** > selecione **OS Only** > Escolha **Ubuntu 20.04 LTS**.


2. No menu *Change SSH key pair*, clique em **Create New**, nomeie como `keys-lightsail-prod` e faça o download do arquivo `.pem` imediatamente.



### Endurecimento de Segurança do Arquivo de Chave (Linux/Mac):

O protocolo SSH local rejeitará conexões se o seu arquivo de chave privada estiver exposto a outros usuários do seu computador. Execute no terminal local:

```bash
# Remove permissões públicas e dá acesso de leitura exclusivo ao dono
chmod 400 keys-lightsail-prod.pem

# Conecte-se ao servidor Ubuntu utilizando o IP público
ssh -i keys-lightsail-prod.pem ubuntu@IP_DA_SUA_MAQUINA

```

---

## ⚖️ CAPÍTULO 4: ARQUITETURA DE ALTA DISPONIBILIDADE E FAILOVER

Para mitigar desastres físicos em data centers, distribuiremos a aplicação em múltiplas **Zonas de Disponibilidade (AZs)** utilizando **Snapshots** e um **Load Balancer**.

```
                     [ USUÁRIO FINAL ]
                             │
                  [ http://loadbalancer-dns/ ]
                             ▼
                    [ LOAD BALANCER ]
                             │
              ┌──────────────┴──────────────┐
              ▼ (Saudável - OK)             ▼ (Falha Física - Isolada)
     [ UBUNTU-PROD-01 ]              [ UBUNTU-PROD-02 ]
      Data Center: AZ-A               Data Center: AZ-B

```

### Passo 1: Clonagem via Snapshot

1. Acesse a máquina virtual `Ubuntu-1` e clique em **Stop** (desligar a máquina evita corrupção de dados ativos).


2. Na aba **Snapshots**, clique em **Create snapshot** e nomeie como `template-nginx`.


3. Nos três pontos do snapshot criado, clique em **Create new instance**.


4. **O Ponto Crítico:** No campo *Availability Zone*, altere explicitamente de *Zone A* para **Zone B**. Nomeie a nova máquina como `Ubuntu-2`.



### Passo 2: Implementação do Load Balancer e Health Check

1. Na aba **Networking**, clique em **Create load balancer** (Custo: ~$18/mês - use apenas para produção/testes rápidos).


2. Em *Target Instances*, anexe a `Ubuntu-1` e a `Ubuntu-2`.


3. Clique em **Customize health checking** e altere o caminho do teste para um arquivo existente nos seus servidores web (ex: `server.txt`).


4. **Validação de Failover:** Copie a URL do Load Balancer, adicione `/server.txt` e dê F5. O tráfego alternará entre os servidores. Dê um *Stop* na máquina 2; o balanceador detectará a falha pelo *Health Check* e redirecionará 100% dos usuários para a máquina 1 sem derrubar o site.



---

## 💾 CAPÍTULO 5: EXPANSÃO DE ARMAZENAMENTO EM BLOCO (LINUX DISK STORAGE)

Diferente de um *Bucket* (que funciona como um Google Drive isolado para mídias), o *Disco Adicional* funciona como um SSD de alta velocidade para expandir o espaço interno do sistema ou banco de dados, e deve ser criado na mesma Zona de Disponibilidade da VM.

### Passo 1: Anexar o Hardware

1. Na aba **Storage**, crie um disco de 8 GB garantindo a mesma Região e Zona de Disponibilidade da sua VM (`Zone B`).


2. Clique em **Attach** e selecione a máquina `Ubuntu-2`. O Lightsail mapeará o caminho físico (ex: `/dev/xvdf`).



### Passo 2: Preparação e Montagem do Disco (Runbook de Comandos Unix)

Conecte-se via SSH à máquina e execute a sequência abaixo para preparar o hardware recém-comprado:

```bash
# 1. Verificar se o sistema reconheceu o disco fisicamente
sudo sfdisk -l

# 2. Iniciar o particionamento lógico do novo disco (/dev/xvdf)
# Pressione em sequência no menu interativo: 'n' (nova), 'p' (primária), 'Enter', 'Enter', 'Enter' e 'w' (escrever/salvar)
sudo fdisk /dev/xvdf

# 3. Formatar a nova partição com o sistema de arquivos padrão do Linux
sudo mkfs.ext4 /dev/xvdf1

# 4. Criar o diretório que servirá de ponto de acesso interno
sudo mkdir /mnt/data

# 5. Realizar a montagem física da partição na pasta criada
sudo mount /dev/xvdf1 /mnt/data

# 6. Validar se o espaço de 8 GB foi adicionado com sucesso
df -h

```

### Passo 3: Garantindo a Persistência no Boot (`fstab`)

Se o servidor for reiniciado, o Linux esquecerá a montagem manual. Para automatizar, edite o arquivo de tabela de sistemas de arquivos:

```bash
sudo vi /etc/fstab

```

Adicione a seguinte linha exatamente ao final do arquivo:

```text
/dev/xvdf1    /mnt/data    ext4    defaults    0 1

```

> **⚠️ CRÍTICO:** Erros no `fstab` travam o boot do Linux permanentemente. Valide a sintaxe imediatamente com o comando abaixo antes de inventar de reiniciar a máquina:
> 
> 

```bash
sudo mount -a

```

*Se o comando rodar em silêncio (sem erros), sua configuração está perfeita. Para liberar a escrita da pasta para seu usuário padrão, execute:* `sudo chown -R ubuntu:ubuntu /mnt/data`.

---

## 📦 CAPÍTULO 6: MODERNIZAÇÃO – DEPLOY DE CONTÊINERES GERENCIADOS

Para arquiteturas modernas de microsserviços, o Lightsail elimina a necessidade de gerenciar o sistema operacional básico, permitindo o deploy direto de imagens Docker.

### Fluxo DevOps de Publicação de Imagem:

No seu terminal local de desenvolvimento, o fluxo para gerar e publicar a aplicação (ex: Servidor Apache) para o registro é:

```bash
docker build -t meu-usuario/apache-labs .
docker push meu-usuario/apache-labs

```

### Provisionamento no Painel Containers:

1. No Lightsail, acesse a aba **Containers** e clique em **Create container service**.


2. Selecione a capacidade computacional (Plano Nano - $7/mês por nó).


3. Clique em **Specify a custom deployment** e preencha:


* **Container name:** `web-app-prod`

* **Image:** `docker.io/meu-usuario/apache-labs`



4. Em *Open Ports*, adicione a porta **80** com o protocolo **HTTP**.


5. No campo **Public Endpoint**, selecione o contêiner `web-app-prod` (isso direciona o tráfego da internet para a porta aberta dele).


6. Clique em **Create container service**.



### Atualização Contínua e Rollback:

Quando o código do seu site mudar, repita o `docker push` com a nova versão. No painel do Lightsail, vá em **Deployment versions**, clique nos três pontos da versão atual e selecione **Modify and redeploy**.

* **Mecanismo de Proteção:** O Lightsail manterá a versão antiga online até que o novo contêiner passe nos testes de inicialização. Se a nova atualização apresentar falhas críticas (*bugs*) em produção, clique nos três pontos da versão anterior inativa no histórico e selecione realocar. O **Rollback** é concluído em segundos, mitigando o tempo de indisponibilidade da aplicação.



---

## 🛑 CHECKLIST DE FINOPS: PROCEDIMENTO DE DESMOBILIZAÇÃO

Sempre que concluir um ambiente de testes, homologação ou laboratório de estudos, execute este *runbook* de remoção para evitar cobranças surpresas no cartão corporativo:

* [ ] **1. Contêineres:** Vá na aba *Containers*, clique nos três pontos do serviço e selecione *Delete*.


* [ ] **2. Load Balancer:** Acesse a aba *Networking*, entre no balanceador de carga e delete-lo (recurso com custo fixo agressivo).


* [ ] **3. Discos Adicionais:** Vá até o disco em *Storage*, clique em *Detach* (pode exigir o *Stop* da máquina), aguarde a liberação física e delete o disco.


* [ ] **4. Instâncias Virtuais:** Na aba *Instances*, exclua todas as máquinas criadas.


* [ ] **5. IPs Estáticos Órfãos:** Acesse a aba *Networking*, localize os IPs que ficaram sem máquinas vinculadas após a exclusão do passo 4 e delete todos eles imediatamente para cessar as cobranças automáticas da AWS.


* [ ] **6. Snapshots:** Vá na aba *Snapshots* e limpe os backups manuais antigos para liberar espaço de armazenamento retido.