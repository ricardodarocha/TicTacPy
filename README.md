# TicTacPy

A fun tutorial of python playing with the Tic Tac Toe

**pt-br**

Jogo da Velha[¶](#Jogo-da-Velha)
================================

###### Ricardo da Rocha ricardodarocha@outlook.com

Definimos um jogo da velha como uma matriz $3×3$

$$ (i+1, j+1) ∀ i,j ∈ range(3)$$

```py
mat = [(i+1, j+1) for i in range(3) for j in range(3)]
def col(x): return mat[x][0]
def lin(x): return mat[x][1]
```

Com as colunas e as linhas variando de $1$ a $3$ podemos obter um mapa das coordenadas


\[[(1, 1), (1, 2), (1, 3), (2, 1), (2, 2), (2, 3), (3, 1), (3, 2), (3, 3)]\]

Nós podemos calcular qualquer casa a partir das coordenadas $i×j$ tal que
$casa=j+3×i$

Representamos com os valores \[casa ∈  [0..8]\]

---
❌⭕⬜⬜❌⬜⬜❌⬜

Dada uma **str** de $9$ caracteres represente um tabuleiro de jogo da velha, podemos formatar esta **str** de $3$ em $3$

```py
def quebratexto(texto, tamanho = 3, separador='\n'):
    res = ''
    for x in range(len(texto)):
        if x % (tamanho) == 0:
            res += separador+texto[x]
        else:
            res += texto[x]
    return res 

str = '❌⭕⬜⬜❌⬜⬜❌⬜'
strf = quebratexto(str, 3, '\\n')
print(strf)

❌⭕⬜
⬜❌⬜
⬜❌⬜
```

Calculando o vencedor[¶](#Calculando-o-vencedor)
================================================

Concatenando a matriz em diferentes posições podemos encontrar o vencedor

Primeiro vamos analisar as linhas, em seguida as colunas e por último as diagonais.

```py
def solve(astr):
    verlinhas, vercolunas, verdiagonais = '', '', ''

    for k in range(9):
        verlinhas += astr\[casa(col(k)-1, lin(k)-1)\]
        vercolunas += astr\[casa(lin(k)-1, col(k)-1)\]
        if col(k) == lin(k):
            verdiagonais = astr\[k\] + verdiagonais
        if 4-col(k) == lin(k):
            verdiagonais += astr\[k\]

    return format(f'{verlinhas}{vercolunas}{verdiagonais}')

def winner(astr):
    res = quebratexto(solve(astr), separador='◾')

    for character in \['❌', '⭕'\]:
        if res.find('◾'+character*3) % 4 == 0:
            return character

    return '⬜'

print(solve(str))

❌⭕⬜⬜❌⬜⬜❌⬜❌⬜⬜⭕❌❌⬜⬜⬜⬜❌❌⬜❌⬜

```
## Protótipo Interativo
Acesse através do jupyter notebook

```py

import os, random

def reiniciar(): return '⬜'*9
def desenhar(astr):
    os.system('cls')
    print(quebratexto(astr, 3, '\\n'))

def humanojoga(astr):
    alfa = int(input(quebratexto('1️⃣2️⃣3️⃣4️⃣5️⃣6️⃣7️⃣8️⃣9️⃣', separador='\\n')))
    if astr\[alfa-1\] != '⬜':
        print(f'invalid {alfa}')
        return humanojoga(astr)
    astr = substituir(alfa-1)
    return astr

def computadorjoga(astr):
    if astr.find('⬜') == -1:
        return astr
    while True:
        alfa = random.randrange(9)
        if astr\[alfa\] != '⬜':
            continue;
        astr = substituir(alfa, '⭕')
        return (astr)

str = reiniciar()
desenhar(str)

def checkWinner():
    w = winner(str)
    if w != '⬜':
        desenhar(str)
        person = '🥳' if w == '❌' else '🤯'
        print(f'\\n\\n{person}\\n{w} venceu')
        return True
    return False

while True:
    str = humanojoga(str)
    if checkWinner(): break
    str = computadorjoga(str)
    if checkWinner(): break
    desenhar(str)

⬜⬜⬜
⬜⬜⬜
⬜⬜⬜

❌⬜⬜
⬜⬜⬜
⭕⬜⬜

❌⬜⭕
⬜⬜❌
⭕⬜⬜

❌⬜⭕
⬜❌❌
⭕⬜⭕

❌⬜⭕
❌❌❌
⭕⬜⭕


🥳
❌ venceu
