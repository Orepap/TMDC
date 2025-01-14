Folder “Example 2” contains the processed and ready to be used data file and correlation file.  
They correspond to the Gene Expression Omnibus (GEO) Series Matrix File for the GSE97075 series.  

GSE97075 provides gene expression data of hyperimmunoglobulin D syndrome (HIDS) patients with periodic fever syndrome patients treated with canakinumab.


Usage:  
```python
from Clust3D.main import Clust3D

data_file = "..." # path to the data file
correlation_file = "..." # path to the correlation file

clusters, neurons, cl_labels = Clust3D(data_file=data_file, correlation_file=correlation_file, n_neurons=-1, max_n_neurons=5, depth="auto")
```  

The code returs a dictionary with the cluster memberships (clusters),  the clustering centers (neurons) and a list with the clustering labels (cl_labels).

- The "max_n_neurons" parameter has been set to a number (5) lower than the default value (8) due to the "points" initiallization process not being able to produce more neurons than samples.
- The "depth" parameter has been set to "auto" for best consistenty, as it is computationally viable due to the low sample size of this dataset
