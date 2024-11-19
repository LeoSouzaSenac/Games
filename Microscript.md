# Como Criar um Jogo no Microstudio

Este guia irá ajudá-lo a criar um jogo simples com um personagem que pula e evita obstáculos. Tudo isso será feito com menos de 70 linhas de código!

## Etapas do Jogo

### 1. **Criando o Personagem**

Primeiro, vamos criar um personagem para o nosso jogo. Clique em **Sprites** e adicione um sprite. Você pode desenhar seu próprio personagem ou importar um sprite já pronto. Se quiser animá-lo, use a ferramenta de **Animação**.

Após criar o sprite, vamos escrevê-lo no código. Adicione o seguinte no corpo da função `draw`:

```lua
-- Preenche a tela com uma cor de fundo roxa
screen.fillRect(0, 0, screen.width, screen.height, "rgb(57,0,57)")
-- O primeiro 0 é onde começa o retângulo (em X)
-- O segundo 0 é onde ele começa em Y
-- o terceiro é a largura
-- o quarto é a altura
-- a cor do retângulo

-- Desenha o personagem na tela nas coordenadas (-80, -50) com uma escala de 20
-- "hero" é o nome do sprite que foi criado
screen.drawSprite("hero", -80, -50, 20)
```

#### Explicação dos parâmetros de `screen.drawSprite`:
- **"hero"**: Nome do sprite que será desenhado.
- **-80**: Posição horizontal do sprite (em pixels).
- **-50**: Posição vertical do sprite (em pixels).
- **20**: Escala do sprite, determinando o tamanho na tela.

---

### 2. **Criando a Plataforma**

Vamos adicionar uma plataforma para o personagem correr. Crie um novo sprite e renomeie-o como `"platform"`. Em seguida, preencha a tela com esse sprite usando um loop `for` para criar uma plataforma contínua.

No corpo da função `draw`, adicione:

```lua
-- Desenha várias plataformas em uma linha contínua
for i = -6 to 6 by 1
  -- A cada iteração, desenha uma plataforma deslocada em 40 pixels (i*40) horizontalmente
  -- "platform" é o nome do sprite da plataforma, -80 é a altura fixa das plataformas
  screen.drawSprite("platform", i * 40, -80, 40)
end
```
#### Explicação do for:
- **for i = -6**: O laço começa com a variável i recebendo o valor de -6. Isso significa que a primeira iteração do laço começa com i igual a -6.
- **to 6**: O laço irá continuar até que i atinja o valor de 6, incluindo esse valor. Ou seja, o laço vai repetir até i ser igual a 6.
- **by 1**: O by 1 especifica o incremento (ou passo) entre cada iteração. Neste caso, significa que a cada iteração i será aumentado em 1. Portanto, a sequência de valores de i será: -6, -5, -4, ..., 6.


#### Explicação dos parâmetros de `screen.drawSprite`:
- **"platform"**: Nome do sprite da plataforma.
- **i * 40**: Posição horizontal do sprite, que varia a cada iteração, criando um efeito de repetição.
- **-80**: Posição vertical fixa das plataformas.
- **40**: Escala da plataforma, com largura de 40 pixels.

---

### 3. **Movendo as Plataformas**

Para fazer as plataformas se moverem, vamos introduzir a variável `position`. Mude o código dentro de `draw` para:

```lua
-- Desenha as plataformas se movendo para a esquerda
for i = -6 to 6 by 1
  -- O valor position % 40 garante que a plataforma se reposicione quando sai da tela
  screen.drawSprite("wall", i * 40 - position % 40, -80, 40)
end
```

### O que é `position % 40`?

O operador `position % 40` calcula o **resto da divisão** de `position` por 40. Isso cria um comportamento interessante, pois o valor de `position` está sempre sendo **incrementado**, mas o operador `% 40` garante que o valor de `position` seja **limitado a um intervalo entre 0 e 39**. O resto da divisão de qualquer número por 40 vai sempre retornar um valor entre **0** e **39**.

#### Exemplo de como funciona `position % 40`:

Vamos supor que `position` vai sendo incrementada ao longo do tempo, como mostrado no código do jogo:

1. **Quando `position = 0`:**
   - `0 % 40 = 0`
   - O valor de `position % 40` é **0** no início.

2. **Quando `position = 1`:**
   - `1 % 40 = 1`
   - O valor de `position % 40` é **1**.

3. **Quando `position = 40`:**
   - `40 % 40 = 0`
   - O valor de `position % 40` é **0** novamente, porque 40 é divisível por 40.

4. **Quando `position = 41`:**
   - `41 % 40 = 1`
   - O valor de `position % 40` volta a ser **1**.

5. **Quando `position = 79`:**
   - `79 % 40 = 39`
   - O valor de `position % 40` é **39**.

6. **Quando `position = 80`:**
   - `80 % 40 = 0`
   - O valor de `position % 40` volta a ser **0**.

#### O que isso significa para o movimento das plataformas?

Esse cálculo `position % 40` cria um efeito de **looping**, fazendo com que as plataformas se movam de forma contínua e **se reposicionem** à medida que a tela se desloca para a esquerda.

- Sem o `% 40`, se você apenas usasse `i * 40 - position`, as plataformas iriam desaparecer à esquerda da tela, porque o valor de `position` ficaria cada vez maior e as plataformas sairiam da tela sem ser reposicionadas.
  
- **Com o `% 40`**, a posição das plataformas vai se "reiniciar" sempre que `position` alcançar um múltiplo de 40 (por exemplo, quando `position` chega a 40, 80, 120, etc.). Isso faz com que as plataformas sejam redesenhadas à direita da tela, criando uma ilusão de **scrolling infinito**.

### Resumo:

O operador **`position % 40`** calcula o resto da divisão de `position` por 40 e garante que a posição das plataformas seja ajustada de maneira cíclica à medida que o valor de `position` aumenta. Isso cria um efeito de **scrolling contínuo**, onde as plataformas desaparecem pela esquerda e reaparecem à direita da tela, sem interrupção, criando a ilusão de um mundo "infinito" no jogo.


Agora, no corpo da função `update`, acrescente:

```lua
-- Incrementa o valor de position a cada atualização, fazendo as plataformas se moverem
position = position + 2
```

#### Explicação dos parâmetros de `screen.drawSprite`:
- **"wall"**: Nome do sprite da plataforma.
- **i * 40 - position % 40**: A posição horizontal é alterada pela variável `position`, criando o movimento contínuo.
- **-80**: Posição vertical fixa das plataformas.
- **40**: Escala da plataforma.

---

### 4. **Pulo do Personagem**

Agora, vamos adicionar a funcionalidade de pulo! Para isso, crie duas variáveis:

- `player_y`: para a posição vertical do personagem.
- `player_vy`: para a velocidade vertical.

No corpo da função `draw`, altere o código para:

```lua
-- Desenha o personagem, levando em conta a posição vertical (player_y)
screen.drawSprite("player", -80, -50 + player_y, 20)
```

Agora, no corpo da função `update`, adicione:

```lua
-- Atualiza a posição vertical do personagem a cada quadro, com base na sua velocidade
player_y = player_y + player_vy
```

#### Explicação dos parâmetros de `screen.drawSprite`:
- **"player"**: Nome do sprite do personagem.
- **-80**: Posição horizontal fixa do personagem.
- **-50 + player_y**: Posição vertical variável, alterada pela velocidade vertical.
- **20**: Escala do personagem.

---

### 5. **Iniciando o Pulo**

Vamos fazer o personagem pular quando o mouse for clicado ou a tecla **espaço** for pressionada.

Adicione o seguinte código na função `update` para iniciar o pulo:

```lua
-- Verifica se o mouse foi clicado ou se a tecla "espaço" foi pressionada
-- Se o personagem estiver no chão (player_y == 0), ele começa a pular
if touch.touching and player_y == 0 then
  player_vy = 7  -- Define a velocidade de pulo (velocidade vertical positiva)
end
```

#### Explicação de `touch.touching`:
- **touch.touching**: Verifica se o mouse está tocando na tela (clicado).

#### Explicação de `keyboard.JUMP` (alternativo):
- **keyboard.JUMP**: Verifica se a tecla "espaço" foi pressionada.

---

### 6. **Adicionando Gravidade**

Agora, o personagem precisa cair de volta. Para isso, vamos aplicar a gravidade, que diminui a velocidade vertical do personagem ao longo do tempo.

Adicione este código na função `update`:

```lua
-- Aplica a gravidade subtraindo uma pequena constante da velocidade vertical
player_vy = player_vy - 0.3
```

Além disso, altere o cálculo de `player_y` para garantir que ele nunca ultrapasse o chão:

```lua
-- Impede que o personagem ultrapasse o chão (posição negativa)
player_y = max(0, player_y + player_vy)
-- A função max retorna o maior valor entre dois números
```

#### Explicação de `max(0, player_y + player_vy)`:
- **max(0, valor)**: A função `max` garante que `player_y` nunca seja negativo, ou seja, o personagem não cairá abaixo da plataforma.


A função `max(a, b)` recebe dois parâmetros e retorna o maior entre eles.

## Parâmetros da Função `max()`

### Primeiro parâmetro: `0`

Este é o valor **mínimo permitido** para a variável `player_y`, ou seja, o valor **não pode ser menor que 0**. Isso é importante porque a posição `y` do personagem no eixo vertical não deve ser negativa (o personagem não deve "desaparecer" para fora da tela para cima). Esse valor atua como um **limite inferior**.

### Segundo parâmetro: `player_y + player_vy`

Este valor representa a **nova posição vertical do jogador** (após a adição da velocidade vertical `player_vy`).
- `player_y` é a **posição atual** do jogador no eixo vertical (a altura em que ele se encontra na tela).
- `player_vy` é a **velocidade vertical** do jogador, que pode ser positiva (movendo para cima, no caso de um pulo) ou negativa (movendo para baixo, devido à gravidade).

A soma de `player_y + player_vy` calcula a **nova posição** do personagem no eixo vertical com base na sua velocidade.

## O que acontece nessa linha?

A linha de código:

```lua
player_y = max(0, player_y + player_vy)
```

- `player_y + player_vy` calcula a **nova posição vertical** do personagem.
- A função `max(0, player_y + player_vy)` garante que, caso o cálculo de `player_y + player_vy` seja **menor que 0**, o valor retornado será **0**. Isso significa que o personagem nunca poderá "atravessar" o chão (o limite inferior da tela).
- Se **`player_y + player_vy`** for **maior ou igual a 0**, o valor de `player_y` será atualizado normalmente com o novo valor da soma.

## Exemplo de Funcionamento:

### Caso 1: O personagem está em cima da plataforma (`player_y = 0`) e a gravidade está atuando (`player_vy = -1`):

- `player_y + player_vy = 0 + (-1) = -1`
- A função `max(0, -1)` vai retornar **0**, impedindo que `player_y` se torne negativa (o personagem não "cai" abaixo da plataforma).
- **Resultado**: `player_y` vai se manter em **0** (o personagem está no chão).

### Caso 2: O personagem está no ar e pulando (`player_y = 10`, `player_vy = -2`):

- `player_y + player_vy = 10 + (-2) = 8`
- A função `max(0, 8)` vai retornar **8**, porque **8** é maior que **0**.
- **Resultado**: `player_y` vai ser atualizado para **8**, o que faz o personagem continuar descendo até alcançar o chão (0).

## Conclusão

A função `max(0, player_y + player_vy)` garante que o personagem não ultrapasse a linha do chão (não caia para fora da tela) ao limitar o valor de `player_y` para 0 no caso de quedas.


---

### 7. **Criando Obstáculos**

Vamos adicionar alguns obstáculos (como lâminas) que o personagem precisa evitar. Primeiro, crie um novo sprite para os obstáculos e adicione este código na função `init`:

```lua
-- Cria um array com a posição inicial dos obstáculos
blades = [200, 300, 400]
-- Array para controlar se o obstáculo foi ultrapassado com sucesso
passed = [0, 0, 0]
```

Agora, adicione o seguinte código no corpo da função `update` para movimentar os obstáculos:

```lua
-- Move os obstáculos da direita para a esquerda
for i = 0 to blades.length - 1
  -- Quando um obstáculo ultrapassa o personagem, reposiciona ele à direita
  if blades[i] < position - 120 then
    blades[i] = position + 280 + random.next() * 200
    passed[i] = 0
  end
end
```

#### Explicação de `random.next()`:
- **random.next()**: Gera um número aleatório entre 0 e 1, usado para randomizar a posição dos obstáculos.

---

### 8. **Exibindo os Obstáculos**

Agora, vamos exibir os obstáculos na tela. Adicione o seguinte código na função `draw`:

```lua
-- Desenha cada obstáculo na tela, com base nas posições do array "blades"
for i = 0 to blades.length - 1
  screen.drawSprite("ouch", blades[i] - position - 80, -50, 20)
end
```

#### Explicação dos parâmetros de `screen.drawSprite`:
- **"ouch"**: Nome do sprite do obstáculo.
- **blades[i] - position - 80**: A posição horizontal dos obstáculos é calculada com base na variável `position`, garantindo o movimento.
- **-50**: Posição vertical fixa dos obstáculos.
- **20**: Escala do obstáculo.

---

### 9. **Colisão com os Obstáculos**

Vamos verificar se o personagem colide com os obstáculos. Adicione o seguinte código na função `update`:

```lua
-- Verifica se o personagem está perto de um obstáculo
for i = 0 to blades.length - 1
  if abs(position - blades[i]) < 10 then
    -- Se o personagem estiver no chão (player_y < 10), o jogo acaba
    if player_y < 10 then
      gameover = 1
    -- Se o personagem ultrapassou o obstáculo, ele ganha 1 ponto
    elsif not passed[i] then
      passed[i] = 1
      score = score + 1
    end
  end
end
```

#### Explicação de `abs(position - blades[i]) < 10`:
- **abs()**: A função `abs` calcula o valor da diferença entre a posição do personagem e o obstáculo. Se a diferença for menor que 10, considera-se que o personagem está colidindo com o obstáculo.

---

### 10. **Exibindo a Pontuação**

Adicione este código na função `draw` para mostrar a pontuação do jogador:

```lua
-- Exibe a pontuação na tela, na posição (120, 80)
screen.drawText(score, 120, 80, 20, "#FFF")
```

#### Explicação dos parâmetros de `screen.drawText`:
- **score**: A variável que contém a pontuação atual do jogador.
- **120**: Posição horizontal do texto.
- **80**: Posição vertical do texto.
- **20**: Tamanho da fonte.
- **"#FFF"**: Cor do texto (branco).

---

### 11. **Tela de Game Over**

Quando o jogador colidir com um obstáculo, vamos exibir a tela de **Game Over**. Adicione este código na função `draw`:

```lua
-- Se o jogo acabou, exibe a tela de Game Over
if gameover then
  screen.fillRect(0, 0, screen.width, screen.height, "rgba(255, 0, 0, .5)")  -- Cor de fundo semi-transparente
  screen.drawText("GAME OVER", 0, 0, 50, "#FFF")  -- Exibe "GAME OVER" no centro da tela
end
```

---

### 12. **Reiniciando o Jogo**

Quando o jogo terminar, ele precisa reiniciar após 5 segundos. Adicione este código na função `update`:

```lua
-- Se o jogo acabou, reinicia após 5 segundos
if gameover > 0 then
  gameover = gameover + 1
  if gameover > 300 then init() end  -- Chama a função init para reiniciar o jogo
else
  -- Código normal do jogo
end
```

---

### 13. **Acelerando o Jogo**

Para aumentar a dificuldade do jogo conforme o tempo passa, vamos acelerar o movimento das plataformas e obstáculos. Adicione a variável `speed` e modifique o código dentro da função `update`:

```lua
-- Aumenta a velocidade das plataformas e obstáculos gradualmente
speed = 2
position = position + speed  -- Move as plataformas
speed = speed + 0.001  -- Aumenta a velocidade com o tempo
```

---

### 14. **Resultado Final**

Seu jogo está agora completo! Você tem um personagem que pula, obstáculos que se movem e uma tela de game over que aparece quando o jogador perde.

---

## Conclusão

Agora você tem um jogo básico em Microstudio com um personagem que pula e tenta evitar obstáculos. Com menos de 70 linhas de código, você aprendeu a criar interatividade, animações, e lógica de jogo! Divirta-se personalizando ainda mais o seu jogo.

---

### Recursos Adicionais

- [Microstudio Documentation](https://microstudio.dev/docs)
- [Exemplos de Jogos Criados no Microstudio](https://microstudio.dev/examples)
