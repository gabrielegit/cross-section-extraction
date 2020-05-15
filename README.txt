Before you do anything: 
-Save and unzip the _follow_fits files. They should go into:
./S01/data_corr and ./S02/data_corr (or change this path in the 3rd cell of the notebooks you run)

-fakespec_x, etc should be in the same place you run modelupdate.

------------------------------------------------------
If you just want to run modelupdate_test_v2 with given fakespec: 

"Correct answers" for weights of model components in fake spectra are 

fakespec_x, _y
-801000003 = 0.8
-802000003 = 1.4
-803000005 = 1.1
-800000001 = 1.0

fakespec_x2, _y2
-800000001 = 12
-801000003 = 33
-803000005 (8f5/2, degenerate with below) = 56
-903000005 (9f5/2, degenerate with above) =  1
-701000001 (7p1/2, degernate with below) = 80
-701000003 (7p3/2, degenerate with above) =  87
-700010102 (He-like 7s1/2 J=1, degen w below) = 11
-800010102 (He-like 8s1/2 J=1, degen w above) = 51
-802010002 (He-like 8d 3/2 J=1, degen w below) = 64
-802010004 (He-like 8d 3/2 J=2, degen w above) = 86

------------------------------------------------------
If you want to generate *new* fake spectra, modify ModelUpdate_fakeIt_v0:

-Adjust cell 7 as necessary: currently it takes 10 models that I define by their keys. 
-These models are weighted by random weights that are then spit out before I plot the
 model component spectra and the composite spectrum
 
-You can remove/add models to make the total composite model more/less than 10;
-You can change the model keys that are used (see list printed at the end of cell 3);
-If you want to adjust the weights, you'll have to put them in by hand and override the
 application of random weights.
 
------------------------------------------------------
Now you want to run the cross section code? Here we go. (It's called 
modelupdate_test_v2.ipynb)

You can run it as is (I think). Currently it's set to try and predict the capture states 
that make up the spectrum saved in fakespec_x2, _y2 (described above). This is basically 
a composite spectrum with 10 distinct components. There are some components in there that 
are (visually, to my eye) degenerate. I did this to make it harder; maybe it's too hard...

The code tries to come up with a best fit to this spectrum by assembling a quasi-random
(will describe this in a sec) selection of capture states, and assembling a different 
quasi-random selection "nrun" times. "nmax" is the normal n_max we think of 
(like q**(0.75)). "num_models" is the number of individual capture states that goes into 
one composite model. 

The pseudo-random sampling means this: I allow the possible n states to be n_max +/- 2 
(the 2 is changeable, that variable is called 'n_spread'). I give each of those n states 
a weight that is the value of a gaussian centered at n_max, with sigma=1 (the 1 is
changeable too, the variable is called 'sigma').  So I then have a list with length 
'num_models' that is a random sampling of integers from nmax-2 --> nmax_2, with the most
probable value = nmax, nmax+/-1 is a bit less probable, nmax+/-2 a bit less probable than
that. Then to get an nljjJ-resolved capture state, within my gaussian-randomly-sampled 
list of n states, I uniformly randomly pick a capture state with that n state. This gives 
me my list of quasi-randomly sampled capture states that I will add together to form a
composite model. 

Run with these parameters, nrun=1000 takes about 40 minutes. When I ran it the first 
time, it did not give me even one instance of getting the "right" answer. 
Time/iteration is about 2.5 seconds. This makes me sad. 

You'll be able to look at some of the supposedly best fits to the data in plots that are 
generated, along with the capture states and the associated norms. 

Options for ways to move forward, the way I see it: 
1. I keep this similar to how it is now, and do a second minimization that only keeps the 
capture states deemed important in the first round of fits. Could use the powell method
for this second round. 
2. I try to build a composite model with ALL of my capture states (i.e. a model with ~275
components) and see how lmfit does

Other thoughts: use cstat instead of chi squared (and just calculate it manually)
