# Laberinto 9x9

laberinto = [
    ['F', 1, 1, 1, 0, 1, 1, 1, 1],
    [-2, 0, 0, -1, 0, 1, 0, 1, 0],
    [1, 1, 0, 1, 1, 1, 0, 1, 0],
    [0, 1, 0, -1, 0, 0, 0, -1, 0],
    [1, 1, 1, 1, 1, 1, 1, 1, 0],
    [-1, 0, 0, 0, 0, 0, 0, 1, 1],
    [1, 1, 1, 1, -1, 1, 1, 1, 0],
    [1, 0, 0, 1, 0, 1, 0, 1, 0],
    ['I', 1, -1, 1, 1, 1, 0, 1, 1]
]

FILAS = 9
COLUMNAS = 9
VIDAS_INICIALES = 3

# Orden de movimiento: abajo, derecha, arriba, izquierda
movimientos = [(1, 0), (0, 1), (-1, 0), (0, -1)]

# Matriz solución
solucion = [[0 for _ in range(COLUMNAS)] for _ in range(FILAS)]

inicio = (8, 0)
fin = (0, 0)


def es_valido(fila, columna, visitados):
    return (
        0 <= fila < FILAS and
        0 <= columna < COLUMNAS and
        laberinto[fila][columna] != 0 and
        not visitados[fila][columna]
    )


def costo_celda(valor):
    if valor == -1:
        return 1
    elif valor == -2:
        return 2
    return 0


def mostrar_paso(fila, columna, vidas):
    print(f"Posición: ({fila}, {columna}) - Vidas restantes: {vidas}")


def backtracking(fila, columna, vidas, visitados):

    if (fila, columna) == fin:
        solucion[fila][columna] = 1
        mostrar_paso(fila, columna, vidas)
        return True

    visitados[fila][columna] = True
    solucion[fila][columna] = 1

    mostrar_paso(fila, columna, vidas)

    for df, dc in movimientos:
        nueva_fila = fila + df
        nueva_columna = columna + dc

        if es_valido(nueva_fila, nueva_columna, visitados):

            valor = laberinto[nueva_fila][nueva_columna]

            if valor in ['I', 'F']:
                perdida = 0
            else:
                perdida = costo_celda(valor)

            nuevas_vidas = vidas - perdida

            if nuevas_vidas > 0:
                if backtracking(
                    nueva_fila,
                    nueva_columna,
                    nuevas_vidas,
                    visitados
                ):
                    return True

    solucion[fila][columna] = 0
    visitados[fila][columna] = False

    return False


print("LABERINTO ORIGINAL:\n")

for fila in laberinto:
    print(fila)

visitados = [[False for _ in range(COLUMNAS)] for _ in range(FILAS)]

print("\nRECORRIDO DEL RATÓN:\n")

encontrado = backtracking(
    inicio[0],
    inicio[1],
    VIDAS_INICIALES,
    visitados
)

if encontrado:
    print("\n¡El ratón logró salir del laberinto!\n")
    print("MATRIZ SOLUCIÓN:")

    for fila in solucion:
        print(fila)
else:
    print("\nNo existe un camino viable.")
