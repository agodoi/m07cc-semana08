CGNAT (*Carrier-Grade NAT*, ou NAT de nível de operadora) é um NAT feito pelo **provedor de Internet**, além do NAT que o seu roteador de casa já faz.

**Como funciona.** Na rede sem CGNAT, seu roteador recebe um IP público do provedor e traduz os endereços privados da sua casa (192.168.x.x) para esse IP. Com CGNAT, o roteador recebe um endereço **também não público**, e o provedor faz uma segunda tradução, colocando centenas ou milhares de clientes atrás de poucos IPs públicos:

```
Notebook 192.168.0.10
   → NAT do roteador de casa → 100.64.12.7
      → CGNAT do provedor → 177.x.x.x (IP público compartilhado)
         → Internet
```

**Por que existe.** Os endereços IPv4 públicos se esgotaram, inclusive no registro da América Latina (LACNIC). Os provedores continuam ganhando clientes e não têm um IP público para cada um. O CGNAT é o remendo; a solução definitiva é o IPv6.

**A ligação com a aula.** O bloco `100.64.0.0/10` que aparece na tabela de endereços especiais foi reservado para isso (RFC 6598). Ele vai de 100.64.0.0 a 100.127.255.255, ou seja, 2²² ≈ 4,2 milhões de endereços. O provedor não usa uma faixa RFC 1918, como 192.168.0.0/16, porque ela poderia coincidir com a rede interna do cliente. Aí o roteador teria a mesma faixa dos dois lados e não saberia para onde encaminhar os pacotes.

**Consequências práticas:**

- Ninguém de fora consegue **iniciar** uma conexão com você. Por isso, hospedar um servidor em casa, fazer redirecionamento de portas ou acessar uma câmera remotamente deixa de funcionar direto.
- Jogos online e aplicações P2P podem ter problemas de conectividade.
- Você divide o IP com outros clientes. Se um deles for bloqueado por abuso, você pode ser afetado, e isso ajuda a explicar aqueles CAPTCHAs frequentes.
- Para identificar um usuário, não basta o IP. É preciso registrar também a porta de origem e o horário.

**Como saber se você está atrás de CGNAT.** Compare o endereço WAN que aparece no painel do roteador com o que um site como "qual é meu IP" mostra. Se forem diferentes, ou se o do roteador estiver entre 100.64 e 100.127, você está atrás de CGNAT. As saídas são usar IPv6, pedir um IP público ao provedor (às vezes cobrado) ou usar túneis e VPNs que fazem a conexão de dentro para fora.

O raciocínio é o mesmo do **NAT Gateway** da sua VPC: vários hosts privados saem para a Internet por um único IP público, e ninguém de fora consegue iniciar uma conexão com eles.
