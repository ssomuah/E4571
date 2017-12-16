Azure HDInisight is the fastest way to get up and running with a spark scala notebook. However the version of mllib has a bug where the coldstartstrategy cannot be specified. This means if the train test split ever causes completely new items to be put in the test set they will score as nan. This is particularly bad because it doesn't throw any kind of exception so crossvalidation simply wont work and won't report an error.

AWS is what I eventually used even though it was more difficult to set up.

It requires a step to actually install jupyter and the apache toree kernel which provides spark scala for the jupyter notebook.  

The `install-jupyter-emr5.sh` script installs all the required dependencies.  
It also adds aws access keys to the jupyter config so that notebooks can be saved to s3.  
There is also a step in the script that copies datafiles from s3 to the clusters hdfs storage for performance reasons.  

Finally, copying datafiles messes up some permissions on hdfs.  
These can be fixed with `hdfs dfs -chown jupyter:jupyter /user/jupyter`  

To actually view the notebooks and cluster web interfaces, follow amazons steps for setting up foxyproxy and opening an ssh tunnel.  

```
JAR location : s3://colon/script-runner.jar
Main class : None
Arguments : s3://colon/install-jupyter-emr5.sh --notebook-dir s3://colon/E4751 --toree --ds-packages --ml-packages --python-packages "ggplot nilearn matplotlib" --port 9998 --password jupyter --jupyterhub --jupyterhub-port 9002 --copy-samples
```