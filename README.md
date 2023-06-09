# import pyttsx3
from pyttsx3 import Engine
from speech_recognition import Microphone, Recognizer

# Configuración del motor de voz
engine = pyttsx3.init()
engine.setProperty('rate', 150)  # Velocidad de habla en palabras por minuto


# Función para que la IA asistente hable
def hablar(texto):
    engine = pyttsx3.init()
    engine.say(texto)
    engine.runAndWait()

    def asistente():
        hablar("Hola, soy Rebeca. ¿En qué puedo ayudarte?")

        while True:
            texto = escuchar().lower()

            if "adiós" in texto:
                hablar("Hasta luego. ¡Que tengas un buen día!")
                break


# Función para que la IA asistente escuche y reconozca el habla
def escuchar():
    r = Recognizer()
    with Microphone() as source:
        print("Di algo...")
        audio = r.listen(source)

    try:
        texto_escuchado = r.recognize_google(audio, language='es-ES')
        print("Has dicho:", texto_escuchado)
        return texto_escuchado
    except Exception as e:
        print("No se pudo reconocer el habla. Error:", str(e))
        return ""


# Función principal de la IA asistente
def asistente():
    hablar("Hola, soy Rebeca. ¿En qué puedo ayudarte?")

    while True:
        texto = escuchar().lower()

        if "hola" in texto:
            hablar("¡Hola! ¿Cómo puedo ayudarte?")
        elif "adiós" in texto:
            hablar("Hasta luego. ¡Que tengas un buen día!")
            break
        else:
            hablar("Lo siento, no entiendo. Por favor, repite.")


# Llamada a la función principal
asistente()


from PyInquirer import prompt, Separator
from pprint import pprint

menu_options = [
    {"type": "list", "name": "Opción 1", "message": "Esta es la opción 1"},
    Separator(),
    {"type": "list", "name": "Opción 2", "message": "Esta es la opción 2"},
    {"type": "list", "name": "Opción 3", "message": "Esta es la opción 3"},
]

print("Hola, soy Rebeca. ¿En qué puedo ayudarte?")

answer = prompt(menu_options)
pprint(answer)

import googlesearch
import requests
from bs4 import BeautifulSoup


# Función para buscar en Google y obtener el resultado
def buscar_en_google(query: object) -> object:
    try:
        # Realizar la búsqueda en Google
        resultados: object = googlesearch.search(query, num_results=1, lang='es')

        if resultados:
            # Obtener el primer resultado de la búsqueda
            url = resultados[0]

            # Hacer una solicitud GET a la URL y obtener el contenido HTML
            response = requests.get(url)
            soup = BeautifulSoup(response.text, 'html.parser')

            # Extraer la información relevante de los resultados
            # Aquí puedes personalizar la extracción según tus necesidades
            respuesta = soup.find('div', class_='answer').text

            # Imprimir la respuesta
            print(respuesta)
        else:
            print("No se encontraron resultados para la búsqueda.")

    except Exception as e:
        print("Error al realizar la búsqueda:", str(e))

# Ejemplo de uso
pregunta = input("Hazme una pregunta: ")
buscar_en_google(pregunta)

import pyttsx3
import io

# Configuración del motor de voz
engine = pyttsx3.init()
engine.setProperty('rate', 150)  # Velocidad de habla en palabras por minuto

# Función para que la IA asistente lea un archivo de texto
def leer_archivo(ruta):
    try:
        with io.open(ruta, 'r', encoding='utf-8') as archivo:
            contenido = archivo.read()
            engine.say(contenido)
            engine.runAndWait()
    except Exception as e:
        print("Error al leer el archivo:", str(e))

# Llamando a la función para leer un archivo de texto
leer_archivo('ruta_del_archivo.txt')
import pyttsx3
from pydub import AudioSegment

def modificar_voz_audio(ruta_audio, velocidad, volumen):
    # Cargar el archivo de audio
    audio = AudioSegment.from_file(ruta_audio, format="mp3")

    # Modificar la velocidad y el volumen del audio
    audio = audio.speedup(playback_speed=velocidad / 100.0)
    audio = audio + volumen

    # Guardar el audio modificado
    ruta_salida = "Sintesis_de_vox.mp3"
    audio.export(ruta_salida, format="mp3")

    # Reproducir el audio modificado
    engine = pyttsx3.init()
    engine.setProperty('rate', velocidad)  # Velocidad de la voz en palabras por minuto
    engine.setProperty('volume', volumen)  # Volumen de la voz (0.0 a 1.0)
    engine.say("Sintesis_de_vox")
    engine.runAndWait()

    return ruta_salida

ruta_audio = "Sintesis_de_vox.mp3"
velocidad = 150
volumen = 0.9

ruta_audio_modificado = modificar_voz_audio(ruta_audio, velocidad, volumen)
