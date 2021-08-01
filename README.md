# Snake-game-
Title explains it all. Basically snake.io but single player. Not perfect but ya know, I tried. 
#snake game 
import turtle 
import time
import random

delay = 0.1

#Score 
score= 0 
high_score = 0 

#set up the screen 
wn = turtle.Screen()
wn.title("Heidi's Snake Game!!!")
wn.bgcolor("light blue")
wn.setup(width=600, height=600)
wn.tracer(0) #turns off the screen updates

#Snake head 1
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("black")
head.penup()
head.goto(0,0)
head.direction = "stop"

#Snake head 2
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("black")
head.penup()
head.goto(0,0)
head.direction = "stop"

# Snek foods
food = turtle.Turtle()
food.speed(0)
food.shape("triangle")
food.color("indigo")
food.penup()
food.goto(0,100)

segments = []

# Pen
pen = turtle.Turtle()
pen.speed(0)
pen.shape("square")
pen.color("white")
pen.penup()
pen.hideturtle()
pen.goto(0, 260)
pen.write("Score: 0  High Score: 0", align="center", font=(" Courier", 24, "normal"))

#functions
def go_up():
    if  head.direction != "up":
       head.direction = "down"

def go_down():
    if  head.direction != "down": 
       head.direction = "up"

def go_left():
    if  head.direction != "left":
       head.direction = "right"

def go_right():
    if  head.direction != "right":
       head.direction = "left"
    
def move():
    if head.direction == "up": 
        y = head.ycor()
        head.sety(y + 20)

    if head.direction == "down": 
        y = head.ycor()
        head.sety(y - 20)
 
    if head.direction == "left":
        x = head.xcor()
        head.setx(x - 20)
  
    if head.direction == "right": 
        x = head.xcor()
        head.setx(x + 20)

#keyboard bindings
wn.listen()
wn.onkeypress(go_up, "s")
wn.onkeypress(go_down, "w")
wn.onkeypress(go_left, "d")
wn.onkeypress(go_right, "a")

#main game loop
while True:
    wn.update() 

    # Check for a collision with the border  
    if head.xcor()>290 or head.xcor()<-290 or head.ycor()>290 or head.ycor()<-290:
        time.sleep(1)
        head.goto(0,0)
        head.direction = "stop"

        # Hide the segements
        for segment in segments:
            segment.goto(1000, 1000)

        # Clear the segments list 
        segments.clear()

        # Reset the score
        score = 0

        pen.clear()
        pen.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))



   # check for a collision with the food
    if head.distance(food) < 20:
        # Move the food to a random spot
        x = random.randint(-290, 290)
        y = random.randint(-290, 290)
        food.goto(x, y)

        # add a segment
        new_segment = turtle.Turtle()
        new_segment.speed(0)
        new_segment.shape("square")
        new_segment.color("violet")
        new_segment.penup()
        segments.append(new_segment)

        # Shorten the delay
        delay -= 0.001

        # Increase the score
        score += 10

        if score > high_score: 
          high_score = score

        pen.clear()
        pen.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))


    #move the end segments first in reverse order
    for index in range(len(segments)-1, 0, -1):
        x = segments[index-1].xcor()
        y = segments[index-1].ycor()
        segments[index].goto(x, y)

    # move segment 0 to where the head is
    if len(segments) > 0:
        x = head.xcor()
        y = head.ycor()
        segments[0].goto(x, y)

     
    move()
 
    # Check for head collisions with the body segments
    for segment in segments:
        if segment.distance(head) < 20:
            time.sleep(1)
            head.goto(0,0)
            head.direction = "stop"

                # Hide the segments
            for segment in segments:
               segment.goto(1000, 1000)
              
      
              # Clear the segments list 
            segments.clear()

  
    time.sleep(delay)

wn.mainloop()
