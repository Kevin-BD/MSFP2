[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/)

# Práctica 2 — Modelado y control del sistema cardiovascular mediante el modelo Windkessel

Repositorio académico de la asignatura **Modelado de Sistemas Fisiológicos (IBF-1020)** de Ingeniería Biomédica, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana. La práctica se desarrolla a partir del apartado **5.4.2. Introducción** del syllabus, y comprende el modelado, la simulación y el control de un sistema cardiovascular simplificado mediante el **modelo Windkessel de cuatro elementos**.

**Docente:** Dr. Paul Antonio Valle Trujillo  
**Correo:** paul.valle@tectijuana.edu.mx  
**Departamento:** Ingeniería Eléctrica y Electrónica  
**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana  
**Asignatura:** Modelado de Sistemas Fisiológicos  
**Programa:** Ingeniería Biomédica  
**Estudiante:** [Nombre completo] · [Número de control] · [Correo institucional]

**Repositorio de la asignatura:** https://github.com/DrPaulValle/MSF-IBF1020

---

## Contenido

1. [Resumen](#resumen)
2. [Competencia y objetivos](#competencia-y-objetivos)
3. [Introducción al modelo Windkessel](#introducción-al-modelo-windkessel)
4. [Variables y analogía eléctrica](#variables-y-analogía-eléctrica)
5. [Parámetros cardiovasculares](#parámetros-cardiovasculares)
6. [Formulación matemática](#formulación-matemática)
7. [Función de transferencia](#función-de-transferencia)
8. [Análisis de estabilidad y error](#análisis-de-estabilidad-y-error)
9. [Señal de excitación y simulación](#señal-de-excitación-y-simulación)
10. [Sistemas en lazo abierto](#sistemas-en-lazo-abierto)
11. [Diseño de controladores PID](#diseño-de-controladores-pid)
12. [Sistemas en lazo cerrado](#sistemas-en-lazo-cerrado)
13. [MATLAB, Simulink y Simscape](#matlab-simulink-y-simscape)
14. [Implementación en Python](#implementación-en-python)
15. [Verificación cruzada y resultados](#verificación-cruzada-y-resultados)
16. [Diagrama fisiológico](#diagrama-fisiológico)
17. [Actividades y entregables](#actividades-y-entregables)
18. [Estructura del repositorio](#estructura-del-repositorio)
19. [Discusión y conclusiones](#discusión-y-conclusiones)
20. [Referencias](#referencias)

---

## Resumen

Esta práctica estudia el comportamiento dinámico del sistema cardiovascular por medio del **modelo Windkessel de cuatro elementos**, compuesto por impedancia característica **Z**, distensibilidad arterial **C**, resistencia periférica **R** e inertancia arterial **L**. El modelo permite analizar la relación entre la presión arterial de entrada $P_a(t)$ y la presión asociada con la distensibilidad arterial $P_p(t)$.

Se comparan tres condiciones simuladas: **hipotensa, normotensa e hipertensa**. Se toma el sistema normotenso como referencia y se diseñan dos controladores PID independientes para reducir la diferencia entre esa respuesta y las de los casos hipotenso e hipertenso. Se utilizan MATLAB, Simulink y Simscape Electrical; adicionalmente, se reproducen resultados mediante Python para verificar la consistencia de las implementaciones.

La práctica es **exclusivamente in silico**: los controladores representan estrategias académicas de simulación y **no** constituyen recomendaciones terapéuticas.

**Palabras clave:** Windkessel; sistema cardiovascular; ingeniería biomédica; presión arterial; distensibilidad; funciones de transferencia; PID; MATLAB; Simulink; Python.

## Competencia y objetivos

### Competencia a desarrollar

Modelizar sistemas fisiológicos mediante circuitos eléctricos equivalentes, ecuaciones diferenciales y funciones de transferencia, analizar su comportamiento dinámico y diseñar estrategias de control mediante técnicas de control clásico.

### Objetivo general

Modelar y simular el sistema cardiovascular mediante el modelo Windkessel de cuatro elementos y diseñar controladores PID que reduzcan las diferencias entre las respuestas de los modelos hipotenso e hipertenso y la referencia normotensa.

### Objetivos específicos

- Identificar los elementos fisiológicos y eléctricos del modelo Windkessel.
- Formular ecuaciones diferenciales y obtener la función de transferencia del sistema.
- Comparar los conjuntos de parámetros de las tres condiciones cardiovasculares.
- Analizar el error en estado estacionario, los polos y la estabilidad.
- Implementar representaciones equivalentes en MATLAB/Simulink y Simscape Electrical.
- Simular las respuestas en lazo abierto ante una misma señal de excitación.
- Diseñar y sintonizar dos controladores PID independientes.
- Comparar las respuestas controladas con la referencia normotensa.
- Verificar las simulaciones mediante Python y documentar los resultados.

## Introducción al modelo Windkessel

El modelo Windkessel es una representación simplificada del comportamiento hemodinámico del sistema arterial. Su formulación original se atribuye a **Otto Frank**, quien utilizó una resistencia periférica y una sección distensible para describir las propiedades del sistema vascular. Versiones posteriores incorporaron una impedancia característica y una inertancia, dando lugar al modelo de cuatro elementos.

Los cuatro elementos son **Z** (impedancia característica proximal), **C** (distensibilidad arterial total), **R** (resistencia periférica) y **L** (inertancia arterial). Aunque el circuito tiene cuatro elementos, solo **C** y **L** almacenan energía de manera independiente; por eso el modelo es un **sistema dinámico de segundo orden**.

El circuito de referencia se presenta en la **Figura 5.1 del syllabus (página 77)**. El esquema general para las comparaciones y los controladores independientes está en la **Figura 5.2 (página 78)**.

## Variables y analogía eléctrica

| Símbolo | Interpretación fisiológica | Analogía eléctrica |
|---|---|---|
| $P_a(t)$ | Presión arterial de entrada | Voltaje de entrada |
| $P_p(t)$ | Presión en la distensibilidad arterial; salida | Voltaje de salida |
| $F_a(t)$ | Flujo dirigido hacia la aorta o arteria pulmonar | Corriente total de entrada |
| $F_L(t)$ | Flujo a través de la inertancia | Corriente del inductor |
| $F_Z(t)$ | Flujo a través de la impedancia característica | Corriente por $Z$ |
| $F_C(t)$ | Flujo asociado con la distensión arterial | Corriente del capacitor |
| $F_R(t)$ | Flujo a través de la resistencia periférica | Corriente por $R$ |
| $Z$ | Impedancia característica arterial | Resistencia eléctrica |
| $R$ | Resistencia periférica | Resistencia eléctrica |
| $C$ | Distensibilidad arterial | Capacitancia |
| $L$ | Inertancia de la sangre en movimiento | Inductancia |

**Correspondencias fundamentales:** presión ↔ voltaje; flujo sanguíneo ↔ corriente.

## Parámetros cardiovasculares

Según la tabla del apartado 5.4.2 del syllabus (página 78), se utilizan los valores:

| Parámetro | Hipotenso | Normotenso | Hipertenso | Unidades |
|---|---:|---:|---:|---|
| $Z$ | 0.010 | 0.010 | 0.010 | mmHg·s/ml |
| $L$ | 0.001 | 0.001 | 0.001 | mmHg·s²/ml |
| $R$ | 1.000 | 1.000 | 1.000 | mmHg·s/ml |
| $C$ | 3.500 | 1.500 | 1.000 | ml/mmHg |

**La única variable paramétrica entre las tres condiciones es $C$.** El caso hipotenso representa una mayor distensibilidad arterial, mientras que el hipertenso representa una menor distensibilidad y mayor rigidez. Estas etiquetas corresponden a **escenarios simplificados del ejercicio**: el modelo no reproduce por sí solo todas las causas clínicas de hipotensión o hipertensión.

## Formulación matemática

Se elige el vector de estados

$$
\mathbf{x}(t)=\begin{bmatrix}F_L(t)\\P_p(t)\end{bmatrix},\qquad u(t)=P_a(t),\qquad y(t)=P_p(t).
$$

La diferencia de presión en el elemento inercial es

$$
P_a(t)-P_p(t)=L\frac{dF_L(t)}{dt}.
$$

El flujo por la impedancia proximal es

$$
F_Z(t)=\frac{P_a(t)-P_p(t)}{Z}.
$$

La conservación del flujo en el nodo arterial se expresa mediante

$$
F_Z(t)+F_L(t)=F_C(t)+F_R(t),
$$

con

$$
F_C(t)=C\frac{dP_p(t)}{dt},\qquad F_R(t)=\frac{P_p(t)}{R}.
$$

De estas relaciones se obtienen las ecuaciones de estado:

$$
\boxed{\frac{dF_L(t)}{dt}=\frac{P_a(t)-P_p(t)}{L}}
$$

$$
\boxed{\frac{dP_p(t)}{dt}=\frac{F_L(t)}{C}+\frac{P_a(t)}{ZC}-\left(\frac{1}{ZC}+\frac{1}{RC}\right)P_p(t)}
$$

Su representación matricial es

$$
\dot{\mathbf{x}}(t)=\underbrace{\begin{bmatrix}0&-1/L\\1/C&-(1/(ZC)+1/(RC))\end{bmatrix}}_{A}\mathbf{x}(t)
+\underbrace{\begin{bmatrix}1/L\\1/(ZC)\end{bmatrix}}_{B}P_a(t),
$$

$$
y(t)=\begin{bmatrix}0&1\end{bmatrix}\mathbf{x}(t).
$$

## Función de transferencia

A partir de las ecuaciones de estado y bajo **condiciones iniciales nulas**, se obtiene la relación entre la presión de entrada y la presión de salida:

$$
\boxed{G(s)=\frac{P_p(s)}{P_a(s)}=\frac{(L/Z)s+1}{LCs^2+L(1/Z+1/R)s+1}.}
$$

Para cada condición se sustituye el valor correspondiente de $C$:

| Modelo | $C$ (ml/mmHg) | Función de transferencia |
|---|---:|---|
| Hipotenso | 3.5 | $G_{hipo}(s)=\dfrac{0.1s+1}{0.0035s^2+0.101s+1}$ |
| Normotenso | 1.5 | $G_{normo}(s)=\dfrac{0.1s+1}{0.0015s^2+0.101s+1}$ |
| Hipertenso | 1.0 | $G_{hiper}(s)=\dfrac{0.1s+1}{0.001s^2+0.101s+1}$ |

Estas expresiones son resultados algebraicos obtenidos a partir de los parámetros del syllabus; **no son resultados experimentales**.

## Análisis de estabilidad y error

Para cada modelo, calcular las raíces del denominador

$$
LCs^2+L\left(\frac{1}{Z}+\frac{1}{R}\right)s+1=0.
$$

La estabilidad se determina examinando la ubicación de los polos en el plano complejo. Con los parámetros positivos del ejercicio, los coeficientes de estos denominadores son positivos, lo que permite verificar la estabilidad de los sistemas de segundo orden considerados.

Para una entrada escalón unitario, la ganancia estática de cada modelo es

$$
G(0)=1.
$$

Por lo tanto, suponiendo estabilidad, el valor final de la salida es $P_p(\infty)=1$ y el error de seguimiento **entrada–salida** en estado estacionario es $e_{ss}=0$ para ese escalón. Este resultado no implica que las tres condiciones tengan la misma respuesta transitoria ante una señal variable.

**Evidencia por agregar:** cálculos de polos, gráficas del plano complejo e interpretación de las respuestas.

## Señal de excitación y simulación

El syllabus establece que todos los sistemas deben recibir **la misma realización de una señal aleatoria**, usando el bloque `Uniform Random Number` de Simulink, configurado con los siguientes valores:

| Parámetro | Valor |
|---|---|
| Mínimo | −0.2 |
| Máximo | 1 |
| Semilla (`seed`) | 106 |
| Periodo de muestreo (`Ts`) | 0.1 s |
| Tiempo de simulación | 0–15 s |

La señal debe compartirse entre los modelos hipotenso, normotenso e hipertenso para que las diferencias observadas se deban a los parámetros de los modelos y no a entradas distintas. Al reproducirla mediante Python debe verificarse que los **valores de la secuencia coincidan efectivamente**, pues una semilla igual no garantiza la misma realización entre generadores diferentes.

## Sistemas en lazo abierto

Se aplica $P_a(t)$ simultáneamente a las tres plantas, sin controlador, para observar las presiones:

$$
P_{p,hipo}(t),\qquad P_{p,normo}(t),\qquad P_{p,hiper}(t).
$$

La comparación debe explicar cómo la distensibilidad arterial modifica la respuesta dinámica. Se toma **$P_{p,normo}(t)$ como referencia**.

**Resultados por incorporar:** gráfica comparativa de las tres señales, intervalos de respuesta y análisis fisiológico de las diferencias.

## Diseño de controladores PID

Se implementan **dos lazos independientes**, uno para el modelo hipotenso y otro para el hipertenso. En ambos casos, el seguimiento se evalúa frente a la referencia normotensa.

Errores de seguimiento:

$$
e_{hipo}(t)=P_{p,normo}(t)-P_{p,hipo}(t),
$$

$$
e_{hiper}(t)=P_{p,normo}(t)-P_{p,hiper}(t).
$$

La ley ideal de control PID es

$$
u(t)=k_Pe(t)+k_I\int_0^t e(\tau)\,d\tau+k_D\frac{de(t)}{dt}.
$$

Las ganancias se determinan **por separado** con `Tune` del bloque `PID Controller` en Simulink:

| Controlador | $k_P$ | $k_I$ | $k_D$ |
|---|---|---|---|
| PID hipotenso | Pendiente | Pendiente | Pendiente |
| PID hipertenso | Pendiente | Pendiente | Pendiente |

Los valores deberán ser los registrados durante la sintonización real. El propósito es reducir el error y obtener respuestas estables; el instructivo no establece una terna única de ganancias.

## Sistemas en lazo cerrado

Para la condición hipotensa se comparan

$$
P_{p,normo}(t),\quad P_{p,hipo}(t),\quad P_{p,hipo,PID}(t),
$$

y para la hipertensa

$$
P_{p,normo}(t),\quad P_{p,hiper}(t),\quad P_{p,hiper,PID}(t).
$$

El análisis debe evaluar seguimiento de la referencia, disminución del error y características de la respuesta transitoria. **Incorporar aquí las gráficas de ambos controladores y las capturas de la sintonización.**

## MATLAB, Simulink y Simscape

Implementar **para cada uno de los tres conjuntos de parámetros**:

1. El sistema de **ecuaciones de estado** mediante bloques de Simulink.
2. La **función de transferencia** mediante el bloque correspondiente de Simulink.
3. El **circuito equivalente** con componentes en Simscape Electrical.

Desarrollar además un cuaderno de MATLAB que ejecute los modelos, recupere las señales almacenadas en el Workspace y genere gráficas con nombres de las variables y unidades. Guardar los resultados en **PDF**. Verificar que las tres representaciones equivalentes respondan de manera consistente ante una misma entrada y condiciones iniciales.

## Implementación en Python

En Visual Studio Code, implementar las funciones de transferencia $G_{hipo}(s)$, $G_{normo}(s)$ y $G_{hiper}(s)$. Reproducir la **misma señal temporal** utilizada en MATLAB/Simulink y obtener las respuestas en lazo abierto. Posteriormente, incorporar las ganancias de los dos controladores sintonizados en Simulink y reproducir los sistemas en lazo cerrado.

Se sugiere organizar el programa en etapas de definición de parámetros, entrada, sistemas de transferencia, respuestas temporales, controladores y exportación de figuras. Las bibliotecas concretas deben corresponder al código implementado y conservarse en un archivo de dependencias si aplica.

## Verificación cruzada y resultados

Comparar los resultados de:

| Representación | Evidencia requerida | Estado |
|---|---|---|
| Ecuaciones de estado en Simulink | Señales de respuesta | Pendiente |
| Funciones de transferencia en Simulink | Señales de respuesta | Pendiente |
| Circuito equivalente en Simscape Electrical | Captura y señales | Pendiente |
| Cuaderno MATLAB | Gráficas y exportaciones PDF | Pendiente |
| Implementación Python | Código y gráficas | Pendiente |
| Controlador PID hipotenso | Ganancias, sintonización y comparación | Pendiente |
| Controlador PID hipertenso | Ganancias, sintonización y comparación | Pendiente |

Revisar consistencia de parámetros, condiciones iniciales, métodos numéricos y discretización de la señal. Si aparecen discrepancias, describirlas con base en las figuras obtenidas sin atribuirlas automáticamente a errores del modelo.

## Diagrama fisiológico

Elaborar mediante **BioRender** o una herramienta equivalente un diagrama del sistema cardiovascular que identifique, al menos, corazón, aorta, grandes arterias, vasos periféricos, flujo sanguíneo, presión arterial, distensibilidad arterial y resistencia periférica. Relacionar estas estructuras y propiedades con $Z$, $C$, $R$ y $L$.

**Evidencia por agregar:** archivo o figura del diagrama biológico final.

## Actividades y entregables

- [ ] Identificar las variables y los cuatro parámetros del circuito Windkessel.
- [ ] Derivar las ecuaciones de estado del sistema.
- [ ] Obtener las tres funciones de transferencia.
- [ ] Calcular el error en estado estacionario ante una entrada escalón.
- [ ] Calcular y representar los polos de los tres modelos.
- [ ] Implementar ecuaciones, transferencias y circuitos en Simulink/Simscape.
- [ ] Configurar la misma señal aleatoria y simular entre 0 y 15 segundos.
- [ ] Graficar las tres respuestas en lazo abierto.
- [ ] Construir los controles normotenso–hipotenso y normotenso–hipertenso.
- [ ] Sintonizar y registrar los dos controladores PID.
- [ ] Comparar respuestas de lazo abierto y cerrado.
- [ ] Ejecutar los modelos desde un cuaderno MATLAB y exportar figuras PDF.
- [ ] Reproducir los resultados en Python.
- [ ] Comparar los resultados entre herramientas.
- [ ] Elaborar el diagrama fisiológico.
- [ ] Incorporar capturas, archivos y comentarios interpretativos propios.
- [ ] Publicar el `README.md` y el archivo `CITATION.cff`.

## Estructura del repositorio

Ejemplo de organización **sugerida** (no se presupone que los archivos existan aún):

```text
Practica_2_Windkessel/
├── README.md
├── CITATION.cff
├── matlab/
│   ├── analisis_windkessel.mlx
│   └── analisis_windkessel.pdf
├── simulink/
│   └── modelo_windkessel.slx
├── python/
│   ├── windkessel.py
│   └── requirements.txt
├── datos/
│   └── excitacion.csv
├── figuras/
│   ├── lazo_abierto.pdf
│   ├── control_hipotenso.pdf
│   ├── control_hipertenso.pdf
│   ├── comparacion_matlab_python.pdf
│   └── diagrama_fisiologico.png
└── evidencias/
    ├── simulink/
    └── pid/
```

Los nombres son orientativos. Actualizar los enlaces y rutas cuando se incorporen los archivos reales.

## Discusión y conclusiones

**Apartado pendiente de completar con los resultados de la práctica.** Discutir específicamente:

- Cómo influye la diferencia de distensibilidad $C$ sobre la respuesta de presión.
- Si la representación matemática, la función de transferencia y el circuito equivalente coinciden.
- Qué diferencias existen entre los modelos hipotenso, normotenso e hipertenso en lazo abierto.
- Si los PID reducen los errores de seguimiento y cuáles fueron sus ganancias.
- Qué tan semejantes fueron los resultados de MATLAB y Python.
- Cuáles son las limitaciones fisiológicas de este modelo cardiovascular simplificado.

La conclusión final debe redactarse **después de completar las simulaciones** y basarse en la evidencia obtenida.

## Referencias

1. M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2.ª ed., IEEE Press, 2018.
2. F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.
3. J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14.ª ed., Elsevier, 2021.
4. K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.
5. N. S. Nise, *Control Systems Engineering*, 8.ª ed., Wiley, 2020.
6. P. A. Valle Trujillo, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, 2026. Apartados 5.4.2–5.4.5.
7. Tecnológico Nacional de México, *Programa de estudio de la asignatura Modelado de Sistemas Fisiológicos*, IBF-1020.

---

**Nota de alcance:** este README documenta una práctica académica de simulación cardiovascular. Las diferencias entre condiciones simuladas no deben interpretarse como diagnósticos clínicos ni los controladores como intervenciones médicas.
