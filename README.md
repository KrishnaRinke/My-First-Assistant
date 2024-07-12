# My-First-Assistant

import speech_recognition as sr
import webbrowser
import pyttsx3
import requests
from openai import OpenAI
#pip install pocketsphinx
recognizer=sr.Recognizer()
engine=pyttsx3.init()
newsapi="842d059876fb48aeb95f6522192fbedd"

def speak(text):
    engine.say(text)
    engine.runAndWait()

def aiProcess(command):
    client = OpenAI(
    api_key="include an openai api id",
    )
    completion = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a virtual assistant named jarvis, skilled in general tasks like alexa and google cloud."},
        {"role": "user", "content": command}
    ]
    )

    return completion.choices[0].message.content


#functio for the the command processing after activating jarvis
def processCommand(c): 
    if "open google" in c.lower():
        webbrowser.open("https://google.com")
    elif"open facebook" in c.lower():
        webbrowser.open("https://facebook.com")
    elif"open linkedin" in c.lower():
        webbrowser.open("https://linkedin.com") 
    elif"open youtube" in c.lower():
        webbrowser.open("https://youtube.com") 
    elif"open instagram" in c.lower():
        webbrowser.open("https://instagram.com") 
    elif"open leetcode" in c.lower():
        webbrowser.open("https://leetcode.com") 
    elif"news" in c.lower():
        r=requests.get(f"https://newsapi.org/v2/top-headlines?country=in&apiKey=842d059876fb48aeb95f6522192fbedd")
        if r.status_code == 200:

            #parse the json response

            data = r.json()

            #extract the articles
            articles = data.get('articles',[])

            #to print the headlines
            for article in articles:
                speak(article['title'])
    else:
        #to give the handling towards the openAi
        output=aiProcess(c)
        speak(output)
        
     

if __name__== "__main__":
    speak("initializing jarvis........")
    while True:
         #listen for the wake word"Jarvis"
         #now obtain the command from the microphone
         
        r = sr.Recognizer()
       

        print("recognizing...")

#use recognize_google
        try:
            with sr.Microphone() as source:
                print("listening")
                audio = r.listen(source,timeout=3,phrase_time_limit=1)
            word = r.recognize_google(audio)
            if(word.lower()=="jarvis"):
                speak("Ya,how can i help you")
                #now we will listen for the command        
                with sr.Microphone() as source:
                    print("how can i help you!")
                    audio = r.listen(source)
                    command=r.recognize_google(audio)
                    
                    processCommand(command)

        except Exception as e:
            print("error; {0}".format(e))

