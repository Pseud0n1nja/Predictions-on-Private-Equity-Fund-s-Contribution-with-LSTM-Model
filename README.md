## Code Explaination

Our project mainly uses three part of code to finish a single model, which are “”data_processing”, “model” and “evaluation”. From raw data input to finally visualizing the model’s results, you need to go through these processes: read the excel file and doing preprocessing with it to turn it into readable input, then fit the input to the model, then evaluate the model. 


## Data processing
The first part “data_processing” includes 7 different functions:
<li>1. “interpolate_gaussian”, which is exactly the same with the previous group, is used for interpolated the inner data point between two given cash flow value. You can specify the number in between two data point by the argument “num_between” and also change the “var” to modify the variance of stochastic interpolation. 
  
<li>2. “data_process” take the raw data (actual cash flow contribution rate), take the log different and do the interpolation using “interpolate_gaussian”. From “data_process” we can turn the quarterly data point into log difference then interpolate it to the sequential data. 
  
<li>3. “get_macro” is used for get data from the excel file, though it is called “macro”, it is also use for getting cash flow data together with macroeconomic features. 
  
<li>4. “prep_dat” is used for creating the rolling window for prediction. From the sequence get from get_macro, it then turn into shape of (271,1101), which means we have a input length of 1101, and each consists of 270 days value as X and the “Q_next” as y value. 

<li>5. “trans_dat” is built upon prep_dat, but it allows to take a pandas dataframe (multivariable input) into shape of input that could be read by LSTM model. For example, for a input with 19 macros and 1 cash flow value, the shape of the result would be (1101, 270, 20), which represents length of input, length of time_step, number of features respectively. 
  
<li>6. Finally, split_dataset_x & split_dataset_y is used for splitting the dataset into 70/30 training and testing set for X and y. 

## Model
The second part “model” includes four different model architectures. “”build_model” is used for building the baseline LSTM. “build_model_encoder” is for building Encoder-Decoder LSTM. “build_model_conv” is used for building the 1-d CNN-LSTM model. Finally “”build_model_conv2” is for building the ConvLSTM model. There are some shared hyperparameters and we keep them the same: we all used 100 training epochs with mini-batch size of 100, and used ReLU activation function for each layer, optimizing the model with Adam Optimizer. For each model’s introduction one might refer to our report to see more about the details. 


## Evaluation
The final part “evaluation” consists of “”evaluation_model” and “evaluation_model_conv”. “evaluation_model_conv” is special for evaluating ConvLSTM model as the X’s shape is different from the other three models. Except for the input shape difference, all the evaluation functions will provide the training mse and testing mse, then visualizing the training loss and plot the actual values versus predicted values for both training set and testing set. 


