# This Github Repository Contains Miscellaneous Data Science Projects

<li>
<strong>“01_data_wrangling_visualization”</strong> demonstrates how I extracted and manipulated multiple data sources and transformed un-structured data into structured data in R with dplyr package and visualize data using ggplot2
</li>

<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/01_data_wrangling_visualization/source_readme/data.PNG" height="450"/>
<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/01_data_wrangling_visualization/out/drug_int_mbs_byMfr.jpeg" height="450"/>
<br>


<li>
<strong>“02_feature_selection_mc_simulation”</strong> demonstrates how I implemented feature engineering, such as, forward selection method to select optimal linear regression models and used Monte Carlo simulation to cross validate a set of classifiers to get robust predictions
      <ul> 
       <li>Implemented parallel computing to increase efficiency of forward selection using utilizing multiple CPU cores</li>
	   <li>Expanded explanatory variable by creating lagged terms, quadratic terms and seasonal effects</li>
       <li>Applied forward selection to select optimal set of explanatory variables for linear regression model</li>
       <li>Performed PCA to solve multicollinearity issue in explanatory variables and applied forward selection to select optimal set of PC variables for linear regression model</li>
       <li>Used Monte Carlo simulations to cross validate two different regression models and performed prediction with robust models</li>
      </ul>
</li>
<br>

<li>
<strong>“03_bayesian_simulation_gibbs_sampling"</strong> demonstrate how I used FGLS to solve solve heteroskedasticity issue and successfully draw 10,000 posterior parameters using algorithms, including EM algorithm, MCMC and Gibbs sampling
</li>
<br>

Key variables in clients’ data sets: 
-	Transaction price: dependent variable in linear regression model 
-	Date of transaction: monthly effect
-	Location of transaction: location effect
-	Customer type: retailer, whole seller,  contractor 
-	Product type: 1/2, 1/4, 3/8 inch 
-	Suppliers

Key explanatory variables (supply, demand, economic variables, these are third party data):
-	Raw gypsum price
-	Recyclable old corrugated containers
-	Electricity price
-	Natural gas price
-	Average hourly earnings in manufacturing 
-	Diesel fuel price
-	Housing units started
-	Existing home sales
-	Total construction spending 
-	US treasury bond interest rate
-	x1, x2, … , x10

Conspiracy indicator (c):
-	A dummy variable, which equals to 1 when transaction date is in conspiracy period 
-	Conspiracy period was pre-defined by attorney’s investigation 

Basic regression model:
-	 Pi =   β0 + β1x1 + β2x2 + … + β10x10 + β11 c  + ε  , ε ~ N(0, σ²)
-	Predicted competitive price = β0 + β1x1 + β2x2 + … + β10x10

Added systematic variation (interaction terms) for explanation variables 
-	[Xi] * [monthly dummies] 
-	[Xi] * [regional dummies] 
-	[Xi] * [customer type] 
-	[Xi] * [supplier] 
-	[Xi] * [product thickness] 

Found optimal interaction terms for each explanation variable using model selection methods with AIC/AICc
-	Theoretically, we need to search 5^10 = 9,765,625 combinations
-	Used iterative feature selection methods with 10-fold cross validation to find early stopping point, aiming to reducing running time of searching for the optimal model 

Applied FGLS in Presence of Heteroscedasticity
-	Heteroscedasticity is a problem because variance of residuals are not constant, which violates OLS regression’s homoscedasticity assumption. Heteroscedasticity could be observed in residuals vs fitted value plot.
-	In the presence of heteroscedasticity, OLS estimators are still unbiased, but it is no longer BLUE (best linear unbiased estimator). The variances of the OLS estimators are biased in this case. Thus, the usual OLS t statistic and confidence intervals are no longer valid for inference problem. This problem can lead you to conclude that a model term is statistically significant when it is actually not significant.
-	One remedy to heteroscedasticity is to use robust covariance matrix. Use of robust covariance matrix leaves the coefficient estimates intact but expands confidence intervals to account for the violated assumption of i.i.d. errors.

(See Theorem 10.1 in Greene (2003)) <br>
var(B̂) = var[B + (x'x)⁻¹x'e]  
       = var[(x'x)⁻¹x'e] 
       = (x'x)⁻¹x' cov(e) x (x'x)⁻¹ 
       = σ²(x'x)⁻¹x' Ω x (x'x)⁻¹
	   
-	Another remedy is to use Feasible GLS. We used covariance of residuals in OLS stage to estimate the error covariance structure and use residual standard deviation to reweight our data. Since we imposed model for σi², we should have smaller variance than OLS's (var(FGLS)<var(OLS)<var(Heteroskedasticity-Robust Estimators))

  <img src="https://github.com/aaronzhuclover/master/blob/master/03_bayesian_simulation_gibbs_sampling/bias_variance.PNG" height="400"/>
  <br>
Then we have <br>
var(εi/σi) = σi²/ σi² = 1 = var(εj/σj), where var(εi) ≠ var(εj) <br>
now we successful solve heteroscedasticity and turn regression into homoscedastic model. 
<br>
<br>
Posterior draws using Gibbs Sampling technique:  <br>
(See Chapter 12 of Bayesian Data Analysis by Andrew Gelman (2014)) <br>

Under Bayesian framework, we have 
-	Mean(β) = B̂ = (x'x)⁻¹ x' y
-	Var(β) = ∑ = σ²(x'x)⁻¹
-	β ~ MVN (B̂, ∑)
-	s² = ∑(pi - Xi B̂)² / N
-	σ² ~ Scale-inv x² (n, s²)
-	cholesky decomposition: ∑ = L’L
-	simulated beta = B̂ + LZ, Z ~ MVN (0, 1)

Applied EM algorithm to find 10 starting points. Applied Gibbs sampling in heteroskedastic model. Gibbs sampler: sequentially drawing from each of the full conditional posteriors eg p(θ1 | θ2, y) and p(θ2 | θ1, y). MCMC was used to simulate a Markov Chain that converges to the posterior distribution. Used gibbs sampling iteratively to draw betas.





<br>
<li>
<strong>“04_data_scraping_redfin"</strong> demonstrates how I automate data scrapping from REDFIN and other websites in rvest and visualize home price changes using ggplot2
</li>

<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/04_data_scraping_redfin/out/sf_home_price.png" height="450"/>
<br>


<li>
<strong>“05_recommendation_system_music"</strong> demonstrates how I used machine learning algorithm, <strong>collaborative filtering</strong> to make music recommendation to users.
<ul>
       <li>visualized a network of relationships among 285 artists based on users’ behavior and customized recommendation lists to users using item based <strong>collaborative filtering</strong> and user based recommendation methods. I also implemented parallel computing to increase recommendation efficiency by 90%</li>
       <li>Created a matrix of artists’ similarity using centered cosine similarity</li>
	   <img src="https://github.com/aaronzhuclover/master/blob/master/05_recommendation_system_music/out/cosine.PNG" height="100"/>
	   <li>Accuracy of recommendation will be improved by including features of artists into similarity calculation and users’ preference will help with user based recommendation methods</li>
	</ul>   

</li>

<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/05_recommendation_system_music/out/wordcloud.png" height="450"/>
<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/05_recommendation_system_music/out/network.png" height="450"/>
<br>



<li><strong>“06_titanic_machine_Learning"</strong> demonstrates how I used machine learning algorithms, including <strong>decision tree, random forest, AdaBoost and Logistic regression</strong> to predict survival on Titanic data
    <ul> 
       <li>Predict survival using decision tree</li>
	   <img src="https://github.com/aaronzhuclover/master/blob/master/06_titanic_machine_Learning/out/decision_tree_readme.PNG" height="450"/>
	   <img src="https://github.com/aaronzhuclover/master/blob/master/06_titanic_machine_Learning/out/overfit.PNG" height="400"/> 
    </ul>
</li>	


<li><strong>“07_twitter_sentiment_analysis"</strong> demonstrates how I used machine learning algorithms, including <strong>Logistic regression, Naive Bayes and SVM</strong> to predict sentiments (negative and positive) of twitter messages
</li>
<br>


<li><strong>“08_dashboard_excel_tableau"</strong> demonstrates how I created dashboard in Excel and Tableau
</li>
<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/08_dashboard_excel_tableau/dashboard_tableau.PNG" height="450"/>
See interaction dashboard in Tableau at https://public.tableau.com/shared/82XHHCJTQ?:showVizHome=no&:embed=true
<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/08_dashboard_excel_tableau/dashboard_excel.PNG" height="450"/>
<br>
See interaction dashboard in Excel at https://1drv.ms/x/s!AopQ7UGmMDONgWc9DuOeMea7WHkh
<br>


<br>
<li>
<strong>“09_fraud_detection_machine_learning”</strong> demonstrates how created Dashboard using VBA and Tableau and predicted frauds in highly unbalanced data by training classifiers, including <strong>logistic regression, neural network and deep learning autoencoders</strong>   
</li>
<br>
<img src="https://github.com/aaronzhuclover/master/blob/master/09_fraud_detection_machine_learning/autoencoder.PNG" height="250"/>
<br>
  <img src="https://github.com/aaronzhuclover/master/blob/master/09_fraud_detection_machine_learning/fraud_dashboard.PNG" height="450"/>
<br>
See interactive dashboard at https://public.tableau.com/views/Fraud_dashboard/FraudDashboard?:showVizHome=no&:embed=true
<br>

