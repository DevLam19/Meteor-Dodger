

    function setup()
    M={}
    displayMode(FULLSCREEN)
    gamestate=0
    ShipX=WIDTH/2
    ShipY=150
    ShipSize=50
    score=0
    timer=0
    frequency=8
    rectW=450
    mus=0
    tx=120
    ty=HEIGHT-25
end

function LaunchMeteor()                             --LAUNCHES METEOR 
    m={}
    m.x=math.random(50, WIDTH-50)
    m.y=HEIGHT
    m.w=100
    m.h=100
    m.s=10
    table.insert(M,m)
end

function draw()                                     
    background(0, 0, 0, 255)
    font("Verdana")
    pushStyle()

    
    if gamestate==0 then  --title screen 
        score=0 
        rectW=450                     
        sprite("Documents:Galaxy", 0, 0, WIDTH*2, HEIGHT*2)
        fill(255, 255, 255, 255)
        fontSize(75) 
        text("Touch to Start", WIDTH/2, HEIGHT-100)
        text("Dodge the Meteors", WIDTH/2, HEIGHT-300)
        fontSize(45)
        text("Keep Your Finger On the Screen", WIDTH/2, HEIGHT-500)
    end

    if gamestate==1 then
        text("Meteors Dodged= "..score, tx, ty)
        if mus==0 then
        music("Dropbox:Star Fox (SNES) Corneria Music", true)
            mus=1
        end
        background(0, 0, 0, 255)
        fontSize(25)
        fill(255, 255, 255, 255)
        if timer>1/frequency then 
            LaunchMeteor()  
            timer=0 
        end
        text("Meteors Dodged= "..score, 110, HEIGHT-25)                             
        fontSize(100)
        fill(255, 255, 255, 255)                        
        sprite("Documents:Galaxy", 0, 0, WIDTH*2, HEIGHT*2)
        ShipX=CurrentTouch.x --touch
        sprite("Space Art:UFO",ShipX, ShipY, ShipSize)
        timer=timer+DeltaTime --when to spawn ship
    end
      
            for i,m in pairs(M) do
            fontSize(25)
            text("Meteors Dodged= "..score, 130, HEIGHT-25) --score
          m.y=m.y-m.s --moves meteor
        if m.y+m.h<-75then table.remove(M,i) score=score+1 end
        if gamestate==2 then table.remove(M,i) end
        sprite("Space Art:Asteroid Small",m.x,m.y,m.w,m.h)
            if ShipY+ShipSize/2>=m.y-m.h/2+20 and --collision
               ShipX+ShipSize/2+30>=m.x and 
               ShipX-ShipSize/2<=m.x+m.w/2-20 and
               ShipY-ShipSize/2<=m.y+m.h/2-20 then
               gamestate=2
               music.stop()
                mus=0
            end
        end
            
        
            if gamestate==2 then
                rectW=rectW-5 --death screen
                fill(255, 255, 255, 255)
                fontSize(50)
                background(0, 0, 0, 255)
                text("Meteors Dodged="..score, WIDTH/2, HEIGHT/2)
                text("Game Over", WIDTH/2, HEIGHT-200)
                rect(WIDTH/2-225, HEIGHT/3, rectW, 30)
                if rectW<=0 then 
                    text("Tap to Try Again!", WIDTH/2, HEIGHT/3)
                end
            end
end

function touched(touch) --all of the touch functions
    if touch.state==BEGAN and
        gamestate==0 then
        gamestate=1
    end
    if touch.state==BEGAN and 
        gamestate==2 and 
        rectW<=0 then
        gamestate=0
    end
    if touch.state==ENDED and 
        gamestate==1 then
        music.stop()
        mus=0
        gamestate=2
    end
end
