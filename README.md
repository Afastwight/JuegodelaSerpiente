import turtle, time, random

posponer = 0.1

#Marcador
score = 0 
high_score = 0

#Configuracion de la ventana
ventana = turtle.Screen()
ventana.title("Juego de Snake")
ventana.bgcolor("black")
ventana.setup(width = 600, height = 600)
ventana.tracer(0)

#Cabeza serpiente
cabeza = turtle.Turtle()
cabeza.speed(0)
cabeza.shape("square")
cabeza.color("white")
cabeza.penup()
cabeza.goto(0,0)
cabeza.direction ="stop"

#comida
comida = turtle.Turtle()
comida.speed(0)
comida.shape("circle")
comida.color("red")
comida.penup()
comida.goto(0,100)


#Textp
texto = turtle.Turtle()
texto.speed(0)
texto.color("white")
texto.penup()
texto.hideturtle()
texto.goto(0,260)
texto.write("Score: 0       High Score: 0", align ="center", font = ("Courier", 24 , "normal"))

#Segmentos / cuerpo serpiente
segmentos = []

#Funciones
def arriba():
    cabeza.direction = "up"
def abajo():
    cabeza.direction= "down"
def izquierda():
    cabeza.direction = "left"
def derecha():
    cabeza.direction = "right"
    
def mov():
    if cabeza.direction == "up":
        y = cabeza.ycor()
        cabeza.sety(y + 20)
    if cabeza.direction == "down":
        y = cabeza.ycor()
        cabeza.sety(y - 20)
    if cabeza.direction == "left":
        x = cabeza.xcor()
        cabeza.setx(x - 20)
    if cabeza.direction == "right":
        x = cabeza.xcor()
        cabeza.setx(x + 20)    
        
#Teclado
ventana.listen()
ventana.onkeypress(arriba, "Up")
ventana.onkeypress(abajo, "Down")
ventana.onkeypress(izquierda, "Left")
ventana.onkeypress(derecha, "Right")

while True:
    ventana.update()
    
    #Colisiones bordes
    if cabeza.xcor() > 280 or cabeza.xcor() < -280 or cabeza.ycor() > 280 or cabeza.ycor() < -280:
        time.sleep(1)
        cabeza.goto(0,0)
        cabeza.direction = "stop"
        
        #Esconder los segmentos.
        for segmento in segmentos:
            segmento.goto(1000, 1000)
            
        #Limpiar lista de segmentos
        segmentos.clear()
        
        #Resetear marcador
        score = 0
        texto.clear()
        texto.write("Score: {}      High Score: {}".format(score, high_score), 
                    align = "center" , font = ("Courier", 24, "normal"))
        
    if cabeza.distance(comida) < 20:
        x = random.randint(-280,280)
        y = random.randint(-280, 280)
        comida.goto(x,y)
        
        nuevo_segmento = turtle.Turtle()
        nuevo_segmento.speed(0)
        nuevo_segmento.shape("square")
        nuevo_segmento.color("grey")
        nuevo_segmento.penup()
        segmentos.append(nuevo_segmento)
        
        #Aumentar el marcador
        score += 10
        
        if score > high_score:
            high_score = score
        
        texto.clear()
        texto.write("Score: {}      High Score: {}".format(score, high_score), 
                    align = "center" , font = ("Courier", 24, "normal"))
        
    #Mover el cuerpo de la serpiente
    totalSeg = len(segmentos)
    for index in range(totalSeg -1, 0, -1):
        x = segmentos[index - 1].xcor()
        y = segmentos[index - 1].ycor()
        segmentos[index].goto(x,y)
        
    if totalSeg > 0:
        x = cabeza.xcor()
        y = cabeza.ycor()
        segmentos[0].goto(x,y)
        
    mov()
    
    #Colisiones cuerpov
    for segmento in segmentos:
        if segmento.distance(cabeza) < 20:
            time.sleep(1)
            cabeza.goto(0,0)
            cabeza.direction = "stop"
            
            #Esconder los segmentos.
            for segmento in segmentos:
                segmento.goto(1000, 1000)
                
            #Limpiar los elementos de la lista
            segmentos.clear()
            
            #Resetear marcador
            score = 0 
            texto.clear()
            texto.write("Score: {}      High Score: {}".format(score, high_score), 
                    align = "center" , font = ("Courier", 24, "normal"))
            
    time.sleep(posponer)

#Mantine la ventana hasta que se cierre manualmente
ventana.mainloop()
