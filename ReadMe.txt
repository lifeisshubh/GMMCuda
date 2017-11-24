Group Member :
Jaya Ingle (CS17M060)
Shubham Patel (CS17M051)

Train File Format:
	first Line : no_of_data_point Dimension_of_data_point no_of_cluster
	other line : Data Points

Test File Format:
	First line : no_of_data_points 
	other line : Data Points 

Folder Contains :
1. train.cu : To Train GMM. 
	     Compile it as nvcc train.cu -o train_gmm
	     To Train model : ./train_gmm modelName < training_Data_File 
	     Output : will be a file containing model information

2. test.cu : To Get Score.
	     nvcc test.cu -o test_gmm
	     To Test model : ./test_gmm model_file test_data output_file
	     output(In output file) : Score corresponding to all of the data point entry.

3. checkA.cu : To get accuracy on 2 class model.
	      Procedure : Train 2 model one with each class.
                          Generate 4 scores - testing_data_of_1 with model1(that is create with train data of class 1)
			  		      testing_data_of_1 with model2
					      testing_data_of_2 with model1
					      testing_data_of_2 with model2

                          compile checkA using : nvcc checkA -o compAcu
			  Generate accuracy as : ./comAcu c1m1 c1m2 c2m1 c2m2
			  c1m1(means) : class 1 Testing Data Score with model 1
			  Output will be confusion matrix.

Challenges :
1. Computing K-means in parallel.
2. Computing Covariance Matrix in parallel.
3. Computing Responsibility of each point with each cluster in parallel.
4. Computing No. of points in each cluster.
5. Computing New_Mean in parallel.
6. Computing new covariance matrix in parallel.
7. Generating Scores in Parallel.

Critical Observation :
Most of the Operation can be done in Transform and Reduce format.
Blocking should be avoid as much as possible. Since only limited no of blocks can be excecute
at a given instance of time.

Issues :

Machine Learning operation needs massive data to train and Test. So, memory on device should be traded carefully.
Also there are memory focused operation that require large input and output.
There are many operation that can be done without atomics by simply using reduction. But it cost memory. In a ideal
System where memory available is high massive parallelism can be achivie by simply shifting all op to store and reduce method.


