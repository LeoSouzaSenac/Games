

init = function()
  gameover = 0
  position = 0
  score = 0
  player_y // posição vertical
  player_vy // velocidade vertical
  blades = [200, 300, 400]
  passed = [0,0,0]
end

update = function()
  
  if gameover > 0 then
  gameover = gameover + 1
  if gameover > 300 then init() end
else
  
  position = position + 2
  player_y = max(0, player_y + player_vy)
  
  if keyboard.SPACE and player_y == 0 then
    player_vy = 7
  end
  
  player_vy -= 0.3
  
  for i = 0 to blades.length - 1
  
    if blades[i] < position - 120 then
      blades[i] = position + 280 + random.next() * 200
      passed[i] = 0
    end
    
  end
  
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
    end
    
    
  
end

draw = function()
  screen.fillRect(0, 0, screen.width, screen.height,"rgb(81,49,147)")
  screen.drawSprite("player", -80, -50 + player_y, 60)
  
  for i = -6 to 6 by 1
    screen.drawSprite("platform", i * 40 - position % 40, -80, 40)
  end
  
  for i = 0 to blades.length - 1
  screen.drawSprite("ouch", blades[i] - position - 80, -65, 20)
  end
  
  screen.drawText(score, 120, 80, 20, "#FFF")
  
  if gameover then
  screen.fillRect(0, 0, screen.width, screen.height, "rgba(255, 0, 0, .5)")  -- Cor de fundo semi-transparente
  screen.drawText("GAME OVER", 0, 0, 50, "#FFF")
  end
  
end
