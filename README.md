# AI-PROJECT

import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser  as wb
import webbrowser
import os
import smtplib
import pyjokes
import pywhatkit


name = 'tania'

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def greet():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour <12:
        speak("Good morning " + name)
    elif hour >=12 and hour < 18:
        speak("good afternoon" + name)
    else:
        speak("good evening" + name)
    speak("How may I help you")


def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        print("recognize")
        query = r.recognize_google(audio, language='en-in')
        print(f"user said: {query}\n")
    except Exception as e:
        print("say again")
        speak("say again")
        return "None"
    return query

def sendEmail(to,content):
    server = smtplib.SMTP('smtp.gmail.com',587)
    server.ehlo()
    server.starttls()
    server.login('taniaayaz24@gmail.com', '24sep2000')
    server.sendmail('taniaayaz24@gmail.com', to, content)
    server.close()

chrome_path = "C:/Program Files/Google/Chrome/Application/chrome.exe %s"
if __name__ == "__main__":
    greet()
    while True:
        query = takeCommand().lower()
        if 'wikipedia' in query:
            speak("searching")
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=3)
            speak("According to wikipedia")
            speak(results)

        elif 'open youtube' in query:
            webbrowser.get(chrome_path).open('youtube.com')

        elif 'open google' in query:
            webbrowser.get(chrome_path).open('google.com')
        elif 'open facebook' in query:
            webbrowser.get(chrome_path).open('facebook.com')

        elif 'the time' in query:
            now_time = datetime.datetime.now().strftime('%I:%M %p')
            speak(f"Its {now_time}")

        elif 'open vs code' in query:
            vs_code_path = "C:\\Users\\Tania Ayaz\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(vs_code_path)

        elif 'open visual studio' in query:
            vs_path = "C:\\Program Files (x86)\\Microsoft Visual Studio 14.0\\Common7\\IDE\\devenv.exe"
            os.startfile(vs_path)

        elif 'send email' in query:
            try:
                speak("What should I say")
                content = takeCommand()
                speak("To whom you want to send email")
                to = takeCommand()
                if to == 'tooba':
                    sendEmail("toobaayaz24@gmail.com", content)
                    speak("Email sent successfully")
            except Exception as e:
                print(e)
                speak("Sorry! Email cannot send")

        elif 'joke' in query:
            speak(pyjokes.get_joke())

        elif 'play' in query:
            song = query.replace('play','')
            speak('playng' + song)
            pywhatkit.playonyt(song)



