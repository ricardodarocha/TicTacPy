Jogo da Velha[¶](#Jogo-da-Velha )
================================
  
######  Ricardo da Rocha ricardodarocha@outlook.com
  
  
Definimos um jogo da velha como uma matriz <img src="https://latex.codecogs.com/gif.latex?3×3"/>
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?(i+1,%20j+1)%20∀%20i,j%20∈%20range(3)"/></p>  
  
  
```py
mat = [(i+1, j+1) for i in range(3) for j in range(3)]
def col(x): return mat[x][0]
def lin(x): return mat[x][1]
```
  
Com as colunas e as linhas variando de <img src="https://latex.codecogs.com/gif.latex?1"/> a <img src="https://latex.codecogs.com/gif.latex?3"/> podemos obter um mapa das coordenadas
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?[(1,%201),%20(1,%202),%20(1,%203),%20(2,%201),%20(2,%202),%20(2,%203),%20(3,%201),%20(3,%202),%20(3,%203)]"/></p>  
  
  
Nós podemos calcular qualquer casa a partir das coordenadas <img src="https://latex.codecogs.com/gif.latex?i×j"/> tal que
<img src="https://latex.codecogs.com/gif.latex?casa=j+3×i"/>
  
Representamos com os valores <p align="center"><img src="https://latex.codecogs.com/gif.latex?casa%20∈%20%20[0..8]"/></p>  
  
  
---
❌⭕⬜⬜❌⬜⬜❌⬜
  
Dada uma **str** de <img src="https://latex.codecogs.com/gif.latex?9"/> caracteres = ```xo~~x~~x~```   que represente um tabuleiro de jogo da velha, podemos formatar esta **str** de $3$ em $3$
  
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
  
\# método para atualizar a string a cada iteração
def substituir(index, entrada='❌'):
    res = ''
    for i in range(9):
        res += entrada if i == index else str<p align="center"><img src="https://latex.codecogs.com/gif.latex?i"/></p>  
  
    return res
  
Calculando o vencedor[¶](#Calculando-o-vencedor )
================================================
  
Concatenando a matriz em diferentes posições podemos encontrar o vencedor
  
Primeiro vamos analisar as linhas, em seguida as colunas e por último as diagonais.
  
In <p align="center"><img src="https://latex.codecogs.com/gif.latex?"/></p>  
:
  
def solve(astr):
    verlinhas, vercolunas, verdiagonais = '', '', ''
  
    for k in range(9):
        verlinhas += astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?casa(col(k)-1,%20lin(k)-1)"/></p>  
  
        vercolunas += astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?casa(lin(k)-1,%20col(k)-1)"/></p>  
  
        if col(k) == lin(k):
            verdiagonais = astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?k"/></p>  
 + verdiagonais
        if 4-col(k) == lin(k):
            verdiagonais += astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?k"/></p>  
  
  
    return format(f'{verlinhas}{vercolunas}{verdiagonais}')
  
def winner(astr):
    res = quebratexto(solve(astr), separador='◾')
  
    for character in <p align="center"><img src="https://latex.codecogs.com/gif.latex?&#x27;❌&#x27;,%20&#x27;⭕&#x27;"/></p>  
:
        if res.find('◾'+character*3) % 4 == 0:
            return character
  
    return '⬜'
  
print(solve(str))
  
❌⭕⬜⬜❌⬜⬜❌⬜❌⬜⬜⭕❌❌⬜⬜⬜⬜❌❌⬜❌⬜
  
In <p align="center"><img src="https://latex.codecogs.com/gif.latex?"/></p>  
:
  
import os, random
  
def reiniciar(): return '⬜'*9
def desenhar(astr):
    os.system('cls')
    print(quebratexto(astr, 3, '\\n'))
  
def humanojoga(astr):
    alfa = int(input(quebratexto('1️⃣2️⃣3️⃣4️⃣5️⃣6️⃣7️⃣8️⃣9️⃣', separador='\\n')))
    if astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?alfa-1"/></p>  
 != '⬜':
        print(f'invalid {alfa}')
        return humanojoga(astr)
    astr = substituir(alfa-1)
    return astr
  
def computadorjoga(astr):
    if astr.find('⬜') == -1:
        return astr
    while True:
        alfa = random.randrange(9)
        if astr<p align="center"><img src="https://latex.codecogs.com/gif.latex?alfa"/></p>  
 != '⬜':
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
  