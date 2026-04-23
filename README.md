<img width="1024" height="572" alt="image" src="https://github.com/user-attachments/assets/3c2212af-d04b-439a-b3fc-d15331180b0f" />


# <img width="1601" height="70" alt="cooltext506609702966455" src="https://github.com/user-attachments/assets/7c1380e5-f0a2-4ee6-94a6-1983c645242e" />

---

- Nombre: Sergio Diaz Enciso
- No.Control: 23211949
- Horario: 4-5

---

## Evidecia de Implementacion del programa 
- Edicion del codigo: https://asciinema.org/a/LXMeH9Ny9xFqaX6g
- Pruebas: https://asciinema.org/a/66vzxiij3DDONhGy

---

## Implementación de un Mini Cloud Log Analyzer en ARM64

**Modalidad:** Individual
**Entorno de trabajo:** AWS Ubuntu ARM64 + GitHub Classroom
**Lenguaje:** ARM64 Assembly (GNU Assembler) + Bash + GNU Make

---
# ☁️ Mini Cloud Log Analyzer en ARM64

## 📌 Introducción

En los sistemas en la nube, el análisis de logs es esencial para detectar errores y monitorear el funcionamiento de los servicios.  
En este proyecto se desarrolla un analizador de códigos HTTP utilizando ensamblador ARM64, lo que permite comprender cómo se procesan los datos a bajo nivel mediante el uso directo de memoria, registros y llamadas al sistema.

## 🧾 Descripción del programa

El programa procesa códigos HTTP leídos desde la entrada estándar (stdin) y realiza las siguientes tareas:

- Clasifica los códigos en:
  - 2xx → Éxitos  
  - 4xx → Errores de cliente  
  - 5xx → Errores de servidor  
- Lleva un conteo de cada tipo
- Implementa la Variante D:
  - Detecta tres errores consecutivos
  - Muestra una alerta cuando esta condición se cumple
- Genera un reporte final en consola

El análisis se realiza mediante lectura de bloques de memoria y procesamiento carácter por carácter.

## 📂 Estructura del proyecto

```
cloud-log-analyzer/
├── README.md
├── Makefile
├── run.sh
├── src/
│   └── analyzer.s
├── data/
│   ├── logs_A.txt
│   ├── logs_B.txt
│   ├── logs_C.txt
│   ├── logs_D.txt
│   └── logs_E.txt
├── tests/
│   ├── test.sh
│   └── expected_outputs.txt
└── instructor/
    └── VARIANTES.md
```

- Makefile: Automatiza la compilación  
- logs.txt: Archivo de prueba con códigos HTTP  
- analyzer.s: Código en ensamblador ARM64  

## ⚙️ Código modificado (Variante D)

Para implementar la detección de tres errores consecutivos, se realizaron los siguientes cambios:

Se agregó un contador de errores consecutivos:

```asm
mov x28, #0   // contador de errores consecutivos
```

Se modificó la función clasificar_codigo para:

- Incrementar el contador si el código es mayor o igual a 400  
- Reiniciar el contador si no es error  
- Mostrar una alerta al detectar 3 errores consecutivos  

```asm
es_error:
    add x28, x28, #1
    cmp x28, #3
    b.ne clasificar_fin

    adrp x0, msg_alerta
    add x0, x0, :lo12:msg_alerta
    bl write_cstr

    mov x28, #0
    b clasificar_fin

no_es_error:
    mov x28, #0
```

También se agregó el mensaje en la sección de datos:

```asm
msg_alerta: .asciz "ALERTA: 3 errores consecutivos\n"
```

## ▶️ Ejecución

```
make
cat logs.txt | ./analyzer
```

## 🧠 Conclusión

El desarrollo de este programa permitió comprender cómo se pueden implementar procesos de análisis de datos directamente en lenguaje ensamblador, utilizando únicamente llamadas al sistema operativo.

Además, se aplicaron conceptos clave como el manejo de memoria y registros, la conversión de datos de ASCII a enteros, el control de flujo mediante saltos condicionales y el procesamiento secuencial de información.

La implementación de la detección de errores consecutivos demuestra cómo es posible construir lógica de monitoreo similar a la utilizada en sistemas reales, pero desde un nivel mucho más bajo, fortaleciendo la comprensión del funcionamiento interno del hardware y del sistema operativo.
