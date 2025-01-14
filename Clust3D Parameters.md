# **Data preprocessing parameters**

**imputation**: String. The method of imputating the missing values in the data file. (Default: “zeros”).

	“zeros”:	 Impute the missing values with zeros.
	“median”:	 Impute the missing values with the median value.
	“knn”: 		 Impute the missing values using k-Nearest Neighbors.
	“none”: 	 No imputation of the missing values.

______________________________________________________________________________


**scaling**: String. The method of scaling the values in the data file. (Default: “minmax”).

	“minmax”: 	Transform features by scaling each feature to a given range.
	“standard”: 	Standardizes features by removing the mean and scaling to unit variance.

*Advanced*:  
	The user can specify the range for the “minmax” scaling (Default: [0-1]) by specifying the range in the line ~143 in main.py.

______________________________________________________________________________



**dim_red**: String. The options to apply or not, a feature dimensionality reduction technique on the data before it is fed into Clust3D’s training. (Default: “pca_auto”)

	“pca_auto”:	Use of Principal Component Analysis (PCA) with 2 principal components.
	“pca_elbow”:	Clust3D automatically chooses the optimal no. of principal components based on the elbow rule on the normalized PCA explained variance plot.  
 			The selection is made based on the elbow point of 45 degrees to the x axis.
	“t-sne”:	Use of t-distributed Stochastic Neighbor Embedding (t-SNE).
	“ica”: 		Use of Independent Component Analysis (ICA).
	“none”: 	No dimensionality reduction.  

*Tips*  
	Applying dimensionality reduction hugely improves training time.  


*Advanced*  
• With the “pca_auto” parameter value, the user can also manually select, if they so choose to, the no. of principal components, by changing the “2” in the source code (line ~83 in dim_red.py) with the desired number.  
• The user can plot the PCA explained variance plot through the source code and obtain more visual information by setting the parameter “show_pca_plot” to True (line ~21 in dim_red.py).  
• The user can modify each technique’s parameters as desired in the dim_red.py  

______________________________________________________________________________

**preprocess**: Boolean. Dictates if Clust3D will apply preprocessing steps to the fed data file. (Default: True)   

	True:	Clust3D applies the user specified preprocessing steps to the data file.
 	False:	The user has already provided a preprocessed data file and no preprocessing steps will take place inside Clust3D.
  
*Tips*  
• Automated prepocessing by Clust3D allows for fast and seemless integration of Clust3D with GEO datasets.  
• Allowing the user to use their own preprocessed data file, allows for more precise and targeted analysis.  

______________________________________________________________________________


# **Neural network training parameters**

______________________________________________________________________________

**distance**: String. The mathematical distance to be used for the distance calculations. (Default: “euclidean”).

	“euclidean”: Use of the Euclidean distance.
	“manhattan”: Use of the Manhattan distance.
 
______________________________________________________________________________

**neighbors**: Boolean. The option to also update or not the weights of the neighbors of the BMU neuron. (Default: True)

	True: 		Updates the weights of the neighbors of the BMU neuron.
	False: 		Only the BMU weights will be updated.
 
_______________________________________________________________

**epochs**: Positive integer. The number of epochs of the neural network training. It determines how many times the whole dataset will be seen by the network. (Default: 5000)

*Tips*  
Literature suggests at least 500 iterations for every neuron [Kohonen, T. (1998). The self-organizing map. Neurocomputing, 21(1-3), 1-6].

______________________________________________________________________________

**lr**: Positive float. The value of the initial learning rate of the neural network training. It adjusts how much the neuron weights will be altered to mimic the data points. (Default: 0.3)

______________________________________________________________________________
**n_neurons**: The number of neurons of the neural network. It determines the number of clusters. (Default: -1)

	Positive (>=2) integer:  	User specified number of neurons.

	-1: 				Automatic selection of the no. of neurons based on the elbow rule on the normalized Sum of Squared Errors (SSE) plot. 
 					The selection is made based on the elbow point of 45 degrees to the x axis. The number of neurons up to which the training will run 
      				is determined by the “max_n_neurons” parameter.

*Advanced*    
The user can plot the SSE plot through the source code and obtain more visual information by setting the “show_sse_plot” to True (line ~47 in auto_neuron_number_selection.py).

_____________________________________________________________________________

**max_n_neurons**: Positive (>2) integer. The number of neurons up to which the automatic neuron selection will run. (Default: 8)

*Tips*  
This value cannot be greater than the number of sample class labels in the dataset.

______________________________________________________________________________

**neuron_init**: String. The neuron initialization technique. (Default: “points”)

	“random”: 	The neuron weights are initialized randomly from a uniform distribution based on the min and max values in the fed data.

	“points”: 	Every neuron is initialized as a randomly selected existing data point.
			No same data point can be selected for two neurons.

By selecting “points”, Clust3D randomly selects data points equal to the no. of neurons and calculates their average in-between Euclidean distance. The combination with the highest average distance is selected as the chosen data points to initialize the neurons. This way, Clust3D initializes the neurons by trying to utilize the largest possible span in the time-related, high dimensionality space. The number of different combinations to be calculated is dictated by the “depth” parameter.

*Tips*  
Due to stochasticity, a good practice is to redo the training a couple of times for whichever neuron initialization technique.

______________________________________________________________________________

**depth**: Positive integer. The number of different combinations for Clust3D to calculate the distance dispersion in order to initialize the neurons. (Default: 100000)

	Positive integer: 	The number of different combinations.
	“auto”:			It automatically calculates every possible combination.

*Tips*  
• In case of high running times, reduce the default value.  
• Very high values (> 500000) can result in very high running times. This depends on the user’s hardware and the number of neurons.  
• High values of this parameter while also using n_neurons = -1, can result in extremely high running times.  
• Higher values result in a higher chance that the algorithm will choose the optimal data points for neuron initialization (depending on the size of the dataset).  

*Advanced*  
• Being able to select and calculate the dispersion of every possible combination of data points, means the exact same initialization for every time Clust3D is used.       This eliminates the stochastic nature of neuron initialization and results in better consistency.  
• The number of possible combinations without repetitions of a given dataset with s samples and n number of neurons is ![εικόνα](https://github.com/Orepap/Clust3D/assets/93657525/f7e34231-b978-4a6a-b937-48ce8207a6fd).  
  For a given dataset, if this number does not exceed the computational threshold, depending on the hardware (e.g., 500000), it is advised to use the “auto” parameter   value for the best consistency.

____________________________________________________________________________

**t1**: Positive integer. Constant value, which controls the exponential decrease of the learning rate. (Default: =epochs / 2)

*Tips*  
A good practice is to solve the exp decrease function (equation (2) in “Equations.docx”) for different t1 and i (current iteration) values and assess the value of the learning rate at that iteration.
 
______________________________________________________________________________

**t2**: Positive integer. Constant value, which controls the exponential decrease of the neighborhood function. (Default: =epochs)

*Tips*  
Higher t2 corresponds to higher and more lasting cooperation between neurons. Solving the neighborhood function (equation (3) in “Equations.docx”) can give insight to the constant’s desired value.
 
______________________________________________________________________________

**random_state**: Positive integer (<=10000). Determines the random state of the algorithm. Choosing a value eliminates the stochastic nature of the “points” neuron initialization and chooses the same combinations every time Clust3D is run as a whole. (Default: a random value each time Clust3D is run)

______________________________________________________________________________
  
**All of Clust3D's parameters can be viewed in the main.py file along with their default values**
