Before you do anything: 
-Save and unzip the _follow_fits files. They should go into:
./S01/data_corr and ./S02/data_corr (or change this path in cell 3)

-fakespec_x, etc should be in the same place you run modelupdate.

------------------------------------------------------

If you just want to run modelupdate_test with given fakespec: 

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
If you want to generate *new* fake spectra, this is how to use ModelUpdate_fakeIt_v0:

-Adjust cell 7 as necessary: currently it takes 10 models that I define by their keys. 
-These models are weighted by random weights that are then spit out before I plot the
 model component spectra and the composite spectrum
 
-You can remove/add models to make the total composite model more/less than 10;
-You can change the model keys that are used (see list printed at the end of cell 3);
-If you want to adjust the weights, you'll have to put them in by hand and override the
 application of random weights.