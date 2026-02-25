# Unidad I. Introducción a la graficación por computadora.  
## 1.1 Historia y evolución de la graficación por computadora
La graficación ha pasado de ser un proceso puramente matemático a una simulación física de la realidad.

* Década de 1950 - 1960: El surgimiento. El sistema SAGE del MIT utilizó por primera vez un lápiz óptico. Ivan Sutherland creó Sketchpad, introduciendo el concepto de "objetos" y "restricciones" en el dibujo técnico.

* Década de 1970 - 1980: La era de los algoritmos. Se desarrollaron técnicas fundamentales como el sombreado de Gouraud y Phong, así como el Z-buffering para la gestión de profundidad.

* Década de 1990 - 2010: La revolución del 3D y las GPUs. Aparecen estándares como OpenGL y DirectX, y el hardware especializado permite la renderización masiva de polígonos.

* Era Moderna (2010-Presente): El dominio del Ray Tracing (trazado de rayos) y la computación en la nube. Hoy, la IA (Deep Learning) se utiliza para reconstruir imágenes y mejorar la resolución en tiempo real (Upscaling).
## 1.2 Áreas de aplicación
1) CAD/CAM (Computer-Aided Design): Esencial en arquitectura e ingeniería para crear planos y prototipos virtuales precisos.

2) Visualización Científica: Representación de fenómenos complejos como el clima, flujos de fluidos o estructuras moleculares que el ojo humano no puede ver.

3) Realidad Virtual y Aumentada (VR/AR): Interfaces inmersivas que superponen gráficos sobre el mundo real o crean mundos completamente digitales.

4) Interfaces Gráficas de Usuario (GUI): El diseño de cómo interactuamos con el software mediante iconos, ventanas y animaciones (como en el framework Flet).
## 1.3 Aspectos matemáticos de la graficación
La computación gráfica es, esencialmente, geometría aplicada.
* Coordenadas Homogéneas: Se añade una cuarta componente ($w$) a los vectores 3D para permitir que las traslaciones se realicen mediante multiplicaciones de matrices de $4x4$.
* Transformaciones Afines: Operaciones que conservan los puntos, rectas y planos (escalado, rotación, traslación).
* Trigonometría Esencial: El uso de funciones seno y coseno es vital para generar curvas, círculos y para calcular la orientación de la cámara respecto a un objeto.
* Álgebra de Vectores: El producto punto (dot product) se usa para calcular la iluminación, y el producto cruz (cross product) para
encontrar la normal de una cara.
## 1.4 Modelos del color: RGB, CMY, HSV y HSL
* RGB (Red, Green, Blue): Modelo aditivo. Se basa en la suma de luces. Es el estándar para cualquier pantalla digital.

* CMY/CMYK (Cyan, Magenta, Yellow, Black): Modelo sustractivo. Funciona mediante la absorción de luz, siendo el estándar para la industria de la impresión.

* HSV (Hue, Saturation, Value): Más intuitivo para humanos. Define el matiz (color puro), la saturación (pureza) y el valor (brillo).

* HSL (Hue, Saturation, Lightness): Similar al HSV, pero define la luminosidad, facilitando el ajuste de sombras y luces de manera uniforme.
### Tutorial: Iluminación de un Cubo en Blender
Para iluminar un objeto en un entorno 3D, no basta con añadir una luz; hay que entender la interacción:
1) Luz de Punto (Point Light): Emite fotones en todas direcciones desde un solo punto. Ideal para simular focos.

2) Sombreado Directo: Ocurre cuando la cara de un cubo tiene una Normal apuntando directamente hacia la fuente de luz.

3) Normales: Si las normales de las caras de tu cubo están invertidas (apuntando hacia adentro), la iluminación fallará y el cubo se verá negro o transparente.

4) Configuración en Python: Se debe definir la intensidad (energy) en Watts y el color de la luz para afectar el material del cubo.
## 1.5 Representación y trazo de líneas y polígonos
Un polígono se define como una secuencia cerrada de vértices.
* Rasterización: Es el proceso de convertir un vector (matemático) en un conjunto de píxeles. El algoritmo de Bresenham es el más eficiente para dibujar líneas en pantallas de píxeles sin usar números decimales.

* Topología: Define cómo se conectan los vértices. En 3D, se prefieren los "Quads" (polígonos de 4 lados) sobre los "Tris" (triángulos) para facilitar la animación y el deformado.
  ### 1.5.1 Formatos de imagen
* Raster: Archivos compuestos por una rejilla de píxeles (Bitmaps).

    JPG: Compresión con pérdida, ideal para fotografías.

    PNG: Sin pérdida, soporta canal Alpha (transparencia).

* Vectores: Archivos basados en rutas matemáticas.

    SVG: Estándar web, escalable infinitamente sin pixelarse.

  # Practica_ Flor(pasos)
## Paso 1: Importación y Limpieza
Primero, importamos las librerías necesarias y eliminamos los objetos existentes para tener un lienzo limpio.
```python
import bpy
import math

# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```
## Paso 2: Creación de un polígono regular
Para dibujar un polígono, calculamos los puntos en una circunferencia usando seno y coseno.
```python
def crear_poligono(lados, radio):
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        # Aquí se colocaría el vértice o un objeto en esa posición
```
## Paso 3: Lógica de la Flor de la Vida
La Flor de la Vida se basa en círculos que se intersectan. El secreto es colocar 6 círculos cuyos centros estén en el perímetro de un círculo central.
```python
def generar_flor_vida(radio):
    # Círculo central
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0,0,0))
    # 6 Círculos periféricos
    for i in range(6):
        angulo = math.radians(i * 60)
        x = math.cos(angulo) * radio
        y = math.sin(angulo) * radio
        bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0))
```
# Codigo completo
```python
import bpy
import math

def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat

def generar_geometria_practica():
    # 1. Limpiar escena
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    radio = 2
    mat_azul = crear_material("Azul", (0, 0.2, 0.8))

    # 2. Dibujar Flor de la Vida (Círculos)
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0))
    for i in range(6):
        angulo = math.radians(i * 60)
        x = math.cos(angulo) * radio
        y = math.sin(angulo) * radio
        bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0))
    
    # 3. Dibujar Polígono (Pentágono de referencia)
    puntos_pentagono = 5
    for i in range(puntos_pentagono):
        angulo = (2 * math.pi * i) / puntos_pentagono
        px = math.cos(angulo) * (radio * 2)
        py = math.sin(angulo) * (radio * 2)
        bpy.ops.mesh.primitive_uv_sphere_add(radius=0.2, location=(px, py, 0))
        bpy.context.active_object.data.materials.append(mat_azul)

    print("Geometría generada exitosamente.")

generar_geometria_practica()
```
## 1.6 Procesamiento de mapas de bits
El procesamiento de mapas de bits implica la alteración de los valores numéricos de los píxeles (brillo, contraste, filtros de detección de bordes). En el desarrollo de software, esto se realiza mediante matrices de convolución que recorren la imagen original para generar una nueva versión procesada.
# Conclusion
La culminación de esta Unidad I: Introducción a la Graficación por Computadora permite establecer una conexión crítica entre la teoría histórica y la implementación técnica moderna. A través del desarrollo de este portafolio, se llega a las siguientes determinaciones:

Fundamentación Matemática: Se ha comprobado que la graficación no es un proceso estético aislado, sino una extensión del álgebra lineal y la trigonometría. La capacidad de renderizar un polígono o animar una cámara depende directamente de la manipulación de matrices de transformación y el cálculo de vectores normales.
Versatilidad de Python: El uso de Python como lenguaje conductor demuestra que la programación de alto nivel es capaz de gestionar tanto interfaces de usuario dinámicas (mediante Flet) como la generación de geometría compleja y procesos de iluminación en entornos 3D (mediante Blender).
Evolución y Estándares: El estudio de los modelos de color (RGB, HSV, CMYK) y los formatos de imagen (Raster vs. Vectorial) subraya la importancia de elegir las herramientas adecuadas según el área de aplicación, ya sea para visualización web, impresión o renderizado fotorrealista.
En conclusión, los conceptos abordados sientan las bases para el desarrollo de software con interfaces más humanas, eficientes y visualmente precisas, alineando el rigor matemático con la creatividad tecnológica.

# Bibliografias

* Blender Foundation. (2026). Blender 4.0 Python API Documentation. Recuperado de https://docs.blender.org/api/current/
* Flet Dev. (2026). Flet: Real-time apps in Python. Recuperado de https://flet.dev/docs/
* Hearn, D., & Baker, M. P. (2006). Graficación por Computadora con OpenGL (3.ª ed.). Pearson Educación.
* Hughes, J. F., van Dam, A., & Foley, J. D. (2013). Computer Graphics: Principles and Practice (3.ª ed.). Addison-Wesley Professional.
* Scribbr. (2026). Generador de citas APA. Recuperado de https://www.scribbr.es/citar/generador/
* Shirley, P., & Marschner, S. (2021). Fundamentals of Computer Graphics (5.ª ed.). A K Peters/CRC Press.
* Vince, J. (2020). Mathematics for Computer Graphics. Springer Nature.
