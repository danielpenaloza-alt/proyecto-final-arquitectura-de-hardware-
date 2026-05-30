# Proyecto Final
**Fundación Universitaria Compensar**  
**Docente:** Diego Alejandro Barragán Vargas  
*Ingeniero Electrónico, Magíster en Ingeniería, Doctorando UDFJC*
---

##  Integrantes

| Nombre | Rol |
|--------|-------|
|Juan Jose Salazar Munoz| creacion de chat bot  |
| David Santiago Chivata Garcia | planeacion de base de datos  |
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
| ![Dashboard principal](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/8b9cde48e4f1dc4c9b0dbcbd2f31881bdf595be2/imagen%20(6).png) | evidencias del codigo final ejecutado mediante visual studio code lo cual se puede tambien abrir y descargar aplicacion en celular y usar el programa para detectar los componentes con alto porcentaje de precision   | 
| ![Detección en plataforma](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/8b9cde48e4f1dc4c9b0dbcbd2f31881bdf595be2/imagen%20(7).png) 
| ![Chatbot en plataforma](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/8b9cde48e4f1dc4c9b0dbcbd2f31881bdf595be2/imagen%20(8).png)| se evidencia que el chat de voz responde a las preguntar de saber cual es el componente |
| ![Demo completo](https://github.com/danielpenaloza-alt/proyecto-final-arquitectura-de-hardware-/blob/8b9cde48e4f1dc4c9b0dbcbd2f31881bdf595be2/imagen%20(9).png) | |

### codigo yolo 
    from ultralytics import YOLO
     import cv2
     import speech_recognition as sr
     import pyttsx3
 
    engine = pyttsx3.init()
 
    def hablar(texto):
     print("\nBOT:", texto)
     engine.say(texto)
     engine.runAndWait()
 
    componentes = {
     "ram": "La memoria RAM almacena datos temporales para mejorar el rendimiento del computador.",
     "procesador": "El procesador ejecuta instrucciones y operaciones.",
     "monitor": "El monitor muestra información visual.",
     "mouse": "El mouse permite controlar el cursor.",
     "teclado": "El teclado sirve para ingresar información.",
     "tarjeta grafica": "La tarjeta gráfica procesa imágenes.",
     "fuente de poder": "La fuente de poder suministra energía.",
     "disco duro": "El disco duro almacena archivos.",
     "placa madre": "La placa madre conecta todo.",
     "ventilador": "El ventilador enfría el sistema."
    }
 
    def escuchar():
     r = sr.Recognizer()
     with sr.Microphone() as source:
        print("\nHabla ahora...")
        r.adjust_for_ambient_noise(source, duration=0.3)
        try:
            audio = r.listen(source, timeout=4, phrase_time_limit=4)
        except:
            print("Tiempo agotado")
            return None
 
    try:
        texto = r.recognize_google(audio, language="es-ES")
        print("TÚ:", texto)
        return texto.lower()
    except:
        print("No se entendió")
        return None
 
 
    model = YOLO("best.pt")  
 
 
    cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)  
 
 
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)
 
    if not cap.isOpened():
     print(" No se pudo conectar a la cámara del celular.")
     print("Revisa que la app (Iriun/DroidCam) esté abierta en la PC y en el móvil.")
     exit()
 
    hablar("Sistema listo conectado al teléfono celular")
 
    objeto_actual = None
 
    while True:
    ret, frame = cap.read()
    if not ret:
        print("Error al leer la transmisión del celular")
        break
 
    results = model.predict(frame, imgsz=640, conf=0.9, verbose=False)
 
    if len(results) > 0 and len(results[0].boxes) > 0:
        box = results[0].boxes[0]
        cls = int(box.cls[0])
        objeto_actual = model.names[cls]
 
        frame = results[0].plot()
        print("Detectado:", objeto_actual)
 
    cv2.imshow("Camara - Celular en vivo", frame)
 
    if cv2.waitKey(1) & 0xFF == ord('s'):
        pregunta = escuchar()
 
        if pregunta is None:
            hablar("No entendí")
            continue
 
        if "salir" in pregunta:
            hablar("Apagando")
            break
 
        if "eso" in pregunta and objeto_actual:
            respuesta = componentes.get(objeto_actual, "No tengo información de eso")
        else:
            encontrado = False
            for c in componentes:
                if c in pregunta:
                    respuesta = componentes[c]
                    encontrado = True
                    break
 
            if not encontrado:
                respuesta = "No entendí la pregunta"
 
        hablar(respuesta)
 
    cap.release()
    cv2.destroyAllWindows()

###codigo pasado para ejecutar en raspberry
---
    rom ultralytics import YOLO
    import cv2
    import speech_recognition as sr
    import os
 
    def hablar(texto):
    print("\nBOT:", texto)
    os.system(f'espeak "{texto}"')
 
    model = YOLO("best.pt")
 
    cap = cv2.VideoCapture(0)
 
    if not cap.isOpened():
    print("No se pudo abrir la cámara")
    exit()
 
    hablar("Sistema listo")
 
    while True:
 
    ret, frame = cap.read()
 
    if not ret:
        break
 
    results = model.predict(
        frame,
        imgsz=640,
        conf=0.7,
        verbose=False
    )
 
    frame = results[0].plot()
 
    cv2.imshow("Raspberry Vision", frame)
 
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
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
