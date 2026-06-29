# Guia de Estudos Unificado: Introdução às Redes de Computadores

Este resumo consolida os conceitos fundamentais de redes, desde a infraestrutura física e lógica até a simulação prática de cenários reais.

---

## 1. Fundamentos: O que é uma Rede?

Uma rede de computadores é uma estrutura de nós interconectados que trocam informações entre si.

### Componentes Básicos

* **Dispositivos Finais (End Devices):** Equipamentos que geram ou consomem dados. Exemplos: smartphones, desktops, smart TVs e servidores.
* **Dispositivos de Rede:** Equipamentos que gerenciam, direcionam e provêm o tráfego de dados. Exemplo: roteadores.

> 🌐 **Interoperabilidade:** É a capacidade de sistemas com hardwares e softwares completamente diferentes (como um iPhone conversando com um servidor Linux) interagirem e trocarem dados perfeitamente.

---

## 2. A Estrutura Lógica: O Modelo de 4 Camadas (TCP/IP)

Para garantir que os dados trafeguem com segurança e integridade, a comunicação é dividida em quatro camadas sequenciais.

| Camada | Função Principal | Exemplo Prático / Protocolo |
| --- | --- | --- |
| **1. Aplicação** | Interface direta com o usuário e o software. | Navegador Web / Protocolo **HTTP** |
| **2. Transporte** | Divide a mensagem e a empacota para o envio seguro. | Segmentação dos dados |
| **3. Rede** | Define a melhor rota de tráfego e faz o endereçamento. | Roteadores / Endereço **IP** |
| **4. Física** | Transforma dados em bits e os transmite via meio físico. | Cabos de rede, fibra óptica ou **Wi-Fi** |

### Fluxo de Dados

* **Envio (Encapsulamento):** A informação desce da camada de *Aplicação* até a *Física*, ganhando "etiquetas" e proteções em cada etapa.
* **Recebimento (Desencapsulamento):** O processo inverso ocorre no destino. Os dados sobem da camada *Física* até a *Aplicação*, onde o usuário final pode ler a mensagem original.

---

## 3. Prática e Simulação: Cisco Packet Tracer

Como manipular hardware real de redes é complexo e custoso, utilizamos o simulador **Cisco Packet Tracer** para projetar e testar topologias virtuais.

### Roteiro de Montagem Científica

1. **Origem:** Insira um **Smartphone** a partir do menu *End Devices*.
2. **Acesso Local:** Adicione um roteador sem fio (**WRT300N**). A conexão sem fio (linha tracejada) será estabelecida automaticamente com o smartphone.
3. **Infraestrutura (Provedor):** Insira três roteadores cabeados (modelo **1240**) para simular o caminho da internet pela cidade.
4. **Destino:** Insira um **Home Router** e outro **Smartphone** para representar a rede local de destino.
5. **Cabeamento:** Use a ferramenta de **Conexão Automática** (ícone do raio) para interligar fisicamente todos os roteadores em série.

---

## Próximos Passos na Investigação

Com a infraestrutura física compreendida e o modelo lógico estruturado, os próximos tópicos abordam a validação prática da rede:

* Como o sistema operacional testa se um nó está ativo?
* Quais comandos nativos (como o `ping` ou `traceroute`) são usados para homologar essa conectividade?