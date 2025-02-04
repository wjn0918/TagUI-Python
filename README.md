<img src="https://raw.githubusercontent.com/tebelorg/Tump/master/TagUI-Python/tebel_logo.png" height="111" align="right">

# TagUI for Python

[**Use Cases**](#use-cases)&ensp;|&ensp;[**API Reference**](#api-reference)&ensp;|&ensp;[**About & Credits**](#about--credits)&ensp;|&ensp;[**Video #PyCon**](https://www.youtube.com/watch?v=F2aQKWx_EAE)&ensp;|&ensp;[**v1.17**](https://github.com/tebelorg/TagUI-Python/releases)

![TagUI for Python demo in Jupyter notebook](https://raw.githubusercontent.com/tebelorg/Tump/master/tagui_python.gif)

To install this Python package for digital process automation (also known as RPA) -
```
pip install tagui
```

To use TagUI for Python in Jupyter notebook, Python script or interactive shell -
```python
import tagui as t
```

To deploy in environments without internet, let me know your constraints here -
```py
t.init(); t.url('https://github.com/tebelorg/TagUI-Python/issues/36')
```

Notes on different operating systems and optional visual automation mode -
- **Windows users -** if visual automation is cranky, try setting your display zoom level to recommended % or 100%
- **macOS users -** Catalina update introduces tighter app security, see solutions for [PhantomJS](https://github.com/tebelorg/TagUI-Python/issues/79) and [Java popups](https://github.com/tebelorg/TagUI-Python/issues/78)
- **Linux users -** visual automation mode requires special setup on Linux, see how to [install OpenCV and Tesseract](https://sikulix-2014.readthedocs.io/en/latest/newslinux.html)

# Use Cases

TagUI for Python's simple, expressive and powerful API makes digital process automation fun and easy!

#### WEB AUTOMATION
```python
t.init()
t.url('https://www.google.com')
t.type('q', 'decentralization[enter]')
t.snap('page', 'results.png')
print(t.read('resultStats'))
t.close()
```

#### VISUAL AUTOMATION
```python
t.init(visual_automation = True)
t.dclick('outlook_icon.png')
t.click('new_mail.png')
...
t.type('message_box.png', 'message')
t.click('send_button.png')
t.close()
```

#### OCR AUTOMATION
```python
t.init(visual_automation = True)
print(t.read('pdf_window.png'))
print(t.read('image_preview.png'))
t.hover('anchor_element.png')
print(t.read(t.mouse_x(), t.mouse_y(), t.mouse_x() + 400, t.mouse_y() + 200))
t.close()
```

#### KEYBOARD AUTOMATION
```python
t.init(visual_automation = True, chrome_browser = False)
t.keyboard('[cmd][space]')
t.keyboard('safari[enter]')
t.keyboard('[cmd]t')
t.keyboard('joker[enter]')
t.wait(2.5)
t.snap('page.png', 'results.png')
t.close()
```

#### MOUSE AUTOMATION
```python
t.init(visual_automation = True)
t.type(600, 300, 'open source')
t.click(900, 300)
t.snap('page.bmp', 'results.bmp')
t.hover('button_to_drag.bmp')
t.mouse('down')
t.hover(t.mouse_x() + 300, t.mouse_y())
t.mouse('up')
t.close()
```

# API Reference

Check out [sample Python script](https://github.com/tebelorg/TagUI-Python/blob/master/sample.py) and [RedMart groceries example](https://github.com/tebelorg/TagUI-Python/issues/24).

- For web automation, web element identifier can be XPath selector, CSS selector or the following attributes - id, name, class, title, aria-label, text(), href (in decreasing order of priority). There is automatic waiting for an element to appear before timeout happens, and error is returned that the element cannot be found. To change the default timeout of 10 seconds, use timeout() function.

- An element identifier can also be a .png or .bmp image snapshot representing the UI element (can be on desktop applications or web browser). x, y coordinates of elements on the screen work as well. Transparency (0% opacity) is supported in .png images, for eg using an image of an UI element with transparent background to enable clicking on an UI element that appears on different backgrounds on different occasions.

>A further image identifier example is an image of the window (PDF viewer, MS Word, textbox etc) with the center content of the image set as transparent. This allows using read() and snap() to perform OCR and save snapshots of application windows, containers, frames, textboxes with varying content. Also for read() and snap(), x1, y1, x2, y2 coordinates pair can be used to define the region of interest on the screen to perform OCR or capture snapshot.

#### CORE FUNCTIONS
Function|Parameters|Purpose
:-------|:---------|:------
init()|visual_automation = False, chrome_browser = True|start TagUI, auto-call setup() on first run
close()||close TagUI, Chrome browser, SikuliX
setup()||setup TagUI to user home folder
pack()|(beta feature - [let me know your constraints here](https://github.com/tebelorg/TagUI-Python/issues/36#issuecomment-543670292))|for deployment without internet

#### DEBUG FUNCTIONS
Function|Parameters|Purpose
:-------|:---------|:------
debug()|True or False|toggle debug mode, ie print and log debug info
send()|tagui_instruction|send TagUI instruction to TagUI for execution

#### BASIC FUNCTIONS
Function|Parameters|Purpose
:-------|:---------|:------
url()|webpage_url (no parameter to return current URL)|go to web URL
click()|element_identifier (or x, y using visual automation)| left-click on element
rclick()|element_identifier (or x, y using visual automation)|right-click on element
dclick()|element_identifier (or x, y using visual automation)|double-click on element
hover()|element_identifier (or x, y using visual automation)|move mouse to element
type()|element_identifier (or x, y), text_to_type ('[enter]', '[clear]')|enter text at element
select()|element_identifier (or x, y), option_value (or x, y)|choose dropdown option
read()|element_identifier (page = web page) (or x1, y1, x2, y2)|fetch & return element text
snap()|element_identifier (page = web page), filename_to_save|save screenshot to file
load()|filename_to_load|load & return file content
dump()|text_to_dump, filename_to_save|save text to file
write()|text_to_write, filename_to_save|append text to file
echo()|text_to_echo|print text to screen
ask()|text_to_prompt|ask & return user input

#### PRO FUNCTIONS
Function|Parameters|Purpose
:-------|:---------|:------
keyboard()|keys_and_modifiers (using visual automation)|send keystrokes to screen
mouse()|'down' or 'up' (using visual automation)|send mouse event to screen
wait()|delay_in_seconds (default 5 seconds)|explicitly wait for some time
check()|condition_to_check, text_if_true, text_if_false|check condition & print result
table()|element_identifier (XPath only), filename_to_save|save basic HTML table to CSV
upload()|element_identifier (CSS only), filename_to_upload|upload file to web element
download()|download_url, filename_to_save(optional)|download from URL to file
unzip()|file_to_unzip, unzip_location (optional)|unzip zip file to specified location
frame()|main_frame id or name, sub_frame (optional)|set web frame, frame() to reset
popup()|string_in_url (no parameter to reset to main page)|set context to web popup tab
run()|command_to_run (use ; between commands)|run OS command & return output
dom()|statement_to_run (JS code to run in browser)|run code in DOM & return output
vision()|command_to_run (Python code for SikuliX)|run custom SikuliX commands
timeout()|timeout_in_seconds (blank returns current timeout)|change wait timeout (default 10s)

**keyboard() - modifiers and special keys**
>[shift] [ctrl] [alt] [cmd] [win] [meta] [clear] [space] [enter] [backspace] [tab] [esc] [up] [down] [left] [right] [pageup] [pagedown] [delete] [home] [end] [insert] [f1] .. [f15] [printscreen] [scrolllock] [pause] [capslock] [numlock]

#### HELPER FUNCTIONS
Function|Parameters|Purpose
:-------|:---------|:------
exist()|element_identifier|return True or False if element exists before timeout
present()|element_identifier|return True or False if element is present now
count()|element_identifier|return number of web elements as integer
clipboard()|text_to_put or no parameter|put text or return clipboard text as string
mouse_xy()||return '(x,y)' coordinates of mouse as string
mouse_x()||return x coordinate of mouse as integer
mouse_y()||return y coordinate of mouse as integer
title()||return page title of current web page as string
text()||return text content of current web page as string
timer()||return time elapsed in sec between calls as float

# About & Credits

TagUI is the number #1 open-source RPA software with thousands of active users. It was created in 2016-2017 when I left DBS Bank as a test automation engineer, to embark on a one-year sabbatical to Eastern Europe. Most of its code base was written in Novi Sad Serbia. My wife and I also spent a couple of months in Budapest Hungary, as well as Chiang Mai Thailand for visa runs. In 2018, I joined AI Singapore to continue development of TagUI.

Over the past 2 months I take on a daddy role full-time, taking care of my newborn baby girl and wife. In between the nannying and caregiving, I use my time pockets to create this Python package for TagUI. I hope that TagUI for Python and ML frameworks would be good friends, and `pip install tagui` would make lives easier for Python users.

Lastly, at only ~1k lines of code, it would make my day to see developers of other languages porting this project over to their favourite coding language. See the ample comments in this [single-file package](https://github.com/tebelorg/TagUI-Python/blob/master/tagui.py), and its intuitive architecture -

![TagUI for Python architecture](https://raw.githubusercontent.com/tebelorg/Tump/master/TagUI-Python/architecture.png)

I would like to credit and express my appreciation below, and you are invited to [connect on LinkedIn](https://www.linkedin.com/in/kensoh) -

- [TagUI](https://github.com/kelaberetiv/TagUI) - AI Singapore from Singapore / [@aisingapore](https://www.aisingapore.org)
- [SikuliX](http://sikulix.com) - Raimund Hocke from Germany / [@RaiMan](https://github.com/RaiMan)
- [CasperJS](http://casperjs.org) - Nicolas Perriault from France / [@n1k0](https://github.com/n1k0)
- [PhantomJS](http://phantomjs.org) - Ariya Hidayat from Indonesia / [@ariya](https://github.com/ariya)
- [SlimerJS](https://slimerjs.org) - Laurent Jouanneau from France / [@laurentj](https://github.com/laurentj)

# License
TagUI for Python is open-source software released under Apache 2.0 license
