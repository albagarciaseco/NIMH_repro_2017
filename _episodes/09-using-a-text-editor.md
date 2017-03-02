---
title: "Working with the command history and script-files"
teaching: 15
exercises: 15
questions:
- "How can I save and edit my previous input commands conveniently?"
objectives:
- "Work with the IPython magic commands `history`, `cat`, `save`, and `run`."
- "Use the Atom text editor."
- "Understand how to run commands and change the settings in Atom."
- "Learn how to use Atom and IPython together."
keypoints:
- "We can explore our input history and save our code to text files. For python
files we use '.py'."
- "We can easily open these files in an editor."
- "Atom is open source"
- "A GUI is convenient. Using the keyboard is faster."
- "Start learning commands using the command palette."
- "Settings can be changed in two different ways."
- "Additional functionality can be added to Atom."
---

## Working with the IPython history
IPython is very convenient for interactive work. It provides ways to build up
reproducible scripts as we go too. The IPython `history`  magic allows us to
view the previous commands that we have run in IPython.

We can view at our input history for this session:
~~~
%history
~~~
{: .python}

It can be more convenient to see the commands associated with the input number
that they are associated with:
~~~
%history -n
~~~
{: .python}

We can also search our history for patterns of text using the `-g` flag. By
default this searches all previous IPython sessions. Let's search for all
previous commands that contain the pattern `roi`:
~~~
%history -n -g roi
~~~
{: .python}


Instead of searching for a pattern we can also retrieve our history by
specifying the session and line numbers (patterns and line numbers don't play 
well together). For the first 10 commands of this session type the following:
~~~
%history -n 1-10
~~~
{: .python}

We can also retrieve history of previous times we have used iPython. To return the history from the first 10 commands that were executed 4 sessions ago:
~~~
%history -n ~4/1-10
~~~
{: .python}


To return the history from the first line of 4 sessions ago to the hundredth
line of this session:
~~~
%history -n ~4/1-~0/100
~~~
{: .python}

Remember to use help if you forget the syntax:
~~~
%history?
~~~
{: .python}


~~~
Docstring:
::

  %history [-n] [-o] [-p] [-t] [-f FILENAME] [-g [PATTERN [PATTERN ...]]]
               [-l [LIMIT]] [-u]
               [range [range ...]]

Print input history (_i<n> variables), with most recent last.

By default, input history is printed without line numbers so it can be
directly pasted into an editor. Use -n to show them.

By default, all input history from the current session is displayed.
Ranges of history can be indicated using the syntax:

``4``
    Line 4, current session
``4-6``
    Lines 4-6, current session
``243/1-5``
    Lines 1-5, session 243
``~2/7``
    Line 7, session 2 before current
``~8/1-~6/5``
    From the first line of 8 sessions ago, to the fifth line of 6
    sessions ago.

Multiple ranges can be entered, separated by spaces

The same syntax is used by %macro, %save, %edit, %rerun

Examples
--------
::

  In [6]: %history -n 4-6
  4:a = 12
  5:print a**2
  6:%history -n 4-6

positional arguments:
  range

optional arguments:
  -n                    print line numbers for each input. This feature is
                        only available if numbered prompts are in use.
  -o                    also print outputs for each input.
  -p                    print classic '>>>' python prompts before each input.
                        This is useful for making documentation, and in
                        conjunction with -o, for producing doctest-ready
                        output.
  -t                    print the 'translated' history, as IPython understands
                        it. IPython filters your input and converts it all
                        into valid Python source before executing it (things
                        like magics or aliases are turned into function calls,
                        for example). With this option, you'll see the native
                        history instead of the user-entered version: '%cd /'
                        will be seen as 'get_ipython().magic("%cd /")' instead
                        of '%cd /'.
  -f FILENAME           FILENAME: instead of printing the output to the
                        screen, redirect it to the given file. The file is
                        always overwritten, though *when it can*, IPython asks
                        for confirmation first. In particular, running the
                        command 'history -f FILENAME' from the IPython
                        Notebook interface will replace FILENAME even if it
                        already exists *without* confirmation.
  -g <[PATTERN [PATTERN ...]]>
                        treat the arg as a glob pattern to search for in
                        (full) history. This includes the saved history
                        (almost all commands ever written). The pattern may
                        contain '?' to match one unknown character and '*' to
                        match any number of unknown characters. Use '%hist -g'
                        to show full saved history (may be very long).
  -l <[LIMIT]>          get the last n lines from all sessions. Specify n as a
                        single arg, or the default is the last 10 lines.
  -u                    when searching history using `-g`, show only unique
                        history.
~~~
{: .output}

## Saving commands to a script file

*   If we want to save some of these commands to keep track of our analysis for
use in the future we can write them to a python script. We can use the IPython
magic `%save` to do this.
*   In order to identify python scripts we use the '.py' extension.
* Save uses the same syntax as history for referring to previous lines.

Let's save the command in which we define the roi_volumes list by specifying
the line-number of the input command. We need to specify a filename as part of
the command:
~~~
%save metasearch_analysis.py N # enter a line number you want to save after the
filename
~~~
{: .python}
~~~
The following commands were written to file `metasearch_analysis.py`:
roi_volumes = [2.73,145.3,12.7,16.2, 27.6]
~~~
{: .output}

We can confirm this by using the %cat ipython magic to view the contents of the file.
~~~
%cat metasearch_analysis.py 
~~~
{: .python}
~~~
roi_volumes = [2.73,145.3,12.7,16.2, 27.6]
~~~
{: .output}

We should remember that the list roi_volumes has changed since it's definition:
~~~
print(roi_volumes)
~~~
{: .python}
~~~
[26.5,145.3,12.7,16.2, 27.6,14.2]
~~~
{: .output}

We'll save this last command to the python script too. We don't want to
overwrite our original command though. If we were to just enter 
~~~
%save metasearch_analysis.py N # enter the line number of the most recent
command
# then type "N" to prevent overwriting our previous command!
~~~
{: .python}
~~~
File `metasearch_analysis.py` exists. Overwrite (y/[N])?
~~~
{: .output}

Instead we need to provide the `-a` flag to append text to the pre-existing
file:
~~~
%save -a metasearch_analysis.py N # enter the line number of the most recent
command
~~~
{: .python}
~~~
The following commands were written to file `metasearch_analysis.py`:
print(roi_volumes)
~~~
{: .output}

Once again lets check the contents of the file:
~~~
%cat metasearch_analysis.py 
~~~
{: .python}
~~~
roi_volumes = [2.73,145.3,12.7,16.2, 27.6]
print(roi_volumes)
~~~
{: .output}

Finally we can run this script in the ipython shell
~~~
%run metasearch_analysis.py
~~~
{: .python}
~~~
[2.73,145.3,12.7,16.2, 27.6]
~~~
{: .output}


> ## Resurrecting variables
>
> Use the `%reset` magic to remove the roi_volumes variable from the current
> environment. Confirm you have done this. If we rerun the script will the
> variable exist in our environment again? Test whether your guess is correct.
> 
>
>> ## Solution
>>  ~~~
>> %reset # and type y + enter to confirm
>> print(roi_volumes)
>> %run metasearch_analysis.py
>> print(roi_volumes)
>> ~~~
>> {: .python}
>{: .solution}
>
{: .challenge}



## Atom as a text editor

The above commands can be very useful to interactively generate a set of working
commands and save them to a script; however, sometimes we want the convenience
of a text editor to lay out our thought a little more clearly and to edit
the flow of our analysis so that it is more coherent later on. We have already
configured the qtconsole to open the Atom text editor. It will open
automatically  when we try to edit a file. Try typing:
~~~
%edit metasearch_analysis.py 
~~~
{: .python}

There are many other good text editors out there. To name a few: vim, sublime,
 emacs, Notepad++. All of these are sensible choices.

The functionality available in the text editor is largely defined by the
packages installed. Some packages come preinstalled with Atom. These are called
core packages. One example is the command palette core package. This allows us
to see many of the commands available in Atom. We can activate this in using
Ctl+Shift+p or CMD+Shift+p depending on the system we are using.

> ## Viewing the editor settings
>
> Use the command palette to view the editor settings.
> >> ## Solution Select "View" from the menu at the top of the atom window  and
> >>    then "Toggle Command Palette". Alternatively use the keyboard shortcut
> >>    "Ctrl + Shift + P"
{: .challenge}

When we first open the settings we see the some tabs on the left of the
settings window. If we select the "Packages" tab we can see we have a number of 
packages that come pre-installed with Atom. #how do you open the settings?

We will now enable one of the core packages. When working with IPython and Atom
we don't want to have a situation where we have written text into Atom that is
unsaved and then try to append text using IPython. The text from IPython won't
save to the file. To avoid this situation we can enable the autosave core
package in Atom. We can do this by selecting the "Packages" tab on the left side
of the settings window, and scroll down to the autosave package in the "Core
Packages" section. 

It is worth noting that these changes can be all viewed in the same file. Below
the "Packages" tab is a "Open Config Folder" tab. When we select this we can see
a file called config.cson. All of our settings are stored here.

Finally in order to keep track of our current work within Atom we will add our
project directory to Atom by using the command palette (or by selecting "File >
Add Project Folder").

## Starting to save our workflow
We are at the point where we can start to begin to save our work as we go. Lets
save the command with which we downloaded our dataset.

~~~
%hist -g git clone # to find out the appropriate line number
%save -a metasearch_analysis.py N # enter the line number 
~~~
{: .python}

We should see the download command appear in our python script file. For now
let's delete the other two lines and comment out the command that downloads the
data. We do not want to download the data every time we run our script. We will
change this later when we learn how to conditionally execute code. 



