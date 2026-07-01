### Amazon EC2: alta disponibilidade e escalabilidade em uma aplicação

# Guia Prático: Criando Sua Primeira Instância Amazon EC2

O **Amazon EC2 (Elastic Compute Cloud)** é o serviço de computação em nuvem da AWS que permite criar e gerenciar máquinas virtuais (chamadas de instâncias) de forma elástica, ou seja, que escalam facilmente conforme a sua necessidade.

---

## 1. O Nível Gratuito da AWS (Free Tier)

Para estudar sem custos, a AWS oferece o **Nível Gratuito**:

* **Disponibilidade:** Cerca de **750 horas por mês** de uso do EC2.
* **Tipo de Instância Elegível:** **`t2.micro`** (ou `t3.micro` em regiões onde a t2 não estiver disponível). O nome (`t2`/`t3`) representa a geração da máquina.
* **Modelos de Precificação Comuns:**
* **Sob Demanda (On-Demand):** Pago por tempo de uso, mais flexível e mais caro.
* **Instâncias Spot:** Utiliza a capacidade ociosa da AWS. É muito mais barata, mas a AWS pode recuperar a máquina a qualquer momento se precisar.



---

## 2. Passo a Passo: Criando a Instância no Console

1. **Acessar o EC2:** Use a barra de pesquisa do Console AWS e vá em **Instâncias > Executar instâncias**.
2. **Nome da Tag:** Defina como `ec2-first`.
3. **Imagem do Sistema (AMI):** Escolha o padrão **Amazon Linux** (mantenha a arquitetura padrão).
4. **Tipo de Instância:** Escolha **`t2.micro`** (para garantir a gratuidade). *Nota: O instrutor utilizou a região "Norte da Virgínia".*
5. **Configurações de Rede e Armazenamento (HD):** Mantenha os valores padrão, pois já estão configurados dentro do limite do nível gratuito.
6. **Executar:** Revise os dados e clique em **Executar instância**.

---

## 3. Gerenciando as Chaves de Acesso (Segurança)

Para acessar o servidor sem senhas, a AWS utiliza um par de chaves (pública no servidor, privada com você).

### Criando a chave no navegador:

* Clique em **Criar par de chaves**.
* Nome: `ec2`.
* Formato: Mantenha o padrão (`.pem`) para uso com OpenSSH no terminal.

### Configurando a chave no terminal (WSL / Ubuntu):

Após baixar o arquivo `ec2.pem`, execute os seguintes comandos para movê-lo para a pasta correta e ajustar as permissões de segurança:

```bash
# Navegar até a pasta onde o arquivo foi baixado (exemplo: /tmp)
cd /tmp

# Mover a chave para a pasta oculta de chaves SSH do seu usuário
mv ec2.pem ~/.ssh/

# Acessar a pasta SSH
cd ~/.ssh/

# Diminuir as permissões do arquivo (obrigatório para o SSH aceitar a chave)
# 400 define que o arquivo será apenas de leitura e exclusivo para o seu usuário
chmod 400 ec2.pem

```

Para saber mais: Termination Protection


Com uma instância criada, nós podemos a qualquer momento removê-la através do processo de “Encerrar instância”. Se for uma instância de produção, que está realmente executando código de um produto real, é interessante evitar que essa instância seja encerrada. Alguém pode acabar encerrando-a sem querer, por alguma confusão. Para isso podemos selecionar nossa instância, ir em “Ações > Configurações de instância > Alterar proteção contra encerramento”. Dessa forma não será possível encerrar a instância. O mesmo vale para a ação de desligamento.



---


# Entendendo os Security Groups (Grupos de Segurança) na AWS

> **O que são?**
> Os Security Groups funcionam como o **firewall** da sua instância EC2. Eles controlam o tráfego que tem permissão para **entrar** (regras de entrada) e **sair** (regras de saída) da sua máquina virtual.

---

## 1. Comportamento Padrão dos Grupos

* **Grupo `default`:** É o grupo padrão da AWS. Cuidado: ele vem configurado para **liberar todo o tráfego** de entrada e saída para qualquer usuário.
* **Grupo `launch-wizard-1`:** Criado automaticamente quando subimos a instância. Por padrão, ele libera o acesso **SSH (porta 22)** vindo de **qualquer origem (`0.0.0.0/0`)**.

---

## 2. Prática: Restringindo o Acesso por IP (Segurança Avançada)

Deixar o SSH aberto para o mundo (`0.0.0.0/0`) é perigoso. Para proteger a máquina, podemos restringir o acesso apenas ao nosso computador:

1. No menu lateral, vá em **Rede e segurança > Security groups**.
2. Selecione o grupo da sua instância (`launch-wizard-1`) e clique em **Editar regras de entrada**.
3. Em **Origem**, mude de "Qualquer lugar-IPv4" para **"Meu IP"**. A AWS detectará seu IP público automaticamente.
4. Salve a regra.

### O Teste do Firewall:

* **Com o seu IP correto:** O comando de conexão no terminal funciona normalmente:
```bash
ssh -i "ec2.pem" ec2-user@seu-dominio-ec2.amazonaws.com

```


* **Com um IP diferente/fictício:** Se você alterar um número do seu IP na regra e tentar conectar, o terminal ficará travado (sem resposta), provando que o firewall da AWS bloqueou o acesso. Para destravar o terminal, use o atalho `Ctrl + C`.

---

## 3. Criando um Grupo de Segurança Reutilizável (Acesso Web)

Você não precisa criar um grupo de segurança novo para cada instância. É possível criar um grupo genérico e "atrelá-lo" a várias máquinas.

### Passo a Passo para Criar o Grupo `acesso-web`:

1. Clique em **Criar grupo de segurança**.
2. **Nome:** `acesso-web` | **Descrição:** "Libera acesso a porta 80 e 443 para acesso HTTP e HTTPS".
3. Em **Regras de entrada**, adicione duas regras:
* **Tipo:** `HTTP` (Porta 80) | **Origem:** `Qualquer lugar-IPv4` (`0.0.0.0/0`, `::/0`)
* **Tipo:** `HTTPS` (Porta 443) | **Origem:** `Qualquer lugar-IPv4` (`0.0.0.0/0`, `::/0`)


4. Clique em **Criar**.

### Como Associar o Novo Grupo à Instância Existente:

1. Vá em **Instâncias > Instâncias**.
2. Selecione a sua máquina (`ec2-first`).
3. Clique em **Ações > Segurança > Alterar grupos de segurança**.
4. Procure pelo grupo `acesso-web`, adicione-o à lista e clique em **Salvar**.

Pronto! Agora a sua máquina aceita conexões SSH (apenas do seu IP) e conexões Web (HTTP/HTTPS vindas de qualquer lugar do mundo).