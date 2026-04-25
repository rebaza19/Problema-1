import random

# Clase Equipo
class Equipo:
    def __init__(self, nombre):
        self.nombre = nombre
        self.partidosGanados = 0
        self.partidosPerdidos = 0
        self.setGanados = 0


# Crear equipos
equipo1 = Equipo("Equipo A")
equipo2 = Equipo("Equipo B")


# Función para registrar set
def RegistraSet(ganador):
    global equipo1, equipo2

    if ganador == 1:
        equipo1.setGanados += 1
        print(equipo1.nombre, "ganó un set")

        if equipo1.setGanados == 3:
            equipo1.partidosGanados += 1
            equipo2.partidosPerdidos += 1
            print(">>>", equipo1.nombre, "ganó el partido")

            equipo1.setGanados = 0
            equipo2.setGanados = 0

    else:
        equipo2.setGanados += 1
        print(equipo2.nombre, "ganó un set")

        if equipo2.setGanados == 3:
            equipo2.partidosGanados += 1
            equipo1.partidosPerdidos += 1
            print(">>>", equipo2.nombre, "ganó el partido")

            equipo1.setGanados = 0
            equipo2.setGanados = 0


# Función puntos
def Puntos():
    return random.randint(10, 28)


# Función puntos extras
def PuntosExtras():
    return random.randint(0, 6)


# Función para jugar un partido
def JugarPartido():
    global equipo1, equipo2

    print("\n--- Nuevo Partido ---")

    while equipo1.setGanados < 3 and equipo2.setGanados < 3:

        p1 = Puntos()
        p2 = Puntos()

        print("Puntos iniciales:", equipo1.nombre, p1, "-", equipo2.nombre, p2)

        # Caso normal (alguien llega a 25 o más)
        if p1 >= 25 or p2 >= 25:
            if p1 > p2:
                RegistraSet(1)
            else:
                RegistraSet(2)

        else:
            # Puntos extra hasta que alguien supere 25
            while True:
                extra1 = PuntosExtras()
                extra2 = PuntosExtras()

                p1 += extra1
                p2 += extra2

                print("Extras:", equipo1.nombre, p1, "-", equipo2.nombre, p2)

                if (p1 >= 25 or p2 >= 25) and p1 != p2:
                    if p1 > p2:
                        RegistraSet(1)
                    else:
                        RegistraSet(2)
                    break


# Resultado final
def ResultadoTorneo():
    print("\n=== RESULTADO FINAL ===")
    print(equipo1.nombre)
    print("Ganados:", equipo1.partidosGanados)
    print("Perdidos:", equipo1.partidosPerdidos)

    print("\n", equipo2.nombre)
    print("Ganados:", equipo2.partidosGanados)
    print("Perdidos:", equipo2.partidosPerdidos)


# Programa principal
cantidad = int(input("¿Cuántos partidos se jugarán? "))

for i in range(cantidad):
    JugarPartido()

ResultadoTorneo()
