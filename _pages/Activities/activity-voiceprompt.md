---
layout: activity
permalink: /Activities/VoicePrompt
title: "CS474: Human Computer Interaction - Modalities - Voice Prompts"


info: 
  goals: 
    - To identify alternative modalities for human-computer interaction
    - To write a program that uses voice prompts for engagement
    - To identify signifiers and affordances for a given application and modality
  models:
    - model: |
        <script type="syntaxhighlighter" class="brush: python"><![CDATA[
        # https://github.com/acgrissom/courses/blob/master/2020-hci/hw1_voiceui.md
        # https://github.com/acgrissom/courses/blob/master/2020-hci/code/recognize_speech.py
        # on linux: sudo apt install portaudio19-dev libespeak-dev libespeak1
        # on mac: brew install portaudio
        # on linux/windows: pip3 install git+https://github.com/BillJr99/pyttsx3.git
        # on mac: pip3 install py3-tts (pip3 install pyobjc followed by pyttsx3 might also work)
        # pip3 install pyaudio speechrecognition disutils setuptools
        # alternatively: pip3 install pipwin pypiwin32 && python -m pipwin install pyaudio
        # Install Visual C++ Tools on Windows https://visualstudio.microsoft.com/visual-cpp-build-tools/
          
        import speech_recognition as sr
        import pyttsx3
        import sys
        import time
          
        tts = pyttsx3.init() # pass 'dummy' to this constructor if this call fails due to a lack of voice drivers (but will disable speech)
          
          
        def speak(tts, text):
            tts.say(text)
            tts.runAndWait()
          
        def main():
            # get audio from the microphone                                                                       
            listener = sr.Recognizer()                                                                                   
            with sr.Microphone() as source:
                listener.adjust_for_ambient_noise(source) # used to detect silence to stop listening after a phrase is spoken
                while True:
                    print("Listening.")
                    speak(tts, "listening") # how do we prevent this from being spoken every time an exception is thrown?
                    time.sleep(1) # used to prevent hearing any spoken text; what else could we do?
                    user_input = None
                    sys.stdout.write(">")
                    #record audio
                    listener.pause_threshold = 0.5 # how long, in seconds, to observe silence before processing what was heard
                    audio = listener.listen(source, timeout=5) #, timeout = N throws an OSError after N seconds if nothing is heard.  can also call listen_in_background(source, callback) and specify a function callback that accepts the recognizer and the audio when data is heard via a thread
                    try:
                        #convert audio to text
                        #user_input = listener.recognize_sphinx(audio) #requires PocketSphinx installation
                        user_input = listener.recognize_google(audio, show_all = False) # set show_all to True to get a dictionary of all possible translations
          
                        print(user_input)
                        speak(tts, user_input)
                    except sr.UnknownValueError:
                        print("Could not understand audio")
                    except sr.RequestError as e:
                        print("Could not request results; {0}".format(e))
                    except OSError:
                        print("No speech detected")
                          
                    sys.stdout.write("\n")
          
          
        if __name__ == "__main__":
            main()
        ]]></script>   
        <br>
        <script type="syntaxhighlighter" class="brush: python"><![CDATA[
        # https://github.com/acgrissom/courses/blob/master/2020-hci/hw1_voiceui.md
        # https://github.com/acgrissom/courses/blob/master/2020-hci/code/recognize_speech.py
        # on linux: sudo apt install portaudio19-dev libespeak-dev libespeak1
        # on mac: brew install portaudio
        # on linux/windows: pip3 install git+https://github.com/BillJr99/pyttsx3.git
        # on mac: pip3 install py3-tts (pip3 install pyobjc followed by pyttsx3 might also work)
        # pip3 install pyaudio speechrecognition disutils setuptools
        # alternatively: pip3 install pipwin pypiwin32 && python -m pipwin install pyaudio
        # Install Visual C++ Tools on Windows https://visualstudio.microsoft.com/visual-cpp-build-tools/

        import speech_recognition as sr
        import pyttsx3
        import sys
        import time
          
        tts = pyttsx3.init() # pass 'dummy' to this constructor if this call fails due to a lack of voice drivers (but will disable speech)

        def process_speech(listener, audio):  
            try:
                #convert audio to text
                #user_input = listener.recognize_sphinx(audio) #requires PocketSphinx installation
                user_input = listener.recognize_google(audio, show_all = False) # set show_all to True to get a dictionary of all possible translations

                print(user_input)
                speak(tts, user_input)
                
                sys.stdout.write("\n")
                sys.stdout.write(">")        
            except sr.UnknownValueError:
                print("Could not understand audio")
            except sr.RequestError as e:
                print("Could not request results; {0}".format(e))
            except OSError:
                print("No speech detected")
                    
        def speak(tts, text):
            tts.say(text)
            tts.runAndWait()
          
        def main():
            # get audio from the microphone                                                                       
            listener = sr.Recognizer()
            with sr.Microphone() as source:
                listener.adjust_for_ambient_noise(source) # used to detect silence to stop listening after a phrase is spoken
                
            speak(tts, "listening") 
            print("Listening.")
            time.sleep(1) # used to prevent hearing any spoken text; what else could we do?
            
            sys.stdout.write("\n")
            sys.stdout.write(">")
                
            #record audio
            stop_listening = listener.listen_in_background(source, process_speech)

            # sleep and stop
            time.sleep(60)
            stop_listening(wait_for_stop=False)
          
        if __name__ == "__main__":
            main()
        ]]></script>          
      title: Voice Prompts
      questions:
        - "What is the difference between the two versions of this program?"
        - "How might you adapt this code for use in a text-based program you've written in the past?"
        - "What challenges might you anticipate when using a voice approach, particularly with respect to accessibility, and how might you address them?"
        - "What other modalities can you think of?"
        - "How might you indicate to a user that it is time to input a certain value, and indicate what kinds of values are permissible?"
        - "How do you enable the user to to provide input and to understand output at the right time?"

tags:
  - modalities
  - voice
  
---

Adapted from Dr. Alvin Grissom's 2020 HCI course and Dr. Bill Mongan's 2024 HCI course.
