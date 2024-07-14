import pyttsx3 as p
import speech_recognition as sr
from cellenium import Infow
from video import Music
from news import *
import randfacts
from jokes import *
from weather import *
import datetime

engine = p.init()
rate = engine.getProperty('rate')
engine.setProperty('rate', 180)

def speak(text):
    engine.say(text)
    engine.runAndWait()

def wishes():
    hour =int(datetime.datetime.now().hour)
    if(hour>=0 and hour<12):
        return("Good Morning")
    elif(hour>=12 and hour<18):
        return("Good Afternoon")
    else:
        return("Good Evening")

r = sr.Recognizer()

speak("Hello sir,"+wishes()+ ", I am your voice assistant. How are you?")

with sr.Microphone() as source:
    r.energy_threshold = 1000
    r.adjust_for_ambient_noise(source, duration=1.2)
    print("Say something!")
    audio = r.listen(source)

try:
    text = r.recognize_google(audio)
    print(text)
except sr.UnknownValueError:
    print("Google Speech Recognition could not understand the audio")
    text = ""
except sr.RequestError as e:
    print(f"Could not request results from Google Speech Recognition service; {e}")
    text = ""

if "what" in text and "about" in text and "you" in text:
    speak("I am having a good day, sir")

speak("What can I do for you?")

with sr.Microphone() as source:
    r.energy_threshold = 10000
    r.adjust_for_ambient_noise(source, duration=1.2)
    print("Listening")
    audio = r.listen(source)

try:
    text2 = r.recognize_google(audio)
    print(text2)
except sr.UnknownValueError:
    print("Google Speech Recognition could not understand the audio")
    text2 = ""
except sr.RequestError as e:
    print(f"Could not request results from Google Speech Recognition service; {e}")
    text2 = ""

if "information" in text2:
    speak("You need information related to which topic?")
    with sr.Microphone() as source:
        r.energy_threshold = 1000
        r.adjust_for_ambient_noise(source, duration=1.2)
        print("Listening")
        audio = r.listen(source)

    try:
        infor = r.recognize_google(audio)
        print(infor)
    except sr.UnknownValueError:
        print("Google Speech Recognition could not understand the audio")
        infor = ""
    except sr.RequestError as e:
        print(f"Could not request results from Google Speech Recognition service; {e}")
        infor = ""

    speak("serching {} in google".format(infor))
    print("searching {} in google".format(infor))
    if infor:
        assist = Infow()
        assist.get_info(infor)
    else:
        speak("I could not understand. Please try again.")


elif "play" in text2 or "video" in text2 or "youtube" in text2:
    speak("you want to play which video")
    with sr.Microphone() as source:
        r.energy_threshold = 10000
        r.adjust_for_ambient_noise(source, duration=1.2)
        print("Listening")
        audio = r.listen(source)
        vid = r.recognize_google(audio)
        print("playing video in youtube".format(vid))
        speak("playing video on youtube")
        assist=Music()
        assist.play(vid)

elif "news"in text2:
    print("sure sir here are your news")
    speak("sure sir,here are your news")
    arr=news()
    for i in range (len(arr)):
        print(arr[i])
        speak(arr[i])

elif "fact" in text2 or "facts" in text2:
    speak("sure sir")
    x=randfacts.get_fact()
    print(x)
    speak("did you know that, "+x)

elif "joke" in text2:
    speak("sure sir here are your jokes")
    arr=fetch_joke()
    print(arr[0])
    speak(arr[0])
    print(arr[1])
    speak(arr[1])

elif "temperature"in text2 or "weather" in text2:
    speak("sure sir")
    print(temp())
    speak("The temperature, "+str(temp()))
    print(des())
    speak("The condition is, "+des())
