# GonzalezFabian_Mayor
Números a comparar entre 3 y 5
# Programa: Número mayor entre N números (mínimo 3, máximo 5)
# Autor: [Fabian Gonzalez]
# Archivo: GonzalezFabian_Mayor.asm
# Descripción: El programa solicita al usuario cuántos números quiere comparar (3 a 5),
#              luego lee cada número por consola y determina cuál es el mayor.
#              Finalmente muestra el número mayor en pantalla.

.data
    msg1: .asciiz "¿Cuántos números desea comparar? (3-5): "
    msg2: .asciiz "Ingrese un numero: "
    msg3: .asciiz "El número mayor es: "

.text
.globl main
main:

    # --- Pedir cantidad de números a comparar ---
    li $v0, 4              # syscall print_string
    la $a0, msg1
    syscall

    li $v0, 5              # syscall read_int
    syscall
    move $t0, $v0          # $t0 = cantidad de números (n)

    # Inicializamos contador
    li $t1, 0              # contador de números ingresados
    li $t2, -2147483648    # mayor inicial (mínimo entero posible)

loop_input:
    # Si contador == cantidad, salir del loop
    beq $t1, $t0, fin

    # Pedir número al usuario
    li $v0, 4
    la $a0, msg2
    syscall

    li $v0, 5
    syscall
    move $t3, $v0          # $t3 = número ingresado

    # Comparar con el mayor actual
    bgt $t3, $t2, update   # si $t3 > $t2, actualizar mayor
    j continue

update:
    move $t2, $t3          # nuevo mayor = número ingresado

continue:
    addi $t1, $t1, 1       # incrementar contador
    j loop_input

# --- Mostrar resultado ---
fin:
    li $v0, 4
    la $a0, msg3
    syscall

    li $v0, 1
    move $a0, $t2
    syscall

    # Salir del programa
    li $v0, 10
    syscall
