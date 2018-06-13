# Strings

## Regular Expressions

### Intro

Resources:
More detailed phone number example: https://stackoverflow.com/questions/16699007/regular-expression-to-match-standard-10-digit-phone-number
Excellent cheat sheet: https://www.rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf
More than you ever wanted: https://www.regular-expressions.info/tutorial.html

A great package for regex in r is Hadley Wickham's 'stringr' package. We'll be using this often.

```r
library(stringr)
```

The main idea behind regular expressions (or regex) is to find blocks of text that match a certain pattern. This pattern can be as simple as a single letter or number, or as complicated as an entire address. Some simple examples.


```r
allchars = '1234567890abcdefghijklmnopqrstuvwxyz'
str_extract_all(allchars, '1')
```

```
## [[1]]
## [1] "1"
```

```r
str_extract_all(allchars, '12')
```

```
## [[1]]
## [1] "12"
```

```r
str_extract_all(allchars, '13')
```

```
## [[1]]
## character(0)
```

```r
str_extract_all(allchars, 'a')
```

```
## [[1]]
## [1] "a"
```
A character class is a set of characters, any one of which will match.

```r
str_extract_all(allchars, '[12345]')
```

```
## [[1]]
## [1] "1" "2" "3" "4" "5"
```

```r
str_extract_all(allchars, '[abcde]')
```

```
## [[1]]
## [1] "a" "b" "c" "d" "e"
```

```r
str_extract_all(allchars, '[[:digit:]]')
```

```
## [[1]]
##  [1] "1" "2" "3" "4" "5" "6" "7" "8" "9" "0"
```

What if you wanted all numbers? There are a number of built in classes to save on typing:
'\\d' is all digits
'[[:lower:]]' is all lower case letters
'\\s' is all white space
'.' is anything at all and is known as the wildcard


```r
str_extract_all(allchars, '[[:digit:]]')
```

```
## [[1]]
##  [1] "1" "2" "3" "4" "5" "6" "7" "8" "9" "0"
```

```r
str_extract_all(allchars, '[[:lower:]]')
```

```
## [[1]]
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
```

```r
str_extract_all(allchars, '\\s')
```

```
## [[1]]
## character(0)
```
We'll use a motivating example to introduce some other thing you can do with regular expressions. Let's say you wanted to extract all the phone numbers in a body of text.

```r
exampleText = "Some phone numbers are 7608675309, perhaps also 403-596-4038. Some people use periods in their numbers, so 402.367.5039. We can also have things like 304 385 1029 or (760)-581-3957. How can we extract all of these?"
```

We'll start simple and try to build expressions that are general enough to get all the numbers in the above paragraph. The first number is the easiest, how can we match on any 10 numbers?

```r
str_extract_all(exampleText, '\\d\\d\\d\\d\\d\\d\\d\\d\\d\\d')
```

```
## [[1]]
## [1] "7608675309"
```
More succinctly we can use a quantifier to match a given number of times: {n} will match what it preceedes it n times.

```r
str_extract_all(exampleText, '\\d{10}')
```

```
## [[1]]
## [1] "7608675309"
```
Other useful quantifiers are \* to match at least 0 times, + to match at least once, and {n,m} to match between n and m times. We can use \* to get the first two numbers. 

```r
str_extract_all(exampleText, '\\d{3}-*\\d{3}-*\\d{4}')
```

```
## [[1]]
## [1] "7608675309"   "403-596-4038"
```
Sometimes the number groups are separated by -, sometimes by '.'. What happens if we try to get the number separated by periods?

```r
str_extract_all(exampleText, '\\d{3}.*\\d{3}.*\\d{4}')
```

```
## [[1]]
## [1] "7608675309, perhaps also 403-596-4038. Some people use periods in their numbers, so 402.367.5039. We can also have things like 304 385 1029 or (760)-581-3957"
```
What happened? Since the period is the wildcard it matched all kinds of things. If we want to treat the period literally, instead of as a special character, we need to escape it with double backslashes:


```r
str_extract_all(exampleText, '\\d{3}\\.*\\d{3}\\.*\\d{4}')
```

```
## [[1]]
## [1] "7608675309"   "402.367.5039"
```
Now it's treating period as a literal character. We can combine the expression matching period to the one matching - using character classes:

```r
str_extract_all(exampleText, '\\d{3}[-\\.]*\\d{3}[-\\.]*\\d{4}')
```

```
## [[1]]
## [1] "7608675309"   "403-596-4038" "402.367.5039"
```
The pattern '[-\\.]*' will match either - or period at least 0 times. If you epect phone numbers with some other delimiter, you can toss it in that character class just the same. We can use the same idea to capture the space separated one.

```r
str_extract_all(exampleText, '\\d{3}[-\\.\\s]*\\d{3}[-\\.\\s]*\\d{4}')
```

```
## [[1]]
## [1] "7608675309"   "403-596-4038" "402.367.5039" "304 385 1029"
```
The last challenge is getting the number with the parentheses. We can use the same idea. Note that since parenthese are a special character we need to escape them.

```r
str_extract_all(exampleText, '\\(*\\d{3}\\)*[-\\.\\s]*\\d{3}[-\\.\\s]*\\d{4}')
```

```
## [[1]]
## [1] "7608675309"     "403-596-4038"   "402.367.5039"   "304 385 1029"  
## [5] "(760)-581-3957"
```

### Basic Web Scraping Example

Regular expressions are very useful in the context of web scraping. This example is from a small scraping job we did on the Pulaski Board of Supervisors website to extract their meeting minutes. First, we should have a look at their website to get a lay of the land. It's at: http://www.pulaskicounty.org/Board-of-Supervisors.html

```r
library(XML)
rooturl = "http://www.pulaskicounty.org/Board-of-Supervisors.html"
bosMainPage = htmlParse(rooturl)
bosMainPage
```

```
## <!DOCTYPE html>
## <html lang="en">
## <head>
## <title>Pulaski County</title>
## <link rel="shortcut icon" type="image/png" href="images/favicon.png">
## <meta charset="utf-8">
## <link href="css/reset.css" rel="stylesheet">
## <link href="css/layout.css" rel="stylesheet">
## <link href="css/style.css" rel="stylesheet">
## <!--[if lt IE 7]>
##       <script src="http://info.template-help.com/files/ie6_warning/ie6_script_other.js"></script>
##    <![endif]--><!--[if IE]>
##       <script src="js/html5.js"></script>
##    <![endif]-->
## </head>
## <body id="page2">
## <div id="main">
##   <!-- header -->
##   <header><h1><a href="index.html"><span>County government</span></a></h1>
##     <h2>Orchestrated By Nature™</h2>
## <img class="Nature" src="images/PulaskiNature.png"><form id="search_form">
##       <input type="text" value="Searching ..." onblur="if(this.value=='') this.value='Searching ...'" onfocus="if(this.value =='Searching ...' ) this.value=''"><input type="image" src="images/search_button.jpg" alt="Start searching">
## </form> 
## 
##   </header><!-- nav --><nav><ul>
## <li class="first"><a href="index.html"><span>Home</span></a></li>
##       <li class="active"><a href="Departments.html"><span>Departments</span></a></li>
##       
##       <li><a href="Things-to-Do.html"><span>Attractions</span></a></li>
##       <li><a href="Employees-Employment.html"><span>Employees/Employment</span></a></li>
##       <li><a href="Companies.html"><span>Economic Development</span></a></li>
##       <li class="last"><a href="Documents-Forms.html"><span>Documents/Forms</span></a></li>
##     </ul></nav><!-- content --><section id="content"><div id="indent">
##       <div class="wrapper">
##         <article class="col-1"><div class="wrapper">
##               <div class="block-1">
##                 <div class="box">
##                   <div class="inner">
##               <h3>Categories:</h3>
##               <ul class="list2">
## <li class="light"><a target="_blank" href="https://www.boarddocs.com/va/copva/Board.nsf">BoardDocs</a></li>
## 				<li><a target="_blank" href="http://www.pulaskichamber.info/">Chamber of Commerce</a></li>
## 				<li class="light"><a target="_blank" href="files/Churches.pdf">Churches</a></li>
##                 <li><a href="telephone-numbers.htm">Contact Directory</a></li>
##                 <li class="light"><a target="_blank" href="mailto:info@pulaskicounty.org">Contact Us</a></li>
##                 <li><a target="_blank" href="https://www.facebook.com/PulaskiVirginia">County Blog</a></li>
##                 <li class="light"><a href="Employment-Opportunities.html">Employment Opportunities</a></li>
##                 <li><a target="_blank" href="GIS.html">GIS</a></li>
##                 <li class="light"><a target="_blank" href="http://www.pclibs.org/">Library</a></li>
##                 <li><a href="Links.html">Links</a></li>
##                 <li class="light"><a target="_blank" href="http://www.pcva.us/">Schools</a></li>
##                 <li><a target="_blank" href="http://www.dublintown.org/">Town of Dublin</a></li>
##                 <li class="light"><a target="_blank" href="http://www.pulaskitown.org/">Town of Pulaski</a></li>
##                 <li><a href="Website-Links-Statement.html">Web Site Links Statement</a></li>
##               </ul>
## </div>
##                 </div>
##               </div>
##               <div class="block-2">
##                 <h3>Pulaski County Board of Supervisors</h3>
## 	Andy McCready, Chairman	<br>			
## 	Massie District		<br>							
## 	Administration Office: 540-980 -7705<br>
## 	Business:  540-980-8700<br>	
## 	e-mail:	<a href="mailto:amc@pulaskicounty.org"> amc@pulaskicounty.org</a><br><br>	
## 	
## 	Charles R. Bopp, Vice-Chairman<br>		
## 	Robinson District<br>	
## 	Home:  540-980-2757 <br>	
## 	e-mail:	<a href="mailto:cbopp@pulaskicounty.org">cbopp@pulaskicounty.org</a><br><br>
## 	
## 	Joseph Guthrie<br>	
## 	Cloyd District<br>	
## 	Home:	540-674-2423<br>	
## 	e-mail:	<a href="mailto:jwguthrie@pulaskicounty.org">jwguthrie@pulaskicounty.org</a><br><br>
## 	
## 	Ranny E. O’Dell	<br>			 
## 	Ingles District		<br>						
## 	Business:  540-980-3407<br>	
## 	e-mail:	   <a href="mailto:odellroho289@aol.com"> odellroho289@aol.com</a><br><br>	
## 
## 	Dean K. Pratt		<br>		
## 	Draper District<br>	
## 	Home:	540-980-7150<br>	
## 	e-mail:	<a href="mailto:dpratt@pulaskicounty.org"> dpratt@pulaskicounty.org</a><br><br>		
## 
## 	Timothy Kirtner<br>	
## 	County Attorney<br>	
## 	c/o Gilmer Sadler Law Offices<br>	
## 	P. O. Box 878<br>	
## 	Pulaski, VA  24301<br>	
## 	Work:	540-980-1360<br>	
## 	e-mail:	<a href="mailto:tkirtner@gsish.com">tkirtner@gsish.com</a>
## 	        <br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
## 			<h3>Procedures, Vision, and Goals Statement</h3>
## 			<a target="_blank" href="bos/Board%20of%20Supervisors%20Procedures.pdf">Board of Supervisiors Procedures regarding citizens comments</a>
## 			<a target="_blank" href="bos/2004-2007%20Board%20Goals.htm">County Vision and Goals Statement</a> 
## 			<br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
##                   <h3>Citizen Comments</h3>
## 				  <a href="files/Citizen%20Comments%20-%20Highway%20Matters.pdf">Highway Matters</a><br><br><br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>                   
## 
## 				  <h3>Noise Ordinance</h3>
## 				  <a href="files/Noise%20Ordinance%20Final.pdf">Noise Ordinance</a><br><br><br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>        
## 				  
## 				  <h3>2016-2017 Budget</h3>
## 					<a href="Budget/FY%202016%202017/Budget%20Advertisement.pdf">Budget Advertisement</a><br><br><a href="Budget/FY%202016%202017/Revenue%20Chart.pdf">Revenue Chart
## <br></a>
## 					<a href="Budget/FY%202016%202017/Revenue%20History%20Chart.pdf">Revenue History Chart
## </a><br><a href="Budget/FY%202016%202017/Revenue%20Chart.pdf">Revenue Chart
## </a><br><br><a href="Budget/FY%202016%202017/Expenditure%20Chart.pdf">Expenditure Chart
## </a><br><a href="Budget/FY%202016%202017/Expenditure%20Summary.pdf">Expenditure Summary</a><br><a href="Budget/FY%202016%202017/Expenditure%20History%20Chart.pdf">Expenditure History Chart
## </a><br><br><a href="Budget/FY%202016%202017/BOS%202016-17%20CIP%20BY%20PRIORITY%20RANK.pdf">Capital Improvement Plans for the Board of Supervisors </a><br><a href="Budget/FY%202016%202017/PSA%202016-17%20CIP%20BY%20PRIORITY%20RANK.pdf">Capital Improvement Plans for the Public Service Authority</a>
## <br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>				 
## 
##  <h3>2015-2016 Budget</h3>
## 					<a href="Budget/FY%202015%202016/Budget%20Advertisement.pdf">Budget Advertisement</a><br><a href="Budget/FY%202015%202016/Revenue%20Projections.pdf">Revenue Projections
## <br></a><br><a href="Budget/FY%202015%202016/Revenue%20History%20Chart.pdf">Revenue History Chart
## </a><br><a href="Budget/FY%202015%202016/Projected%20Revenues.pdf">Projected Revenues
## </a><br><a href="Budget/FY%202015%202016/Expenditure%20Summary.pdf">Expenditure Summary</a><br><br><a href="Budget/FY%202015%202016/Expenditure%20History%20Chart.pdf">Expenditure History Chart
## </a><br><a href="Budget/FY%202015%202016/Proposed%20Expenditures.pdf">Projected Expenditures
## </a> 
## <br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
##                   <h3>Regional Tax Rate Comparisons</h3>
## <a href="Budget/FY%202015%202016/Comparison%20of%20Real%20Estate%20Rates.pdf">Comparison of Real Estate Rates
## </a><br><a href="Budget/FY%202015%202016/Personal%20Property%20Tax%20Rate%20Comparison.pdf">Personal Property Taxes
## <br></a><br><br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
## 			<h3>Pulaski County Audits</h3>
## 			<a target="_blank" href="Audits.html">County Audit Reports</a><br><br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
## 			<h3>Pulaski County Goals</h3>
## 			<a target="_blank" href="files/Goals-Project.pdf">Goals and Projects</a><br><br><p style="border-bottom:1px solid rgba(100, 100, 100, 0.05);"></p>
## 			<h3>Roads</h3>
## 			<a target="_blank" href="bos/2008%20Six-Year%20Plan,%20and%20Waiting%20lists.pdf">Six Year Road Improvement Plan</a><br><a target="_blank" href="bos/2008%20Six-Year%20Waiting%20lists.pdf">Six Year Road Improvement Waiting List</a><br><a target="_blank" href="files/Road%20Names.pdf">County Road Names</a>
##                 <div class="image_list  extra">
##                   <div class="clear"></div>
##                 </div>
##                 <div class="clear"></div>
##               </div>
##               <div class="clear"></div>
##             </div>
##         </article><article class="col-2"><div class="box indent">
##             <div class="inner">
##               <h3>Departments</h3>
##               <ul class="list2">
## <li class="light"><a target="_blank" href="files/Animal%20Control.pdf">Animal Control</a></li>
## 				<li><a href="Board-of-Supervisors.html">Board of Supervisors</a></li>
##                 <li class="light"><a href="Building-Department.html">Building Department</a></li>
##                 <li><a href="Commissioner-of-the-Revenue.html">Commissioner of the Revenue</a></li>
##                 <li class="light"><a href="Courts.html">Courts</a></li>
##                 <li><a href="Emergency-Management.html">Emergency Management</a></li>
##                 <li class="light"><a href="Planning-Zoning.html">Planning and Zoning Office</a></li>
##                 <li><a href="Recreation.html">Recreation</a></li>
##                 <li class="light"><a href="Registrar.html">Registrar</a></li>
##                 <li><a href="Treasurer's-Office.html">Treasurer's Office</a></li>
##                 <li class="light"><a href="Utilities-PSA.html">Utilities - PSA</a></li>
##                 
##               </ul>
## </div>
##           </div>
##           <div class="inner">
## 			   <p>The Pulaski County Board of Supervisors meet regularly on the fourth Monday of each month beginning at 7:00 pm in the Board Room of the County Administration Building located at 143 Third Street, N.W. in the town of Pulaski.</p>
## 		       <p>Board Minutes are not posted until officially approved by the Board of Supervisors. This Approval is normally completed at the next meeting of the board. </p>
## 			   <h3 class="extra red">Minutes and Agendas</h3>
##  	               <p class="aligncenter2">
## 				   <a href="Board-of-Supervisors-Minutes-2017.html">2017</a><br><a href="Board-of-Supervisors-Minutes-2016.html">2016</a><br><a href="Board-of-Supervisors-Minutes-2015.html">2015</a><br><a href="Board-of-Supervisors-Minutes-2014.html">2014</a><br><a href="Board-of-Supervisors-Minutes-2013.html">2013</a><br><a href="Board-of-Supervisors-Minutes-2012.html">2012</a><br><a href="Board-of-Supervisors-Minutes-2011.html">2011</a><br><a href="Board-of-Supervisors-Minutes-2010.html">2010</a><br><a href="Board-of-Supervisors-Minutes-2009.html">2009</a><br><a href="Board-of-Supervisors-Minutes-2008.html">2008</a><br><a href="Board-of-Supervisors-Minutes-2007.html">2007</a><br><a href="Board-of-Supervisors-Minutes-2006.html">2006</a><br><a href="Board-of-Supervisors-Minutes-2005.html">2005</a><br><a href="Board-of-Supervisors-Minutes-2004.html">2004</a><br><a href="Board-of-Supervisors-Minutes-2003.html">2003</a><br><a href="Board-of-Supervisors-Minutes-2002.html">2002</a><br><a href="Board-of-Supervisors-Minutes-2000.html">2000</a><br><a href="Board-of-Supervisors-Minutes-2001.html">2001</a><br><a href="Board-of-Supervisors-Minutes-1999.html">1999</a><br><a href="Board-of-Supervisors-Minutes-1998.html">1998</a><br><a href="Board-of-Supervisors-Minutes-1997.html">1997</a><br><a href="Board-of-Supervisors-Minutes-1996.html">1996</a><br><a href="Board-of-Supervisors-Minutes-1995.html">1995</a><br><a href="Board-of-Supervisors-Minutes-1994.html">1994</a><br><a href="Board-of-Supervisors-Minutes-1993.html">1993</a><br><a href="Board-of-Supervisors-Minutes-1992.html">1992</a><br><a href="Board-of-Supervisors-Minutes-1991.html">1991</a><br></p>
## 			<img class="sidebarlogo" src="images/logored.png" alt="county seal" width="150" height="145"><p class="homeimg">County of Pulaski, VA<br>
##               143 3rd ST NW, Suite 1<br>
##               Pulaski VA 24301<br>
##               540-980-7705<br></p>
##           </div>
##         </article>
## </div>
##     </div>
##   </section><!-- footer --><footer><div class="inner">
##       <div class="wrapper">
##         <div class="col-1">
##           <p class="copy"><strong>Pulaski County</strong>  © 2014<br><a target="_blank" href="https://www.facebook.com/PulaskiVirginia/timeline?ref=page_internal">
## 			<img src="images/facebooksmall.png"></a></p>
##           <div>
##           </div>
##         </div>
##         <div class="col-2">
##           <ul class="footer_list">
## <li><a target="_blank" href="https://www.boarddocs.com/va/copva/Board.nsf">BoardDocs</a></li>
##             <li><a target="_blank" href="http://www.pulaskichamber.info/">Chamber of Commerce</a></li>
##             <li><a target="_blank" href="files/Churches.pdf">Churches</a></li>
##             <li><a href="telephone-numbers.htm">Contact Directory</a></li>
##           </ul>
## </div>
##         <div class="col-3">
##           <ul class="footer_list">
## <li><a target="_blank" href="mailto:info@pulaskicounty.org">Contact Us</a></li>
##             <li><a target="_blank" href="https://www.facebook.com/PulaskiVirginia">County Blog</a></li>
##             <li><a target="_blank" href="GIS.html">GIS</a></li>
##             <li><a target="_blank" href="http://www.pclibs.org/">Library</a></li>
##           </ul>
## </div>
##         <div class="col-4">
##           <ul class="footer_list">
## <li><a href="links.html">Links</a></li>
##             <li><a target="_blank" href="http://www.pcva.us/">Schools</a></li>
##             <li><a target="_blank" href="http://www.dublintown.org/">Town of Dublin</a></li>
##             <li><a target="_blank" href="http://www.pulaskitown.org/">Town of Pulaski</a></li>
##           </ul>
## </div>
##         </div>
##       </div>
##     </footer>
## </div>
##   
## 
## </body>
## </html>
## 
```

```r
links = xpathSApply(bosMainPage, "//a/@href")
links
```

```
##                                                                  href 
##                                                          "index.html" 
##                                                                  href 
##                                                          "index.html" 
##                                                                  href 
##                                                    "Departments.html" 
##                                                                  href 
##                                                   "Things-to-Do.html" 
##                                                                  href 
##                                           "Employees-Employment.html" 
##                                                                  href 
##                                                      "Companies.html" 
##                                                                  href 
##                                                "Documents-Forms.html" 
##                                                                  href 
##                        "https://www.boarddocs.com/va/copva/Board.nsf" 
##                                                                  href 
##                                     "http://www.pulaskichamber.info/" 
##                                                                  href 
##                                                  "files/Churches.pdf" 
##                                                                  href 
##                                               "telephone-numbers.htm" 
##                                                                  href 
##                                       "mailto:info@pulaskicounty.org" 
##                                                                  href 
##                            "https://www.facebook.com/PulaskiVirginia" 
##                                                                  href 
##                                       "Employment-Opportunities.html" 
##                                                                  href 
##                                                            "GIS.html" 
##                                                                  href 
##                                              "http://www.pclibs.org/" 
##                                                                  href 
##                                                          "Links.html" 
##                                                                  href 
##                                                 "http://www.pcva.us/" 
##                                                                  href 
##                                          "http://www.dublintown.org/" 
##                                                                  href 
##                                         "http://www.pulaskitown.org/" 
##                                                                  href 
##                                        "Website-Links-Statement.html" 
##                                                                  href 
##                                        "mailto:amc@pulaskicounty.org" 
##                                                                  href 
##                                      "mailto:cbopp@pulaskicounty.org" 
##                                                                  href 
##                                  "mailto:jwguthrie@pulaskicounty.org" 
##                                                                  href 
##                                         "mailto:odellroho289@aol.com" 
##                                                                  href 
##                                     "mailto:dpratt@pulaskicounty.org" 
##                                                                  href 
##                                           "mailto:tkirtner@gsish.com" 
##                                                                  href 
##                             "bos/Board of Supervisors Procedures.pdf" 
##                                                                  href 
##                                       "bos/2004-2007 Board Goals.htm" 
##                                                                  href 
##                        "files/Citizen Comments - Highway Matters.pdf" 
##                                                                  href 
##                                     "files/Noise Ordinance Final.pdf" 
##                                                                  href 
##                        "Budget/FY 2016 2017/Budget Advertisement.pdf" 
##                                                                  href 
##                               "Budget/FY 2016 2017/Revenue Chart.pdf" 
##                                                                  href 
##                       "Budget/FY 2016 2017/Revenue History Chart.pdf" 
##                                                                  href 
##                               "Budget/FY 2016 2017/Revenue Chart.pdf" 
##                                                                  href 
##                           "Budget/FY 2016 2017/Expenditure Chart.pdf" 
##                                                                  href 
##                         "Budget/FY 2016 2017/Expenditure Summary.pdf" 
##                                                                  href 
##                   "Budget/FY 2016 2017/Expenditure History Chart.pdf" 
##                                                                  href 
##            "Budget/FY 2016 2017/BOS 2016-17 CIP BY PRIORITY RANK.pdf" 
##                                                                  href 
##            "Budget/FY 2016 2017/PSA 2016-17 CIP BY PRIORITY RANK.pdf" 
##                                                                  href 
##                        "Budget/FY 2015 2016/Budget Advertisement.pdf" 
##                                                                  href 
##                         "Budget/FY 2015 2016/Revenue Projections.pdf" 
##                                                                  href 
##                       "Budget/FY 2015 2016/Revenue History Chart.pdf" 
##                                                                  href 
##                          "Budget/FY 2015 2016/Projected Revenues.pdf" 
##                                                                  href 
##                         "Budget/FY 2015 2016/Expenditure Summary.pdf" 
##                                                                  href 
##                   "Budget/FY 2015 2016/Expenditure History Chart.pdf" 
##                                                                  href 
##                       "Budget/FY 2015 2016/Proposed Expenditures.pdf" 
##                                                                  href 
##             "Budget/FY 2015 2016/Comparison of Real Estate Rates.pdf" 
##                                                                  href 
##       "Budget/FY 2015 2016/Personal Property Tax Rate Comparison.pdf" 
##                                                                  href 
##                                                         "Audits.html" 
##                                                                  href 
##                                             "files/Goals-Project.pdf" 
##                                                                  href 
##                       "bos/2008 Six-Year Plan, and Waiting lists.pdf" 
##                                                                  href 
##                                 "bos/2008 Six-Year Waiting lists.pdf" 
##                                                                  href 
##                                                "files/Road Names.pdf" 
##                                                                  href 
##                                            "files/Animal Control.pdf" 
##                                                                  href 
##                                           "Board-of-Supervisors.html" 
##                                                                  href 
##                                            "Building-Department.html" 
##                                                                  href 
##                                    "Commissioner-of-the-Revenue.html" 
##                                                                  href 
##                                                         "Courts.html" 
##                                                                  href 
##                                           "Emergency-Management.html" 
##                                                                  href 
##                                                "Planning-Zoning.html" 
##                                                                  href 
##                                                     "Recreation.html" 
##                                                                  href 
##                                                      "Registrar.html" 
##                                                                  href 
##                                             "Treasurer's-Office.html" 
##                                                                  href 
##                                                  "Utilities-PSA.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2017.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2016.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2015.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2014.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2013.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2012.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2011.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2010.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2009.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2008.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2007.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2006.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2005.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2004.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2003.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2002.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2000.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-2001.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1999.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1998.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1997.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1996.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1995.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1994.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1993.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1992.html" 
##                                                                  href 
##                              "Board-of-Supervisors-Minutes-1991.html" 
##                                                                  href 
## "https://www.facebook.com/PulaskiVirginia/timeline?ref=page_internal" 
##                                                                  href 
##                        "https://www.boarddocs.com/va/copva/Board.nsf" 
##                                                                  href 
##                                     "http://www.pulaskichamber.info/" 
##                                                                  href 
##                                                  "files/Churches.pdf" 
##                                                                  href 
##                                               "telephone-numbers.htm" 
##                                                                  href 
##                                       "mailto:info@pulaskicounty.org" 
##                                                                  href 
##                            "https://www.facebook.com/PulaskiVirginia" 
##                                                                  href 
##                                                            "GIS.html" 
##                                                                  href 
##                                              "http://www.pclibs.org/" 
##                                                                  href 
##                                                          "links.html" 
##                                                                  href 
##                                                 "http://www.pcva.us/" 
##                                                                  href 
##                                          "http://www.dublintown.org/" 
##                                                                  href 
##                                         "http://www.pulaskitown.org/"
```

The above code downloads the html for the Board's website and saves it in bosMainPage. The next line extracts all the links leading away from that page. It looks like a promising place to look is in the links such as 'Board-of-Supervisors-Minutes-1992.html'. We can get just those links with the following:


```r
minutesLinks = grep("Minutes", links, value = T)
fullMinutesUrl = paste0("http://www.pulaskicounty.org/", minutesLinks)
fullMinutesUrl
```

```
##  [1] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2017.html"
##  [2] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2016.html"
##  [3] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2015.html"
##  [4] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2014.html"
##  [5] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2013.html"
##  [6] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2012.html"
##  [7] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2011.html"
##  [8] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2010.html"
##  [9] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2009.html"
## [10] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2008.html"
## [11] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2007.html"
## [12] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2006.html"
## [13] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2005.html"
## [14] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2004.html"
## [15] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2003.html"
## [16] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2002.html"
## [17] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2000.html"
## [18] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-2001.html"
## [19] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1999.html"
## [20] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1998.html"
## [21] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1997.html"
## [22] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1996.html"
## [23] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1995.html"
## [24] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1994.html"
## [25] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1993.html"
## [26] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1992.html"
## [27] "http://www.pulaskicounty.org/Board-of-Supervisors-Minutes-1991.html"
```
The function grep is a base r function that returns strings which contain a pattern. We used it above to take all the urls on the main page and extract only the ones which contain 'Minutes', since those are the ones that contain links to the meeting minutes.

Now we have all the urls to the pages where the minutes are kept. To work with all of them we could use a loop or an apply function, but we'll just do one for illustration. We want to go to the page, find all the pdf's with the minutes, and download them. A natural idea is to find everything with '.pdf', but observe:

```r
minutePage2016 = htmlParse(fullMinutesUrl[2])
pdfLinks = grep(".pdf", xpathSApply(minutePage2016, "//a/@href"), value = T)
pdfLinks
```

```
##                                                             href 
##                                             "files/Churches.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/01 18 minutes organizational.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/01 25 minutes regular.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/02 16 minutes budget.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/02 22 minutes regular.pdf" 
##                                                             href 
##           "bos/Minutes and Agendas/2016/02 26 minutes joint.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/02 29 minutes budget.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/03 07 minutes budget.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/03 14 minutes budget.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/03 21 minutes budget.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/03 28 minutes regular.pdf" 
##                                                             href 
##           "bos/Minutes and Agendas/2016/03 31 minutes joint.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/04 04 minutes budget.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/04 11 minutes budget.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/04 25 minutes regular.pdf" 
##                                                             href 
##          "bos/Minutes and Agendas/2016/05 02 minutes budget.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/05 23 minutes regular.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/07 25 minutes regular.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/08 02 minutes closed session.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/08 15 minutes closed session.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/08 22 minutes regular.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/09 07 minutes closed session.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/09 26 minutes regular.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/09 28 minutes closed session.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/10 14 minutes special.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/10 24 minutes regular.pdf" 
##                                                             href 
## "bos/Minutes and Agendas/2016/11 09 minutes electoral board.pdf" 
##                                                             href 
##         "bos/Minutes and Agendas/2016/11 28 minutes regular.pdf" 
##                                                             href 
##  "bos/Minutes and Agendas/2016/11 30 minutes closed session.pdf" 
##                                                             href 
##                                       "files/Animal Control.pdf" 
##                                                             href 
##                                             "files/Churches.pdf"
```

It's always wise to make sure your expressions are getting you the things you want. Here, we have all the meeting pdfs, but also some extra page images. In this case, we can match like so:

```r
pdfLinks = grep("Minutes and Agendas", xpathSApply(minutePage2016, "//a/@href"), value = T)
fullPdfUrl = paste0("http://www.pulaskicounty.org/", pdfLinks)
fullPdfUrl
```

```
##  [1] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/01 18 minutes organizational.pdf" 
##  [2] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/01 25 minutes regular.pdf"        
##  [3] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/02 16 minutes budget.pdf"         
##  [4] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/02 22 minutes regular.pdf"        
##  [5] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/02 26 minutes joint.pdf"          
##  [6] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/02 29 minutes budget.pdf"         
##  [7] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/03 07 minutes budget.pdf"         
##  [8] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/03 14 minutes budget.pdf"         
##  [9] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/03 21 minutes budget.pdf"         
## [10] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/03 28 minutes regular.pdf"        
## [11] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/03 31 minutes joint.pdf"          
## [12] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/04 04 minutes budget.pdf"         
## [13] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/04 11 minutes budget.pdf"         
## [14] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/04 25 minutes regular.pdf"        
## [15] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/05 02 minutes budget.pdf"         
## [16] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/05 23 minutes regular.pdf"        
## [17] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/07 25 minutes regular.pdf"        
## [18] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/08 02 minutes closed session.pdf" 
## [19] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/08 15 minutes closed session.pdf" 
## [20] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/08 22 minutes regular.pdf"        
## [21] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/09 07 minutes closed session.pdf" 
## [22] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/09 26 minutes regular.pdf"        
## [23] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/09 28 minutes closed session.pdf" 
## [24] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/10 14 minutes special.pdf"        
## [25] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/10 24 minutes regular.pdf"        
## [26] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/11 09 minutes electoral board.pdf"
## [27] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/11 28 minutes regular.pdf"        
## [28] "http://www.pulaskicounty.org/bos/Minutes and Agendas/2016/11 30 minutes closed session.pdf"
```

By looping through all the years above and combining the results, we can get a full list of all the urls for the Pulaski Board of Supervisors minutes pdfs. Then, it's a simple application of a loop to go in and download them all for later work.

### Lookaround Examples


```r
naicsAndCompanies = c("Name: Herbalife Ltd; NAICS: 325411, 325412, 424490",                                    
"Name: Sanofi-Aventis SA; NAICS: 325412",                      
"Name: Abbott Laboratories; NAICS: 325411, 325412, 325413, 325620, 334516, 339112",  
"Name: Alexion Pharmaceuticals Inc; NAICS: 325412",    
"Name: Purdue Pharma LP; NAICS: 325412; Name: Transcept Pharmaceuticals Inc; NAICS: 325412",
"Name: Sanofi-Aventis SA; NAICS: 325412")
naicsAndCompanies
```

```
## [1] "Name: Herbalife Ltd; NAICS: 325411, 325412, 424490"                                       
## [2] "Name: Sanofi-Aventis SA; NAICS: 325412"                                                   
## [3] "Name: Abbott Laboratories; NAICS: 325411, 325412, 325413, 325620, 334516, 339112"         
## [4] "Name: Alexion Pharmaceuticals Inc; NAICS: 325412"                                         
## [5] "Name: Purdue Pharma LP; NAICS: 325412; Name: Transcept Pharmaceuticals Inc; NAICS: 325412"
## [6] "Name: Sanofi-Aventis SA; NAICS: 325412"
```
We want a table with company names in one column and NAICS codes in another column. How can we extract the components we want?

Let's focus on the first entry for now. Notice that a semicolon separates the name from the NAICS codes. We can use that to split the string using lookarounds.

(?=X) is the lookahead operator. It looks AHEAD for the pattern X; that is, it matches things that come BEFORE it
(?<=X) is the lookbehind operator. It looks BEHIND for the pattern X; that is, it matches things that come AFTER it
(X) contains the pattern X that comes before the look ahead or after the lookbehind
'.\*' means match the wildcard '.' any number of times '*'.


```r
firstRegexCase = naicsAndCompanies[[1]]
firstRegexCase
```

```
## [1] "Name: Herbalife Ltd; NAICS: 325411, 325412, 424490"
```

```r
company = str_extract(firstRegexCase, "(.*?)(?=;)")
naicsCodes = str_extract(firstRegexCase, "(?<=NAICS: )(.*)")
company
```

```
## [1] "Name: Herbalife Ltd"
```

```r
naicsCodes
```

```
## [1] "325411, 325412, 424490"
```
We're getting there, but not quite. We don't want that annoying 'Name: ' in front of the company. We can achieve that with the following. The lookbehind '(?<=Name: )' will find a place in the string that matches the pattern 'Name: ' and then match what comes next, which in this case is anything (.*).

```r
cleanerCompany = str_extract(company, "(?<=Name: )(.*)")
cleanerCompany
```

```
## [1] "Herbalife Ltd"
```
Note that we could have removed the 'Name: ' directly using a function called gsub, but the lookbehind will be useful later. gsub replaces the pattern in the first argument with the pattern in the second wherever it finds it in the third argument.

```r
gsub("Name: ", "", company)
```

```
## [1] "Herbalife Ltd"
```

Now we can tackle the whole vector. We'll use the same idea of separating names from codes using semicolons, but with two new challenges. 

```r
naicsAndCompanies[[5]]
```

```
## [1] "Name: Purdue Pharma LP; NAICS: 325412; Name: Transcept Pharmaceuticals Inc; NAICS: 325412"
```
This entry has multiple companies in the same line. To get at both of them, we need to use str_extract_all() instead of just str_extract().

```r
str_extract_all(naicsAndCompanies, "(?<=Name: )(.*)(?=;)")
```

```
## [[1]]
## [1] "Herbalife Ltd"
## 
## [[2]]
## [1] "Sanofi-Aventis SA"
## 
## [[3]]
## [1] "Abbott Laboratories"
## 
## [[4]]
## [1] "Alexion Pharmaceuticals Inc"
## 
## [[5]]
## [1] "Purdue Pharma LP; NAICS: 325412; Name: Transcept Pharmaceuticals Inc"
## 
## [[6]]
## [1] "Sanofi-Aventis SA"
```
Whoops, looks like we didn't split the 5th string at the right place! This is because the regular expression by default tries to match the longest string possible. This is called greedy matching. We can disable it by adding '?' after the wild card

```r
companies = str_extract_all(naicsAndCompanies, "(?<=Name: )(.*?)(?=;)")
companies
```

```
## [[1]]
## [1] "Herbalife Ltd"
## 
## [[2]]
## [1] "Sanofi-Aventis SA"
## 
## [[3]]
## [1] "Abbott Laboratories"
## 
## [[4]]
## [1] "Alexion Pharmaceuticals Inc"
## 
## [[5]]
## [1] "Purdue Pharma LP"              "Transcept Pharmaceuticals Inc"
## 
## [[6]]
## [1] "Sanofi-Aventis SA"
```

Now let's do the same for the NAICS codes.


```r
str_extract_all(paste0(naicsAndCompanies), "(?<=NAICS: )(.*?)(?=;)")
```

```
## [[1]]
## character(0)
## 
## [[2]]
## character(0)
## 
## [[3]]
## character(0)
## 
## [[4]]
## character(0)
## 
## [[5]]
## [1] "325412"
## 
## [[6]]
## character(0)
```
What's going on here? This code looks for stuff between 'NAICS: ' and ';', but except for the 5th line there's never a ';' after 'NAICS: '. To get around this, we can use a logical OR operator, along with a special symbol meaning 'end of the string'. Here, the expression '(;|$)' means 'match either ; OR the end of the line;.


```r
allNaics = str_extract_all(paste0(naicsAndCompanies), "(?<=NAICS: )(.*?)(?=(;|$))")
allNaics
```

```
## [[1]]
## [1] "325411, 325412, 424490"
## 
## [[2]]
## [1] "325412"
## 
## [[3]]
## [1] "325411, 325412, 325413, 325620, 334516, 339112"
## 
## [[4]]
## [1] "325412"
## 
## [[5]]
## [1] "325412" "325412"
## 
## [[6]]
## [1] "325412"
```

Finally, we can put it all together and we have a clean company by naics table:

```r
companyNaicsList = data.frame(company = unlist(companies), naicsCodes = unlist(allNaics))
companyNaicsList
```

```
##                         company
## 1                 Herbalife Ltd
## 2             Sanofi-Aventis SA
## 3           Abbott Laboratories
## 4   Alexion Pharmaceuticals Inc
## 5              Purdue Pharma LP
## 6 Transcept Pharmaceuticals Inc
## 7             Sanofi-Aventis SA
##                                       naicsCodes
## 1                         325411, 325412, 424490
## 2                                         325412
## 3 325411, 325412, 325413, 325620, 334516, 339112
## 4                                         325412
## 5                                         325412
## 6                                         325412
## 7                                         325412
```
