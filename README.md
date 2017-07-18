# PWS-swarm-intelligence


# PWS 0.5 

import pygame as pg, math, random
pg.init()


#Set up screen
width,height = 600,600
screen = pg.display.set_mode((width,height))
clock = pg.time.Clock()


#Variables
black, white, obstaclecolor, antcolor, grey = (0,0,0), (255,255,255), (255,0,0), (244, 241, 66), (40,40,40)
rows, columns = 10, 10
rectangles = []
rectborder = 1
rectx = width/columns - 2*rectborder
recty = height/rows - 2*rectborder
richtingen = [[1,0],[-1,0],[0,1],[0,-1]]

print rectangles



# define object rectangle
class rectangle(object):
    def __init__(self, c, r, colornumber, rwidth=rectx, rheight=recty,rectborder=rectborder):
        self.c = c
        self.r = r
        self.width = rwidth
        self.height = rheight
        self.border = rectborder
        self.colornumber = colornumber
    
    def draw(self):
        if self.colornumber == 0:
            self.color = black
        elif self.colornumber == 1:
            self.color = antcolor
        elif self.colornumber == 2:
            self.color = obstaclecolor
        pg.draw.rect(screen,self.color,(self.c*(self.width+2*self.border)+self.border, self.r*(self.height+2*self.border)+self.border, self.width, self.height))


#append rect to rectangles list
for x in range(columns):
    for y in range(rows):
        rectangles.append(rectangle(x, y, 0))
#obstacles side
for x in range(columns+2):
    for y in range(rows+2):
        if (x==0 or x==columns+2) and (y==0 or y==rows+2):
            rectangles.append(rectangle(x-1, y-1, 2))


class ant(object):
    def __init__(self, x, y, colornumber=1):
        self.x = x
        self.y = y
        self.colornumber = colornumber
        self.directions = richtingen
    
    def pos(self):
        rectangles[self.x*columns + self.y].colornumber = self.colornumber
        
    def look(self):
        if rectangles[(self.x-1)*columns + self.y].colornumber == 2:
            self.directions.pop(1)
            print "1"
        if rectangles[(self.x+1)*columns + self.y].colornumber == 2:
            self.directions.pop(0)
            print "2"
        if rectangles[(self.x)*columns + self.y-1].colornumber == 2:
            self.directions.pop(4)
            print "3"
        if rectangles[(self.x)*columns + self.y+1].colornumber == 2:
            self.directions.pop(3)
            print "4"
        
    def move(self):
        self.directions = richtingen
        self.look()
        randnumber = random.randint(0,len(self.directions)-1)
        self.x += self.directions[randnumber][0]
        self.y += self.directions[randnumber][1]



freek = ant(3,6)
    
while True:
#    check if exit
    for event in pg.event.get():
        if event.type == pg.QUIT:
            pg.quit()
            quit(0)   
    
    
#    draw background
    screen.fill(grey)
    
#    drawrect()
    freek.move()    
    freek.pos()
    for element in rectangles:
            element.draw()

            
    pg.display.update()
    clock.tick(3)
