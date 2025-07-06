# Calculadora de Promedio Distribuido con MPI
Este repositorio contiene un programa en C que calcula el promedio de valores generados aleatoriamente de forma distribuida entre múltiples procesos, utilizando la interfaz de paso de mensajes (MPI) para la comunicación colectiva. Sirve como una demostración práctica del uso de MPI_Bcast y MPI_Reduce.

## 🚀 Características
- Generación de Datos Distribuida: Cada proceso genera su propio conjunto de números aleatorios.
- Distribución de Parámetros (MPI_Bcast): El proceso raíz distribuye el número N de valores a generar a todos los demás procesos.
- Agregación de Sumas (MPI_Reduce): Las sumas parciales de cada proceso se agregan en el proceso raíz para obtener una suma total.
- Distribución del Promedio Final (MPI_Bcast): El promedio calculado por el proceso raíz se distribuye a todos los procesos para su visualización.
- Sincronización Implícita: Demuestra cómo las operaciones colectivas de MPI actúan como puntos de sincronización naturales.

## 📋 Requisitos

Para compilar y ejecutar este programa, necesitas tener una implementación de MPI instalada en tu sistema. 
Las opciones populares incluyen:
- Open MPI (recomendado y ampliamente utilizado)
- MPICH

### Instalación de MPI (Ejemplo para macOS con Homebrew)

Si usas macOS, puedes instalar Open MPI fácilmente a través de Homebrew:

1. Instala las Herramientas de Línea de Comandos de Xcode:
```bash  
xcode-select --install
```
2. Instala Homebrew (si no lo tienes):
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

3. Instala Open MPI:
```bash
brew install open-mpi
```
Para otros sistemas operativos (Linux, Windows con WSL), consulta la documentación oficial de Open MPI o MPICH para instrucciones de instalación.

## 🛠️ Compilación

Navega hasta el directorio donde has guardado average_calculator.c y compila el programa utilizando el compilador wrapper de MPI (mpicc):
```bash
mpicc average_calculator.c -o average_calculator
```
Esto generará un archivo ejecutable llamado average_calculator.

## 🚀 Ejecución
Para ejecutar el programa y aprovechar el paralelismo, utiliza el comando mpirun (o mpiexec), especificando el número de procesos (-np) que deseas utilizar.Cuando inicies el programa, el proceso raíz (Rank 0) te pedirá que ingreses un valor entero positivo para N (la cantidad de valores aleatorios que cada proceso generará).

- Ejecutar con 2 procesos:
```bash
mpirun -np 2 ./average_calculator
```

- Ejecutar con 4 procesos:
```bash
mpirun -np 4 ./average_calculator
```

- Ejecutar con X procesos (reemplaza X por el número deseado):
```bash
mpirun -np X ./average_calculator
```
- Ejemplo de Salida (con N=3 y 2 procesos):

```bash
Proceso Raiz (Rank 0): Ingrese el tamaño N (cantidad de valores por proceso): 3
Proceso Raiz (Rank 0): N = 3. Distribuyendo N a todos los procesos.
Proceso 0: N recibido = 3.
Proceso 1: N recibido = 3.
Proceso 0: Generando 3 valores aleatorios...
Proceso 1: Generando 3 valores aleatorios...
Proceso 1: Suma parcial calculada = 143.512345
Proceso 0: Suma parcial calculada = 105.234567
Proceso Raiz (Rank 0): Suma total de todos los valores = 248.746912
Proceso Raiz (Rank 0): Promedio total calculado = 41.457819
Proceso 1: Promedio total recibido = 41.457819
Proceso 0: Promedio total recibido = 41.457819
```
🧠 ¿Cómo funciona?

1. Inicialización: Todos los procesos se inician y obtienen su rank (ID único) y num_procs (total de procesos).
2. Input N & Broadcast: El proceso con rank 0 (raíz) solicita el valor N al usuario y lo envía a todos los demás procesos utilizando MPI_Bcast.
3. Trabajo Local: Cada proceso, ahora conociendo N, genera N números aleatorios localmente y calcula su partial_sum.
4. Reducción: Todos los partial_sum se envían al proceso raíz y se suman colectivamente usando MPI_Reduce (operación MPI_SUM). El resultado (total_sum) reside en el proceso raíz.
5. Cálculo y Broadcast del Promedio Final: El proceso raíz calcula el promedio global (total_sum / (N * num_procs)) y lo distribuye a todos los procesos usando otro MPI_Bcast.
6. Visualización: Cada proceso imprime el promedio final.
7. Finalización: Se cierran los recursos de MPI.

## 📚 Documentación Adicional

Para una explicación más profunda del diseño del programa, la justificación del uso de cada operación colectiva, y un análisis de la sincronización y posibles puntos críticos en aplicaciones MPI, por favor, consulta el informe completo del proyecto.

## 🎥 Video Explicativo

Un video demostrativo que explica el código, su funcionamiento y los conceptos de MPI aplicados.
