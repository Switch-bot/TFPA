Repository for the "Topics in Particle Physics and Astroparticles II" course from IST.

This will contain all of the content provided by the teachers for the classes, such as templates and small tutorials.

In order to download the contents on this github you can copy it using git clone, but notice that there is no need to download the ROOT folder.
```
git clone https://github.com/Switch-bot/TFPA
```

### Creating a git repository

In order to create a git repository so that you can submit your notebooks you can utilize github.

If you do not already have an account, create one following the usual procedures. For GitHub sign up using your e-mail. If it is an institutional e-mail you might get the option to register as a student and apply for student priviledges. Although you should not need it for this course, you are free to do so.

The first thing you will do once your account is set up is to create a repository where you can upload your notebooks to. In order to do this select the option "Create repository" on the top left of the first screen that shows up after creating your account.

After creating a name for your repository you can create it without any further work, but feel free to look at the options available. Just remember that if you set your repository private you will need to grant access to it so that we can retrieve your projects.

From here, several options appear on how to initiate your repository. Following "or create a new repository on the command line" should yield you with a repository containing a simple file, corresponding to a specific main folder from which you can upload your progress.

As you can see from the example code, if you want to upload files to your repository save them inside the main folder, and run the command
'''
git add file.txt
'''

After adding a file to your repository, then you have to commit your changes through
'''
git commit -m "small message that describes the changes in the repository that you commited to"
'''

However, this only changes you local version of the repository. In order to upload the changes of your local repository to your online one that everyone can access to you need to push the local version by running
'''
git push -u origin main
'''

## ROOT Basics

If this is the first time you are using ROOT you can look at Template_0.ipynb, which reads zjet.root.

zjet.ROOT is a very simple root file where all the variables that we need to access have a corresponding branch.

The goal with this file is to get used to the basic functionalities of ROOT: How to read a file, how to check what is inside, and in the case of the dimensions not being explicit, what can we do with it.

The first thing to do is to check how to read what is inside a .root file. A quick way to check this is to utilize the TFile::ls() function, which states what are the imediate contents inside the file. Notice that root stores information in the form of TTrees, which has branches and subsequently leaves.

By following the template you can see that zjet.root has a TTree called Tdata, under which all the information is stored. In order to see what is inside this TTree one can use the Print() function, which most default ROOT objects have. In this case, it prints out all of the branches of Tdata, with their types.

With this file it is easy to plot the variables by using the default TFile.TTree.Draw("TBranch") function. However, if we check the branches which are TVectors, we can see that they all have dimension 2. Why is it?

Lastly, if we want to plot Px vs Py we can use the TH2F class, and fill the histogram one particle at a time by utilizing TH2F::Fill("valuex","valuey") within a loop over all events and all entries for each event.


## ROOT Fitting real data from Dimuon events

The next step is to utilize slightly more complex files, with data that is saved in a realistic format.

In order to do this you can follow Template_RooFit.ipynb, which reads the file Skim4.root. The first blocks of code mimick the previous template, where we can see the main TTree of the file, and its branches. This time around there are only 4 branches, with the 3 most relevant being muonP_p4, muonN_p4 and dimuon_p4.

Unlike for zjet.root, these branches are more complex TLorentzVectors, and as such we cannot use the trivial TTree::Draw("TBranch") function to plot the variables. In this case we need to create a TH1F histogram and fill it one event at a time in order to plot the kinematic properties.

Use the functions corresponding to the [TLorentzVector object](https://root.cern.ch/doc/master/classTLorentzVector.html) in order to plot said properties, like the mass and momenta, and vary the range and scale of the axes in order to get a better sense of the information contained file. In the template there is an example on how to create heterogenous bins in a TH1F object by creating an array where entry x corresponds to the dimension of bin x (remember that python creates lists by default, hence the inclusion of a numpy array).

Compare the mass plots for all 3 branches. By eye, what is the mass value for the muonP and muonN branches, and their respective errors?

Create a new histogram containing the masses of a new TLorentzVector that is the sum of muonP and muonN, and compare it to the mass spectrum for dimuon_p4.

If you wish to utilize C++ there is a dimuon class created by LIP personel with code analogous to the one presented in Template_ROOFit.ipynb, that can be found at "https://github.com/aboletti/LIP-analysis-tutorial".

### Applying fits to the data using RooFit

There are several ways of applying a fit to data using ROOT's capacities. One of these is by taking advantage of [RooFit](https://root.cern/manual/roofit/).

The template exemplifies how to fit the J/Psi peak with a Gaussian function describing the signal, accompanied by a background with an exponential behaviour.

In order to do this we start by defining the mass range to be fitted and create a TH1F histogram with the dimuon mass data. Notice that the TH1F::Fill() function only adds data if the value is within the histogram's limits, and as such there is no need to select the data within the relevant range a priori. We do this because we need to create a RooDataHist object that contains the histogram data to be fitted, and one of the inputs can be the relevant data in a TH1F format.

One peculiarity of RooFit is the need to create a RooRealVar for each variable that we will be using in our models. As such, we aso need to create our "mass" variable, that will be the x axis of our histogram. In order to do so we need to define its minimum and maximum values. Notice that this variable is also used to initialize the RooDataHist object (for initialization purposes the actual object used on the template is the RooArgList "args", but its only entry is the RooRealVar "mass").

The next step is to create our fitting functions. RooFit already has several functions implemented, such as the exponential and gaussian ones that we will be using.

Once again, we need to define a RooRealVar for each variable in our models. Starting with the background model we create a "Lambda" variable, which will be the exponential coeficient of our RooExponential function. Notice that since this is a variable that the model will try to fit, besides defining minimum (-4.0) and maximum (0.0) values we also need to give a starting estimation value (-0.3). By doing something analogous for the Gaussian function (RooGaussian) we have our signal and background functions defined.

The last thing to do before finishing our fitting model and performing the fit is to define variables for the number of predicted signal and background events, which are the RooRealVars n_signal and n_background.

At last we can compile all our functions and variables in a single model through model = RooAddPdf("name", "title", RooArgList(Functions), RooArgList(Vars)).

In order to perform a fit to the model we can simply run model.fitTo(RooDataHist), that spits out all of the relevant information regarding the fit.

The remainder of the code shows how to plot the different RooFit models, the RooDataHist that we created, and a nice legend to accompany it.

### Performing an Unbinned Fit
