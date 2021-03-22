# Pig_BigDataTechnologies

- Connect to EMR

	ssh hadoop@ec2-3-83-4-13.compute-1.amazonaws.com -i C:/BigDataTechnologies/emr-key-pair.pem

- Copy Class File into EMR

	scp -i C:/BigDataTechnologies/emr-key-pair.pem C:/BigDataTechnologies/pigdemo.zip hadoop@ec2-3-83-4-13.compute-1.amazonaws.com:/home/hadoop

- Set directory to demo

	cd /home/hadoop/pigdemo
	
- Command to open PIG console 

	pig   --> Case sensitive
	CTRL + C --> Close console
  
# Examples:
  
- Load file to hadoop directory
  
  hadoop fs -copyFromLocal foodratings80162.txt /user/hadoop
  
- Filter and create a subset dataset

  food_ratings = LOAD 'foodratings80162.txt' USING PigStorage(',')
  AS (name:chararray, f1:int, f2:int,f3:int,f4:int,placeid:int);
  
  food_ratings_subset = FOREACH food_ratings GENERATE name, f4;
  
  STORE food_ratings_subset INTO '/user/hadoop/output/food_ratings_subset' USING PigStorage(',');
 
- Display data
  
  sixrows = LIMIT food_ratings_subset 6;  
  Dump sixrows;
  
- Summary of  data
 
  Describe food_ratings;

- Operations of datasest

  food_ratings_filtered = FILTER food_ratings BY (food_ratings.f1 < 20) AND (food_ratings.f3 > 5)
  grouped   = group food_ratings all;
  avg       = foreach grouped generate grouped.f1,grouped.f3
  dump avg

- Perform GROUP BY

  food_ratings_subset = FOREACH food_ratings GENERATE name, f4;
  filtered_events = FILTER food_ratings BY (f1 == 50);
  grouped_events = GROUP filtered_events BY name;
  DESCRIBE grouped_events;
  DUMP grouped_events;
  grouped_events = GROUP filtered_events all;


  food_ratings_subset = FOREACH food_ratings GENERATE f1, f3 ;
  food_ratings_filtered = FILTER food_ratings_subset BY (f1 < 20) AND (f3 > 5);
  
- Display only certain percentage of entire dataset

  food_ratings_2percent= sample food_ratings 0.02;
  LimitedData = LIMIT food_ratings_2percent 10;
  DUMP LimitedData;

- Perform JOIN

  food_ratings_w_place_names = JOIN  food_ratings BY (placeid), food_places BY (placeid);
  TOP6Data = LIMIT food_ratings_w_place_names 6;
  DUMP TOP6Data;
