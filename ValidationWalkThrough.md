
Installing the validation package in R and importing the package to the library


```R
install.packages("validate")
library(validate)
```

    also installing the dependency ‘settings’
    
    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done


Importing the data files to the R environment


```R
df<-read.csv("../user/Documents/Mustafa Project/Production/164512.csv")
```

Summary about the data


```R
summary(df)
```


           Date     Production.GW.Gas..MCF.  Disposition 
     01-01-00:  1   272    :  2             272    :  2  
     01-01-01:  1   279    :  2             279    :  2  
     01-01-02:  1   312    :  2             312    :  2  
     01-01-03:  1   3272   :  2             3272   :  2  
     01-01-04:  1   329    :  2             329    :  2  
     01-01-05:  1   336    :  2             336    :  2  
     (Other) :246   (Other):240             (Other):240  
     Production.Condensate..BBL. Disposition.1
     0     :250                  0     :250   
     NO RPT:  2                  NO RPT:  2   
                                              
                                              
                                              
                                              
                                              
                             Operator.Name  Operator.No.   
                                    :247   Min.   : 20528  
     ANADARKO E&P COMPANY LP        :  1   1st Qu.: 20542  
     ANADARKO E&P ONSHORE LLC       :  1   Median :120023  
     CCI EAST TEXAS UPSTREAM LLC    :  1   Mean   :350393  
     RME PETROLEUM COMPANY          :  1   3rd Qu.:714229  
     UNION PACIFIC RESOURCES COMPANY:  1   Max.   :876645  
                                           NA's   :247     
                        Field.Name    Field.No.           X          
                             :247   Min.   :16032174   Mode:logical  
     CARTHAGE (COTTON VALLEY):  5   1st Qu.:16032174   NA's:252      
                                    Median :16032174                 
                                    Mean   :16032174                 
                                    3rd Qu.:16032174                 
                                    Max.   :16032174                 
                                    NA's   :247                      
                                               Search.Criteria.
                                                       :247    
     Date Range: Jan 1993 - May 2018                   :  1    
     District: 06                                      :  1    
     Lease Name: CGU 7, Lease No.: 164512 , Well No.: 8:  1    
     Lease Production and Disposition                  :  1    
     Well Type: Gas                                    :  1    
                                                               


Checking number of columns


```R
print(paste("Number of Columns = ", ncol(df)))
```

    [1] "Number of Columns =  11"



```R
Checking the Formats of the fields
```


```R
sapply(df, class)
```


<dl class=dl-horizontal>
	<dt>Date</dt>
		<dd>'factor'</dd>
	<dt>Production.GW.Gas..MCF.</dt>
		<dd>'factor'</dd>
	<dt>Disposition</dt>
		<dd>'factor'</dd>
	<dt>Production.Condensate..BBL.</dt>
		<dd>'factor'</dd>
	<dt>Disposition.1</dt>
		<dd>'factor'</dd>
	<dt>Operator.Name</dt>
		<dd>'factor'</dd>
	<dt>Operator.No.</dt>
		<dd>'integer'</dd>
	<dt>Field.Name</dt>
		<dd>'factor'</dd>
	<dt>Field.No.</dt>
		<dd>'integer'</dd>
	<dt>X</dt>
		<dd>'logical'</dd>
	<dt>Search.Criteria.</dt>
		<dd>'factor'</dd>
</dl>



We can see that most of the fields are imported as factors and that needs to be fixed in addition to the columns names correction.


```R
colnames(df)<-c('Date','Prod_GW_MCF','Disposition','Prod_Condens_BBL', 'Disposition_1', 'Op_Name', 'Op_No', 'Field_Name','Field_No', 'X','Search')
```

Identify and eliminate the unwanted data


```R
df[c('Field_No', 'X','Search')]<-NULL
```

Adjusting the data types for each column


```R
sapply(df,class)
```


<dl class=dl-horizontal>
	<dt>Date</dt>
		<dd>'factor'</dd>
	<dt>Prod_GW_MCF</dt>
		<dd>'factor'</dd>
	<dt>Disposition</dt>
		<dd>'factor'</dd>
	<dt>Prod_Condens_BBL</dt>
		<dd>'factor'</dd>
	<dt>Disposition_1</dt>
		<dd>'factor'</dd>
	<dt>Op_Name</dt>
		<dd>'factor'</dd>
	<dt>Op_No</dt>
		<dd>'integer'</dd>
	<dt>Field_Name</dt>
		<dd>'factor'</dd>
</dl>




```R
df$Date<-as.Date("10-10-99","%m-%d-%y")
#df[c('Prod_GW_MCF','Disposition','Prod_Condens_BBL','Disposition_1')]<-as.numeric(df[c('Prod_GW_MCF','Disposition','Prod_Condens_BBL','Disposition_1')])
df$Prod_GW_MCF<-as.numeric(df$Prod_GW_MCF)
df$Disposition<-as.numeric(df$Disposition)
df$Prod_Condens_BBL<-as.numeric(df$Prod_Condens_BBL)
df$Disposition_1<-as.numeric(df$Disposition_1)
df$Op_Name<-as.character(df$Op_Name)
df$Field_Name<-as.character(df$Field_Name)
```


```R
sapply(df,class)
```


<dl class=dl-horizontal>
	<dt>Date</dt>
		<dd>'Date'</dd>
	<dt>Prod_GW_MCF</dt>
		<dd>'numeric'</dd>
	<dt>Disposition</dt>
		<dd>'numeric'</dd>
	<dt>Prod_Condens_BBL</dt>
		<dd>'numeric'</dd>
	<dt>Disposition_1</dt>
		<dd>'numeric'</dd>
	<dt>Op_Name</dt>
		<dd>'character'</dd>
	<dt>Op_No</dt>
		<dd>'integer'</dd>
	<dt>Field_Name</dt>
		<dd>'character'</dd>
</dl>




```R
Null Data treatment: This is a case specific decision in our case we will look at the nulls in all the variables.
```


```R
print (paste("% of NULL Values in Field_Name is" ,round(sum(df$Field_Name=="")/nrow(df),2)))
print (paste("% of NULL Values in Prod_GW is" ,round(sum(df$Prod_GW_MCF=="")/nrow(df),2)))
```

    [1] "% of NULL Values in Field_Name is 0.98"
    [1] "% of NULL Values in Prod_GW is 0"



```R
#Satistical Summary for All the Data
summary(as.data.frame(df))
barplot(df$Disposition)
```


          Date             Prod_GW_MCF      Disposition     Prod_Condens_BBL
     Min.   :1999-10-10   Min.   :  1.00   Min.   :  1.00   Min.   :1.000   
     1st Qu.:1999-10-10   1st Qu.: 63.75   1st Qu.: 63.75   1st Qu.:1.000   
     Median :1999-10-10   Median :120.50   Median :120.50   Median :1.000   
     Mean   :1999-10-10   Mean   :121.97   Mean   :121.97   Mean   :1.008   
     3rd Qu.:1999-10-10   3rd Qu.:181.25   3rd Qu.:181.25   3rd Qu.:1.000   
     Max.   :1999-10-10   Max.   :243.00   Max.   :243.00   Max.   :2.000   
                                                                            
     Disposition_1     Op_Name              Op_No         Field_Name       
     Min.   :1.000   Length:252         Min.   : 20528   Length:252        
     1st Qu.:1.000   Class :character   1st Qu.: 20542   Class :character  
     Median :1.000   Mode  :character   Median :120023   Mode  :character  
     Mean   :1.008                      Mean   :350393                     
     3rd Qu.:1.000                      3rd Qu.:714229                     
     Max.   :2.000                      Max.   :876645                     
                                        NA's   :247                        



![png](output_20_1.png)

