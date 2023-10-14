#  DNS - Domain Name Service

    # Servidores
        Os servidores DNS estão distribuídos ao redor do mundo.
            Servidores Raiz
                Letra	Endereço IPv4	Endereço IPv6	ASN	Antigo nome do servidor	Operador	Localização	Software
                A	198.41.0.4	2001:503: ba3e :: 02:30	AS26415	ns.internic.net	Estados UnidosVerisign Inc.	distribuído (anycast)
                6/0	BIND
                B	192.228.79.201 (desde janeiro de 2004; originalmente era 128.9.0.107)	2001:478:65::53 (na zona raiz)	AS4	ns1.isi.edu	Estados UnidosUSC - ISI	Estados UnidosMarina del Rey, CA, Estados Unidos
                0/1	BIND
                C	192.33.4.12	2001:500:2::c (na zona raiz)	AS2149	c.psi.net	Estados UnidosCogent Communications	distribuído (anycast)
                6/0	BIND
                D	199.7.91.13 (desde 3 de janeiro de 2013; originalmente era 128.8.10.90)	2001:500:2d::d	AS27	terp.umd.edu	Estados UnidosUniversidade de Maryland	Estados UnidosCollege Park, MD, Estados Unidos
                1/0	BIND
                E	192.203.230.10	2001:500:a8::e	AS297	ns.nasa.gov	Estados UnidosNASA	Estados UnidosMountain View , CA, Estados Unidos
                1/0	BIND
                F	192.5.5.241	2001:500:2f::f	AS3557	ns.isc.org	Estados UnidosInternet Systems Consortium	distribuído (anycast)
                2/47	BIND 9[2]
                G	192.112.36.4	2001:500:12::d0d	AS5927	ns.nic.ddn.mil	Estados UnidosDefense Information Systems Agency	distribuído (anycast)
                6/0	BIND
                H	128.63.2.53	2001:500:1::803f:235	AS13	aos.arl.army.mil	Estados UnidosUnited States Army Research Laboratory	Estados UnidosAberdeen Proving Ground, MD, Estados Unidos
                2/0	NSD
                I	192.36.148.17	2001:7fe::53	AS29216	nic.nordu.net	SuéciaNetnod (antes Autonomica)	distribuído (anycast)
                38	BIND
                J	192.58.128.30 (desde Novembro de 2002; originalmente era 198.41.0.10)	2001:503:c27::2:30	AS26415	—	Estados UnidosVersign Inc.	distribuído (anycast)
                63/7	BIND
                K	193.0.14.129	2001:7fd::1	AS25152	—	Países BaixosRIPE NCC	distribuído (anycast)
                5/13	NSD[3]
                L	199.7.83.42 (desde novembro de 2007; originalmente era 198.32.64.12)[4]	2001:500:3::42	AS20144	—	Estados UnidosICANN	distribuído (anycast)
                103/0	NSD[5]
                M	202.12.27.33	2001:dc3::35	AS7500	—	JapãoProjeto WIDE	distribuído (anycast)
                5/1	BIND
    
    # Começo o DNS
        Poucos computadores, as informações eram armazenadas no arquivo "Hosts", que servia de banco de dados.
        Havia a necessidade de compartilhamento do arquivo para cada inserção de um registro. (Nome <> IP)
        Função do DNS
            - Converter NOMES em IP´s www.google.com <<>> 
            - Converter IP´s em NOMES
            - Fazer cache dos endereços resolvidos
        
    # Estrutura do DNS (https://root-servers.org/) 13 ROOT DOMAIN SERVER
        Global Top-Level Domain: .edu .info .com
        Country Code Top-Level Domain: .us .br .cn 

    # Tipos de DNS
        DNS Autoritativo
            Um servidor DNS autoritativo é considerado a fonte definitiva para as informações de DNS de um domínio. Quando um servidor DNS autoritativo responde a uma consulta, ele fornece informações confiáveis e atualizadas diretamente relacionadas ao domínio em questão.
            Autoritativo - É o servidor que está autorizado a responder por um domínio, ou seja, nele estão registradas as entradas (registros do domínio).
        DNS Recursivo
            Ele é responsável por buscar informações de servidores DNS autoritativos em nome de um cliente (por exemplo, um navegador da web ou um dispositivo) quando uma solicitação de DNS é feita.


    # Como funciona a resolução de nomes
        Exemplo
            www.xyz.com

            Computador << consulta DNS da empresa RECURSIVO>> DNS RECURSIVO  << TLD ROOT SERVERS >> ROOT SERVER . 
                Computador envia uma solicitação ao DNS da empresa, que é recursivo. O servidor recursivo da empresa, repassa o pedido para os root servers. Os Root Servers não sabem o IP do servidor autoritativo do site www.xyz.com, mas sabem quem é o servidor autoritativo para o domínio ".com". Então repassa o IP do servidor autoritativo ".com" para o serivdor recursivo e de posse da informação o servidor recursivo envia uma "query" destinada ao servidor autoritativo do domínio ".com" e recebe de volta o IP do servidor autoritativo para o domínio www.xpto.com. A partir desse momento o servidor recursivo envia uma nova "query" para o servidor autoritativo www.xyz.com e recebe o IP do servidor da página como resposta. Repassa a informação para o computador solicitante.
                Além disso o servidor recursivo armazena temporariamente o registro de acordo com o tempo determinado na TTL do servidor.   

    # Verificar o processo de resolução de nomes
        
        Ferramenta utilizada "dig"
            Para mostrar o servidor da página, digite: 
                dig www.xyz.com
            Para exibir todo o trajeto do comando "dig" faça assim:
                dig +trace www.xyz.com


    # DNS Reverso (também conhecido como DNS PTR "Pointer Record" (Registro de Apontador) no sistema de nomes de domínio (DNS))
        DNS reverso é usado para traduzir endereços IP em nomes de domínio. Ele desempenha um papel importante em verificar e autenticar as informações associadas a um endereço IP, especialmente em ambientes de rede. Isso pode ajudar a verificar a identidade de um servidor ou dispositivo na Internet.

        Consultar DNS reverso usando o "dig"
            dig -x 172.217.173.110
                Retorno:
                    ; <<>> DiG 9.18.12-0ubuntu0.22.04.3-Ubuntu <<>> -x 172.217.173.110
                    ;; global options: +cmd
                    ;; Got answer:
                    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34291
                    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
                    ;; OPT PSEUDOSECTION:
                    ; EDNS: version: 0, flags:; udp: 65494
                    ;; QUESTION SECTION:
                    ;110.173.217.172.in-addr.arpa.	IN	PTR
                    ;; ANSWER SECTION:
                    110.173.217.172.in-addr.arpa. 213 IN	PTR	google.com.
                    ;; Query time: 24 msec
                    ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
                    ;; WHEN: Sat Oct 14 16:19:44 -03 2023
                    ;; MSG SIZE  rcvd: 81


    # Interagindo com o DNS
        Comando "host"
            O comando "host" é uma ferramenta de linha de comando comum em sistemas Unix e Linux que é usada para realizar consultas de DNS. 
            Ex:
                host www.google.com
                    Retorno do comando
                        www.google.com has address 142.250.219.196
                        www.google.com has IPv6 address 2800:3f0:4001:81d::2004

                    IPv4 endereço tipo A
                    IPv6 endereço tipo AAAA

                    Buscando apenas o endereço IPv4 | Usar a opção -t (type) para especificar um tipo de registro específico
                        Pesquisar o IPv4 para o domínio www.google.com
                        host -t A www.google.com
                        Pesquisar o IPv4 para o domínio www.google.com
                        host -t AAAA www.google.com

                    Buscar o servidor autoritativo do endereço google.com | Especificar no tipo NS (Name Server)    
                        host -t NS google.com

                     Buscar o servidor de correio eletrônico de outlook.com | Especificar o MX (Mail Exchange) na opção -t
                        host -t MX outlook.com

        Comando "dig"
            Instalar no linux
                apt install dnsutils
            
           O comando "dig" (Domain Information Groper) é uma ferramenta de linha de comando amplamente utilizada para realizar consultas de DNS em sistemas Unix e Linux. Ele fornece informações detalhadas sobre registros DNS, resolução de nomes de domínio e servidores DNS. 

           dig www.yahoo.com.br 

           Apenas a resolução de nomes
            dig www.yahoo.com.br +short

            Fazer um arquivo para consultaar vários endereços
                Crie um aruivo "minha-query.txt" e dentro coloque os endereços que deseja consultar
                    Exemplo:
                        www.google.com
                        www.yahoo.com   
                        www.alura.com.br
                Salve o arquivo
                Consulte com o seguinte comando
                    dig -f /caminho-do-arquivo/minha-query.txt

                Consultar tipos de registros
                    dig -t SOA google.com

                    O tempo de expiração do registro, ou seja, o tempo que seu recursivo guarda a informação do registro consultado
                    
                Consultar o servidor autoritativo pelo "dig"
                    dig NS google.com
                
                