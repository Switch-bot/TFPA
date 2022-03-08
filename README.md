Repository for the "Topics in Particle Physics and Astroparticles II" course from IST.

This will contain all of the content provided by the teachers for the classes, such as templates and small tutorials.

Submissions of the students' work will also be done through this repository.

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




If you wish to utilize C++ there is a dimuon class created by LIP personel with code analogous to the one presented in Template_ROOFit.ipynb, that can be found at "https://github.com/aboletti/LIP-analysis-tutorial".
