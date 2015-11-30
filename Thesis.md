---
layout: pagewithjs
title: ML For Radiotherapy
permalink: /thesis/
---
<p class="message"> A Statistical Learning Approach to the Accurate Prediction of MLC Errors During VMAT Delivery </p> 

Here is the presentation I gave at [AAPM 2015](http://www.aapm.org/meetings/2015AM/) regarding this work:

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/dkzHQwmlWf5ud0" width="476" height="400" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

To test whether the plan is deliverable, the planning system first 
calculates what it believes the dose distribution should look like, and 
the plan then is physically delivered to a measurement device. It is the 
job of the medical physicist to compare the measured and calculated 
distributions. 

Gamma analysis is one way of interpreting the results of the comparison. 
By way of example, if the institution wants a gamma passing rate of 90% 
for 2%/2mm, then 90% of the 2mm by 2mm voxels must be within 2% of each 
other. 

The process of physically delivering and measuring the distribution is 
time consuming and laborious for Medical Physicists. Thus to spare 
valuable clinical resources, for my MSc thesis project I am looking to 
find a way to *predict* gamma passing rates using only information 
available in the calculated plan, obviating the need for the measurement 
step in certain cases. 

There has been a certain number of approaches to solving this issue, and 
several papers proposing different *modulation indices* published. Most 
of these have taken into account the accelerations of MLC leaves, gantry 
rotation speeds, and number of monitor units delivered per control 
point. Recently, medical physicists at Seoul National University 
Hospital published a novel method using image analysis techniques 
(texture analysis - see my 
[publications](http://joelcarlson.github.io/publications/)) on fluence 
maps extracted from the calculated plans. In this paper, six textural 
features were investigated and found to have considerable correlations 
with plan deliverability. 

It is my hypothesis that there are ways to combine more conventional 
modulation indices with textural, and other features from the plans to 
increase predictive capability. From these features I will create a 
model, most likely using 
[GBM](http://en.wikipedia.org/wiki/Gradient_boosting) or [Random 
Forest](http://en.wikipedia.org/wiki/Random_forest) algorithms. 

#Introduction Gamma analysis is used in Radiotherapy to gauge the 
accuracy of calculated vs measured dose distributions. When a patient is 
to undergo a radiation treatment, several images are taken for 
simulation using CT. These images are then used to create a treatment 
plan. The treatment planning system (TPS) generates what it believes the 
dose distribution will be as delivered. This distribution is not always 
accurate due to 'real world' considerations such as the mechanical 
operation of the linear accelerator. To assess the deliverability Gamma 
analysis is utilized. 

A quantitative method of assessing deliverability considers the planar 
dose as a collection of point doses, and then to find the percent 
difference between calculated and measured at each point. The percentage 
of points passing a predetermined criterion (eg. \\(3\%\\), \\(2\%\\), 
\\(1\%\\)) is reported. This method is straightforward, but may be 
differentially reliable in regions of high and low dose gradients. 

#Gamma Analysis A method put forth by Low et al (1998) and Low and 
Dempsey (2003) is known as Gamma Analysis. Consider two dose 
distributions, a reference and an evaluated distribution. The Gamma at 
each point is given by 

![Gamma formula](/figs/Thesis/gammanalysis.png) 

<div class="box" style=50%><img src="/figs/Thesis/gammanalysis.png" alt="image" /></div><br>

The delta terms are the DTA and dose difference criteria. The other 
terms: 

![Gamma formula terms](/figs/Thesis/gammanalysisterms.png) 

Are the distance and dose difference between the reference and evaluated 
points. D<sub>r</sub>(r<sub>r</sub>) and D<sub>e</sub>(r<sub>e</sub>) 
are the reference and evaluated doses at points r<sub>r</sub> and 
r<sub>e</sub>. Gamma is found for each point in the evaluated 
distribution, and the smallest of these values becomes the gamma at that 
reference point, this is repeated for each reference point. 

A point is said to pass gamma if the gamma value at that point is less 
than one. For gamma analysis a plan for which passing rate remains above 
\\(90\%\\) with strict tolerance, such as \\((2/2)\\) can be considered 
dosimetrically robust. 

#VMAT Modulation Indices In VMAT plans excessive modulation may lead to 
errors in delivery. Gamma pass rates are one way to quanitfy the errors. 
A further method is to calculate an index based on the DICOM files from 
the TPS that may capture the features that make a plan undeliverable. If 
these features can be discovered it would save the medical physicist the 
trouble of physically measuring for gamma analysis. Although gamma 
analysis is not a ground truth for plan deliverability, it will be used 
as a surrogate in this study. Other patential indicators are DVHs and 
log files, to be investigated at some point. 

The major modulation indices form the literature are Masi et al's 
*MCS<sub>v</sub>*, *LTMCS*, Li and Xing's *MI<sub>SPORT</sub>*, and Park 
et al's (pile of MIs), and Park et al's *ASM*, *IDM*, *Contrast*, 
*Variance*, *Correlation* and *Entropy*. 

Let's examine these in turn: 

#LTMCS, MCSv In the 2013 
[paper](http://www.ncbi.nlm.nih.gov/pubmed/23822422) "Impact of plan 
parameters on the dosimetric accuracy of volumetric modulated arc 
therapy", Masi et al proposed two modulation indices: LTMCS, 
MCS<sub>v</sub>. 

An attempt was made to determine if there was a correlation between 
several plan parameters and results obtained by patient-specific QA 
results for VMAT. 

Parameters taken into account included: 

- Average leaf travel - Adapted version of MCS from 
[McNiven](http://www.ncbi.nlm.nih.gov/pubmed/20229859) - Impact of CP 
angular separation The hypothesis in this paper being that if MLC shapes 
change significantly between adjacent CPs there is a large amount of 
leaf travel, and this may lead to disprepancies between calculated and 
delivered dose. Values greater than \\(5mm/deg\\) were found to lead, in 
some cases, to less accurate delivery. 

Detailed calculations of MCS are given in the appendix of the paper. 

##Materials and Methods 142 VMAT plans for different anatomical sites 
used, including 80 prostate carcinoma, 14 prostate and pelvic lymph 
nodes, 18 spinal tumors, 10 mediastinal legions, 6 H&N tumors, 7 rectum, 
7 other. All plans optimized using a 4 degree gantry angle sampling 
between CPs. A leaf motion constraint of \\(5mm/deg\\) was used in 123 
of the plans, and \\(8mm/deg\\) or \\(3mm/deg\\) for the others. Plans 
were delivered using an Elekta synergy linac of 6MV with a 1 cm leaf 
width MLC. 

Gamma analysis in this study used two tolerance criteria: \\(3\%/3mm\\) 
and \\(2\%/2mm\\) using the local method. Results expressed as the 
percentage of points where gamma was less than one, the gamma pass rate. 

In this study a selection of the 40 plans of the 142 which had 
\\(2\%/2mm\\) passing rates less than \\(92\%\\) were considered. 

####Leaf Travel For each leaf, the entire travel over the VMAT arc was 
computed, and averaged over all in-field moving leaves of the considered 
plan. Closed leaves were not considered. (*This doesn't seem like a very 
good method...totally lacking in granularity. averaged over the entire 
arc??*) 

####MCS<sub>v</sub> As stated above, MCS was defined for IMRT plans and 
discussed in [Mcniven](http://www.ncbi.nlm.nih.gov/pubmed/20229859). To 
modify this index to apply to VMAT, the authors considered controls of 
the arc rather than segments. **The values of MCSv range from 0 to 1** 
with 1 meaning no modulation, as exemplified by an arc with a fixed 
aperture and no leaf movements. As modulation increases, the score 
decreases. 

####LTi: LT and MCS<sub>v</sub> combined The combined action of LT and 
MCS<sub>v</sub> was calculated as \\(LTi = (1000 - LT)/1000\\). The 
values in this case range between 0 and 1, where 0 is obtained for \\(LT 
= 1000\\), and 1 when \\(LT = 0\\). That is to say, LTi values are 
higher for lower values of leaf travel. 

####LTMCS The LTMCS index is derived by multiplying LTi by 
MCS<sub>v</sub>, which with both parameters has **values in the range of 
0 to 1**, and goes to 0 for increasing modulation and leaf motion. 

####Data Analysis For analysis, CP spacing was reduced to 2 or 3 degrees 
and the plans re-optimized. Of the 40 plans used, *LT*, *MCS*, *LTMCS* 
and *MU* were averaged and compared to the respective values of the 
original plans. The purpose of this was to examine whether the 
complexity of the new group of plans with smaller spacing was comparable 
to the original 4 degree spacing. The Student's *t*-test was used to 
analyze differences in means. 

The initial 142 plans created with 4 degree sampling constituted a 
starting point for analysis. Descriptive statistics were calculated for 
the examined parameters (LT, MCS, LTMCS, MU) and for gamma pass rates at 
each tolerance level (\\(3/3\\), \\(2/2\\)). 

Correlations were then found between dosimetric accuracy (as calcualted 
with pass rates) and described parameters, using Pearsons r with the 
following criteria: 

- \\(r < 0.4\\) - Weak - \\(0.4 < r < 0.7\\) - Moderate - \\(r > 0.7\\) 
- Strong - \\(p < 0.05\\) - Statistically significant ##Results 
####Gamma pass rates: 

![MasiPass](/figs/Thesis/Masipassrates.png) 

####Plan Parameters: 

![Masiparams](/figs/Thesis/Masiparams.png) 

####Correlations: 

![Masicorr](/figs/Thesis/Masicorr.png) 

A moderate correlation was observed between every parameter and gamma 
passing rates, with higher values for MCS<sub>v</sub> and LTMCS compared 
to MU. For leaf travel, negative correlation is observed, indicating 
that for higher LT values, lower pass rates are more frequent, as 
intuition would dictate. Plans with very high LT values (greater than 
500mm) mostly had pass rates below \\(90\%\\) and almost half had pass 
rates below \\(80\%\\). 

All plans, with the exception of two, with MCS<sub>v</sub> values 
greater than 0.5 yielded pass rates above \\(90\%\\). For plans with 
MCS<sub>v</sub> below 0.3 more than half had passing rates below 
\\(90\%\\). All plans with LTMCS above 0.48 had (\\(2/2\\)) pass rates 
above \\(90\%\\), and \\(70\%\\) of plans with LTMCS below 0.2 had pass 
rates below \\(90\%\\). 

##Implementation of Results in VMAT creation workflow When planning a 
treatment, average LT is calculated. If the value is above 450mm or leaf 
motion is greater than \\(5mm/deg\\) then CP sampling is decreased from 
4 to 2 or 2 degrees to ensure accurate calculation. 

Once the plan is developed, LTMCS values are calculated. Since all plans 
with a value above 0.48 yielded \\(2/2\\) pass rates above \\(90\%\\) 
patient specific QA is not performed, leading to the assumption that 
patient specific QA can be avoided for such plans. 

#MIsport 

The next index we will investigate is the *MI<sub>SPORT</sub>* created 
by Xing and Li in the 2013 
[paper](http://www.ncbi.nlm.nih.gov/pubmed/23635247) "An adaptive 
planning strategy for station parameter optimized radiation therapy 
(SPORT): Segmentally boosted VMAT". 

This paper set out to propose a novel method for altering the rotations 
of VMAT treatment to spend extra time in certain positions based on 
priorities. In the paper, however, they specify a new modulation index 
to quanitfy the accuracy of their results, and this modulation index 
will be of use. 

From the paper: "To quantify the level of intensity modulation of a 
station point, we introduce a demand metric called modulation index (MI) 
as follows:" 

![XingMI](/figs/Thesis/XingMI.png) 

The MI is therefore given per angle, with results for a representative 
plan: 

![Xingrep](/figs/Thesis/Xingrep.png) 

For this modulation index, large values indicate increased modulation, 
and therefore decreased deliverability. 

#Miura 

In the 2014 
[paper](http://www.scirp.org/journal/PaperInformation.aspx?paperID=45720 
#.VEhlCfmUc3k) "DICOM-RT Plan Complexity Verification for Volumetric 
Modulated Arc Therapy" Miura et al. investigated the relationship 
between several plan parameters and dosimetric results for VMAT. 

