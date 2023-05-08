Download Link: https://assignmentchef.com/product/solved-pols6480-lab8-test-hypotheses-regarding-two-populations
<br>
<ol>

 <li><strong> Objectives</strong>: Primary objective is to test hypotheses regarding two populations. Secondary objective is to understand research design issues involving panel studies and paired/related samples.</li>

 <li><strong> Datasets</strong>: “cereal.csv” and “anorexia.csv”</li>

</ol>

<strong>III. Packages</strong>:  none

<ol>

 <li><strong> Preparation</strong></li>

</ol>

1) Open RStudio by double-clicking the icon or selecting RStudio from the Windows Start menu.

2) Clear any data in memory:                &gt; rm(list=ls())

3) Download datasets “cereal.csv” and “anorexia.csv” and place them in your working directory

4) Download R script “POLS 6480 Lab 08.R” and place it in your working directory.

5) Open the R script by typing <em>Ctrl</em>+<em>O</em> or by clicking on File in the upper-left corner, using the dropdown menu, and navigating to the script in your working directory.

<ol>

 <li><strong> Instructions for Lab 08</strong></li>

</ol>

The first dataset you will use is a sample of 24 breakfast cereals – 12 cereals intended for children and 12 intended for adults – which you used last week. The five variables are the number of grams per serving, the number of calories per serving, the milligrams of sodium per serving, the grams of fiber per serving, and the grams of refined sugar per serving.




The second dataset you will use is from a well-designed experiment on treating eating disorders. Subjects’ weights were measured before and after treatment, allowing us to calculate the change in weight, and the study also included a control group.

<strong> </strong>

<ol>

 <li><strong> Cereals</strong></li>

</ol>




<ol>

 <li>To load the first dataset, type the following lines, changing the directory if needed:</li>

</ol>

&gt; cereal &lt;- read.csv(“C:/cereal.csv”)




The variable named Intended.for identifies whether the cereal is for children or adults. We are going to compare the two types cereals. To create a subset of the data type:

&gt; children &lt;- cereal[cereal$Intended.for == “Children”, ]

&gt; adults &lt;- cereal[cereal$Intended.for == “Adults”, ]




<ol start="2">

 <li>Let us shift our attention to summary statistics, focusing on how many grams of sugar are contained in a serving of each cereal. To find the average and the standard deviation in each sample, type the following two lines of code:</li>

</ol>

&gt; m1 &lt;- mean(children$Sugar); s1 &lt;- sd(children$Sugar)

&gt; m2 &lt;- mean(adults$Sugar); s2 &lt;- sd(adults$Sugar)

Compare these quantities by answering the following questions: In the sample, do children’s cereals and adults’ cereals have similar average sugar content? In the sample, do children’s and adults’ cereals have similar variability in their sugar content?




You can also examine the distribution of the data visually.




<ol start="3">

 <li>In carrying out a difference-of-means test, usually we set our null hypothesis at zero, indicating that there is no difference between the <em>populations</em>.</li>

</ol>




To calculate the t-statistic, you must compute two quantities. The numerator subtracts the null hypothesis from the difference of sample means:

&gt; diff.of.means &lt;- m1 – m2

The denominator is the standard error of the difference of means:

&gt; var.unequal &lt;- (((s1^2)/12) + ((s2^2)/12))

&gt; se.unequal &lt;- sqrt(var.unpooled)

calculate the <em>t</em> statistic:

&gt; t.unequal &lt;- diff.of.means/se.unequal




In order to know whether the difference between the two sample means is statistically significant, you can compare it to the <em>t</em> critical value for the correct degrees of and for a given confidence level, and/or calculate the <em>p</em>-value.

&gt; t.critical &lt;- qt(0.975, df.welch)

&gt; p &lt;- 2*(1-pt(t.welch,df.welch))

we use .975 rather than .95 and we have to multiply by 2 because R automatically assumes we are looking at one tail.




<ol start="4">

 <li>Before using R’s built-in commands for carrying out t-tests, look again at the R help file. You can type ?t.test in the <strong>Console </strong>window, and hit return. This will open the help file, which shows the default language:</li>

</ol>

t.test(x, y = NULL, alternative = c(“two.sided”, “less”, “greater”),

mu = 0, paired = FALSE, var.equal = FALSE, conf.level = 0.95)




There are six key elements you will need to type (unless you allow some to remain at default):

sample one’s response variable                          x

sample two’s response variable                         y

what type of t-test you wish to carry out             alternative = c(“”)

the null hypothesis value                                mu =

whether equal variances are assumed                  var.equal =

whether the data are paired                                paired =

Five of these elements are obvious: the variables that you are using are the sugar contents of children’s cereals (x = children$Sugar) and adults’ cereals (y = adults$Sugar); the test is non-directional (alternative = c(“two.sided”)); the null hypothesis is zero difference between populations (mu = 0); and the data are not paired in any way (paired = FALSE).




One element is your choice. You can assume the populations have unequal variances (var.equal = FALSE), or you can pool the data before calculating the pooled population variance, which is often recommended when each sample is small (var.equal = TRUE). The former is the default, but it requires R to calculate degrees of freedom using a complicated formula (to address the “Behrens-Fisher problem”) rather than just using <em>n</em><sub>1 </sub>+ <em>n</em><sub>2 </sub>– 2.




With the data frames we have already created and the code for t.test shown above, you can test whether the amounts of sugar in the children’s cereal sample is equal to the amounts of sugar in the adult cereals. Here is the code I would use to run the test:

&gt; t.test(children$Sugar, adults$Sugar, alternative=”two.sided”)

this formulation uses the original dataset (cereal) and uses the variable Intended.for as a factor variable, circumventing the steps of creating separate data frames.




We are about to move to the other dataset. You should consider clearing the existing data from the Environment by typing rm(list=ls()), and then clear the console by typing Ctrl-<em>l</em>.




<ol>

 <li><strong> Effectiveness of Treatments for Eating Disorders</strong></li>

</ol>




A study reported by Brian Everett (University College, London) examined the effectiveness of two different treatments for anorexia. Family therapy stresses the social causes of eating disorders and attempts to achieve desirable behavior by improving a family’s norms. Cognitive behavioral therapy stresses the psychological causes of eating disorders and attempts to achieve desirable behavior by retraining and focusing the individual’s mental powers.




<ol start="5">

 <li>Load the anorexia dataset, typing the following line, changing the directory if needed:</li>

</ol>

&gt; experiment &lt;- read.csv(“C:/anorexia.csv”)




As before, let us start by making three separate data frames according to the variable experiment.therapy. To examine the subjects’ weights after therapy, you can graph horizontal box-and-whisker plots.




<ol start="6">

 <li>The first test we will carry out is a comparison of family therapy to the control group. Using the built-in R commands for carrying out a difference-of-means test, :</li>

</ol>

&gt; t.test(treatment.f$after, control$after)

You should notice that R made three assumptions by default: R presents the test statistic for a two-tailed test, R uses a 95% confidence level, and R uses the unequal variances test.




R reports the mean of each variable (average weight in the family therapy group = ______; average weight in the control group = _____ ) and the difference-of-means <em>t</em> statistic ( = _____ ), but does not report the difference of means or the standard error of the difference of means.




<ol start="7">

 <li>Suppose we are actually interested in the directional hypothesis that family therapy leads to higher post-treatment weight than the control. You can run this test with only a slight change:</li>

</ol>

&gt; t.test(treatment.f$after, control$after, alt=”greater”)

Notice that I shortened the word alternative to alt. Answer the following three questions:

Did the <em>t</em> statistic change? Did the confidence interval change? Did the <em>p</em> value change?




<ol start="8">

 <li>8. While it is good to know that subjects in the family treatment group had higher weight after treatment than those in the control group, it is impossible to say that the treatment had a causal relationship, because it is conceivable that subjects in the treatment group had higher weight before treatment. The research design utilized for questions 6 and 7 is called the “post-test only with non-equivalent control groups” design. An alternative is the “pre-test and post-test with no control group” design, which simply examines whether weight changed from before treatment to after treatment. You can carry out this comparison using a two-sample test:</li>

</ol>

&gt; t.test(treatment.f$after, treatment.f$before, alt=”greater”)




Is there a statistically significant difference between pre- and post-treatment weights?




Compare these results to a one-sample t test using R’s built-in t.test command.

&gt; treatment.f$delta &lt;- treatment.f$after – treatment.f$before

&gt; t.test(treatment.f$delta, mu=0, alt=”greater”)




Is the mean difference (reported as “mean of x” after the One Sample test) equal to the difference of means (subtracting “mean of y” from “mean of x” reported after the Two Sample test?)

Is the <em>t</em> statistic for the One Sample test equal to the <em>t</em> statistic for the Two Sample test? <em>Why not?</em>




Data from an experiment with the same subjects having the response variable measured two times (before and after treatment) are an example of paired data. To calculate the correct standard error of the difference of means, you need to take into account that the values of the response variable (in this case, the subject’s weight) are correlated! Patients who start heavier typically also finish heavier. Find the correlation between pre- and post-treatment weights by typing:

&gt; cor(treatment.f$after, treatment.f$before)




The paired-samples difference of means test requires adding a statement to the code:

&gt; t.test(treatment.f$after, treatment.f$before, alt=”greater”,

paired = TRUE)




Check again: is the <em>t</em> statistic for the Paired Sample test equal to the <em>t</em> statistic for the One Sample test that you carried out earlier?




<ol start="9">

 <li>The final task will be to perform what is called a differences-in-differences test. Because we have measures of pre- and post-treatment weight for the control group also, the most persuasive test is to compare the changes in weights for subjects in the treatment group against the changes in weights for subjects in the control group, which must be computed before the <em>t </em>test:</li>

</ol>

&gt; control$delta &lt;- control$after – control$before

&gt; t.test(treatment.f$delta, control$delta, alt=”greater”)




Is there a statistically significant difference between the average weight change in the treatment group (= _____ ) and the average weight change in the control group (= _____ ) ?




<ol start="10">

 <li>On your own, repeat 6–9 using the cognitive behavioral therapy data. Answer the following:</li>

</ol>




Is the average post-treatment weight higher for subjects receiving cognitive behavioral therapy higher than average post-treatment weight for subjects in the control group? Is the difference large enough to attain statistical significance?




Did cognitive behavioral therapy increase the average weight of subjects? Is the difference large enough to attain statistical significance?




Is the difference between average weight gain of subjects receiving cognitive behavioral therapy larger than the average weight gain of subjects in the control group? Is the difference large enough to attain statistical significance?




Next week, we will use all three groups to perform one-way ANOVA.




<ol start="11">

 <li>To clear the <strong>Environment</strong>, type rm(list=ls()) or click on the broom icon.</li>

</ol>

To clear the <strong>Console</strong> window, type Ctrl-<em>l</em>