# python3

import speech_recognition as sr
import pyttsx3
import datetime
import pywhatkit

# Initialize recognizer and text-to-speech engine
listener = sr.Recognizer()
engine = pyttsx3.init()

def talk(text):
    print(f"💬 Assistant: {text}")
    engine.say(text)
    engine.runAndWait()

def take_command():
    try:
        with sr.Microphone() as source:
            print("🎤 Listening...")
            voice = listener.listen(source, timeout=5, phrase_time_limit=5)
            command = listener.recognize_google(voice)
            command = command.lower()
            print(f"🗣️ You said: {command}")
            return command
    except sr.UnknownValueError:
        talk("Sorry, I could not understand you.")
        return ""
    except sr.RequestError:
        talk("Speech recognition service is not available right now.")
        return ""
    except Exception as e:
        talk(f"An error occurred: {str(e)}")
        return ""

def run_assistant():
    command = take_command()
    if not command:
        return  # Skip if empty

    if "hello" in command:
        talk("Hello Bhoomika, how can I help you today?")
    elif "time" in command:
        time_now = datetime.datetime.now().strftime('%I:%M %p')
        talk(f"The current time is {time_now}")
    elif "date" in command:
        date_today = datetime.datetime.now().strftime('%B %d, %Y')
        talk(f"Today's date is {date_today}")
    elif "play" in command:
        song = command.replace("play", "").strip()
        if song:
            talk(f"Playing {song}")
            pywhatkit.playonyt(song)
        else:
            talk("Please tell me the song name.")
    elif "search" in command:
        search_term = command.replace("search", "").strip()
        if search_term:
            talk(f"Searching for {search_term}")
            pywhatkit.search(search_term)
        else:
            talk("Please tell me what to search.")
    else:
        talk("Sorry, I didn't understand that.")

if __name__ == "__main__":
    talk("Hi Bhoomika, I am your voice assistant.")
    while True:
        run_assistant()
