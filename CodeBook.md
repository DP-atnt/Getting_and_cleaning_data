#CodeBook

This file contains the variables used for this project and a list of the manipulations done to the raw data.
The manipulation of the raw data assumes the package dlyr is installed in the host. The first step is to create a directory
that will hold the raw data and the results. This directory is called "Project" and the script does check if it exists, if it
does not, it will create one, otherwise it will use the existing "Project" folder.
The script then calls the URL of the data set repository and downloads it to disk. This data set is part of the UCI assets and is meant to perform "Human Activity Recognition Using Smartphones". The researchers captured the readings of the accelerometer and gyroscope of a Samsung GS2 on a group of 30 people performing the following activities:
WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING.
The sensors produced an set of 561 variables ("features") capturing acceleration and rotation on multiple axis. The data has been filtered to separate the effects of gravity, and multiple descriptive statistics have been calculated, the data contains time and frequency domain measurements and calculations.. The data set is split on a train (30% of observations) and test set (70%) . These sets are in turn is split in 3 files: X.train contains the values meausured or calculated, one observation per line on a space delimited file, y.train contains the subject identity per record (observation) and subject.train contains the subject ID (values 1-30) per record. A similar structure is used in the Test data set.
These files are .txt and are loaded into R wiht read.table. The "features" file contains the names of the variables, and the "activity_labels" is a table that links the activity ID with the activity name.
The script then column binds the subject, y.train and X.train files, the corresponding test files, and they row binds the resulting two frames. Next, the names of the variables are replaced (V1 to Vx were introduced in the previous step) and the subject and activity variables are so labeled.
For this project only the variables with the "Mean" and Standard Deviation (std) values are selected using Grep and regular expressions to index the appropiate columns. The resulting data frame contains 88 variables, down from 563. Next the script uses nerging to replace the activiy labels (numeric factors) with textual descriptions. The data set contains multiple samples of the same activity for the same individual so this needs to be addressed in order to tidy this dataset.. It then cleans up the variable names by expanding "t" into "time", "f" into "frequency", and "Acc" into "Acceleration", special characters comma, dash and parenthesis are removed from variable names as well. At this point scrip cleans (deletes) all unused variables.
Finally, the script creates a new data frame where the data is grouped by activity and the mean of multiple measurementes of each individual is calculated. The resulting data_frame is now sent to disk using write.table()
The variables that were left after cleaning and tidying up are:

 [1] "activity"                                    
 [2] "subject"                                     
 [3] "timeBodyAccelerationmeanX"                   
 [4] "timeBodyAccelerationmeanY"                   
 [5] "timeBodyAccelerationmeanZ"                   
 [6] "timeBodyAccelerationstdX"                    
 [7] "timeBodyAccelerationstdY"                    
 [8] "timeBodyAccelerationstdZ"                    
 [9] "timeGravityAccelerationmeanX"                
[10] "timeGravityAccelerationmeanY"                
[11] "timeGravityAccelerationmeanZ"                
[12] "timeGravityAccelerationstdX"                 
[13] "timeGravityAccelerationstdY"                 
[14] "timeGravityAccelerationstdZ"                 
[15] "timeBodyAccelerationJerkmeanX"               
[16] "timeBodyAccelerationJerkmeanY"               
[17] "timeBodyAccelerationJerkmeanZ"               
[18] "timeBodyAccelerationJerkstdX"                
[19] "timeBodyAccelerationJerkstdY"                
[20] "timeBodyAccelerationJerkstdZ"                
[21] "timeBodyGyromeanX"                           
[22] "timeBodyGyromeanY"                           
[23] "timeBodyGyromeanZ"                           
[24] "timeBodyGyrostdX"                            
[25] "timeBodyGyrostdY"                            
[26] "timeBodyGyrostdZ"                            
[27] "timeBodyGyroJerkmeanX"                       
[28] "timeBodyGyroJerkmeanY"                       
[29] "timeBodyGyroJerkmeanZ"                       
[30] "timeBodyGyroJerkstdX"                        
[31] "timeBodyGyroJerkstdY"                        
[32] "timeBodyGyroJerkstdZ"                        
[33] "timeBodyAccelerationMagmean"                 
[34] "timeBodyAccelerationMagstd"                  
[35] "timeGravityAccelerationMagmean"              
[36] "timeGravityAccelerationMagstd"               
[37] "timeBodyAccelerationJerkMagmean"             
[38] "timeBodyAccelerationJerkMagstd"              
[39] "timeBodyGyroMagmean"                         
[40] "timeBodyGyroMagstd"                          
[41] "timeBodyGyroJerkMagmean"                     
[42] "timeBodyGyroJerkMagstd"                      
[43] "frequencyBodyAccelerationmeanX"              
[44] "frequencyBodyAccelerationmeanY"              
[45] "frequencyBodyAccelerationmeanZ"              
[46] "frequencyBodyAccelerationstdX"               
[47] "frequencyBodyAccelerationstdY"               
[48] "frequencyBodyAccelerationstdZ"               
[49] "frequencyBodyAccelerationmeanFreqX"          
[50] "frequencyBodyAccelerationmeanFreqY"          
[51] "frequencyBodyAccelerationmeanFreqZ"          
[52] "frequencyBodyAccelerationJerkmeanX"          
[53] "frequencyBodyAccelerationJerkmeanY"          
[54] "frequencyBodyAccelerationJerkmeanZ"          
[55] "frequencyBodyAccelerationJerkstdX"           
[56] "frequencyBodyAccelerationJerkstdY"           
[57] "frequencyBodyAccelerationJerkstdZ"           
[58] "frequencyBodyAccelerationJerkmeanFreqX"      
[59] "frequencyBodyAccelerationJerkmeanFreqY"      
[60] "frequencyBodyAccelerationJerkmeanFreqZ"      
[61] "frequencyBodyGyromeanX"                      
[62] "frequencyBodyGyromeanY"                      
[63] "frequencyBodyGyromeanZ"                      
[64] "frequencyBodyGyrostdX"                       
[65] "frequencyBodyGyrostdY"                       
[66] "frequencyBodyGyrostdZ"                       
[67] "frequencyBodyGyromeanFreqX"                  
[68] "frequencyBodyGyromeanFreqY"                  
[69] "frequencyBodyGyromeanFreqZ"                  
[70] "frequencyBodyAccelerationMagmean"            
[71] "frequencyBodyAccelerationMagstd"             
[72] "frequencyBodyAccelerationMagmeanFreq"        
[73] "frequencyBodyBodyAccelerationJerkMagmean"    
[74] "frequencyBodyBodyAccelerationJerkMagstd"     
[75] "frequencyBodyBodyAccelerationJerkMagmeanFreq"
[76] "frequencyBodyBodyGyroMagmean"                
[77] "frequencyBodyBodyGyroMagstd"                 
[78] "frequencyBodyBodyGyroMagmeanFreq"            
[79] "frequencyBodyBodyGyroJerkMagmean"            
[80] "frequencyBodyBodyGyroJerkMagstd"             
[81] "frequencyBodyBodyGyroJerkMagmeanFreq"        
[82] "angletBodyAccelerationMeangravity"           
[83] "angletBodyAccelerationJerkMeangravityMean"   
[84] "angletBodyGyroMeangravityMean"               
[85] "angletBodyGyroJerkMeangravityMean"           
[86] "angleXgravityMean"                           
[87] "angleYgravityMean"                           
[88] "angleZgravityMean" 
