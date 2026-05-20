+++
date = '2026-05-19T20:59:46-03:00'
draft = false
math  = true
title = 'Construindo um simulador de atitude e órbita para satélites - Parte 1'
+++

No nível mais básico, um satélite pode ser modelado como um corpo rígido com 6 graus de liberdade em órbita ao redor da Terra: 3 de posição (onde ele está no espaço)
e 3 de orientação (como ele está rotacionado a respeito de algum outro sistema de referência).

A cinemática (como essas 6 variáveis muda com o tempo) e sua dinâmica (como as forças e torques que atuam no satélite afetam essa variação) são também relativamente simples. 
Se contruirmos as equações dinâmicas que descrevem como a posição do satélite evolui e como ele gira ao longo de sua órbita, já temos algo bastante interessante.

Mas usualmente queremos que o satélite faça algumas coisas além de se mover sem sentido pelo espaço; queremos que ele meça alguma coisa ou aponte para algum lugar e transmita algo. Essas funções requerem muitas outras coisas que precisamos modelar, como sensores, atuadores, algoritmos de controle, o ambiente espacial e bla bla bla.

Um jeito de dividirmos uma simulador simples seria algo como:

1. Dinâmica e Cinemática de Atitude e de Órbita: os seis graus de liberdade que mencionei acima e como eles evoluem. Esse é o principal.
2. Sensores e Atuadores: os principais são sensores de atitude (magnetômetros, sensores solares, sensores estelares), para sabermos como o satélite está orientado. A posição também é importante (poderíamos usar algo como GPS dependendo da altura da órbita). Os atuadores são os mecanismo que fazem o satélite se mexer; exemplos comuns são rodas de reação ou propulsores.
3. Ambiente: o campo magnético da Terra, a atmosfera (assumindo um órbita baixa, já que em altitudes maiores não vai ser relevante), a posição do Sol e dos outros astros, o campo gravitacional...Modelar isso não só é importante para que seja possível modelar certos sensores, mas porque o ambiente espacial causa certas perturbações no satélite que afetam sua operação (por exemplo, o satélite interage com o campo magnético da Terra causando um torque nele. Isso afeta nosso apontamento, por exemplo).
4. Software de Voo: os algoritmos de controle e de estimação que efetivamente processam os dados que o satélite recebe e enviam comandos para os atuadores do satélite. São necessários porque não tem ninguém pra operar o satélite lá em cima.

A organização que acabei usando foi a seguinte:

1. Dinâmica de Atitude: tudo relacionado a como o satélite efetivamente gira em função dos torque que afetam ele.
2. Dinâmica Orbital: como a posição do satélite muda com o tempo
3. Controle: as leis de controle que comandam os atuadores do satélite
4. Atuadores: naturalmente, os atuadores do satélite (no meu caso, rodas de reação e atuadores magnéticos)
5. Sensores: idem. Os que modelei foram o giroscópio (mede velocidade angular), sensor solar (direção do sol), magnetômetro (campo magnético) e sensor estelar (mede a orientação do satélite baseado no campo de estrelas que ele vê)
6. Navegação: os algoritmos que estimam a atitude atual do satélite baseado nas saídas dos sensores. É basicamente só um filtro de Kalman não linear adaptado para estimação de atitude (O MEKF, Filtro de Kalman Estendido Multiplicativo)
7. Ambiente: toda modelagem do ambiente espacial em baixa órbita terrestre
8. Perturbações: os distúrbios que afetam o satélite
9. Guidance: eu precisava de algo pra chamar toda a lógica de modos de operação do satélite, e Guidance parecia bem legal e profissional
10. Extras: aqui entra lógica auxiliar (mudanças entre sistemas de referência principalmente) e performance (como os algoritmos de controle e estimação estão funcionando)

Claro, pode-se notar que o que eu fiz visa uma aplicação específica, um satélite em baixa órbita terrestre, e um bem simples: algo como um CubeSat, ou pelo menos um satélite sem
muitas coisas "complicadas" (paíneis solares, uma geometria esquisita, algum atuador mais exótico etc)

Só isso já inclui bastante coisa e te deixa com um simualdor relativamente decente, pelo menos até onde eu sei. Não é nada profissional, mas não tá errado nem nada. Não é muito útil pra análises de longo prazo no entanto, nem algo como missões multiplanetárias, transferências de órbitas, dinâmica com mais de um corpo, coisas como docking em uma estação espacial etc.

Então não tem muito propósito como ferramenta, mas ainda tem algum propósito como um projeto educativo e acadêmico, então achei que valia meu tempo investir nele.

Nos próximos posts que eu fizer, vou tentar explicar cada módulo relevante, tanto como documentação sobre o que eu fiz e como ajuda pra qualquer outra pessoa que se interesse.