import pygame
import sys

# Inicializar Pygame
pygame.init()

# Configuración de la ventana
ancho_ventana = 800
alto_ventana = 600
ventana = pygame.display.set_mode((ancho_ventana, alto_ventana))
pygame.display.set_caption("Escape de Troya")

# Colores
NEGRO = (0, 0, 0)
BLANCO = (255, 255, 255)

# Clases del juego
class Jugador(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([40, 50])
        self.image.fill(BLANCO)
        self.rect = self.image.get_rect()
        self.rect.center = (ancho_ventana // 2, alto_ventana // 2)

    def update(self, movimientos):
        self.rect.x += movimientos[0]
        self.rect.y += movimientos[1]

class Soldado(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([30, 40])
        self.image.fill(BLANCO)
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(ancho_ventana - self.rect.width)
        self.rect.y = random.randrange(alto_ventana - self.rect.height)

    def update(self):
        # Aquí iría la lógica para que los soldados busquen al jugador
        pass

# Funciones del juego
def juego():
    jugador = Jugador()
    soldados = pygame.sprite.Group()

    for i in range(5):
        soldado = Soldado()
        soldados.add(soldado)

    todos_los_sprites = pygame.sprite.Group(jugador, soldados)

    reloj = pygame.time.Clock()
    ejecutando = True
    while ejecutando:
        reloj.tick(60)
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                ejecutando = False

        movimientos = [0, 0]
        teclas = pygame.key.get_pressed()
        if teclas[pygame.K_LEFT]:
            movimientos[0] = -5
        if teclas[pygame.K_RIGHT]:
            movimientos[0] = 5
        if teclas[pygame.K_UP]:
            movimientos[1] = -5
        if teclas[pygame.K_DOWN]:
            movimientos[1] = 5

        jugador.update(movimientos)
        soldados.update()

        ventana.fill(NEGRO)
        todos_los_sprites.draw(ventana)
        pygame.display.flip()

    pygame.quit()
    sys.exit()

# Iniciar el juego
if __name__ == "__main__":
    juego()
