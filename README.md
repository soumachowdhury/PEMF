PEMF User's Guide

version 2016.v1

Predictive Estimation of Model Fidelity (PEMF) is a model-independent approach to quantify surrogate model error.  PEMF takes as input a model trainer, sample data on which to train the model, and hyper-parameter values to apply to the model.  As output, it provides an estimate of the median or maximum error in the surrogate model.

% Any feedback or comments are welcome. Please direct them to soumacho@buffalo.edu

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%    First time use instructions

%      >In order to call PEMF from anywhere, add path to the PEMF directory

%

%      >In order to use any 3rd party surrogate modeling package 

%       (e.g., "dace" for Kriging, or "Libsvm" for SVR),

%       put it inside the "/Models" subfolder

%    Included Model codes: Radial Basis Functions or RBF (with 5 different Kernels)

%    Third-Party Models tested: Kriging (DACE) and SVR (LibSVM). They can be found at:

%    DACE: http://www2.imm.dtu.dk/projects/dace/

%    LibSVM: https://www.csie.ntu.edu.tw/~cjlin/libsvm/

%      >Try the example demo_PEMF (under the Examples directory) to learn how to use PEMF

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%    Required Inputs:

%        X - x data, with each row of x corresponding to a single data point
 
%        Y - y data, a single column where each element corresponds to a row of x.
 
%        surrogate_trainer - 

%            This input is a function which trains and produces a callable surrogate model.  It takes as input the data for the model and produces as output a function handle which will give the model's prediction of Y for a given X. The trainer function should be of the following form:
          
              function [trained_model] = train_model(X_data,Y_data)

 			       coeffs = ... fit a model...

 			       trained_model = @(x) call_to_model(coeffs,x)

 	           end
                
%            The input to train_model is a set of data, X and Y. The output of train_model is the function, trained_model, trained_model(x1) should return an estimate of y at x1. Subsets of the X and Y data given to PEMF will be used to train models.  This is why PEMF needs the trainer and not just the model.  Matlab's fit function is compatible as a trainer.  If you're confused, experimentwith "fit" first.  Note that it produces a callable function as an output. Example code for "fit" on 2D data:
           
              X = 1:.1:10; Y = log(X);

              surrogate_trainer = @(X,Y) fit(X,Y,'smoothingspline');

              err = PEMF(surrogate_trainer, X,Y);
 
% 	         Note that PEMF does not want a trained model, but the model trainer itself.  The first input to PEMF, 
surrogate_trainer, should be @train_model (following the convention shown above).
 
%            Any model hyper-parameters or kernel settings should be handled by using anonymous functions (default values are used if hyperparameter/kernel choices are not specified). An example, of calling RBF with the multiqudric kernel and a shape factor of 0.9, is shown below:
                
             surrogate_trainer = ...
                @(X,Y) train_model(X, Y, 0.9, 'Multiquadric')
            
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%    Optional Inputs:

%        error_type  - type of error. Options:

%            'median' - modal value of median error

%            'max'    - modal value of maximum error  

%            'both'   - modal values of both errors

%        verbosity   - This input tells PEMF how much output you want.  

% 		     'high' - plot & command line output 

%            'low' - command line output

%            'none' - no command line or plot output 

%        n_pnts_final - number of test points in the last iteration

%        n_pnts_step - step size in predictive error model

%        n_steps - number of steps in predictive error model 

%        n_permutations - number of combinations tried in each step


%    Default behavior of optional inputs:

%        error_type - median

%        verbosity - default

%        n_pnts_final - max(5% of sample points, 3)

%        n_pnts_step - equal to n_pnts_final

%        n_steps - 5

%        n_permutations - 40 (recommended: between 10 to 40)
 
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%    Outut: 

%        PEMF_Error - Estimation of model error

%        model - Final surrogate model

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%    For further explaination of PEMF's optional inputs, refer to the PEMF paper. "Predictive quantification of surrogate model fidelity based on modal variations with sample density." by Chowdhury and Mehmani. DOI: 10.1007/s00158-015-1234-z
http://link.springer.com/article/10.1007/s00158-015-1234-z

%    The above article and its citation (bibtex) can be found at: http://adams.eng.buffalo.edu/publications/
    
%    Cite PEMF as:

%       A. Mehmani, S. Chowdhury, and A. Messac, "Predictive quantification of surrogate model fidelity based on modal variations with sample density," Structural and Multidisciplinary Optimization, vol. 52, pp. 353-373, 2015.
    
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

Author: Souma Chowdhury, Ali Mehmani, and Benjamin Rinauto
