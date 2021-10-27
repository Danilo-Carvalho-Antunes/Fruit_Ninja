# Fruit_Ninja
import pyxel
import random
from math import sqrt

radius = 8
gravity = 5 / 60

cores = [
    0,
    0,
    0,
    0,
    0,
    0,
    2,
    3,
    4,
    6,
    7,
    9,
    10,
    11,
    12,
    13,
    14,
    15,
]


class Fruta:
    def __init__(
        self, x, y, radius, vx , vy, color=pyxel.COLOR_BROWN, life=float("inf")
    ):
        self.x = x
        self.y = y
        self.radius = radius
        self.color = color
        self.life = life
        self.vx = vx
        self.vy = vy

    def clic(self, x, y, radius):
        if pyxel.btnp(pyxel.MOUSE_LEFT_BUTTON):
            dx = pyxel.mouse_x - x
            dy = pyxel.mouse_y - y
            if sqrt(dx ** 2 + dy ** 2) < radius:
                self.vx = 0
                self.vy = 0
        self.x += self.vx
        self.y += self.vy
        if self.x == 0:
            self.color = pyxel.frame_count % 16

    def fall(self):
        self.x += self.vx 
        self.y += self.vy 
        self.vy += gravity

    def draw(self):
        pyxel.circ (self.x, self.y, self.radius, self.color)

def update():
    if pyxel.btnp(pyxel.KEY_Q):
        pyxel.quit()
    for circ in circ_list.copy():
        circ.life -= 1
        if circ.life <= 0:
            i = circ_list.index(circ)
            del circ_list[i]
        else :
            circ.fall()
    

    # Cria um fruta novo a cada 15 frames e adiciona na lista
    if pyxel.frame_count % 8 == 0 and len(circ_list) < 100:
        circ = random_circ()
        circ_list.append(circ)

def random_circ():
    global radius
    color = random.choice(cores)
    x = random.uniform(0, 160)
    y = random.uniform(120, 120)
    radius = random.uniform(7, 10)
    vx = random.uniform(-2,2)
    vy = random.uniform ( -4.2 ,  -3)
    return Fruta(x, y,radius = radius ,vx = vx , vy = vy,color= color , life=110)


def draw():
    pyxel.cls(5)
    pyxel.bltm(15, 0, 0, 0, 0, 20, 16, pyxel.COLOR_CYAN)
    for circ in circ_list:
        circ.draw()

pyxel.init(160, 124, caption="Fruit Ninja", fps=30)
circ_list = [random_circ()]
pyxel.load("dojo.pyxres")
pyxel.mouse(True)
pyxel.run(update, draw)
