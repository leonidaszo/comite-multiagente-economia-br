# Notas da v2: Comitê Multiagente

Essa página não é documentação técnica, sou eu contando pra qualquer um que ler o que realmente aconteceu na v2 desse projeto. O que eu queria fazer, o que eu descobri no meio do caminho, e onde isso deixou o projeto no final.

## O que a v2 se propôs a fazer

A v1 já debatia economia brasileira com três agentes de IA (Economista, Crítico, Ministro), mas só com PIB e Inflação na mão. O problema é que, com só esses dois números, o debate ficava filosófico. Os agentes falavam coisas como "é preciso responsabilidade fiscal" ou "não há espaço para estímulo", frases que soam sérias mas não comprometem nada de concreto, porque não tem número nenhum ali pra alguém contestar.

A ideia da v2 era simples: dar mais dado de verdade pro comitê, principalmente Selic e Dívida Pública sobre o PIB, que são as duas ferramentas que o governo brasileiro realmente usa quando decide entre ajustar ou estimular a economia. Junto, também trouxemos Câmbio e Desemprego do Banco Mundial, pra fechar o quadro. A pergunta que eu queria responder era: será que mais contexto realmente muda a qualidade do debate, ou é só mais dado enfeitando a mesma conversa vazia de antes?

## O que mudou quando o dado chegou

Funcionou, e funcionou melhor do que eu esperava. O Economista Chefe passou a ancorar a conclusão dele em número real, tipo "a Dívida Bruta em 76,89% do PIB elimina qualquer margem para expansionismo fiscal", em vez de só falar em generalidades. E o efeito mais bonito foi que isso deixou o Revisor Crítico mais afiado também: com um número concreto pra atacar, ele trouxe contra-argumento técnico de verdade, como a diferença entre Dívida Bruta e Dívida Líquida, em vez de só discordar em tom.

Então esse foi o primeiro aprendizado grande: mais dado não deixa só a resposta melhor, deixa o debate inteiro mais rigoroso, dos dois lados.

## O problema que eu não esperava: números que não vieram de lugar nenhum

No meio de uma dessas execuções, o Revisor Crítico citou "R$ 400 bilhões em subsídios fiscais ineficientes". Só que esse número não está em nenhuma das duas tabelas que a gente fornece pro sistema. Fui checar se existia no mundo real, e existe: é um dado real de renúncia fiscal brasileira, que circulou bastante na imprensa econômica. O modelo não inventou do nada, ele lembrou de uma estatística real que provavelmente apareceu em notícias durante o treinamento dele, e encaixou no relatório como se tivesse sido calculada a partir do dado que a gente forneceu.

Isso não deixa o problema menor, deixa mais traiçoeiro. Se o número fosse absurdo, seria fácil desconfiar. Como ele é plausível e real, passa despercebido. Dado real e lembrança de treinamento saem escritos com a mesma confiança, no mesmo parágrafo, sem nenhuma marca que diferencie um do outro. Numa execução seguinte, o mesmo tipo de coisa aconteceu de novo, só que com outro número (dessa vez "gastos tributários que superam 4% do PIB"), confirmando que não foi um evento isolado.

## A reflexão que ficou depois disso: você direciona, mas não controla

Escrevi um prompt bem específico dizendo pro Economista usar só as duas tabelas fornecidas. Isso deveria funcionar como uma cerca. Só que o sistema pulou essa cerca sem avisar, trazendo informação de fora e costurando no meio da resposta como se fizesse parte do mesmo raciocínio fundamentado nos dados.

O que aprendi com isso foi que a fronteira entre "o que eu te dei" e "tudo que você já sabe" não existe de verdade dentro de como esses modelos funcionam. É sempre uma sugestão forte, nunca uma parede. E isso é bom e perigoso ao mesmo tempo, pelo mesmo motivo: é exatamente essa capacidade de misturar dado novo com conhecimento acumulado que faz o sistema parecer um analista de verdade, e não uma calculadora chata repetindo números de volta. Só que também é isso que impede de simplesmente confiar no relatório final sem auditar as afirmações mais específicas antes de usar em qualquer lugar sério.

## A tentativa de mitigação: obrigar o sistema a se declarar

Em vez de só documentar o problema, tentei reduzir o risco dele. Mudei os três prompts (Economista, Crítico e Ministro) pra exigir que todo número específico venha marcado com a fonte entre colchetes: Tabela 1, Tabela 2, ou Fonte externa quando o número não estiver nas tabelas e vier do conhecimento geral do modelo.

Isso não fecha o problema, só reduz o risco dele. Continua sendo uma instrução de prompt, não uma trava técnica de verdade.

Quando tentei de novo, o resultado veio melhor do que esperava: praticamente todo número apareceu marcado corretamente, e quando o Crítico trouxe uma cifra de fora de novo, ele marcou como Fonte externa, exatamente como devia. Só apareceu um efeito colateral engraçado: numa frase, o Economista se confundiu sobre qual tabela era a fonte certa e produziu uma marcação bagunçada, tipo "Tabela 2/1 (nota: dado da Tabela 1)". E o mais bonito de tudo: o próprio Revisor Crítico pegou esse erro sozinho, sem ninguém pedir, apontou explicitamente que a marcação estava confusa, e quando a informação chegou na síntese final do Ministro, já estava corrigida e limpa. O erro nasceu no Economista, foi identificado pelo Crítico, e chegou certo no Ministro, tudo dentro do próprio pipeline, sem a gente precisar intervir na mão. É a tese central do projeto (debate em cadeia é melhor que uma IA sozinha) se confirmando na prática.

## O teste de ruído: será que o comitê percebe um erro grosseiro de propósito?

Depois disso, quis ir além de só observar o que acontecia naturalmente e testar o sistema de propósito. A ideia foi plantar um dado obviamente errado, na cara, e ver se o comitê percebia.

Troquei a Inflação do Brasil no ano mais recente pelo valor real (por volta de 5%) por 400%, um nível de hiperinflação tipo Venezuela, bem fora de qualquer coisa plausível pra década. E aqui tem um detalhe importante da arquitetura que faz esse teste ser mais rico do que parece: o Revisor Crítico nunca vê a tabela de dados, ele só lê o texto que o Economista escreveu. Então esse teste não mede só "o comitê percebe erro", mede também onde, na cadeia, essa percepção acontece, ou deixa de acontecer.

Rodei 40 vezes com esse dado corrompido, e medi, em cada execução, se o Economista e o Crítico usaram alguma palavra de desconfiança no texto deles (coisas como "incoerente", "implausível", "fora do padrão", "hiperinflação").

## Os números finais

Em 40 execuções:

O Economista detectou o problema em 27 das 40, ou seja, 67,5% das vezes.
O Crítico detectou em 28 das 40, ou seja, 70% das vezes.
Os dois detectaram juntos em 18 execuções, 45%.
Só o Economista detectou, sem o Crítico pegar, em 9 execuções, 22,5%.
Só o Crítico detectou, sem o Economista ter sinalizado nada, em 10 execuções, 25%.
E em 3 execuções, 7,5% do total, nenhum dos dois percebeu nada. O dado de hiperinflação passou batido pelos dois agentes.

## O que a gente tira de tudo isso

O número mais importante desse teste inteiro é aquele "só o Crítico detectou": 25% das vezes. Isso é prova numérica, não só argumento bonito, de que ter um segundo agente revisando pega coisa que o primeiro deixou passar. A ideia central do projeto, que debate em cadeia produz resultado melhor que uma IA sozinha, deixou de ser só uma aposta de design e virou um número real.

Mas o número que merece mais respeito é o outro: 7,5% das vezes, um erro absurdamente grosseiro, uma hiperinflação de 400%, passou pelos dois agentes sem ninguém questionar nada. Pra um sistema que um dia poderia embasar decisão real, essa é a taxa de "erro grave passa despercebido mesmo com duas camadas de revisão". Não é um número que inspira confiança cega, e não deveria ser tratado como detalhe.

Juntando tudo que a v2 trouxe: mais dado deixou o comitê mais inteligente e mais convincente ao mesmo tempo, só que convincente e correto são coisas diferentes. O sistema aprendeu a citar fonte quando a gente pediu, se autocorrigiu numa falha real dentro do próprio pipeline, e pegou um erro grosseiro plantado de propósito na maioria das vezes. Mas ele também mostrou, com número, que tem uma fração real de vezes em que nada disso funciona, e um erro grave passa batido. É exatamente essa mistura de capacidade real e falha real que separa quem usa IA com espírito crítico de quem só copia e cola o que ela entrega.

---
**Muito obrigado, nos vemos na V3!**
