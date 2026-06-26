### Amazon EC2: alta disponibilidade e escalabilidade em uma aplicação

# Resumo da Aula: Como Funciona a Internet?

A internet faz parte do nosso cotidiano, mas o seu funcionamento envolve uma estrutura organizada de dispositivos e regras que garantem que a informação chegue ao destino correto.

---

## 1. O Caminho da Informação (O Exemplo Prático)

Quando você envia uma mensagem de texto pelo celular (como avisar a mãe que vai se atrasar), acontece o seguinte caminho:

1. **Origem:** Você digita e envia a mensagem no seu smartphone.
2. **Primeiro Salto:** A informação é enviada em forma de **pacote** até o dispositivo de rede mais próximo — o **roteador**.
3. **Trânsito:** O roteador encaminha a mensagem por diversos outros dispositivos espalhados pela cidade.
4. **Destino:** A mensagem chega ao roteador da sua casa e, finalmente, é entregue ao celular da sua mãe.

---

## 2. O que é uma Rede de Computadores?

Assim como uma rede de pesca ou de futebol, a rede de computadores é um emaranhado de conexões interligadas. Ela é composta por **nós**, que se dividem em dois tipos:

* **Dispositivos Computacionais (Finais):** Celulares, smart TVs, computadores e notebooks (quem cria ou consome a informação).
* **Dispositivos de Rede:** Equipamentos como o roteador, que servem exclusivamente para prover e gerenciar a conexão entre os dispositivos finais.

> 💡 **Resiliência da Rede:** Assim como na rede de pesca, os nós possuem múltiplas conexões entre si. Se uma conexão se romper, a informação encontra outro caminho, mantendo a rede funcionando.

---

## 3. O que são Protocolos de Rede?

Para que dispositivos diferentes consigam se entender, eles precisam falar a mesma "língua". É aí que entram os **protocolos**.

* **Analogia Humana:** Quando encontramos alguém, seguimos um padrão: dizemos "Oi", a pessoa responde "Tudo bem?", e a conversa flui. Isso é um protocolo social.
* **No Computador:** A comunicação entre dispositivos também segue regras estritas:
1. Início da conexão.
2. Confirmação de conexão iniciada.
3. Envio da mensagem.
4. Confirmação de recebimento.



---

## Conclusão

* **Internet:** É uma rede global de computadores interligados ao redor do mundo, permitindo a comunicação entre diferentes países e continentes.
* **Protocolos:** São as regras e padrões essenciais para a comunicação. Um único protocolo não é suficiente para dar conta de toda a internet; por isso, existem vários trabalhando juntos (assunto para as próximas aulas).