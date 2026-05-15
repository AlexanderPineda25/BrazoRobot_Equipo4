# 🦾 BrazoRobot_Equipo4

**Diseño Colaborativo, Manufactura Remota y Construcción de un Pequeño Brazo Robot de 4 Grados de Libertad**

> Proyecto integrador — Manufactura Aditiva e Industria 4.0 (Electiva Profesional 4)  
> Facultad de Ciencias Básicas e Ingeniería — Universidad de los Llanos, Villavicencio, Meta  
> Mayo 2026 · Docente: Julio Hernando Vargas Riaño

---

## 👥 Equipo

| Código | Integrante | Rol en el proyecto |
|--------|-----------|-------------------|
| 160004716 | J. Martínez Hernández | Diseño base y hombro |
| 160004899 | Nicolás O. Hernández | Diseño antebrazo y muñeca |
| 160004505 | Jhorman C. Pérez | Diseño garra y ensamblaje |
| 160004726 | Steve A. Pineda Rincón | Programación Arduino y pruebas |
| 160004510 | Brian R. Duran | Documentación técnica e integración STL |

---

## 📋 Descripción del proyecto

Este repositorio contiene todos los archivos de diseño, fabricación y documentación del brazo robot de 4 grados de libertad desarrollado como proyecto integrador del módulo de Industria 4.0. El proyecto articula tres dimensiones fundamentales:

- **Diseño paramétrico colaborativo** en FreeCAD con control de versiones en GitHub
- **Fabricación aditiva** en filamento PLA mediante impresión FDM
- **Ensamblaje y control electromecánico** con servomotores y Arduino Uno
- **Montaje final** integrado en el archivo `montaje.FCStd`

El brazo completó exitosamente la tarea de levantar un lápiz con una tasa de éxito del 90 % en 10 ciclos de prueba, alcance máximo de 235 mm y carga útil validada de 45 g.

---

## 📁 Estructura del repositorio

```
BrazoRobot_Equipo4/
├── CAD/
│   ├── base.FCStd          # Base giratoria (servo MG995)
│   ├── hombro.FCStd        # Articulación hombro (servo MG995, eslabón 80 mm)
│   ├── antebrazo.FCStd     # Antebrazo (servo SG90, eslabón 75 mm)
│   ├── muneca.FCStd        # Muñeca (servo SG90, eslabón 50 mm)
│   ├── garra.FCStd         # Garra (servo SG90, apertura 30 mm)
│   └── montaje.FCStd       # Ensamblaje completo del brazo robot ⬅ archivo final
├── STL/
│   ├── base.stl
│   ├── hombro.stl
│   ├── antebrazo.stl
│   ├── muneca.stl
│   └── garra.stl
├── Docs/
│   ├── Informe_Tecnico_Brazo_Robot.pdf   # Informe técnico completo
└── README.md
```

---

## 🛠️ Materiales y componentes

| Componente | Especificación | Cantidad |
|-----------|---------------|----------|
| Filamento PLA | 1.75 mm, blanco | ~480 g |
| Servo motor | MG995 (base y hombro) | 2 |
| Servo motor | SG90 (codo, muñeca y garra) | 2 |
| Microcontrolador | Arduino Uno | 1 |
| Fuente de alimentación | 5 V / 2 A (servos) | 1 |
| Tornillería | M3 × 8 mm | 10 |
| Tornillería | M3 × 12 mm | 8 |
| Tornillería | M3 × 16 mm | 6 |
| Tuercas | M3 | 24 |

---

## ⚙️ Parámetros de impresión

| Parámetro | Valor |
|-----------|-------|
| Altura de capa | 0.20 mm |
| Temperatura de extrusión | 210 °C |
| Temperatura de cama | 60 °C |
| Velocidad de impresión | 45 mm/s |
| Relleno | 25 % (rejilla) — base al 40 % |
| Perímetros | 2 |
| Soporte | Solo donde necesario |
| Software de laminado | Ultimaker Cura |

> **Nota:** La base giratoria requiere 40 % de relleno por las cargas estáticas de los servomotores MG995. Las demás piezas funcionan correctamente al 25 %.

---

## 🔧 Instrucciones de ensamblaje

### 1. Preparación de piezas
1. Imprimir las 5 piezas en el orden: `base → hombro → antebrazo → muneca → garra`
2. Inspeccionar dimensionalmente con calibrador (tolerancia ±0.5 mm)
3. Lijar suavemente las zonas de acoplamiento si hay interferencia

### 2. Instalación de servomotores
1. Instalar los servos MG995 en `base` y `hombro` con tornillos autorroscantes M2
2. Instalar los servos SG90 en `antebrazo`, `muneca` y `garra` con tornillos autorroscantes M2
3. **No conectar al Arduino todavía**

### 3. Ensamblaje mecánico
1. Unir `base` + `hombro` con tornillería M3 × 16 mm
2. Unir `hombro` + `antebrazo` con tornillería M3 × 12 mm
3. Unir `antebrazo` + `muneca` con tornillería M3 × 8 mm
4. Unir `muneca` + `garra` con tornillería M3 × 8 mm
5. Verificar movilidad libre en todas las articulaciones sin holgura excesiva

### 4. Conexión eléctrica

```
Arduino Uno
├── Pin 3  → Señal servo BASE    (MG995)
├── Pin 5  → Señal servo HOMBRO  (MG995)
├── Pin 6  → Señal servo CODO    (SG90)
├── Pin 9  → Señal servo MUÑECA  (SG90)
└── GND    → GND compartido con fuente externa

Fuente externa 5V/2A
├── VCC → VCC servos MG995 y SG90
└── GND → GND Arduino + GND servos
```

> ⚠️ **Importante:** No alimentar los servos MG995 desde el puerto USB del Arduino. Los picos de corriente en el arranque (hasta 1.8 A) causan reinicios y comportamiento errático. Usar siempre fuente externa de 5 V / 2 A.

### 5. Programación Arduino

Cargar el sketch de control desde `Docs/` usando Arduino IDE con la biblioteca `Servo.h`. La secuencia de demostración incluye:

1. Posicionamiento en ángulo cero (reposo)
2. Movimiento secuencial de cada articulación a 90°
3. Cierre de garra (agarre)
4. Transporte y depósito del objeto
5. Retorno a posición de reposo

Usar incrementos de 1° con retardos de 15 ms entre pasos para movimientos suaves.

---

## 📊 Resultados obtenidos

| Pieza | Dim. nominal (mm) | Dim. medida (mm) | Desviación | Estado |
|-------|-------------------|------------------|-----------|--------|
| Base | Ø servo: 40.0 | 39.8 | −0.2 mm | ✅ OK |
| Hombro | L: 80.0 | 80.3 | +0.3 mm | ✅ OK |
| Antebrazo | L: 75.0 | 74.7 | −0.3 mm | ✅ OK |
| Muñeca | L: 50.0 | 50.4 | +0.4 mm | ✅ OK |
| Garra | Apertura: 30.0 | 30.2 | +0.2 mm | ✅ OK |

- **Tasa de éxito en tarea de manipulación:** 90 % (9/10 ciclos)
- **Alcance máximo:** 235 mm desde el eje de la base
- **Carga máxima validada:** 45 g
- **Tiempo total de fabricación:** ~18 horas distribuidas en varias sesiones
- **Consumo de filamento:** ~480 g de PLA

---

## 🔄 Flujo de trabajo colaborativo (Git)

Este proyecto siguió la metodología de Industria 4.0 para colaboración en diseño CAD:

```
git clone https://github.com/AlexanderPineda25/BrazoRobot_Equipo4.git
cd BrazoRobot_Equipo4
```

**Regla de oro:** un archivo `.FCStd` por persona a la vez. Antes de editar, verificar que el archivo no esté bloqueado (Anchorpoint) y anunciarlo en el chat del equipo.

```bash
# Flujo por sesión de diseño
git pull origin main                          # Actualizar antes de editar
# ... editar el archivo .FCStd en FreeCAD ...
git add CAD/nombre_pieza.FCStd
git commit -m "Verbo: descripción específica del cambio"
git push origin main
```

**Convención de mensajes de commit:**
- ✅ `Añadida cavidad para servo MG995 en articulación hombro`
- ✅ `Corregido diámetro de pasador en unión antebrazo-muñeca`
- ❌ `actualización` / `cambios` / `fix`

---

## 📚 Bibliografía

- Blum, J. (2013). *Exploring Arduino: Tools and Techniques for Engineering Wizardry*. Wiley.
- Chacon, S. & Straub, B. (2014). *Pro Git* (2.ª ed.). Apress. https://git-scm.com/book/en/v2
- Gibson, I., Rosen, D. & Stucker, B. (2021). *Additive Manufacturing Technologies* (3.ª ed.). Springer.
- Schwab, K. (2016). *The Fourth Industrial Revolution*. World Economic Forum.
- Tofail, S. A. M. et al. (2018). Additive manufacturing: Scientific and technological challenges. *Materials Today*, 21(1), 22–37. https://doi.org/10.1016/j.mattod.2017.07.001
- FreeCAD Documentation (2024). https://wiki.freecad.org
- GitHub Docs (2024). https://docs.github.com
- Anchorpoint (2024). https://anchorpoint.app

---

## 📄 Licencia

Proyecto académico — Universidad de los Llanos, 2026.  
Los archivos de diseño CAD se comparten con fines educativos.
