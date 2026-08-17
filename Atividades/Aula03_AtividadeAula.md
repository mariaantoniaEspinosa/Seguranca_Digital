<img width="826" height="416" alt="image" src="https://github.com/user-attachments/assets/ff216831-fe82-4c72-bc9f-c9a893d32444" />

# Respostas
1. Esse endereço é ignorado por padrão dos roteadores, pela regra de convenção (RFC 1918) a descartar qualquer pacote cujo endereço de origem ou destino pertença a uma faixa privada. Desse modo, esse endereço não consegue se comunicar com a internet pública.
2.  Static NAT: cada endereço IP privado é mapeado permanentemente para um endereço IP público específico.
   
    Dynamic NAT: o roteador mantém um conjunto de endereços públicos e atribui um deles, temporariamente, a cada host que precisa sair para internet.
    
    NAT Overload:  Todos os hosts saem usando o mesmo endereço IP público, o que os diferencia é a porta de origem, reescrita pelo roteador para cada conexão.

3. Ele consegue atender por meio do PAT/ NAT Overload, onde muitos hosts privados compartilhando um único endereço público, diferenciado pela porta.

4. Porque quando há dois dispositivos atrás de NAT/Firewall, os endereços privados que não são acessíveis diretamente pela internet, resultando no uso dessas técnicas para aplicações modernas para funcionar mesmo atrás de um ou mais NATs.
   
    STUN: descobre o endereço público

    TURN: retransmite quando não dá

    ICE: escolhe a melhor rota
   
5. CGNAT é a aplicação de mais uma camada de NAT: o cliente recebe um IP privado do próprio provedor, que é traduzido, junto com o de centenas de outros clientes, para um único IP público na saída da operadora. A dificuldade de exposição de um serviço próprio à internet se dá por causa de que um mesmo endereço IP público, visto em um log de acesso, pode corresponder a centenas de clientes diferentes de um provedor que usa CGNAT, todos compartilhando aquele endereço ao mesmo tempo.
