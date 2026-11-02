# =============================================================================
# SISTEMA DE GESTIÓN DE ESTUDIANTES (SGE) BÁSICO
# Desarrollado para la Actividad 4 - Fundamentos de Programación
# =============================================================================

# Estructura de datos global: Lista para almacenar los estudiantes dinámicamente
lista_estudiantes = []

# Variables especiales requeridas
contador_estudiantes = 0  # Variable de conteo para el total de registros

def mostrar_estudiante(estudiante):
    """
    Función reutilizable para imprimir de forma ordenada los datos de un estudiante.
    Complejidad Temporal: O(1) - Tiempo constante.
    """
    print(f"ID: {estudiante['id']} | Nombre: {estudiante['nombre']} | Edad: {estudiante['edad']} | Carrera: {estudiante['carrera']}")


def agregar_estudiante():
    """
    Registra un nuevo estudiante validando las entradas para evitar errores de tipo.
    Permite volver al menú principal si se ingresa 'X'.
    """
    global contador_estudiantes
    print("\n--- REGISTRAR NUEVO ESTUDIANTE ---")
    print("(Escribe 'X' en cualquier momento para cancelar y volver al menú)")
    
    # Validación de la entrada del ID (debe ser un número entero)
    while True:
        entrada_id = input("Ingrese el ID único del estudiante (solo números): ").strip()
        
        # Opción para salir al menú principal
        if entrada_id.lower() == 'x':
            print(" Operación cancelada. Volviendo al menú principal...")
            return
            
        try:
            id_estudiante = int(entrada_id)
            
            # Validar que el ID no esté repetido
            id_existe = False
            for est in lista_estudiantes:
                if est['id'] == id_estudiante:
                    id_existe = True
                    break
            
            if id_existe:
                print(" Error: Ya existe un estudiante registrado con este ID.")
                continue
            break
        except ValueError:
            print(" Entrada inválida. Por favor, ingrese un número entero o 'X' para cancelar.")

    # Validación del nombre
    nombre = input("Ingrese el nombre completo: ").strip()
    if nombre.lower() == 'x':
        print(" Operación cancelada. Volviendo al menú principal...")
        return
    
    # Validación de la edad
    while True:
        entrada_edad = input("Ingrese la edad del estudiante: ").strip()
        if entrada_edad.lower() == 'x':
            print(" Operación cancelada. Volviendo al menú principal...")
            return
            
        try:
            edad = int(entrada_edad)
            if edad <= 0 or edad > 100:
                print(" Por favor, ingrese una edad válida (1 - 100).")
                continue
            break
        except ValueError:
            print(" Entrada inválida. Ingrese un número para la edad o 'X' para cancelar.")
            
    carrera = input("Ingrese la carrera universitaria: ").strip()
    if carrera.lower() == 'x':
        print(" Operación cancelada. Volviendo al menú principal...")
        return

    # Creación del diccionario del estudiante
    nuevo_estudiante = {
        "id": id_estudiante,
        "nombre": nombre,
        "edad": edad,
        "carrera": carrera
    }

    # Guardar en la estructura de datos simple (Lista)
    lista_estudiantes.append(nuevo_estudiante)
    contador_estudiantes += 1  # Incremento del contador (Variable especial)
    print(f" Estudiante '{nombre}' agregado correctamente. Total en el sistema: {contador_estudiantes}")


def mostrar_lista_completa():
    """
    Recorre e imprime todos los estudiantes registrados.
    Complejidad Temporal: O(n) - Depende linealmente del número de estudiantes.
    """
    print("\n--- LISTA COMPLETA DE ESTUDIANTES ---")
    if not lista_estudiantes:
        print(" No hay estudiantes registrados en el sistema actualmente.")
        return

    # Estructura de repetición iterativa (for) para procesar la lista
    for estudiante in lista_estudiantes:
        mostrar_estudiante(estudiante)


def buscar_estudiante_por_id():
    """
    Busca un estudiante específico mediante su ID numérico.
    Permite volver al menú principal si se ingresa 'X'.
    """
    print("\n--- BÚSQUEDA DE ESTUDIANTE POR ID ---")
    if not lista_estudiantes:
        print(" El sistema está vacío. No hay registros para buscar.")
        return

    print("(Escribe 'X' para cancelar y volver al menú)")
    entrada_buscar = input("Ingrese el ID del estudiante que desea buscar: ").strip()
    
    if entrada_buscar.lower() == 'x':
        print(" Operación cancelada. Volviendo al menú principal...")
        return

    try:
        id_buscar = int(entrada_buscar)
    except ValueError:
        print(" ID inválido. Debe ingresar un valor numérico.")
        return

    estudiante_encontrado = None

    # Algoritmo de búsqueda lineal utilizando un ciclo for
    for estudiante in lista_estudiantes:
        if estudiante['id'] == id_buscar:
            estudiante_encontrado = estudiante
            break  # Interrupción optimizada al encontrar el objetivo

    # Estructura de decisión simple (if-else) para procesar el resultado de la búsqueda
    if estudiante_encontrado is not None:
        print("\n  ¡Estudiante encontrado con éxito!")
        mostrar_estudiante(estudiante_encontrado)
    else:
        print(f" No se encontró ningún estudiante con el ID {id_buscar}.")


def ejecutar_sge_principal():
    """
    Controlador principal del flujo del programa.
    Maneja el menú interactivo por consola mediante ciclos y banderas.
    """
    # Variable especial: Bandera lógica para controlar el ciclo de vida del menú
    programa_activo = True  

    # Estructura de repetición condicionada al comienzo (while)
    while programa_activo:
        print("\n========================================")
        print("   SISTEMA DE GESTIÓN DE ESTUDIANTES    ")
        print("========================================")
        print("1. Agregar nuevo estudiante")
        print("2. Mostrar lista completa")
        print("3. Buscar estudiante por ID")
        print("4. Salir del sistema")
        print("----------------------------------------")
        
        opcion = input("Seleccione una opción (1-4): ").strip()

        # Estructura de decisión múltiple simulada mediante un bloque condicional anidado
        if opcion == "1":
            agregar_estudiante()
        elif opcion == "2":
            mostrar_lista_completa()
        elif opcion == "3":
            buscar_estudiante_por_id()
        elif opcion == "4":
            print("\n Saliendo del sistema... ¡Gracias por utilizar el SGE!")
            programa_activo = False  # Cambio de estado de la bandera para romper el ciclo
        else:
            print(" Opción no válida. Por favor, digite un número entre 1 y 4.")

# Punto de entrada de la aplicación por consola
if __name__ == "__main__":
    ejecutar_sge_principal()