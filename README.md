# virtual-python-assistant
virtual python assistant

import pyttsx3 as p
import speech_recognition as sr
import datetime
import wikipedia
import smtplib
import webbrowser as wb
import os
import psutil
import  pyjokes

r = sr.Recognizer()
engine = p.init()

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def time():
    timeis = datetime.datetime.now().strftime("%I:%M:%S")
    speak("the time is-")
    speak(timeis)



def date():
    year = int(datetime.datetime.now().year)
    month = int(datetime.datetime.now().month)
    day = int(datetime.datetime.now().day)
    speak("the date is")
    speak(day)
    speak(month)
    speak(year)

def wishme():
    ##speak("welcome back sai..")
    ##time()
    ##date()

    hour = datetime.datetime.now().hour
    if hour >= 6 and hour < 12:
        speak("good morning ")

    elif hour >= 12 and hour < 17:
        speak("good afternoon ")

    elif hour >=17  and hour < 20:
        speak("good evening ")

    else:
        speak("good night sai!!")

    speak("your assistant at the service. What do u want me to do?")


def takecommand():
    with sr.Microphone() as source:
        print("listening..")
        text = r.listen(source)
        r.pause_threshold = 1

    try:
        query = r.recognize_google(text , language='in-en')
        print("Recognizing...")
        print(query)

    except sr.UnknownValueError:
        speak("say once again.")
        return 'None'

    except sr.RequestError:
        print("sorry!! i did not get that..")
        return 'None'
        exit(0)
    return query


def sendemail(to,content):
    server = smtplib.SMTP("smtb.gmail.com",587)
    server.ehlo()
    server.starttls()
    server.login("my@gmail.com","1234")
    server.sendmail("my@gmail.com",to,content)
    server.close()


def cpu():
    usage = str(psutil.cpu_percent())
    speak("your cpu usage is at" +usage)

    battery = psutil.sensors_battery()
    speak("your battery life is at")
    speak(battery.percent)


def joke():
    speak(pyjokes.get_joke())


if __name__ == '__main__':
    wishme()
    while True:
        query = takecommand().lower()

        if 'time' in query:
            time()

        elif 'date' in query:
            date()

        elif 'wikipedia' in query:
            speak('searching..')
            query = query.replace('wikipedia',"")
            results = wikipedia.summary(query , sentences=1)
            print(results)
            speak(results)


        elif 'email' in query:
            try:
                speak("what should i say??")
                content = takecommand()
                to = "yours@gmail.com"
                sendemail(to,content)
                speak("email has been sent..")

            except:
                speak("unable to send the mail")



        elif 'chrome' in query:
            speak('what should i search for??')
            chromepath = 'C:/Program Files (x86)/Google/Chrome/Application/chrome.exe %s'
            search = takecommand().lower()
            wb.get(chromepath).open_new_tab(search+ '.com')


        elif 'song' in query:
            song_dir = "D:\movies"
            song = os.listdir(song_dir)
            os.startfile(os.path.join(so4ng_dir,song[0]))


        elif 'remember' in query:
            speak("what should i remember?")
            data = takecommand()
            speak('you said me to remember that'+data)
            remember = open('data.txt','w')
            remember.write(data)
            remember.close()

        elif 'do you know that' in query:
            remember = open('data.txt','r')
            speak('u said me to remember thhat' +remember.read())


        elif 'cpu' in query:
            cpu()

        elif 'joke' in query:
            joke()

        elif 'sign out' in query:
            os.system('shutdown -l')

        elif 'offline' in query:
            quit()





