# Motivação
**Criminosa**: Objetiva alguma recompensa financeira, roubo de identidade, credenciais, dados, espionagem, ou até mesmo sequestro de dados
**Ativistas/Politica**: Exposição de dados sigilosos, geralmente tem poucas habilidades então é bem comum Defacing de site, DoS

# Tipos de invasor
Espião: Conduz espionagem e/ou sabotagem de atividades, geralmente envolve ter acesso a um sistema por um longo período de tempo
Hackers/Crackers: Motivação vai além das listadas, como um desafio técnico, mais comum explorar técnicas como BufferOverflow
	Hacker: benigno, encontra a vuln e reporta ao dono do sistema, cria e divulga CVE para ganhar nome
	Cracker: Maligno, objetivo malicioso/criminoso

Masqueradar: Externo, que por padrão não tem acesso a aquele sistema e consegue acesso ao sistema (seja por roubo de credencial, backdoor, invasão de algum sistema com acesso)
	Aqui a detecção de intrusão vai ser muito efetiva e mais fácil
Misfeasor: Usuários que tem acesso ao sistema e vão agir de maneira má intencionada, tenta fazer alguma escalada de privilégios por exemplo
Clandestine: Interrompe de alguma forma a supervisão do acesso de controle 
## Níveis de habilidade
Apprentice/ Novato : Hackers com mínimo conhecimento técnico e faz uso principalmente de ferramentas (toolkits) já existentes 
Journeyman: Conhecimento suficiente para modificar e extender as ferramentas mas não do 0
Master: Desenvolvedores das ferramentas, com novas e mais eficientes, descobre novas categorias de vuln
	Alguns são empregados, seja por organizações privadas ou por estado
	Maior dificuldade de se defender contra esse tipo de indivíduo

Basicamente, todos ataques em que temos um intruso na rede e/ou sistema com acesso indevido


# Comportamento intruso
1. Ele estuda, consegue informações sobre o alvo
2. Tem acesso inicial
3. Escala os privilégios a partir desse acesso inicial
4. Consegue as informações que buscava e/ou exploita, explora o sistema
5. Mantém o acesso
6. Apaga os rastros 

Escolhe o alvo com IP lookup
Faz scan com NMAP por exemplo
Identifica os serviços
Brute-force nas senhas (possivelmente), seja remotamente ou roubando algum hash e tentando decodificar na máquina dele
Instala alguma ferramenta de administração remota


SNORT - IDS (Sistema de detecção de intrusão) para detectar o scan das portas
IPS - Sistema de prevenção de intrusão

Darknets - Buraco negro, redes que oferecem endereços válidos e que tem alguém ouvindo naquele endereço mas sem prover nenhum serviços, sem responder em nenhuma porta, intenção de estar completamente invisível
	Só para receber scan e divulgar origem, com o objetivo de popular um banco de suspeitos

O IPS pode se alimentar constantemente dessa base de dados para impedir acesso desses endereços suspeitos, elabora filtros para prevenir o ataque

# Intrusion detection systems
HIDS (Host-based) - Monitora um host pelas informações das máquinas, do sistema, uso de disco, versão de sistema, lista de processos ativos EX: Falcon - CrowdStrike, que deu ruim
NIDS (Network-based)- Baseado na rede, tamanho de pacotes, frequência, tipo de protocolo, tipo de criptografia usada, informações conseguidas a partir da rede para tentar caracterizar a intrusão
Hibrido - Combina sensores, análise da rede e dados da máquina para melhor identificar e responder a atividade intrusa
Distribuidas - Multiplos pontos de coleta, espalha "sensores" (monitora métricas em pontos diferentes) ao londo da sua organização/empresa

## Principios da IDS
Um intruso teria um comportamento diferente do usuário normal, com alguma sobreposição/coincidência com o usuário legítimo mas de regra seria possível diferenciar os dois usuários.
	Sendo a maior dificuldade a detecção dos intrusos nessa sobreposição (área em comum entre os não legítimos e legítimos). Quanto aos invasores, várias vezes pode ser interessante tentar se manter nessa faixa de sobreposição mas nem sempre é possível
	Ex de métrica que pode ser utilizada : O quanto foi feito de upload/O fluxo de upload pedido por uma máquina
## Requerimentos
Rodar continuamente
Ser tolerante a falhas
Resistir a downgrade de versão
Utilizar o mínimo de recursos do sistema (min overhead)
	E que o degrado ao sistema não seja tão ruim, inconstante
Estar configurado com as políticas de segurança do sistema
Se adaptar a mudanças de sistemas e usuários (Que possamos adicionar regras diversas)
Escalar bem para monitorar uma larga quantidade de sistemas
Permitir reconfiguração dinamicamente 

## Formas de detecção
Detecção de anomalia (anomalia nos comportamentos)
	Você tem um dataset correspondente ao comportamento legítimo dos usuários ao longo de um tempo
	E então analisa o quanto o comportamento se difere 
	Pode ser baseada em threshold - Checa a ocorrência de um certo evento ao longo do tempo ()
		Pode ser inefetivo sozinho 
	Profile based - Cada grupo de usuário tem a sua própria caracterização de comportamento legítimo
		É mais efetivo do que ver o completo, como no anterior

Detecção de assinatura (seja a assinatura de um arquivo ou a assinatura de comportamento do invasor, as portas, as ferramentas, o processo)
	Tendo uma assinatura, o IDS só checa se as condições estão satisfeitas e caso estejam feitas, reporta
	Comportamento muito parecido com um anti-virus, no antivirus vc procura assinatura de código, aqui você não está especificando a um código, pode ser um comportamento humano de sequências de comandos que a pessoa dá por exemplo
	Exemplo - Usuário fazendo cópias do arquivo de senha

## Métricas
Contadores - Numero de logins efetuados (sempre incremental)
Gauge - Número de logins efetuados em uma hora (pode variar)
Intervalo de tempo - Intervalo entre tentativas de login
Uso de recursos - Uso de CPU
Média e desvio padrão

# Ferramentas de auditoria
Uso de logs para elementos críticos da sua rede
Para então tirar deles informações relevantes, seja possíveis culpados, quando ocorreu esse ataque

Utilizar algum parser de log (em tempo real ou não) identificar possíveis ataques

Muitas vezes os sistemas de log tem um tradeoff de quão verboso deve ser o log, um log pesado, muito verboso, pode ser difícil de ler e além disso ser custoso em espaço e processamento

# Fontes comuns de informação
- Chamadas do sistema operacional
- Auditar os logs, os registros
- Hash de integridade dos arquivos (para detectar alterações de arquivos) - Recomendado fazer só com arquivos chave que são mais imutáveis 
- Acesso a registradores

# Baseados em rede
Basicamente análise em tempo real, detecção dos padrões em (quase) tempo real
Assim que algo é detectado, é disparado um alarme ao admin para o mesmo determinar (por meio de uma ferramenta automatizada ou não) o que fazer e/ou interrupção do ataque

O switch detecta algo levemente suspeito (ou em pior caso, todo o tráfego) e então envia cópias para uma IDS que vai fazer uma análise mais complexa e então 
	A porta que é conectada à IDS deve ter uma largura de banda maior que o resto das conexões do switch, já que o link do switch com a IDS vai ser o elo mais fraco da conexão e caso ele seja saturado com os pacotes sendo mandadas cópias a rede irá se tornar irresponsiva ( o que não queremos) 

## Sistemas hibridos/rede distribuídos
Tem como fazer isso distribuído, NIDS
O problema maior é não ter a base de dados centralizada, cada um tem uma visão local dos seus dados.