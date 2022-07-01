# TextEditor-Project
import tkinter as tk
from tkinter import  Scrollbar, Variable, ttk
from tkinter import font,colorchooser,filedialog,messagebox
from tkinter.constants import COMMAND, LEFT
import os
from typing import Match, Text


main_application=tk.Tk()
main_application.geometry("800x600")
main_application.title("Text Editor")
main_menu=tk.Menu()

new_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\snew.gif")
open_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sopen.gif")
save_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\save.gif")
saveas_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\saveas.gif")
exit_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\exit.gif")

copy_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\copy.gif")
paste_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\paste.gif")
cut_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\cut.gif")
clear_all_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\clear-all.gif")
find_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sfind.gif")


file=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="File",menu=file)

#file Menu
text_url=" "
def new_file(event=None):
  global text_url
  text_url=" "
  text_editor.delete(1.0,tk.END)
file.add_command(label="New",image=new_icon,compound=tk.LEFT,accelerator="Ctrl+N",command=new_file)


def open_file(event=None):
    global text_url
    text_url=filedialog.askopenfilename(initialdir=os.getcwd(),title="select file",filetypes=(("Text file","*.txt"),("All files","*.*")))
    try:
      with open(text_url,"r") as for_red:
        text_editor.delete(1.0,tk.END)
        text_editor.insert(1.0,for_read.read())
    except FileNotFoundError:
      return
    except:
      return
    main_application.title(os.path.basename(text_url))


#file.add_command(label="New",image=new_icon,compound=tk.LEFT,accelerator="Ctrl+N")
file.add_command(label="Open",image=open_icon,compound=tk.LEFT,accelerator="Ctrl+O",command=open_file)

def save_file(event=None):
  global text_url
  try:
    if text_url:
      content=str(text_editor.get(1.0,tk.END))
      with open(text_url,"w",encoding="utf-8") as for_read:
        for_read.write(content)
    else:
      text_url=filedialog.asksaveasfile(mode="w",defaultextension="txt",filetypes=(("Text file","*.txt"),("All files","*.*")))
      content2=text_editor.get(1.0,tk.END)
      text_url.write(content2)
      text_url.close()
  except:
    return
file.add_command(label="Save",image=save_icon,compound=tk.LEFT,accelerator="Ctrl+S",command=save_file)

def Save_as_file(event=None):
  global text_url
  try:
      content=text_editor.get(1.0,tk.END)
      text_url=filedialog.asksaveasfile(mode="w",defaultextension="txt",filetypes=(("Text file","*.txt"),("All files","*.*")))
      text_url.write(content)
      text_url.close()
  except:
    return
file.add_command(label="Save_as",image=saveas_icon,compound=tk.LEFT,accelerator="Ctrl+Shift+S",command=Save_as_file)

def Exit_fun(event=None):
  global text_url,text_change
  try:
    if text_change:
      mbox=messagebox.askyesnocancel("warning","Do you want to save this file")
      if mbox is True:
        if text_url:
          content=text_editor.get(1.0,tk.END)
          with open(text_url,"w",encoding="utf-8") as for_read:
            for_read.write(content)
            main_application.destroy()
        else:
          content2=str(text_editor.get(1.0,tk.END))
          text_url=filedialog.asksaveasfile(mode="w",defaultextension="txt",filetypes=(("Text file","*.txt"),("All files","*.*")))
          text_url.write(content2)
          text_url.close()
          main_application.destroy()
      elif mbox is False:
        main_application.destroy()
    else:
      main_application.destroy()
  except:
    return
file.add_command(label="Exit",image=exit_icon,compound=tk.LEFT,accelerator="Ctrl+",command=Exit_fun)


main_application.config(menu=main_menu)

#Edit Menu
Edit=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Edit",menu=Edit)
Edit.add_command(label="Copy",image=copy_icon,compound=tk.LEFT,accelerator="Ctrl+C",command=lambda:text_editor.event_generate("<Control c>"))
Edit.add_command(label="Paste",image=paste_icon,compound=tk.LEFT,accelerator="Ctrl+V",command=lambda:text_editor.event_generate("<Control c>"))
Edit.add_command(label="Cut",image=cut_icon,compound=tk.LEFT,accelerator="Ctrl+X",command=lambda:text_editor.event_generate("<Control c>"))
Edit.add_command(label="Clear all",image=clear_all_icon,compound=tk.LEFT,accelerator="Ctrl+Alt+X",command=lambda:text_editor.delete(1.0,tk.END))
#define function

def find_fun(event=None):
  def find():
    word=find_input.get()
    text_editor.tag_remove("match","1.0",tk.END)
    matches=0
    if word:
      start_pos="1.0"
      while True:
        start_pos=text_editor.search(word,start_pos,stopindex=tk.END)
        if not start_pos:
          break
        end_pos=f"{start_pos}+{len(word)}c"
        text_editor.tag_add("match",start_pos,end_pos)
        matches+=1
        start_pos=end_pos
        text_editor.tag_config('match',foreground="red",background="blue")

  def replace():
    word=find_input.get()
    replace_text=replace_input.get()
    content=text_editor.get(1.0,tk.END)
    new_content=content.replace(word,replace_text)
    text_editor.delete(1.0,tk.END)
    text_editor.insert(1.0,new_content)

  find_popup=tk.Toplevel()
  find_popup.geometry("450x200")
  find_popup.title("find word")
  find_popup.resizable(0,0)

  #frame for find
  find_fram=ttk.LabelFrame(find_popup,text="Find and replace word")
  find_fram.pack(pady=20)

  #label
  text_find=ttk.Label(find_fram,text="Find")
  text_replace=ttk.Label(find_fram,text="Replace")

  #entry box
  find_input=ttk.Entry(find_fram,width=30)
  replace_input=ttk.Entry(find_fram,width=30)
  #botton
  find_button=ttk.Button(find_fram,text="find",command=find)
  replace_button=ttk.Button(find_fram,text="replace",command=replace)
  #text label gride
  text_find.grid(row=0,column=0,padx=4,pady=4)
  text_replace.grid(row=0,column=1,padx=4,pady=4)
  #entry gride
  find_input.grid(row=0,column=0,padx=4,pady=4)
  replace_input.grid(row=0,column=1,padx=4,pady=4)
  #button grid
  find_button.grid(row=1,column=0,padx=8,pady=4)
  replace_button.grid(row=1,column=1,padx=8,pady=4)
Edit.add_command(label="Find",image=find_icon,compound=tk.LEFT,accelerator="Ctrl+F",command=find_fun)
main_application.config(menu=main_menu)


#status bar=ttk.label(main_application,text="status bar")
#status bar.pack(side=tk.BUTTOM)

show_status_bar=tk.BooleanVar()
show_status_bar.set(True)
show_toolbar=tk.BooleanVar()
show_toolbar.set(True)

def hide_toolbar():
  global show_toolbar
  if show_toolbar:
    tool_bar_label.pack_forget()
    show_toolbar=False
  else:
    text_editor.pack_forget()
    status_bar.pack_forget()
    tool_bar_label.pack(side=tk.TOP,fill=tk.X)
    text_editor.pack(fill=tk.BOTH,expand=True)
    status_bar.pack(side=tk.BOTTOM)
    show_toolbar=True
def hide_statusbar():
  global show_status_bar
  if show_status_bar:
    status_bar.pack_forget()
    show_status_bar=False
  else:
    status_bar.pack(side=tk.BOTTOM)
    show_status_bar=True


View=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="View",menu=View)
View.add_checkbutton(label="Toolbar",onvalue=True,offvalue=0,variable=show_toolbar,compound=LEFT,command=hide_toolbar)
View.add_checkbutton(label="statusBar",onvalue=True,offvalue=0,variable=show_toolbar, compound=tk.LEFT,command=hide_statusbar)
#View.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
#View.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
#View.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)

tool_bar_label=ttk.Label(main_application)
tool_bar_label.pack(side=tk.TOP,fill=tk.X)
font_tuple=tk.font.families()
font_family=tk.StringVar()
font_box=ttk.Combobox(tool_bar_label,width=30,textvariable=font_family,state="readonly")
font_box["values"]=font_tuple
font_box.current(font_tuple.index("Arial"))
font_box.grid(row=0,column=0,padx=5)


size_variable=tk.IntVar()
font_size=ttk.Combobox(tool_bar_label,width=20,textvariable=size_variable,state="readonly")
font_size["values"]=tuple(range(8,100,2))
font_size.current(4)
font_size.grid(row=0,column=1,padx=5)


bold_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sbold.gif")
bold_btn=ttk.Button(tool_bar_label,image=bold_icon)
bold_btn.grid(row=0,column=2,padx=5)


italic_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sitalic.gif")
italic_btn=ttk.Button(tool_bar_label,image=italic_icon)
italic_btn.grid(row=0,column=3,padx=5)


underline_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sunderline.gif")
underline_btn=ttk.Button(tool_bar_label,image=underline_icon)
underline_btn.grid(row=0,column=4,padx=5)


font_color_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\sf1.gif")
font_color_btn=ttk.Button(tool_bar_label,image=font_color_icon)
font_color_btn.grid(row=0,column=5,padx=5)


aline_left_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\slefta.gif")
aline_left_btn=ttk.Button(tool_bar_label,image=aline_left_icon)
aline_left_btn.grid(row=0,column=6,padx=5)


aline_center_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\slefta.gif")
aline_center_btn=ttk.Button(tool_bar_label,image=aline_center_icon)
aline_center_btn.grid(row=0,column=8,padx=5)

aline_right_icon=tk.PhotoImage(file="C:\HTML PROJECT\list of text edditor image\srighta.gif")
aline_right_btn=ttk.Button(tool_bar_label,image=aline_right_icon)
aline_right_btn.grid(row=0,column=7,padx=5)


#text editor
text_editor=tk.Text(main_application)
text_editor.config(wrap="word",relief=tk.FLAT)

scroll_bar=tk.Scrollbar(main_application)
text_editor.focus_set()
scroll_bar.pack(side=tk.RIGHT,fill=tk.Y)
text_editor.pack(fill=tk.BOTH,expand=True)
scroll_bar.config(command=text_editor.yview)
text_editor.config(yscrollcommand=scroll_bar.set)

#status bar


status_bar=ttk.Label(main_application,text="status_bar")
status_bar.pack(side=tk.BOTTOM)
text_change=False
def change_word(event=None):
  global text_change
  if text_editor.edit_modified():
    text_change=True
    word=len(text_editor.get(1.0,"end-1c").split())
    charecter=len(text_editor.get(1.0,'end-1c').replace(" ",""))
    status_bar.config(text=f"charecter:{charecter},word:{word}")
  text_editor.edit_modified(False)
text_editor.bind("<<Modified>>",change_word)

#font family and function
font_now="Arial"
font_now=16
def change_font(main_application):
  global font_now
  font_now=font_family.get()
  text_editor.configure(font=(font_now,font_size_now))
def change_size(main_application):
  global font_size_now
  font_size_now=size_variable.get()
  text_editor.configure(font=(font_now,font_size_now))

font_box.bind("<<ComboboxSelected>>",change_font)
font_size.bind("<<ComboboxSelected>>",change_size)

#Bold function
#print(tk.font.Font(font=text_editor["font"]).actual())
def bold_fun():
  text_get=tk.font.Font(font=text_editor["font"])
  if text_get.actual()["weight"]=='normal':
    text_editor.configure(font=(font_now,font_size_now,"bold"))
  if text_get.actual()["weight"]=='bold':
    text_editor.configure(font=(font_now,font_size_now,"normal"))
bold_btn.configure(command=bold_fun)        
           
def Italic_fun():
  text_get=tk.font.Font(font=text_editor["font"])
  if text_get.actual()["slant"]=='roman':
    text_editor.configure(font=(font_now,font_size_now,"italic"))
  if text_get.actual()["slant"]=="italic":
    text_editor.configure(font=(font_now,font_size_now,'roman'))
italic_btn.configure(command=Italic_fun)        

def under_line_fun():
  text_get=tk.font.Font(font=text_editor["font"])
  if text_get.actual()["underline"]==0:
    text_editor.configure(font=(font_now,font_size_now,"underline"))
  if text_get.actual()["underline"]==1:
    text_editor.configure(font=(font_now,font_size_now,"normal"))
#underline_btn.configure(command=under_line_fun_)        

def Color_choose():
  color_var=tk.colorchooser.askcolor()
  text_editor.configure(fg=color_var[1])
font_color_btn.configure(command=Color_choose)

def align_left():
  text_get_all=text_editor.get(1.0,"end")
  text_editor.tag_config("left",justify=tk.LEFT)
  text_editor.delete(1.0,tk.END)
  text_editor.insert(tk.INSERT,text_get_all,"left")
aline_left_btn.configure(command=align_left)

def align_right():
  text_get_all=text_editor.get(1.0,"end")
  text_editor.tag_config("right",justify=tk.RIGHT)
  text_editor.delete(1.0,tk.END)
  text_editor.insert(tk.INSERT,text_get_all,"right")
aline_right_btn.configure(command=align_right)

def align_center():
  text_get_all=text_editor.get(1.0,"center")
  text_editor.tag_config("left",justify=tk.CENTER)
  text_editor.delete(1.0,tk.END)
  text_editor.insert(tk.INSERT,text_get_all,"center")
aline_center_btn.configure(command=align_left)

#file Menu
"""text_url=" "
def new_file(main_application):
  global text_url
  text_url=" "
  text_editor.delete(1.0,tk.END)
  file.add_command(label="New",image=new_icon,command=tk.LEFT,accelerator="Ctrl+N",command=new_file)

  def open_file(main_application):
    global text_url
    text_url=filedialog.askopenfilename(initialdir=os.getcwd(),title="select file",filetypes=(("Text file","*.txt"),("All files","*.")))
    try:
      with open(text_url,"r") as for_red:
        text_editor.delete(1.0,tk.END)
        text_editor.insert(1.0,for_red())
    except: FileNotFoundError:
      return
    except:
      return
    main_application.title(os.path.basename(text_url))"""


main_application.mainloop()





"""
#color image

#light_theme=tk.PhotoImage()
#light_plus_icon=tk=tk.PhotoImage()
#dark_theme=tk.PhotoImage()
#red_theme=tk.PhotoImage()
#monokia_theme=tk.PhotoImage()
#night_theme=tk.PhotoImage()
#main_application.config(menu=main_menu)

color_theme=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Color Theme",menu=color_theme)

#color theme set

light_theme=tk.PhotoImage()
light_plus_icon=tk=tk.PhotoImage()
#dark_theme=tk.PhotoImage()
#red_theme=tk.PhotoImage()
#monokia_theme=tk.PhotoImage()
#night_theme=tk.PhotoImage()
#color_icons=(light_plus_icon,light_theme,dark_theme,red_theme,monokia_theme,night_theme)
main_application.config(menu=main_menu)
#color theme set

color_dict={

             'Light Default':('#000000',"#ffffff")
             #'Light Plus':('#474747','#e0e0e0)
             
             #'Dark':('#c4c4c4','#2d2d2d'),
             #'Red':('#d3b774','#ffe8e8'),
             #'Monokia':('3d3b774','#474747'),
             #'Night Blue':('#ededed','#6b9dc2')

}

#count=0
#for i in color_dict:
 #   color_theme.add_radiobutton(label=i,image=color_icons[count],compound=tk.LEFT)
  #  count +=1
#main_application.config(menu=main_menu)
main_application.mainloop()"""

"""Go=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Go",menu=Go)
Go.add_command(label="Copy",compound=tk.LEFT,accelerator="Ctrl+C")
Go.add_command(label="Paste",compound=tk.LEFT,accelerator="Ctrl+V")
Go.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
Go.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
Go.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)"""


"""Run=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Run",menu=Run)
Run.add_command(label="Copy",compound=tk.LEFT,accelerator="Ctrl+C")
Run.add_command(label="Paste",compound=tk.LEFT,accelerator="Ctrl+V")
Run.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
Run.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
Run.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)"""


"""Terminal=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Terminal",menu=Terminal)
Terminal.add_command(label="Copy",compound=tk.LEFT,accelerator="Ctrl+C")
Terminal.add_command(label="Paste",compound=tk.LEFT,accelerator="Ctrl+V")
Terminal.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
Terminal.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
Terminal.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)"""


"""Help=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Help",menu=Help)
Help.add_command(label="Copy",compound=tk.LEFT,accelerator="Ctrl+C")
Help.add_command(label="Paste",compound=tk.LEFT,accelerator="Ctrl+V")
Help.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
Help.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
Help.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)"""
"""Selection=tk.Menu(main_menu,tearoff=False)
main_menu.add_cascade(label="Selection",menu=Selection)
Selection.add_command(label="Copy",compound=tk.LEFT,accelerator="Ctrl+")
Selection.add_command(label="Paste",compound=tk.LEFT,accelerator="Ctrl+V")
Selection.add_command(label="Cut",compound=tk.LEFT,accelerator="Ctrl+X")
Selection.add_command(label="Clear all",compound=tk.LEFT,accelerator="Ctrl+Alt+X")
Selection.add_command(label="Find",compound=tk.LEFT,accelerator="Ctrl+F")
main_application.config(menu=main_menu)
clear
 main_application.mainloop()"""

