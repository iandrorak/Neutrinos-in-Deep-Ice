<img src="src/header.png">

# IceCube - Neutrinos in Deep Ice

This is my notebook for the [Neutrino in deep ice challenge]("https://www.kaggle.com/competitions/icecube-neutrinos-in-deep-ice")

It is a competition from Kaggle that aims to identify the direction from which neutrinos detected by the IceCube neutrino observatory came. The purpose of this task is to help partner observatories limit their search space and associate neutrinos with sources like supernovae or gamma-ray bursts. The challenge is to process a large number of events quickly and accurately.

## Dataset


The dataset is divided into training and testing sets, stored in Parquet format, and contains the following files:

* [train/test]_meta.parquet: Contains metadata for the events, including `event_ID`, `batch_ID`, and first/last pulse index. The target variables are the azimuth and zenith angles.

* [train/test]/batch_[n].parquet: Each batch contains tens of thousands of events. Each event may have thousands of pulses, which are the digitized output from photomultiplier tubes. The pulses' data include `event_ID`, `time`, `sensor_ID`, `charge`, and an `auxiliary` flag.

* `sample_submission.parquet`: An example submission file with the correct columns and properly ordered event IDs. The final submission must be in CSV format.

* `sensor_geometry.csv`: Provides the `x`, `y`, and `z` positions for each of the 5160 IceCube sensors in meters, with the origin at the center of the IceCube detector. The coordinate system is right-handed, and the z-axis points upwards when standing at the South Pole. `azimuth` and `zenith` angles can be calculated from these coordinates.

Example event image, a visual representation of the features of an IceCube event in the dataset. 
The colorful dots represent sensors that logged at least one pulse, with the size of the dots corresponding to the total charge of all pulses and the color indicating the time of the first pulse.

<img src="https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F1132983%2F6891ec67d9d40315637b1b292c3a486b%2FExample_event.png?generation=1666631264548536&alt=media">

## Notebook

This is a simple deep learning approach with LSTM and Convolution Layers that can serve for a base for other architectures. The problem involves predicting the azimuth and zenith angles of the incident neutrino direction. The dataset is provided in the form of parquet files, with separate files for train and test data, metadata, and sensor geometry.

The notebook is organized as follows:

1. **Imports:** Importing necessary libraries and modules.
2. **Look at the data:** Exploring the dataset by printing filenames, shapes, and looking at the contents.
3. **Model:** Defining the custom data generator class `IceCubeDataGenerator` and implementing the custom metric function `angular_dist_score`.
4. **Build the model:** Defining the model architecture using a combination of Conv1D, Bidirectional LSTM, GlobalAveragePooling1D, and Dense layers.
5. **Training:** Setting up a train data generator and training the model.
6. **Prediction:** Defining a function to get predictions on the test dataset using the trained model.
7. **Submission:** Aggregating the predicted `azimuth` and `zenith` angles for each event_id and saving the aggregated predictions to a CSV file.

The model is built using TensorFlow and Keras, with a combination of Conv1D, Bidirectional LSTM, GlobalAveragePooling1D, Dense, and Dropout layers. The custom metric, `angular_dist_score`, is used to evaluate the model. The model is then trained for 4 epochs using the custom data generator.

After training, the model is used to predict `azimuth` and `zenith` angles on the test dataset. The predictions are aggregated by averaging for each event_id and saved in a CSV file for submission.