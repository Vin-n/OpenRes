# Installing Python

This is guide for those trying to install the Python packages required for the *OpenRes* Day 1 tutorials run by **Research Platforms** on Monday 20th November, 2017. 

If you have previously installed Python 3.5 or Python 3.6 using Anaconda, then you will already have all of the dependencies required for this tutorial. If you do not currently have Python installed on your laptop, you can find a complete set of installation instructions [here](https://github.com/resbaz/Intro_Python_Nov2017/blob/master/Python_Installation.md).

For those who have previously installed Python using pip or miniconda though, you will find the steps required for installing the required packages below.


## Miniconda

### Windows Installation

1) Find and open a program called `Select Anaconda Prompt` from the Windows Start Menu. 

This will open a new window with a black background (it might keep blinking if the computer is slow).  Wait until you can start typing

2) Type `conda --version` into this command prompt to test that miniconda has been installed. Conda should respond with the version number that you have installed, like: conda 3.11.0

3) Type `conda install jupyter numpy pandas matplotlib seaborn bokeh` and hit `ENTER` key.  When asked do you want to proceed, type `y` and hit `ENTER`.

   This will take a few minutes to download the jupyter packages. 
   
    If you wish to install more packages in the future, you just need to use the `conda install <package name>` from your command line.

3) To view a list of packages and versions installed, or to confirm that a package has been added or removed, type `conda list`. Confirm that the jupyter, numpy, pandas, matplotlib, seaborn and bokeh packages have been installed

4) The workshop will be using jupyter notebooks. To see how you can launch this, go to the Launching Jupyter Notebook section below.

5) If you need to uninstall miniconda for any reason, you can do this through "Add or remove Program" in the control panel, by removing "Python 3.6(Miniconda)"


### OS X Setup

1) Open your terminal window, and type `conda --version` into the command terminal to test that miniconda has been installed. Conda should respond with the version number that you have installed, like: conda 3.11.0

NOTE: If you see an error message, check to see that you are logged into the same user account that you used to install Anaconda or Miniconda, and that you have closed and re-opened the terminal window after installing it.

2) Type `conda install jupyter numpy pandas matplotlib seaborn bokeh` and hit `ENTER` key.  When asked do you want to proceed, type `y` and hit `ENTER`. If you wish to install more packages in the future, you just need to use this `conda install <package name>` from your command line.

3) To view a list of packages and versions installed, or to confirm that a package has been added or removed, type `conda list`. Confirm that jupyter, numpy and matplotlib have been installed

4) The workshop will be using jupyter notebooks. To see how you can launch this, go to the Launching Jupyter Notebook section below.

5) If you need to uninstall miniconda for any reason, open a terminal window and remove the entire miniconda install directory by typing: `rm -rf ~/miniconda` 

You may also edit ~/.bash_profile and remove the miniconda directory from your PATH environment variable, and remove the hidden .condarc file and .conda and .continuum directories which may have been created in the home directory with: `rm -rf ~/.condarc ~/.conda ~/.continuum`

### Linux Installation

1) Open your terminal window. Type `conda --version` into the command terminal to test that miniconda has been installed. Conda should respond with the version number that you have installed, like: conda 3.11.0

2) Type `conda install jupyter numpy pandas matplotlib seaborn bokeh` and hit `ENTER` key.  When asked do you want to proceed, type `y` and hit `ENTER`. If you wish to install more packages in the future, you just need to use this `conda install <package name>` from your command line.

3) To view a list of packages and versions installed, or to confirm that a package has been added or removed, type `conda list`. Confirm that the previously listed packages have been installed

4) The workshop will be using jupyter notebooks. To see how you can launch this, go to the Launching Jupyter Notebook section below.

5) If you need to uninstall miniconda for any reason, open a terminal window and remove the entire miniconda install directory: `rm -rf ~/miniconda`. 

You may also edit `~/.bash_profile` and remove the miniconda directory from your PATH environment variable, and remove the hidden .condarc file and .conda and .continuum directories which may have been created in the home directory with `rm -rf ~/.condarc ~/.conda ~/.continuum`.


## pip Installation

1) Ensure that you have the latest pip; older versions may have trouble with some dependencies. In your command prompt (Windows) or command terminal (MacOS/Linux), type: `pip3 install --upgrade pip`    
(Use `pip` instead of `pip3` if using legacy Python 2.x)

2) Install the Jupyter Notebook, Pandas, numpy, matplotlib, seaborn and bokeh libraries using: `pip3 install jupyter numpy pandas matplotlib seaborn bokeh`   
  (Use `pip` instead of `pip3` if using legacy Python 2).   
  Close and re-open your terminal window.  
  Similarly, if you wish to install other packages in the future, you can use `pip3 install <package names>    


# Launching the Jupyter Notebook

For many of these sessions, we're going to be working in the Jupyter notebook. Jupyter is launched from your command prompt (windows) or command terminal (MacOS/Linux), which you can find by searching your applications. 

After opening the command terminal, you should see a black screen and a `>` prompt after a file path, usually within your `C:\` drive. 
When Jupyter launches, it sets its "home" folder to be the folder you are in when you launch the notebook, and it can access any sub-folders and files within that location. 
This means it's in our best interest to launch from an area where we'll be able to access _all_ of the files we'll require. This is usually in your user folder.

Therefore, in your terminal, type `cd C:/Users/<YourUserName>`. Once there, type in the command `jupyter notebook` to launch Jupyter.
![command terminal](https://github.com/resbaz/August2017_introPython/blob/master/controlpanel.PNG "Command Line Launch")

You should then see the terminal generate a bunch of text, and it should then open the jupyter home page within a browser window. 
This should be within whichever browser application is currently set as your default. You can change this in your settings. 

(MacOS/Linux) If the terminal does not automatically open within your browser, you can copy the `http://localhost:888...etc` link into a new browser tab.

This should open to the Jupyter home folder, which will look something like this

![Jupyter Home](https://github.com/resbaz/August2017_introPython/blob/master/jupyterHome.PNG "Jupyter Home Page")

You can click on the blue links inside the main console to navigate through your folders and files. To the top-right of your files 
box are some command buttons. These allow you to do things like create new folders, new text (*.txt) files, or a new iPython notebook. 
We will be writing and running our code from inside these *.ipynb files.

When you create a new *.ipynb notebook, you'll get something that looks like this, except empty:

![Notebook](https://github.com/resbaz/August2017_introPython/blob/master/jupyterNotebook.PNG "A guided tour to the *.ipynb")

Play around with the notebook, and have a look to see what you can do. If you'd like a more guided tour/playbox, you can find one [here](http://nbviewer.jupyter.org/github/jupyter/notebook/blob/master/docs/source/examples/Notebook/Notebook%20Basics.ipynb)
