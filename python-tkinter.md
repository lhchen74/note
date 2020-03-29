---
title: python tkinter
tags: python
date: 2019-12-22
---

```python
from tkinter import *
from tkinter import messagebox
from tkinter import filedialog
import os
from tkinter.filedialog import askopenfilename, asksaveasfilename


filename = ''
root = Tk()


def author():
    messagebox.showinfo('Author', 'The software author is jbn')


def about():
    messagebox.showinfo('About', 'The software copyright is belongs to jbn')


def openfile():
    global filename
    filename = askopenfilename(defaultextension='.txt')
    if filename == '':
        filename = None
    else:
        root.title(os.path.basename(filename))
        content.delete(1.0, END)  # 1.0  1行0列: 文件开头
        with open(filename, 'r') as f:
            content.insert(1.0, f.read())


def newfile():
    global filename
    root.title('未命名文件')
    filename = None
    content.delete(1.0, END)


def saveasfile():
    saveasname = asksaveasfilename(
        initialfile='未命名.txt', defaultextension='.txt')
    global filename
    filename = saveasname
    with open(saveasname, 'w') as f:
        f.write(content.get(1.0, END))
    root.title(os.path.basename(saveasname))


def savefile():
    global filename
    try:
        with open(filename, 'w') as f:
            f.write(content.get(1.0, END))
    except:
        saveasfile()


def cut():
    content.event_generate('<<Cut>>')


def copy():
    content.event_generate('<<Copy>>')


def paste():
    content.event_generate('<<Paste>>')


def redo():
    content.event_generate('<<Redo>>')


def undo():
    content.event_generate('<<Undo>>')


def selectall():
    content.tag_add('sel', 1.0, END)


def search():

    def exsearch():
        pass

    topsearch = Toplevel(root)
    topsearch.geometry('300x30+200+250')
    label = Label(topsearch, text='Find')
    label.grid(row=0, column=0, padx=5)
    entry = Entry(topsearch, width=20)
    entry.grid(row=0, column=1, padx=5)
    btn = Button(topsearch, text='Search', command=exsearch)
    btn.grid(row=0, column=2)


root.title('Simple Note')

menubar = Menu(root)
root.config(menu=menubar)
root.geometry('500x500+100+100')  # W x H + X + Y

filemenu = Menu(menubar)
filemenu.add_command(label='新建', accelerator='Ctrl + N', command=newfile)
filemenu.add_command(label='打开', accelerator='Ctrl + O', command=openfile)
filemenu.add_command(label='保存', accelerator='Ctrl + S', command=savefile)
filemenu.add_command(
    label='另存为', accelerator='Ctrl + Shift + S', command=saveasfile)
menubar.add_cascade(label='文件', menu=filemenu)

editmenu = Menu(menubar)
editmenu.add_command(label='撤销', accelerator='Ctrl + Z', command=undo)
editmenu.add_command(label='重做', accelerator='Ctrl + Y', command=redo)
editmenu.add_separator()
editmenu.add_command(label='剪切', accelerator='Ctrl + X', command=cut)
editmenu.add_command(label='复制', accelerator='Ctrl + C', command=copy)
editmenu.add_command(label='粘贴', accelerator='Ctrl + V', command=paste)
editmenu.add_separator()
editmenu.add_command(label='查找', accelerator='Ctrl + F', command=search)
editmenu.add_command(label='全选', accelerator='Ctrl + A', command=selectall)
menubar.add_cascade(label='编辑', menu=editmenu)

aboutmenu = Menu(menubar)
aboutmenu.add_command(label='作者', command=author)
aboutmenu.add_command(label='版权', command=about)
menubar.add_cascade(label='关于', menu=aboutmenu)


toolbar = Frame(root, height=25, bg='light sea green')
openbtn = Button(toolbar, text='打开', command=openfile)
openbtn.pack(side=LEFT, padx=5, pady=5)
savebtn = Button(toolbar, text='保存', command=savefile)
savebtn.pack(side=LEFT)
toolbar.pack(fill=X)


status = Label(root, text='Ln20', bd=1, relief=SUNKEN, anchor=W)
status.pack(side=BOTTOM, fill=X)

# linenumber & content
lnlabel = Label(root, width=2, bg='antique white')
lnlabel.pack(side=LEFT, fill=Y)

content = Text(root, undo=True)
content.pack(expand=YES, fill=BOTH)

scroll = Scrollbar(content)
content.config(yscrollcommand=scroll.set)
scroll.config(command=content.yview)
scroll.pack(side=RIGHT, fill=Y)


root.mainloop()
```

