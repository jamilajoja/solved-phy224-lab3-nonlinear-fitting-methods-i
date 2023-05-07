Download Link: https://assignmentchef.com/product/solved-phy224-lab3-nonlinear-fitting-methods-i
<br>



<strong>Radiation and nonlinear circuits</strong>

We continue with the curve fitting from the past exercise, but extend it to nonlinear models. Radioactive decay is an example of an exponential relation. We will measure the intensity from a radioactive isotope, and use curve fitting tools to find the half-life.

<h1>Background knowledge for exercise 3</h1>

<strong>Python </strong>lists, arrays, numpy, SciPy, PyPlot, curve_fit()

<strong>Error Analysis </strong>Chi squared (<em>χ</em><sup>2</sup>), goodness of the fit <strong>Physics </strong>Nuclear decay. Review this from your first year text.

<h1>1           Introduction</h1>

You’ve probabably learned about exponential decay of radioactive materials in the past. The number of emissions (during a sampling period) depends on the number of atoms of that isotope. Because an emission event corresponds to a death of an isotope, it follows that the intensity of radiation decays exponentially,

<em>,                                                                                      </em>(1)

where <em>I</em>(<em>t</em>) is the intensity of radiation at time <em>t</em>, and <em>τ </em>is the mean lifetime of the isotope. A more intuitive parameter is the half-life, <em>t</em><sub>1<em>/</em>2</sub>, defined so that

<em> .                                                                                      </em>(2)

Our interest for this experiment is calculating the half-life of an isotope by measuring the intensity (emissions/sample) of radiation.

The sample you will measure today is a metastable Barium (Ba-137m). It is prepared by flowing an acid through a generator containing Cesium (Cs-137). Inside the generator, the Cs-137 beta decays (with a half-life of 30.1 years), into Ba-137m. The acid washes out the Ba-137m which decays by gamma emission (662 keV) with a half-life of 2.6 minutes. We will measure the rate of emissions using a Geiger counter.

<h1>2           Analysis with logarithms</h1>

Nonlinear relations become more complicated to analyze in general. There are analytical formulae to find the parameters in a linear regression, but typically not for a nonlinear regression.

One method for nonlinear regression is transforming the data into a linear model and using linear regression methods. Logarithms are a particularly useful tool for analyzing exponential relations. Consider the general exponential relation,

<em>y </em>= <em>y</em><sub>0</sub><em>e<sup>ax </sup>.                                                                                          </em>(3)

Taking the logarithm of both sides,

log(<em>y</em>) = <em>ax </em>+ log(<em>y</em><sub>0</sub>)<em>.                                                                               </em>(4)

Thus the exponential relation for <em>y </em>became a linear relation for log(<em>y</em>). Using a semi-log plot, with a logarithmic scale for the y-axis turns an exponential into a straight line, where the slope of the line is the growth/decay rate.

By first transforming the relation, linear regression can be used to find the parameter <em>a</em>. The model function is the same, <em>f</em>(<em>x</em>) = <em>ax </em>+ <em>b</em>, but now, the data to match is <em>z<sub>i </sub></em>= log(<em>y<sub>i</sub></em>).

The transformation of the data also requires an error propagation to get more accurate results from the regression,

<em> .                                                                                        </em>(5)

This propagation formula is the standard one for logarithms.

<h1>3           Nonlinear regression</h1>

Traditionally, the method described in Section 2 was used for analyzing exponential relations, because a formula could be used to calculate the parameters. However, there is a problem with performing a linear regression on <em>z<sub>i </sub></em>= <em>ax<sub>i</sub></em>.

The maximum likelihood of a <em>χ</em><sup>2 </sup>linear model assumes that the <em>measurement errors </em>are normally distributed. When the axes are rescaled, this assumption is <em>potentially </em>invalidated. A related side-effect is that certain ranges of <em>y </em>are given more weight in the regression.

With the use of computers and nonlinear optimization, it is now possible to perform a least-squares optimization using a wide range of nonlinear models. Recall that curve_fit() uses least squares to minimize

<em> ,                                                                           </em>(6)

where <em>y<sub>i </sub></em>is the <em>dependent </em>data you measured, <em>x<sub>i </sub></em>is the <em>independent </em>data, <em>σ<sub>i </sub></em>is the measurement error in the <em>dependent </em>data. For an exponential model, we simply use <em>y</em>(<em>x</em>) = <em>ae<sup>bx </sup></em>as the model function.

Computer-aided nonlinear regression brings with it other things to consider like whether the model function is suitable, if the initial guess for parameters is close enough to the true optimum values, and still if the measurement errors fit a normal distribution.

<h1>4           The experiment</h1>

The radioactive sample used in this experiment will be prepared for you by a lab technologist, as a demonstration.

You will not acquire the data on your computer. Instead youll find the radioactive decay text file at: /Desktop/2nd Year lab Files/Exercises/Cesium Radioactive Decay.txt. The instrument used to acquire the data is called a Geiger counter.

You will also be provided with a text file with data for background radiation: /Desktop/2nd Year lab

Files/Exercises/Radioactive Background 20min 20secdwell.txt

<strong>The radioactive energy is quite low and safe. We are using a single demonstration Geiger counter because of the complexity of setting up 30 identical experiments before the sample has completely decayed. You still need to document the experiment in the lab–book as if you were conducting it.</strong>

<h2>4.1         Uncertainty in the Geiger counter</h2>

The Geiger counter follows <em>counting statistics </em>for determining the uncertainty. This is much like counting√

the number of heads in a series of coin-flip trials (the <em>N </em>rule). The rate is the number of events <em>N </em>divided by the sample time ∆<em>t</em>. The best estimate of the count rate is given by,

<em>.                                                                                    </em>(7)

There is also background radiation to consider; you should have heard the counter beeping with no sample present. The counts from the two sources of radiation add together for our final reading,

<em>N</em><sub>s </sub>= <em>N</em><sub>T </sub>− <em>N</em><sub>b </sub><em>.                                                                                       </em>(8)

The uncertainty can be calculated via standard error propagation, so

<em>.</em>

Finally, because the background radiation cannot be measured at the same time, we assume it has constant statistics, and use its mean in the calculations (<em>N</em><sub>b </sub>= <em>N</em><sup>¯</sup><sub>b</sub>). The mean background radiation must be subtracted from each data point before analysis.

<h1>5           The Python program</h1>

We will use our programs to compare the transformation method (Section 2) and the non-linear leastsquares method (Section 3). The best parameters will be found with both methods, and used to plot best fit curves over the data. For this experiment, we have the luxury of comparing our calculated half-time to the theoretical value.

The program should be organized as follows:

<ul>

 <li>Import the required functions and modules (numpy, optimize.curve_fit(), matplotlib.pyplot). • Define the model functions (<em>f</em>(<em>x,a,b</em>) = <em>ax </em>+ <em>b</em>, <em>g</em>(<em>x,a,b</em>) = <em>be<sup>ax</sup></em>)</li>

 <li>Load the data using loadtxt().</li>

 <li>Subtract the mean background radiation from the data.</li>

 <li>Calculate the standard deviation for each data point.</li>

 <li>Convert the count data into rates (divide by ∆<em>t</em>).</li>

 <li>Perform the linear regression on (<em>x<sub>i</sub>,</em>log(<em>y<sub>i</sub></em>)) using <em>f </em>as the model function.</li>

 <li>Perform the nonlinear regression on (<em>x<sub>i</sub>,y<sub>i</sub></em>) using <em>g </em>as the model function.</li>

 <li>Output both of the half-life values you calculated.</li>

 <li>Plot the errorbars, both curves of best fit, and the theoretical curve. Include a legend for the different curves.</li>

 <li>Make a second plot of the same data, but using a logarithmic <em>y </em> One way of doing this is to call the pylab.semilogy function. Another way is to call pylab.yscale(‘log’) after you make the plot.</li>

</ul>

An alternate form of the non–linear <em>g </em>function can be used where the exp is replaced by  or 2. You can also treat the linearized model <em>f </em>as a natural logarithm (ln), or a base–2 logarithm (log<sub>2</sub>). These transforms might change the way you process your data for input into the function, or change the way you interpret the errors from the curve_fit.

Write the program, and run it using the data gathered in the experiment. Save all plots and the parameters you determined.

<strong>Which regression method gave a half-life closer to the expected half-life of 2.6 minutes?</strong>

<strong>Can you see the difference on the plots?</strong>

<h1>6           Analyzing the quality of the fit</h1>

There are two ways that we can assess the quality of the fit of our model: variance of the calculated parameters, and the reduced chi-squared statistic.

<h2>6.1         Variance of parameters</h2>

The variance of the parameters is returned by curve_fit() as the diagonal entries in the covariance matrix, p_cov. Recall that the error in measurements is understood as the standard deviation,

<table width="624">

 <tbody>

  <tr>

   <td width="600"><em>t</em>1<em>/</em>2 = <em>t</em>˜1<em>/</em>2 ± <em>σ</em><em>t</em>1<em>/</em>2 <em>,</em>where <em>σ<sub>t</sub></em><sub>1<em>/</em>2 </sub>= <sup>p</sup>Var(<em>t</em><sub>1<em>/</em>2</sub>). The variance of the parameter a is p_cov[0, 0].To get <em>σ<sub>t</sub></em><sub>1<em>/</em>2 </sub>from <em>σ</em><sub>a</sub>, you will need to use the error propagation formula for a reciprocal,</td>

   <td width="24">(9)</td>

  </tr>

 </tbody>

</table>

<em>.                                                                                   </em>(10)

<strong>Modify your program from Section </strong><strong>5 to calculate the standard deviation of the half-life. What values did you find? Does the nominal half-life (2.6 minutes) fall in the range?</strong>

<h2>6.2         Reduced <em>χ</em><sup>2</sup></h2>

Recall that the <em>χ</em><sup>2 </sup>distribution gives a measure of how unlikely a measurement is. The more a model deviates from the measurements, the higher the value of <em>χ</em><sup>2</sup>. But if <em>χ</em><sup>2 </sup>is too small, it’s also an indication of a problem: usually that there weren’t enough samples.

This best (most likely) value of <em>χ</em><sup>2 </sup>depends on the number of degrees of freedom of the data, <em>ν </em>= <em>N </em>−<em>n</em>, where <em>N </em>is the number of observations and <em>n </em>is the number of parameters. This dependence is removed with the <em>reduced chi-squared </em>distribution,

<em> .                                                                     </em>(11)

For <em>χ</em><sup>2</sup><sub>red</sub>, the best value is 1:

indicates that the model is a poor fit;

indicates an incomplete fit or underestimated error variance;  indicates an over-fit model (not enough data, or the model somehow is fitting the noise).

<strong>Add a function to your program to calculate </strong><em>χ</em><sup>2</sup><sub>red</sub><strong>. What values were computed? How would you interpret your results?</strong>

<em>Prepared by John Ladan and Ruxandra Serbanescu, 2016. Edited by Christopher Lee, 2017.</em>