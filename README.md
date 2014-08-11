# memoo

Programa simples feito em python para a convers�o de m�quinas de Mealy para m�quinas de Moore e vice versa. O nome (memoo) vem das primeiras letras dos nomes Mealy e Moore. O c�digo est� hospedado nesse link: https://github.com/possatti/memoo

## Autor

Eu (Lucas Possatti) sou o autor de todo o c�digo nesse projeto, com excess�o do arquivo `sexp.py` na raiz do projeto, que usei para fazer o parsing das S-Expressions.

O c�digo do arquivo `sexp.py` foi obitido [nesse link][http://rosettacode.org/wiki/S-Expressions#Python]. E fiz apenas algumas modifica��es nele. Apesar disso, n�o assumo qualquer cr�dito sobre esse c�digo, e infelizmente no link eu n�o encontrei qualquer men��o ao verdadeiro autor. Ent�o deixo apenas o link.

Com excess�o desse arquivo todo o resto do c�digo � de minha autoria.

## Descri��o do projeto

O projeto foi realizado como uma atividade da disciplina de Linguagens Formais e Automatos (LFA) do meu curso. O objetivo � criar um programa que leia uma defini��o de uma m�quina de Mealy ou de Moore e converta para o seu oposto.

O projeto foi codificado inteiramente na linguagem Python 3. E apesar do c�digo inteiro estar em ingl�s, ele est� todo muito bem documentado em portugu�s.

As defini��es das m�quinas de Moore e Mealy obedecem a sintaxe de S-Expressions. Eu n�o irei descrever aqui uma defini��o formal, mas voc� pode tomar os seguintes exemplos para entender como as m�quinas devem ser definidas:

### M�quina de Moore

```lisp
(moore
  (symbols-in a b)
  (symbols-out 0 1)
  (states q0 q1 q2 q3 q4 q5)
  (start q0)
  (finals q4 q5)
  (trans
    (q0 q2 a) (q0 q4 b) (q2 q5 a) (q2 q3 b)
    (q3 q1 a) (q3 q5 b) (q4 q5 a) (q4 q1 b)
    (q5 q5 a) (q5 q1 b))
  (out-fn
    (q0 ()) (q1 1) (q2 0)
    (q3 1) (q4 0) (q5 1)))
```

### M�quina de Mealy

```lisp
(mealy
  (symbols-in a b)
  (symbols-out 0 1)
  (states q0 q1 q2 q3)
  (start q0)
  (finals q3)
  (trans
    (q0 q1 a 0) (q0 q3 b 0) (q1 q2 b 1) (q1 q3 a 1)
    (q2 q3 a 0) (q2 q3 b 1) (q3 q0 b 1) (q3 q3 a 1)))
```

*ATEN��O:* Quando voc� for definir uma m�quina, voc� n�o deve usar asteriscos (*) para compor o nome dos estados, pois isso atrapalharia a conves�o. Isso porque o asterisco � usado na cria��o de novos estados na convers�o de Mealy para Moore. Portanto, n�o use estados que contenham o caracter asterisco no nome, pois o programa os usa internamente.

## Estrutura do projeto

O projeto se divide basicamente em tr�s arquivos, s�o eles:
 - `memoo.py` : Programa principal. Este � o execut�vel.
 - `converter.py` : Biblioteca com as rotinas de convers�o das m�quinas.
 - `sexp.py` : Biblioteca para leitura e escrita de S-Expressions.

Adicionalmente, na pasta `tests`, h� um conjunto de testes que podem ser executados usando a framework [cram][https://bitheap.org/cram/]. Para utilizar a framework, � necess�rio instala-la previamente. Eu sugiro instala-la atrav�s do [pip][https://pypi.python.org/pypi/pip], que talvez j� venha instalado na sua distribui��o. Assim para instalar o `cram` � necess�rio um �nico comando:

```bash
$ sudo pip install cram
```

Para executar os testes, basta executar o `cram` indicando o diret�rio que cont�m os testes, da seguinte forma:

```bash
$ cram tests/
```

Tamb�m h� uma pasta chamada `samples`, onde est�o tr�s exemplos de m�quinas de Moore e tr�s de Mealy. Inclusive, essas m�quinas foram usadas como base para a elabora��o dos testes.

## Modo de uso

O programa principal (memoo.py) � executado atrav�s da linha de comando, e deve ser interpretado usando um interpretador de Python 3.

Se o programa for executado sem qualquer argumento, ele l� o standard input em busca da S-Expression com a defini��o de uma m�quina v�lida. E ao conseguir uma leitura v�lida, converte a m�quina e escreve o resultado para o standard output.

Ou se o usu�rio preferir, � poss�vel indicar arquivos para a leitura e sa�da atrav�s das seguintes op��es:
 - -i FILE, --input FILE: Indica o local que cont�m a defini��o da m�quina a ser convertida.
 - -o FILE, --output FILE: Indica o local onde a m�quina convertida dever� ser escrita.

Assim, um exemplo de uma poss�vel chamada ao programa �:

```bash
$ ./memoo.py -i input-machine.txt -o output-machine.txt
```

Tamb�m � poss�vel ler um pequeno texto de ajuda atrav�s da op��o `-h`.
