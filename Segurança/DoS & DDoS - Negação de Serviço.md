EM sua grande maioria, não visa roubar dados ou fazer acesso indevido. O objetivo principal é afetar a disponibilidade, tornar o serviço inacessível a usuários legítimos, por exaustão de recursos (seja de banda, CPU, armazenamento, memória)
O D a mais de DoS é uma variação em que o ataque é distribuído, vários "clientes" falsos causando essa exaustão

Uma forma que vai te deixar "conviver" com o ataque e disponibilizar atendimento a ALGUNS dos clientes legítimos é descartar por padrão e sortear algumas conexões para manter, assim alguns legítimos terão maior chance de serem devidamente atendidos.

# Flooding ping command (ICMP)
Busca gerar um congestionamento na rede, exaurir a largura de banda, com o uso de ping e sua resposta.
Atualmente precisaria de uma capacidade imensa de geração de tráfego para realmente afetar, já que o pacote de ping é muito pequeno
Uma observação importante é que ela pode atingir links da rede (um link da ISP por exemplo) ou roteadores ou firewalls 

Aqui vale a observação de que no modo mais básico, sem [IP Spoofing](https://www.cloudflare.com/pt-br/learning/ddos/glossary/ip-spoofing/) (que forja o endereço de origem, você gera um pacote que diz que o IP de origem é o IP de outra pessoa), irá revelar o IP de origem e que provavelmente será o seu.
O reply do ping será destinado ao IP de origem escrito no pacote (caso tenha utilizado IP spoofing, será ao dono do IP falso)
Tem como "counterar" o IP Spoofing com recursos do roteador/firewall em que há uma checagem se o pacote que ele irá levar para fora possui um IP interno válido

É fácil de detectar mas difícil de parar, a não ser que a rota do tráfego mude (mude no DNS e destine o tráfego a um outro servidor) ou de alguma forma consiga detectar a origem e fazer algum bloqueio.

# Source address spoofing
Utiliza dos blocos de IPV4 reservados que não são usados como endereço de origem, vai cair num "buraco negro" 
Quadrilhas, botnets, ficam buscando esses endereços para utilizar, mandando ping, portmap. Scaneando mesmo

Assim o melhor a se fazer é se antecipar a esse ataque, com autorização do dono, reservar esses IP's que frequentemente não estarão sendo utilizados e destinar essas rotas a um Honeypot, que nos permitirá prever que seremos atacados, tomar previdências sobre e saber o comportamento do atacante

Backscatter traffic - Questão do uso de honeypots

# SYN spoofing
Mais comum de ser utilizado, SYN é a flag de abertura de conexão TCP, você manda um pacote com essa flag para indicar que quer abrir uma conexão.
Lembrando que em TCP tem o handshake SYN -> SYN-ACK -> ACK
	E no servidor, para suportar múltiplos clientes e portas, cada conexão vira uma thread, uma instância para concorrência

O atacante manda um SYN, recebe um SYN-ACK mas não responde com o ACK, deixa o servidor esperando até o timeout. Com várias requisições você vai conseguir afetar várias capacidades, seja de memória, porta, CPU.
Daí quando chega um usuário legítimo para tentar utilizar o serviço, você não consegue atender.

## Para se proteger disso
- Diminuir o tempo de timeout - pouco efetivo
- Diminuir/Limitar a quantidade de conexões simultâneas - Poupa o servidor de exaustão, apesar de ainda assim deixar o serviço vulnerável, o serviço ainda vai cair mas a máquina ainda vai estar responsiva
- Limitar a quantidade de conexões simultâneas de um mesmo IP - Efetivo contra DoS mas DDoS, distribuído, não vai funcionar

## SYN spoofing
Se a "origem" for válida, o ataque não vai funcionar porquê caso o IP seja válido, automaticamente iriam responder o SYN-ACK com RST. 
Mas caso esse SYN-ACK caia em um limbo, como o dos IPs ditos anteriormente, vai funcionar porquê o servidor atacado vai ficar esperando resposta sem sucesso.

# UDP
Vai servir bem igual ao SYN mas não tem a parte de conexão para mandar, vai ser uma competição de banda, não vai ser uma competição desigual.
O bom de usar UDP ao invés de ping é que podemos usar pacotes bem maiores e ser mais efetivo. Além disso também pode ser direcionado a praticamente qualquer serviço/porta, porquê sempre vai precisar ser processado.
Mesmo que seja enviado a uma porta que não esteja associada a algum serviço, não irá exaurir o processamento, mas vai afetar a banda

# Botnets
Infecta várias máquinas, nelas ficam um backdoor, que ao comando do atacante, servirá recursos ao atacante, servir banda para ataque, capacidade de gerar tráfego.
Aqui também é possível ter uma hierarquia, em que um atacante tem Handler Zombies que enviam os comandos aos demais zombies

# Application-based 
Como requisições HTTP flood (funcional apenas em servidores mais antigos e específicos, que é pouco efetivo atualmente mas muito interessante já que o pacote chega até a camada de aplicação, gastando mais recursos. Tira proveito basicamente da necessidade de um EOF no final do arquivo, daí o servidor recebe apenas uma parte da requisição e fica esperando o EOF), pedidos mais complexos como pedir uma extração de arquivos.

# Reflection attacks
Utiliza de requests de DNS infectadas, gerados por uma botnet.
Basicamente a botnet envia a requisição de DNS aos servidores DNS públicos com o endereço da vítima. Assim várias respostas de DNS chegam a vitima e como o pacote de resposta do DNS é muito maior do que o de requisição, temos uma amplificação de tráfego.

# A defesa
- Prevenção e preempção (antes do ataque)
	- Bloquear scan
	- Bloquear os IP's inválidos
	- Diferenciar humanos de máquinas com captcha por exemplo
	- Uso de clusters e distribuição dos servidores
- Detecção e filtrar (durante o ataque)
- Identificação e traceback da origem (durante e após o ataque)
- Reação (após o ataque)