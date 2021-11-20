import tkinter as tk


# These packages is used to for giving command to functions
# PIL = Python imaging library
from PIL import Image
from PIL import ImageTk
from PIL import ImageEnhance # for brightness and contrast
from tkinter import filedialog
import random
import cv2
import numpy as np

def YellowButton_callback():
    # we have to open the image in open cv format for applying different types of colour filter
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) # this will covert the image in to array format
    opencvImage[:,:,0] = 20 # now array will be used to change filters
    # the image is in array form converting it in to RGB form
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def blueButton_callback():
    # we have to open the image in open cv format for applying different types of colour filter
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) # this will covert the image in to array format
    opencvImage[:,:,2] = 100 # now array will be used to change filters
    # the image is in array form converting it in to RGB form
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def pinkButton_callback():
    # we have to open the image in open cv format for applying different types of colour filter
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) # this will covert the image in to array format
    opencvImage[:,:,1] = 100 # now array will be used to change filters
    # the image is in array form converting it in to RGB form
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def orangeButton_callback():
    # we have to open the image in open cv format for applying different types of colour filter
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) # this will covert the image in to array format
    opencvImage[:,:,2] = 200 # now array will be used to change filters
    # the image is in array form converting it in to RGB form
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def greenButton_callback():
    # we have to open the image in open cv format for applying different types of colour filter
    opencvImage = cv2.cvtColor(np.array(orignalImage), cv2.COLOR_RGB2BGR) # this will covert the image in to array format
    opencvImage[:,:,1] = 200 # now array will be used to change filters
    # the image is in array form converting it in to RGB form
    global outputImage
    outputImage = Image.fromarray(cv2.cvtColor(opencvImage, cv2.COLOR_BGR2RGB))
    displayImage(outputImage)

def noneButton_callback():
    displayImage(orignalImage)


# to display our image to fit in window write this function
def  displayImage(displayImage):
    # instead of displaying the full image on GUI window we will resize the image then display it
    ImagetoDisplay = displayImage.resize((700,400), Image.ANTIALIAS)
    ImagetoDisplay = ImageTk.PhotoImage(ImagetoDisplay)# displayimage
    showWindow.config(image = ImagetoDisplay)# config window to load the image
    showWindow.photo_ref(image=ImagetoDisplay)# keep ref of image
    showWindow.pack() # pack the operations we performed

#command function to be implemented later on which is used here starting at line 27
def importButton_callback():
    global orignalImage
    filename = filedialog.askopenfilename()# open window to import image
    orignalImage = Image.open(filename)# This is used to load image to window
    # call the display image function to display on the window
    displayImage(orignalImage)

    # the function will not fit the image in full window

def saveButton_callback():
    savefile = filedialog.asksaveasfile(defaultextension = ".jpg") # asking the
    outputImage.save(savefile)

def closeButton_callback():
    window.destroy()

# we have to give argument which give the position of the slider
def brightness_callback(brightness_pos):
    brightness_pos = float(brightness_pos)
    print(brightness_pos)
    global outputImage
    enhancer = ImageEnhance.Brightness(orignalImage)
    outputImage = enhancer.enhance(brightness_pos)
    displayImage(outputImage)

def contrast_callback(contrast_pos):
    contrast_pos = float(contrast_pos)
    print(contrast_pos)
    global outputImage
    enhancer = ImageEnhance.Contrast(orignalImage)
    outputImage = enhancer.enhance(contrast_pos)
    displayImage(outputImage)


window = tk.Tk()
screen_width = window.winfo_screenwidth()  # for screen width
screen_height = window.winfo_screenheight()  # for screen height
window.configure(bg="powderblue")
window.geometry(f'{screen_width}x{screen_height}')  # for making screen of the above height and width

# create a list of different colors
colors = ["red","orange","yellow","green","indigo","hotpink"]
#color changer for font welcome
def color_changer():
   # choose and configure random color to the label text
   fg = random.choice(colors)
   fontlabel.config(fg=fg)

   # call the color_changer() method after 200 micro seconds
   fontlabel.after(200, color_changer)

   # create a list of different texts
   labels = ["Image Editor App"]
   # choose and configure random text to the label
   text = random.choice(labels)
   fontlabel.config(text=text)

fontlabel = tk.Label(window, font=('ariel', 45, 'bold'), bg="powderblue")
fontlabel.pack()
color_changer()

null_label=tk.Label(window,bg="powderblue")
null_label.pack()
# create a frame as widget to create buttons
Frame1 = tk.Frame(window, height=20, width=200)
Frame1.configure(bg="powderblue")
Frame1.pack(anchor=tk.NE)

Frame2 = tk.Frame(window, height=20,width=screen_width)
Frame2.configure(bg="powderblue")
Frame2.pack(anchor=tk.NW)

# adding radio buttons
Frame3 = tk.Frame(window, height=20)
Frame3.configure(bg="powderblue")
Frame3.pack(anchor=tk.N)

# create a import button
# to make the button big use the padding parameter
# command  is used by clicking on the radio buttons to take action of the corresponding command
importButton = tk.Button(Frame1, text="Import", padx=10,pady=5, command= importButton_callback)

# to place the button
importButton.grid(row=0, column=0)

# creating save button #placing save button
# to make the button big used the padding parameter
# for the command to be run when button will be pressed then use command=
saveButton = tk.Button(Frame1, text="Save", padx=10,pady=5, command = saveButton_callback)
saveButton.grid(row=0, column=1)

# creating close button #placing close button
# to make the button big use the padding parameter
# for the command to be run when button will be pressed then use command=

closeButton = tk.Button(Frame1, text="close", padx=10,pady=5, command=closeButton_callback)
closeButton.grid(row=0, column=2,)

null_btn=tk.Label(Frame1,padx=10,pady=5,bg="powderblue")
null_btn.grid(row=0,column=3)
sliderlabel=tk.Label(Frame2,text="Change Brightness and Contrast",bg="powderblue",fg="red",font=('airal',20,'italic bold'))
sliderlabel.pack()

#for slider we have a method called scale
# there must a min and max value for slider for this we use from_ and to command
# for orientation of slider we use 'orient' command and its length with length command
# resolution is also added for the taking the decimal point consideration
brightnessSlider = tk.Scale(Frame2, label="Brightness",fg="red",font="bold", from_=0, to=2, orient=tk.HORIZONTAL, length=screen_width, resolution=0.1, command=brightness_callback)
brightnessSlider.set(1)
brightnessSlider.pack(anchor=tk.N)# N is North

contrastSlider = tk.Scale(Frame2, label="Contrast",fg="red",font="bold", from_=0, to=2, orient=tk.HORIZONTAL, length=screen_width, command=contrast_callback, resolution=0.1)
contrastSlider.set(1)
contrastSlider.pack(anchor=tk.N)# N is North

null_label=tk.Label(Frame2,bg="powderblue")
null_label.pack()
#radio buttons of different colours
# with attribute is used to add spacing between radiobuttons
btnlabel=tk.Label(Frame2,text="Apply filter",fg="red",bg="powderblue",font=('airal',20,'italic bold'))
btnlabel.pack()
#creating yellow button
YellowButton = tk.Radiobutton(Frame3,fg="red",font="bold",bg="yellow", text="Yellow", width=30, value=1, command=YellowButton_callback)
YellowButton.grid(row=1, column=0)

# creating blue button
blueButton = tk.Radiobutton(Frame3,fg="red",font="bold",bg="blue", text="Blue", width=30,value=2, command=blueButton_callback)
blueButton.grid(row=1, column=1)

# creating pink button
pinkButton = tk.Radiobutton(Frame3,fg="red",font="bold", text="Pink",bg="pink", width=30, value=3, command=pinkButton_callback)
pinkButton.grid(row=1, column=2)

# creating orange button
orangeButton = tk.Radiobutton(Frame3,fg="red",font="bold", text="Orange",bg="orange", width=30, value=4, command=orangeButton_callback)
orangeButton.grid(row=1, column=3)

#greenbtn
greenButton = tk.Radiobutton(Frame3,fg="red",font="bold", text="green",bg="lightgreen", width=30, value=4, command=greenButton_callback)
greenButton.grid(row=1, column=4)
# to create no filter button
noneButton = tk.Radiobutton(Frame3,fg="red",font="bold", text="None", width=30,bg="lightgrey", value=5, command=noneButton_callback)
noneButton.grid(row=1, column=5)
noneButton.select() # this line will by default selects none button

showWindow = tk.Label(window,bg="powderblue")
showWindow.pack()
null_label=tk.Label(Frame3,bg="powderblue")
null_label.grid(row=2)
tk.mainloop()
