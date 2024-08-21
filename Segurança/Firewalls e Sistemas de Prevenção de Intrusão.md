Meio de proteção para proteção da LAN, proteção de redes locais de ameaças da WAN
Surgiu quando teve a necessidade de as redes locais serem expostas a internet
Separa o que é interno e o que é externo

Firewall está relacionado com o tráfego da rede interna de/para o ambiente externo
	Se impõe entre esse tráfego, se posiciona no meio

# Como é especificado o que pode e o que não pode
Regras -> programação que define o que pode sair e o que pode entrar
Políticas de acesso
	Deny all - Rejeita tudo por padrão
	Accept all - Aceita tudo por padrão
	E então vem as exceções, que devem ser explicitamente programadas como permitido/bloqueado (a depender se é uma política deny ou accept)

Essas regras vão ser definidas com base na direção (in - Externo para interno ou out), Endereço de fonte e destino, protocolo (TCP, UDP), porta e a ação

A política de acesso é definida com uma ultima regra no final, com menor prioridade, que diz "ambas direções", "any" para os endereços de fonte e destino, protocol e port, com a ação (Deny/Accept)

A prioridade entre as regras é feita pela posição, a regra mais acima é a mais importante e definirá a saída (encontrou um match, aplica e sai)

## Stateful Inspection - Stateful Firewall
Aqui guarda um estado, uma informação para as regras que tem precedente
Funciona parecido com uma máquina de estados, guarda o estado, avança quando recebe um pacote que atende o precedente.
Tem como fazer Stateful Inspection, que define um precedente, algo que deve ocorrer antes
Ex.: Para permitir que tenha um pacote entrando de um ip y, deve ocorrer primeiro um pacote de x interno para y na porta 80.
É um tipo de firewall mais inteligente mas vai precisar de mais memória e processamento, é o que é feito atualmente

## Application level Firewall (Proxy/Gateway)
Firewalls de aplicação, frequentemente baseados em HTTP (inclusive HTTPS provavelmente irá bloquear esse firewall)
Firewalls que permitem basear regras em conteúdo de aplicação
Ex.: Um firewall que bloqueie todo conteúdo que tenha bitcoin no texto da aplicação
Vai ter que abrir o IP, TCP e HTTP para então aplicar a regra.
Mais seguro do que filtros de pacotes, mas tem um grande overhead

Em aplicações que podemos especificar um proxy, a aplicação irá se comunicar com o Proxy utilizando HTTP, para fazer os requests em seu nome. Assim a chave de criptografia em um HTTPS seria entre o proxy e o serviço sendo acessado.
	O proxy vai agir como um procurador e inclusive terá acesso ao conteúdo criptografado
	Além do sentido de segurança, proxy também é muito utilizado para agilizar, funcionar como um cache de conteúdo

# Hardware/Software
Pode ser um elemento de hardware ou software, já que existem equipamentos dedicados a tarefa, otimizados para isso 

No linux tem por padrão um módulo do kernel (**netfilter**) que é responsável por inspecionar e filtrar todos os pacotes que chegam. A ferramenta/utilitário p comunicar com o netfilter é o **iptables**

# Capacidades
Define um único ponto de "estrangulamento", concentra o tráfego nele
	Por conta disso é comum além de ter o firewall, também acumular algumas funções como NAT (Network Address Translation), monitoramento da rede, IPSEC e VPNs (Serviços a parte, que não são do firewall mas que por comodidade, de estar no lugar certo, ter todo o tráfego, costumam ir para o mesmo lugar)

# Limitações
Existem ataques que podem bypassar o firewall, o principio do firewall proteger é que tudo passe por ele
	Ex.: Um Access Point  que foi adicionado a rede com seu próprio NAT
		Assim, quem um intruso pode se conectar a esse Access Point (wifi) e ter acesso a rede sem passar pelo firewall
Não irá proteger contra ameaças/threats internas
Dispositivos móveis podem ser infectados fora da rede e serem utilizados dentro da rede