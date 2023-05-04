Download Link: https://assignmentchef.com/product/solved-cs524-homework-7-convex-programming
<br>



<ol>

 <li><strong>Enclosing circle. </strong>Given a set of points in the plane <em>x<sub>i </sub></em>∈ R<sup>2</sup>, we would like to find the circle with smallest possible area that contains all of the points. Explain how to model this as an optimization problem. To test your model, generate a set of 50 random points using the code X = 4.+randn(2,50) (this generates a 2×50 matrix <em>X </em>whose columns are the <em>x<sub>i</sub></em>). Produce a plot of the randomly generated points along with the enclosing circle of smallest area.</li>

</ol>

To get you started, the following Julia code generates the points and plots a circle:

using PyPlot

X = 4 .+ randn(2,50)                                                              # generate 50 random points

t = range(0,stop=2pi,length=100)               # parameter that traverses the circle r = 2; x1 = 4; x2 = 4          # radius and coordinates of the center

plot( x1 .+ r*cos.(t), x2 .+ r*sin.(t))             # plot circle radius r with center (x1,x2) scatter( X[1,:], X[2,:], color=”black”) # plot the 50 points

axis(“equal”)                                                                            # make x and y scales equal

<ol start="2">

 <li><strong>The Huber loss. </strong>In statistics, we frequently encounter data sets containing <em>outliers</em>, which are bad data points arising from experimental error or abnormally high noise. Consider for example the following data set consisting of 15 pairs (<em>x,y</em>).</li>

</ol>

<table width="0">

 <tbody>

  <tr>

   <td width="34"><em>x</em></td>

   <td width="37">1</td>

   <td width="37">2</td>

   <td width="37">3</td>

   <td width="37">4</td>

   <td width="37">5</td>

   <td width="37">6</td>

   <td width="37">7</td>

   <td width="37">8</td>

   <td width="37">9</td>

   <td width="35">10</td>

   <td width="37">11</td>

   <td width="35">12</td>

   <td width="37">13</td>

   <td width="37">14</td>

   <td width="37">15</td>

  </tr>

  <tr>

   <td width="34"><em>y</em></td>

   <td width="37">6.31</td>

   <td width="37">3.78</td>

   <td width="37">5.12</td>

   <td width="37">1.71</td>

   <td width="37">2.99</td>

   <td width="37">4.53</td>

   <td width="37">2.11</td>

   <td width="37">3.88</td>

   <td width="37">4.67</td>

   <td width="35">26</td>

   <td width="37">2.06</td>

   <td width="35">23</td>

   <td width="37">1.58</td>

   <td width="37">2.17</td>

   <td width="37">0.02</td>

  </tr>

 </tbody>

</table>

The <em>y </em>values corresponding to <em>x </em>= 10 and <em>x </em>= 12 are outliers because they are far outside the expected range of values for the experiment.

<ol>

 <li>Compute the best linear fit to the data using an <em>`</em><sub>2 </sub>cost (least squares). In other words, we are looking for the <em>a </em>and <em>b </em>that minimize the expression:</li>

</ol>

<em>`</em><sub>2 </sub>cost:

Repeat the linear fit computation but this time exclude the outliers from your data set. On a single plot, show the data points and both linear fits. Explain the difference between both fits.

<ol>

 <li>It’s not always practical to remove outliers from the data manually, so we’ll investigate ways of automatically dealing with outliers by changing our cost function. Find the best linear fit again (including the outliers), but this time use the <em>`</em><sub>1 </sub>cost function:</li>

</ol>

<em>`</em><sub>1 </sub>cost:

Include a plot containing the data and the best <em>`</em><sub>1 </sub>linear fit. Does the <em>`</em><sub>1 </sub>cost handle outliers better or worse than least squares? Explain why.

<ol>

 <li>Another approach is to use an <em>`</em><sub>2 </sub>penalty for points that are close to the line but an <em>`</em><sub>1 </sub>penalty for points that are far away. Specifically, we’ll use something called the <em>Huber loss</em>, defined as:</li>

</ol>

<sup>(</sup><em>x</em><sup>2                                             </sup>if − <em>M </em>≤ <em>x </em>≤ <em>M</em>

<em>φ</em>(<em>x</em>) =

2<em>M</em>|<em>x</em>| − <em>M</em><sup>2           </sup>otherwise

Here, <em>M </em>is a parameter that determines where the quadratic function transitions to a linear function. The plot on the right shows what the Huber loss function looks like for <em>M </em>= 1.

The formula above is simple, but not in a form that is useful for us. As it turns out, we can evaluate the Huber loss function at any point <em>x </em>by solving the following convex QP instead:



minimize

 <em>v,w                                                             </em>

<em><sup>φ</sup></em><sup>(<em>x</em>) =      </sup>subject to:       |<em>x</em>| ≤ <em>w </em>+ <em>v</em>

                       <em>v </em>≥ 0<em>, w </em>≤ <em>M</em>

Verify this fact by solving the above QP (with <em>M </em>= 1) for many values of <em>x </em>in the interval −3 ≤ <em>x </em>≤ 3 and reproducing the plot above. Finally, find the best linear fit to our data using a Huber loss with <em>M </em>= 1 and produce a plot showing your fit. The cost function is:

Huber loss:

<ol start="3">

 <li><strong> Heat pipe design. </strong>A heated fluid at temperature <em>T </em>(degrees above ambient temperature) flows in a pipe with fixed length and circular cross section with radius <em>r</em>. A layer of insulation, with thickness <em>w</em>, surrounds the pipe to reduce heat loss through the pipe walls (<em>w </em>is much smaller than <em>r</em>). The design variables in this problem are <em>T</em>, <em>r</em>, and <em>w</em>.</li>

</ol>

The energy cost due to heat loss is roughly equal to <em>α</em><sub>1</sub><em>Tr/w</em>. The cost of the pipe, which has a fixed wall thickness, is approximately proportional to the total material, i.e., it is given by <em>α</em><sub>2</sub><em>r</em>. The cost of the insulation is also approximately proportional to the total insulation material, i.e., roughly <em>α</em><sub>3</sub><em>rw</em>. The total cost is the sum of these three costs.

The heat flow down the pipe is entirely due to the flow of the fluid, which has a fixed velocity, i.e., it is given by <em>α</em><sub>4</sub><em>Tr</em><sup>2</sup>. The constants <em>α<sub>i </sub></em>are all positive, as are the variables <em>T</em>, <em>r</em>, and <em>w</em>.

Now the problem: maximize the total heat flow down the pipe, subject to an upper limit <em>C</em><sub>max </sub>on total cost, and the constraints

<em>T</em>min ≤ <em>T </em>≤ <em>T</em>max<em>,                r</em>min ≤ <em>r </em>≤ <em>r</em>max                        <em>w</em>min ≤ <em>w </em>≤ <em>w</em>max<em>,              w </em>≤ 0<em>.</em>1<em>r</em>

.

<ol>

 <li><strong>a) </strong>Express this problem as a geometric program, and convert it into a convex optimization problem. Recall that a generic geometric program has the following form</li>

</ol>

X <em>β</em><em>j</em>01 <em>β</em><em>j</em>02                            <em>β</em>

min <em>c</em><em>j</em>0<em>x</em>1            <em>x</em>2                   ···<em>x</em><em>n</em><em>j</em>0<em>n </em><em>x</em>

<em>j</em>

<em>s.t. </em>X<em>c<sub>ji</sub>x</em><em>β</em><sub>1</sub><em>ji</em>1<em>x</em><em>β</em><sub>2</sub><em>ji</em>2 ···<em>x</em><em>β<sub>n</sub></em><em><sub>ji</sub></em>3 ≤ 1<em>,                  i </em>= 1<em>,</em>··· <em>,m</em>

<em>j</em>

X <em>β</em><em>jk</em>1 <em>β</em><em>jk</em>2 <em>β</em><em>jk</em>3 <em>d<sub>jk</sub>x</em><sub>1 </sub><em>x</em><sub>2 </sub>···<em>x<sub>n </sub></em>= 1<em>, k </em>= 1<em>,</em>··· <em>,p</em>

<em>j</em>

<em>x<sub>q </sub>&gt; </em>0<em>,         q </em>= 1<em>,…,n               c<sub>ji </sub>&gt; </em>0<em>,d<sub>jk </sub>&gt; </em>0<em>,           </em>∀<em>ji,jk</em>

<strong>b) </strong>Consider a simple instance of this problem, where <em>C</em><sub>max </sub>= 500 and <em>α</em><sub>1 </sub>= <em>α</em><sub>2 </sub>= <em>α</em><sub>3 </sub>= <em>α</em><sub>4 </sub>= 1. Also assume for simplicity that each variable has a lower bound of zero and no upper bound. Solve this problem using JuMP. Use the Ipopt solver and the command @NLconstraint(…) to specify nonlinear constraints such as log-sum-exp functions. What is the optimal <em>T</em>, <em>r</em>, and