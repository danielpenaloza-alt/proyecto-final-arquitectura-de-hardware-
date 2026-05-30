# Proyecto Final
**Fundación Universitaria Compensar**  
**Docente:** Diego Alejandro Barragán Vargas  
*Ingeniero Electrónico, Magíster en Ingeniería, Doctorando UDFJC*
---

##  Integrantes

| Nombre | Rol |
|--------|-------|
|Juan Jose Salazar Munoz| creacion dechat bot  |
| David Santiago Chivata Garcia |planeacion de base de datos  |
|Victor Manuel Sarmiento puentes  | progamador del sistema de detecion de objetos  |
| Daniel Felipe Peñaloza Bravo | crear y reportar probra y dar evidencias en git del proyecto  |

---

## Descripción General

Desarrollo de una plataforma de **visión computacional** para el reconocimiento automático de partes de un computador. El sistema integra un modelo de detección de objetos (YOLO), un chatbot generativo con soporte de voz, y se despliega en Streamlit. 
---

## Tabla de Contenidos

- [Integrantes](#integrantes)
- [Punto 1 — Web Scraping y Base de Datos](#punto-1--web-scraping-y-base-de-datos)
- [Punto 2 — ETL de la Base de Datos](#punto-2--etl-de-la-base-de-datos)
- [Punto 3 — Modelo YOLO (Detección de Partes)](#punto-3--modelo-yolo-detección-de-partes)
- [Punto 4 — Chatbot Generativo con Voz](#punto-4--chatbot-generativo-con-voz)
- [Punto 5 — Despliegue en Streamlit](#punto-5--despliegue-en-streamlit)
- [Estructura del Repositorio](#estructura-del-repositorio)

---

## Punto 1 — Web Scraping y Base de Datos

###  Descripción
Recolección automatizada de imágenes de al menos **10 componentes del PC** . La base de datos debe contener un mínimo de **100 imágenes por componente**.

**Componentes objetivo:**
`monitor` · `chasis` · `mouse` · `teclado` · `altavoces` · `impresora` · `webcam` · `CPU` · `memoria RAM` · `disco duro` · `tarjeta de video` · `tarjeta de red`

###  Requerimientos
- [ ] Script de web scraping 
- [ ] Búsqueda de mínimo 10 componentes
- [ ] Mínimo 100 imágenes por componente
- [ ] El chatbot debe poder explicar definición y función de cada elemento

### Evidencias Fotográficas

> Agrega aquí capturas de pantalla del proceso. Ejemplo:
> ```
> ![Descripción](evidencias/punto1/nombre_imagen.png)
> ```

| Evidencia | Descripción |
|-----------|-------------|
| ![Estructura de carpetas](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/a57ded814c9b29adce2b5d923e5a3bc346388657/base%20de%20datos.png) | Organización de la base de datos de imágenes |
| ![Muestra de imágenes](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/44a558e21a9bc1636da516b4fd12c6cc77cb8222/orgimg.png) | Muestra de imágenes recolectadas por clase |
| ![Muestra de imágenes](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/d1da68608a460d300d9fa94f9720fb41dc11c642/chatexp.png) | chat de voz en ejecucion |

###codigo de scrip 
---
    from selenium import webdriver
    from selenium.webdriver.common.by import By
    import requests
    import time
    import os
    carpeta_destino = r'C:\Users\VictorManuelSarmient\Desktop\Descarga de imagenes\imagenes'
    if not os.path.exists(carpeta_destino):
    os.makedirs(carpeta_destino)
    url="https://www.google.com/search?q=ram+pc&tbm=isch"
    driver = webdriver.Chrome()
    driver.maximize_window()
    driver.get(url)
 

    driver.execute_script("window.scrollBy(0, 1000);")
    time.sleep(10)
    print("Cargando más resultados...")
    for _ in range(10):
    driver.execute_script("window.scrollBy(0, 2000);")
    time.sleep(2)
    enlaces = driver.find_elements(By.CLASS_NAME, "YQ4gaf")
    print(f"Se encontraron {len(enlaces)} imágenes en total. Empezando descarga...")
    i=0
    for enlace in enlaces:
      try:
        link = enlace.get_attribute('src')
          if link and link.startswith('http'):
          Imagen_Ram = requests.get(link)
 
      if  Imagen_Ram.status_code == 200:
        print (f"Descargando: {link}")  
        i+=1
        nombreImagen = 'Ram' + str(i) + '.jpg'
        nombre_archivo = f"Ram_{i}.jpg"
        ruta_completa = os.path.join(carpeta_destino, nombre_archivo)
               
        print(f"Descargando en carpeta: {nombre_archivo}")
        with open(ruta_completa, 'wb') as img:
         img.write(Imagen_Ram.content)
 
        if i >= 300:
           break
    except Exception as e:
              print(f"Error al descargar la imagen: {e}")
    continue
    driver.quit()

---

## Punto 2 — Modelo YOLO (Detección de Partes)

###  Descripción
Entrenamiento y configuración de un modelo **YOLO** para clasificar y detectar las partes del PC en imágenes y video en tiempo real.

###  Requerimientos
- [ ] Anotación del dataset (bounding boxes por componente)
- [ ] Entrenamiento del modelo YOLO
- [ ] Validación y métricas (mAP, precisión, recall)
- [ ] Pruebas de detección sobre imágenes y video

###  Evidencias Fotográficas

| Evidencia | Descripción |
|-----------|-------------|
| ![Anotaciones](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/eb541e378014c9f51b4f4dc49223ffa82ea8b988/yolo1.png) | codigo en colab de entrenamiento de yolo |
| ![Entrenamiento](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/eb541e378014c9f51b4f4dc49223ffa82ea8b988/results.png) | Curvas de pérdida durante el entrenamiento |
| ![Métricas](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/eb541e378014c9f51b4f4dc49223ffa82ea8b988/matrixes.png) | Métricas finales del modelo (mAP, precisión, recall) | 
| ![Detección en acción](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/eb541e378014c9f51b4f4dc49223ffa82ea8b988/detector.jpg) | Resultado de detección sobre imagen de prueba | 

---

## Punto 4 — Chatbot Generativo con Voz

###  Descripción
Chatbot capaz de recibir preguntas mediante **reconocimiento de voz** y responder también por **voz**, explicando la definición y función de cada componente del PC detectado.

###  Requerimientos
- [ ] Reconocimiento de voz (Speech-to-Text)
- [ ] Generación de respuesta (modelo generativo / LLM)
- [ ] Síntesis de voz (Text-to-Speech)
- [ ] Integración con los componentes detectados por YOLO

###  Evidencias Fotográficas

| Evidencia | Descripción | Fecha |
|-----------|-------------|-------|
![imagen](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/b7085ae5040a8abbd5793b804c084257afc9ffc2/chat%20bot1.png).|lo que se muestra a continuacion en las imagenes es el proceso en el cual se desarrolo el chat voz que a podra vincular con el detector de objetos 
| ![Interfaz chatbot](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/afa1e4cc635ed203ff556bf35c8e27065004a32d/chat%20bot%202.png) 
| ![Respuesta de voz](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/60d14260fc327eee6c854248d7ed47e47d215bb3/chat%20bot%203.png) 

---

## Punto 5 — Despliegue en Streamlit

###  Descripción
Integración de todos los módulos (detección YOLO + chatbot con voz) en una **plataforma web desplegada con Streamlit**.

###  Requerimientos
- [ ] Interfaz completa en Streamlit
- [ ] Módulo de detección de partes del PC (cámara / imagen)
- [ ] Módulo de chatbot con voz integrado
- [ ] Todo el desarrollo documentado paso a paso en GitHub
 
###  Evidencias Fotográficas

| Evidencia | Descripción | 
|-----------|-------------|
| ![Dashboard principal](evidencias/punto5/dashboard_principal.png) | evidencias del codigo final ejecutado mediante visual studio code lo cual se puede tambien abrir y descargar aplicacion en celular y usar el programa para detectar los componentes con alto porcentaje de precision   | 
| ![Detección en plataforma](evidencias/punto5/deteccion_streamlit.png) 
| ![Chatbot en plataforma](evidencias/punto5/chatbot_streamlit.png)
| ![Demo completo](evidencias/punto5/demo_completo.png) 

### codigo yolo 
    import cv2
    import speech_recognition as sr
    import pyttsx3
 
## ==========================================
## 1. CONFIGURACIÓN INICIAL (BOT Y YOLO)
## ==========================================
    engine = pyttsx3.init()
    model = YOLO("best.pt")  # Tu modelo entrenado
 
    def hablar(texto):
      print("\nBOT:", texto)
      engine.say(texto)
      engine.runAndWait()
 
## Diccionario de componentes de tu amigo
    componentes = {
     "ram": "La memoria RAM almacena datos temporales para mejorar el rendimiento del computador.",
     "procesador": "El procesador ejecuta instrucciones y operaciones del computador.",
     "monitor": "El monitor muestra información visual.",
     "mouse": "El mouse permite controlar el cursor.",
     "teclado": "El teclado sirve para ingresar información.",
     "tarjeta grafica": "La tarjeta gráfica procesa imágenes y videojuegos.",
     "fuente de poder": "La fuente de poder suministra energía al computador.",
     "disco duro": "El disco duro almacena archivos y programas.",
     "placa madre": "La placa madre conecta todos los componentes del computador.",
     "ventilador": "El ventilador ayuda a enfriar el computador."
}
 
    def escuchar():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("\nHabla ahora...")
        r.adjust_for_ambient_noise(source, duration=0.3)
        try:
            audio = r.listen(source, timeout=4, phrase_time_limit=4)
        except:
            print("Tiempo de escucha agotado")
            return None
 
    try:
        texto = r.recognize_google(audio, language="es-ES")
        print("TÚ:", texto)
        return texto.lower()
    except:
        print("No se entendió la voz")
        return None
 
## ==========================================
## 2. INICIALIZAR CÁMARA
## ==========================================
    cap = cv2.VideoCapture(0)
    cap.set(3, 640)
    cap.set(4, 480)
 
    hablar("Sistema de Inteligencia Artificial listo.")
    print("Presiona ESC en la ventana de la cámara para apagar el sistema.")
 
    objeto_detectado = None
 
## ==========================================
## 3. BUCLE PRINCIPAL EN TIEMPO REAL
## ==========================================
    while True:
      ret, frame = cap.read()
      if not ret:
        print("Error al acceder a la cámara")
        break
 
    annotated_frame = frame.copy()
 
    # Ejecutar tu modelo YOLO (confianza 40%)
    results = model.predict(frame, imgsz=640, conf=0.4, verbose=False)
 
    # Si el modelo ve algo, actualizamos qué objeto está en pantalla
    if len(results) > 0 and len(results[0].boxes) > 0:
        annotated_frame = results[0].plot()
        # Tomamos el nombre del primer objeto que detecte en el fotograma
        for box in results[0].boxes:
            id_clase = int(box.cls[0])
            objeto_detectado = model.names[id_clase].lower() 
            break # Nos quedamos con el principal detectado
 
    # Mostrar la cámara con los recuadros de YOLO
    cv2.imshow("Camara IA - Deteccion", annotated_frame)
 
    # Escuchar el teclado por si quieren cerrar con ESC (27)
    tecla = cv2.waitKey(1) & 0xFF
    if tecla == 27:
        hablar("Apagando sistema")
        break
 
    # Si hay un objeto frente a la cámara, el bot interactúa por voz
    if objeto_detectado:
        print(f"[Viendo actualmente: {objeto_detectado}]")
        pregunta = escuchar()
 
        if pregunta is None:
            # Si no habló, el bucle sigue actualizando la cámara
            continue
 
        if "salir" in pregunta:
            hablar("Apagando sistema")
            break
 
        # Lógica de respuesta basada en lo que ve el ojo de YOLO
        if "eso" in pregunta or "qué es" in pregunta:
            respuesta = componentes.get(objeto_detectado, f"Veo un {objeto_detectado}, pero no tengo información de él.")
        else:
            encontrado = False
            for c in componentes:
                if c in pregunta:
                    respuesta = componentes[c]
                    encontrado = True
                    break
            if not encontrado:
                respuesta = "No entendí tu pregunta, intenta preguntarme qué es eso."
 
        hablar(respuesta)
        # Limpiamos el objeto para forzar a que la cámara busque de nuevo en el siguiente ciclo
        objeto_detectado = None
 
    # Liberar todo al cerrar
    cap.release()
    cv2.destroyAllWindows()
---


---

##  Estructura del Repositorio

```
 proyecto-arquitectura-hardware
├── 📂 evidencias/
│   ├── punto1/        # Capturas del web scraping y BD
│   ├── punto2/        # Capturas del entrenamiento YOLO
│   ├── punto3/        # Capturas del chatbot con voz
│   ├── punto4/        # Capturas del despliegue Streamlit
├── 📂 punto1_scraping/
│   └── scraper.py
├── 📂 punto2_yolo/
│   ├── train.py
│   └── detect.py
├── 📂 punto3_chatbot/
│   └── chatbot_voz.py
├── 📂 punto4_streamlit/
│   └── app.py
└── README.md
```



## Estado General del Proyecto

| Punto | Descripción | Estado |
|-------|-------------|--------|
| 1 | Web Scraping y Base de Datos |  completo |
| 2 | Modelo YOLO |  completo |
| 3 | Chatbot con Voz | completo |
| 4 | Despliegue Streamlit |  en proceso |


> **Leyenda:**  Sin iniciar ·  En progreso ·  Completado

---

*Fundación Universitaria Compensar — Materia: Arquitectura de Hardware*
