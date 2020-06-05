# code for generating morse code with LED using Tkinter on RPi.

    from tkinter import *
    import tkinter.font
    from gpiozero import LED
    import RPi.GPIO as GPIO
    import time

    GPIO.setmode(GPIO.BCM)

    led = LED(12)
    
    #create a GUI window
    gui = Tk()
    gui.title("MORSE CODE ENCRYPTER")
    font = tkinter.font.Font(family = 'Athelas', size = 14, weight = 'bold')

    morseCodeTranslations = { 'A':'.-', 'B':'-...', 'C':'-.-.', 'D':'-..', 'E':'.','F':'..-.', 'G':'--.', 'H':'....', 
                            'I':'..', 'J':'.---', 'K':'-.-','L':'.-..', 'M':'--', 'N':'-.', 'O':'---', 'P':'.--.', 
                            'Q':'--.-', 'R':'.-.', 'S':'...', 'T':'-', 'U':'..-', 'V':'...-', 'W':'.--', 'X':'-..-',
                            'Y':'-.--', 'Z':'--..', '1':'.----', '2':'..---', '3':'...--', '4':'....-', '5':'.....', 
                            '6':'-....', '7':'--...', '8':'---..', '9':'----.', '0':'-----', ', ':'--..--', '.':'.-.-.-', 
                            '?':'..--..', '/':'-..-.', '-':'-....-', '(':'-.--.', ')':'-.--.-'} 

    #function to convert letter in input text to morse code
    def cipher(text): 
        code = '' 
        for letter in text: 
            if letter != ' ':
                code += morseCodeTranslations[letter] + ' '
            else:
                code += ' '
        return code 

    #function to blink letter according to dots, dashes and spaces
    def blink(text):
        for letter in text: 
            if (letter == '-'):
                led.on()
                time.sleep(0.6)
                led.off()
                time.sleep(0.2)
            elif (letter == '.'):
                led.on()
                time.sleep(0.2)
                led.off()
                time.sleep(0.2)
            else:
                time.sleep(0.5)
        return
        
    # function to retrieve input text and encrypt it
    def convertToMorse(): 
        cipherText = cipher(text.get().upper())
        blink(cipherText)
        return
        
    # function to close the window
    def exitWindow():
        GPIO.cleanup()
        gui.destroy()
        return

    label = Label(gui, text = "Enter Message:", font = font, height = 1, width=30)
    label.grid(row = 0, column = 1)
    
    #entry box widget used to allow user to input text
    text = StringVar()
    inputBox = Entry(gui, textvariable = text, font = font, width=20, bg = 'pink')
    inputBox.grid(row = 1, column = 1)
    
    #Button widget for converter; when clicked conversion begins with the input text as the subject
    start = Button(gui, text="Generate Morse Code",font = font, command = convertToMorse, height=2, width=18, bg = "lightblue")
    start.grid(row = 2, column = 1)

    #Button widget to close the window
    exitBtn = Button(gui, text = 'EXIT', font = font, command = exitWindow, height = 1, width = 5, bg = 'grey')
    exitBtn.grid(row = 3, column = 1)
