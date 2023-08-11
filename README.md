# boat-detection

This page is a work in progress for automated detection of motorboats from underwater soundscape recordings. Some code is messy/incomplete but the workflow below can be executed:

Step by step
- Create audio files, sample-more-audio.ipynb was used for this. This will name files must be named in a certain way. Before the first period '.' it will prefix the filename with: 'y_' if they contain a boat, 'n_' if they do not contain boat noise, then three characters for the site, then the files number (counting up), with underscore to separate each component, e.g n_CCC_106.20220628_144100. May need to check its compatible with your existing filename though. Files can be of any length and must be sampled at 16khz. To simulate real world data **there should be many more boat files than non-boat files**. Files will be cut into 0.96khz by vggish so the entirety of the file must contain boat noise or none at all.
- Create a conda env using the yml file (vggish-env.yml). This is needed for running feature extraction using the pretrained vggish model.
- Run feature extraction using the pretrained vggish model (Boat-noise-pretrained-CNN-feature-extraction.ipynb). This will require the Audioset folder saved in my other github repo (https://github.com/BenUCL/Reef-acoustics-and-AI). This will output a csv that contains rows for every 0.96s section of each file and the 128 features output by vggish.
- Train an XGBoost classified on the csv (Boat-noise-xgboost.ipynb). This will perform a hyperparamter gridsearch and use 'leave one out crss validation' to maximise the training data and give a highly representative accuracy score. This will then train a final model, save this, and this can be loaded and used on new data. 

