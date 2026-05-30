import random

import sys

def generar_matriz(n):
    """Crea matriz NxN con números aleatorios entre 99 y 999"""
    return [[random.randint(99, 999) for _ in range(n)] for _ in range(n)]


def mostrar_matriz(matriz):
    """Muestra la matriz en formato cuadrado"""
    for fila in matriz:
        print("  ".join(f"{num:4d}" for num in fila))
    print()


def contar_multiplos_dyv(matriz, fi=0, ff=None, ci=0, cf=None):
    """
    Algoritmo DIVIDE Y VENCERÁS
    Divide la matriz en 4 submatrices recursivamente
    """
    if ff is None:
        ff = len(matriz)
    if cf is None:
        cf = len(matriz[0])

    filas = ff - fi
    columnas = cf - ci

    # ==================== CASO BASE ====================
    if filas == 1 and columnas == 1:
        num = matriz[fi][ci]
        return 1 if num % 5 == 0 or num % 7 == 0 else 0

    # Si es muy pequeño, procesamos directamente (sin bucles grandes)
    if filas * columnas <= 4:   # Umbral pequeño
        count = 0
        for i in range(fi, ff):
            for j in range(ci, cf):
                if matriz[i][j] % 5 == 0 or matriz[i][j] % 7 == 0:
                    count += 1
        return count

    # ==================== DIVIDE ====================
    fila_mid = (fi + ff) // 2
    col_mid = (ci + cf) // 2

    # Cuatro submatrices
    c1 = contar_multiplos_dyv(matriz, fi, fila_mid, ci, col_mid)
    c2 = contar_multiplos_dyv(matriz, fi, fila_mid, col_mid, cf)
    c3 = contar_multiplos_dyv(matriz, fila_mid, ff, ci, col_mid)
    c4 = contar_multiplos_dyv(matriz, fila_mid, ff, col_mid, cf)

    # ==================== VENCE (combina) ====================
    return c1 + c2 + c3 + c4


# ====================== PROGRAMA PRINCIPAL ======================
if __name__ == "__main__":
    print("=== Matriz NxN con Divide y Vencerás ===\n")

    try:
        n = int(input("Ingrese el tamaño de la matriz (N x N): "))
        if n < 1:
            print("El tamaño debe ser mayor que 0.")
            sys.exit(1)
    except ValueError:
        print("Entrada inválida. Debe ser un número entero.")
        sys.exit(1)

    print(f"\nGenerando matriz {n} x {n}...\n")
    
    matriz = generar_matriz(n)

    print("Matriz generada:")
    mostrar_matriz(matriz)

    # Conteo usando Divide y Vencerás
    cantidad = contar_multiplos_dyv(matriz)

    print(f"Cantidad de números múltiplos de 5 o 7: **{cantidad}**")
