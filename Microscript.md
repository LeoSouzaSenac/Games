# Como Criar um Jogo no Microstudio

Este guia irá ajudá-lo a criar um jogo simples com um personagem que pula e evita obstáculos. Tudo isso será feito com menos de 70 linhas de código!

## Etapas do Jogo

### 1. **Criando o Personagem**

Primeiro, vamos criar um personagem para o nosso jogo. Clique em **Sprites** e adicione um sprite. Você pode desenhar seu próprio personagem ou importar um sprite já pronto. Se quiser animá-lo, use a ferramenta de **Animação**.

Após criar o sprite, vamos escrevê-lo no código. Adicione o seguinte no corpo da função `draw`:

```lua
screen.fillRect(0, 0, screen.width, screen.height, "rgb(57,0,57)")
screen.drawSprite("hero", -80, -50, 20)
```

Esse código vai desenhar o nosso personagem na tela. A função `screen.drawSprite` vai desenhar o sprite com o nome `"hero"` nas coordenadas fornecidas.

---

### 2. **Criando a Plataforma**

Vamos adicionar uma plataforma para o personagem correr. Crie um novo sprite e renomeie-o como `"platform"`. Em seguida, preencha a tela com esse sprite usando um loop `for` para criar uma plataforma contínua.

No corpo da função `draw`, adicione:

```lua
for i = -6 to 6 by 1
  screen.drawSprite("platform", i * 40, -80, 40)
end
```

Agora, o seu jogo tem uma plataforma que se estende de ponta a ponta na tela.

---

### 3. **Movendo as Plataformas**

Para fazer as plataformas se moverem, vamos introduzir a variável `position`. Mude o código dentro de `draw` para:

```lua
screen.drawSprite("wall", i * 40 - position % 40, -80, 40)
```

Agora, no corpo da função `update`, acrescente:

```lua
position = position + 2
```

Com isso, as plataformas vão se mover para a esquerda. A variável `position % 40` faz com que a plataforma se reposicione quando chega ao fim da tela, criando uma ilusão de movimento contínuo.

---

### 4. **Pulo do Personagem**

Agora, vamos adicionar a funcionalidade de pulo! Para isso, crie duas variáveis:

- `player_y`: para a posição vertical do personagem.
- `player_vy`: para a velocidade vertical.

No corpo da função `draw`, altere o código para:

```lua
screen.drawSprite("player", -80, -50 + player_y, 20)
```

Agora, no corpo da função `update`, adicione:

```lua
player_y = player_y + player_vy
```

A variável `player_vy` vai controlar a velocidade de subida e descida do personagem.

---

### 5. **Iniciando o Pulo**

Vamos fazer o personagem pular quando o mouse for clicado ou a tecla **espaço** for pressionada.

Adicione o seguinte código na função `update` para iniciar o pulo:

```lua
if touch.touching and player_y == 0 then
  player_vy = 7
end
```

Esse código permite que o personagem pule se ele estiver no chão (quando `player_y == 0`).

---

### 6. **Adicionando Gravidade**

Agora, o personagem precisa cair de volta. Para isso, vamos aplicar a gravidade, que diminui a velocidade vertical do personagem ao longo do tempo.

Adicione este código na função `update`:

```lua
player_vy = player_vy - 0.3
```

Além disso, altere o cálculo de `player_y` para garantir que ele nunca ultrapasse o chão:

```lua
player_y = max(0, player_y + player_vy)
```

Com isso, o personagem vai cair de volta ao chão após pular.

---

### 7. **Criando Obstáculos**

Vamos adicionar alguns obstáculos (como lâminas) que o personagem precisa evitar. Primeiro, crie um novo sprite para os obstáculos e adicione este código na função `init`:

```lua
blades = [200, 300, 400]
passed = [0, 0, 0]
```

Agora, adicione o seguinte código no corpo da função `update` para movimentar os obstáculos:

```lua
for i = 0 to blades.length - 1
  if blades[i] < position - 120 then
    blades[i] = position + 280 + random.next() * 200
    passed[i] = 0
  end
end
```

Este código move os obstáculos para a esquerda, e quando um obstáculo sai da tela, ele reaparece à direita.

---

### 8. **Exibindo os Obstáculos**

Agora, vamos exibir os obstáculos na tela. Adicione o seguinte código na função `draw`:

```lua
for i = 0 to blades.length - 1
  screen.drawSprite("ouch", blades[i] - position - 80, -50, 20)
end
```

Esse código desenha os obstáculos na tela, e eles vão se mover à medida que a variável `position` é atualizada.

---

### 9. **Colisão com os Obstáculos**

Vamos verificar se o personagem colide com os obstáculos. Adicione o seguinte código na função `update`:

```lua
for i = 0 to blades.length - 1
  if abs(position - blades[i]) < 10 then
    if player_y < 10 then
      gameover = 1
    elsif not passed[i] then
      passed[i] = 1
      score = score + 1
    end
  end
end
```

Aqui, verificamos se o personagem está perto de um obstáculo e se ele não está pulando. Se colidir, o jogo termina.

---

### 10. **Exibindo a Pontuação**

Adicione este código na função `draw` para mostrar a pontuação do jogador:

```lua
screen.drawText(score, 120, 80, 20, "#FFF")
```

A pontuação será incrementada sempre que o personagem pular sobre um obstáculo com sucesso.

---

### 11. **Tela de Game Over**

Quando o jogador colidir com um obstáculo, vamos exibir a tela de **Game Over**. Adicione este código na função `draw`:

```lua
if gameover then
  screen.fillRect(0, 0, screen.width, screen.height, "rgba(255, 0, 0, .5)")
  screen.drawText("GAME OVER", 0, 0, 50, "#FFF")
end
```

A tela de **Game Over** ficará semi-transparente, cobrindo a interface do jogo.

---

### 12. **Reiniciando o Jogo**

Quando o jogo terminar, ele precisa reiniciar após 5 segundos. Adicione este código na função `update`:

```lua
if gameover > 0 then
  gameover = gameover + 1
  if gameover > 300 then init() end
else
  -- código do jogo
end
```

Isso vai reiniciar o jogo 5 segundos após o game over.

---

### 13. **Acelerando o Jogo**

Para aumentar a dificuldade do jogo conforme o tempo passa, vamos acelerar o movimento das plataformas e obstáculos. Adicione a variável `speed` e modifique o código dentro da função `update`:

```lua
speed = 2
position = position + speed
speed = speed + 0.001
```

Com isso, a velocidade do jogo vai aumentar ao longo do tempo.

---

### 14. **Resultado Final**

Seu jogo está agora completo! Você tem um personagem que pula, obstáculos que se movem e uma tela de game over que aparece quando o jogador perde.

Abaixo está o fluxo básico do jogo:

```plaintext
Início → Personagem aparece → Plataformas se movem → Obstáculos surgem → Pulo do personagem → Game Over (se colidir) → Reinício
```

---

## Conclusão

Agora você tem um jogo básico em Microstudio com um personagem que pula e tenta evitar obstáculos. Com menos de 70 linhas de código, você aprendeu a criar interatividade, animações, e lógica de jogo! Divirta-se personalizando ainda mais o seu jogo.

![Imagem do Jogo](https://example.com/imagem_do_jogo.png)  
(Imagem ilustrativa do jogo)

---

### Recursos Adicionais

- [Microstudio Documentation](https://microstudio.dev/docs)
- [Exemplos de Jogos Criados no Microstudio](https://microstudio.dev/examples)

