# Laboratorio 3D de Leyes de Kirchhoff en Android Studio

## Archivo principal

Copia [kirchhoff3d-android.html](/home/ubuntu/kirchhoff3d-android.html) en:

```text
app/src/main/assets/kirchhoff3d-android.html
```

Si la carpeta `assets` no existe, créala dentro de `app/src/main/`.

## Integración rápida con Kotlin

En `MainActivity.kt` puedes cargar el laboratorio sin servidor y sin conexión a internet:

```kotlin
package com.example.kirchhoff3d

import android.os.Bundle
import android.webkit.WebView
import android.webkit.WebViewClient
import androidx.activity.OnBackPressedCallback
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        webView = WebView(this)
        setContentView(webView)

        webView.settings.javaScriptEnabled = true
        webView.settings.domStorageEnabled = true
        webView.settings.allowFileAccess = true
        webView.settings.allowContentAccess = true
        webView.webViewClient = WebViewClient()
        webView.loadUrl("file:///android_asset/kirchhoff3d-android.html")

        onBackPressedDispatcher.addCallback(this, object : OnBackPressedCallback(true) {
            override fun handleOnBackPressed() {
                if (webView.canGoBack()) webView.goBack() else finish()
            }
        })
    }
}
```

No se necesita permiso de internet para la visualización local. Si después agregas enlaces externos o contenido remoto, añade el permiso correspondiente en `AndroidManifest.xml`.

## Qué incluye esta versión

- **Barreras laterales visibles** en las áreas de visualización LVK y LCK.
- **Flujo de corriente animado** con puntos, flechas y líneas móviles en las ramas.
- **Resistencias R1–R5 dibujadas dentro del diagrama** de tres mallas.
- **Ejemplo de red triangular Δ** dentro de la misma ilustración, con indicación de conversión Δ↔Y.
- **Cubo LCK tipo dibujo técnico** con ocho nodos N1–N8 y doce conexiones dentro de la propia maqueta.
- Selección de **1 a 10 mallas**, edición de voltaje y resistencias, cambio de sentidos, selección de nodo y GND.
- Comprobación analítica de LVK/LCK y tabla de corriente, voltaje y potencia.
- Comportamiento visual uniforme para todas las mallas: resistencia, corriente, potencia, sentido y flujo.
- Diseño adaptable para teléfono; se puede usar en orientación vertical y horizontal.
- El teclado Android permanece abierto mientras se escriben valores.

## Recomendaciones de Android Studio

1. Usa un proyecto **Empty Views Activity** con Kotlin.
2. Mantén el nombre exacto `kirchhoff3d-android.html` en `assets`.
3. Ejecuta primero en un teléfono o emulador en orientación vertical y después gira a horizontal.
4. La vista LVK de tres mallas es la demostración principal; el selector permite probar otros tamaños con la misma lectura visual.
5. Para que la animación sea fluida, conserva la aceleración por hardware predeterminada de Android.

## Orientación del teléfono

La interfaz incluye reglas responsive para:

- **Vertical:** adapta tarjetas, botones, campos y diagramas al ancho del teléfono.
- **Horizontal:** reduce la altura del escenario y escala el diagrama para aprovechar el ancho disponible.

No bloquees la orientación en el `AndroidManifest.xml`. Si tu proyecto tiene esta propiedad, elimínala:

```xml
android:screenOrientation="portrait"
```

Así Android podrá girar entre vertical y horizontal conservando la visualización completa. El paquete usa `100dvh`, `orientation: landscape` y escalado proporcional de los diagramas para WebView moderno.


## Selección de nodos en el cubo

En la vista **LCK** puedes seleccionar un nodo de dos formas:

1. Elige `N1`–`N8` en el selector **Nodo analizado**.
2. Toca directamente cualquier punto del cubo.

El nodo seleccionado se marca en naranja, sus conexiones directas se iluminan en verde y el recuadro inferior muestra la lista `Conectados`. El selector y el cubo permanecen sincronizados.


## Créditos institucionales incluidos

La sección **Créditos** ya contiene:

- **Autor:** Erick Vega Carrasco
- **Escuela:** UPIBI
- **Institución:** IPN · Instituto Politécnico Nacional
- Escudo local del **IPN**: `app/src/main/assets/ipn-escudo.png`
- Logotipo local de **UPIBI**: `app/src/main/assets/upibi-escudo.png`

Los escudos se cargan desde `assets`, por lo que también se muestran sin conexión a internet.


## Escudos en el inicio

El encabezado superior del inicio muestra los dos recursos PNG institucionales: el escudo del IPN a la izquierda y el logotipo de UPIBI a la derecha. El tamaño se adapta automáticamente a la orientación vertical u horizontal del celular.


## Solucionador de problemas reales

La pantalla principal incluye tres casos precargados: limitador de corriente para LED de 12 V, red de dos mallas para diagnóstico automotriz y tarjeta de control de tres mallas. El botón **Cargar caso** coloca automáticamente la fuente y las resistencias en el modelo, calcula corrientes, voltajes y potencia por rama, y muestra una recomendación de potencia nominal para los resistores. También puedes cambiar cualquier valor y utilizar el modelo como solucionador de un circuito propio.


## Separación LCK y constructor libre LVK

En el modo **LCK** se ocultan la configuración, los casos prácticos y el constructor de mallas para dejar únicamente el análisis de nodos y el cubo. En el modo **LVK** aparece el **Constructor libre de mallas**: puedes agregar o quitar mallas, definir la fuente y resistencia propia de cada una, y asignar resistencias compartidas entre cualquier par. El botón **Usar mallas libres** resuelve automáticamente el sistema personalizado; el valor 0 Ω deja una conexión sin utilizar.


## Requisitos de la entrega

La maqueta incorpora LVK con mallas y sentido horario/antihorario, LCK con nodos, corrientes entrantes/salientes y GND seleccionable, tres casos prácticos de una a tres mallas, constructor libre, animación de flujo, resultados de corriente/voltaje/potencia por elemento, créditos con escudos, comprobación analítica por ambos métodos y mapa esquemático del desarrollo. La pestaña **Entrega / rúbrica** concentra la lista de verificación.

Antes de presentar el proyecto deben sustituirse en esa pestaña las dos URLs públicas reales —maqueta y video— y agregarse los nombres y evidencias comprobables del equipo. El archivo `GUION-VIDEO-DIVULGACION.md` contiene una secuencia sugerida para demostrar el cambio de convenciones, el circuito analítico por LVK/LCK y el funcionamiento de la maqueta.

## Enlace público temporal

La maqueta se comprobó en el siguiente enlace temporal de la sesión:

https://8765-iq168vdarljkufjkb0kak-570bddca.us4.manus.computer/kirchhoff3d-android.html

Este enlace sirve para demostrar la maqueta mientras se publica una URL permanente. Antes de entregar, conviene reemplazarlo por GitHub Pages, Netlify o Vercel.


## Énfasis visual

La pantalla principal prioriza la visualización antes de los controles extensos. El diagrama muestra una insignia de estado, una leyenda de flujo, resistencias y barreras, y el cubo LCK destaca nodos activos, conexiones y GND. La composición se adapta a vertical y horizontal para que el usuario vea primero el comportamiento de la maqueta.


## Distribución radial de nodos

En la vista **LCK**, el nodo seleccionado se convierte en el origen visual y se coloca al centro del cubo. Sus vecinos directos se distribuyen alrededor de él, se resaltan en verde y muestran las conexiones y el flujo. Los demás nodos quedan en el anillo exterior, mientras una silueta punteada conserva la referencia del cubo técnico. Al seleccionar otro nodo, la distribución se recalcula automáticamente.


## Cubo LCK 3D manipulable

El cubo LCK utiliza una vista vectorial nítida con perspectiva 3D. El nodo seleccionado se muestra ampliado, con halo luminoso, etiqueta **ORIGEN** y contraste blanco; los nodos conectados también aumentan de tamaño y se iluminan en verde. El usuario puede arrastrar directamente sobre el cubo para rotarlo en cualquier dirección y usar **Vista 3D** para recuperar la perspectiva inicial.


## Cubo geométrico 3D real

La vista LCK ya no utiliza una imagen plana inclinada. Ahora se construye con seis caras tridimensionales, doce aristas y ocho nodos ubicados en vértices con profundidad real. El usuario puede arrastrar el cubo para girarlo, seleccionar cualquiera de sus nodos y restablecer la perspectiva con **Vista 3D**.


## Proyecto individual y navegación

El proyecto está registrado como trabajo individual de **Erick Vega Carrasco**; no se solicitan nombres de integrantes adicionales. La pestaña **Entrega / rúbrica** ya muestra la evidencia personal que debes anexar: capturas, bitácora, commits o fotografías del proceso. Además, cada apartado superior funciona como una vista independiente: **Maqueta**, **Comprobación analítica**, **Mapa del desarrollo**, **Créditos y enlaces** y **Entrega / rúbrica**. Al seleccionar uno, se ocultan las demás secciones y se muestra únicamente su contenido correspondiente.


## Voltajes editables en LCK

En la vista **LCK** aparece el bloque **Voltajes editables por nodo** con un único campo que cambia según el nodo seleccionado: N1–N8. El nodo configurado como GND se mantiene automáticamente en **0 V**; al cambiar GND, los demás voltajes se ajustan a la nueva referencia. La edición solo afecta al nodo seleccionado y conserva el campo activo para no cerrar el teclado y actualiza el listado de voltajes, la selección visual y el sentido del flujo en las aristas del cubo.


## Actualización visual del voltaje seleccionado

Se conserva el cálculo anterior de la maqueta. Al cambiar el voltaje del nodo seleccionado, el cambio se muestra únicamente en la tabla visual **Elementos: corriente, voltaje y potencia**, mediante una fila resaltada `V(Nx) · nodo seleccionado`. La ecuación general y los demás cálculos permanecen con el comportamiento anterior.


## Propagación visual entre nodos

Cuando se cambia el voltaje del nodo seleccionado, el incremento se refleja también en los demás nodos de la red conectada. GND permanece en `0 V`. El listado de voltajes y las etiquetas de cada vértice del cubo 3D se actualizan para mostrar los nuevos valores.


## Explicación del comportamiento de los nodos

La tabla LCK de elementos fue sustituida por ocho fichas explicativas, una por nodo. Cada ficha muestra el voltaje actual, si es GND o el nodo seleccionado, sus conexiones directas y el sentido visual de la corriente convencional según los voltajes de sus vecinos. En teléfonos, las fichas pasan a una sola columna para conservar la lectura completa.

## Pantalla inicial de la aplicación

Al abrir la aplicación Android se muestra durante aproximadamente 1.5 segundos el símbolo de corriente `I →` junto con el título del laboratorio. Después se carga automáticamente la interfaz interactiva en WebView. La pantalla está implementada en `MainActivity.kt`, por lo que no depende de internet ni de un archivo HTML externo.


## Repositorio de GitHub

El repositorio del proyecto fue agregado en las secciones **Créditos y enlaces** y **Entrega / rúbrica**:

<https://github.com/EVC09/laboratorio-de-Kirchhoff-.git>
