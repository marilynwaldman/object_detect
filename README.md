# Running the object detector prediction  using Cloud Machine Learning Engine from Google Cloud

The object detector  [tutorial](https://cloud.google.com/blog/big-data/2017/06/training-an-object-detector-using-cloud-machine-learning-engine) is written for a Ubuntu platform.  This repo creates a docker image and container image on your local machine for object detection.  All dependencies are loaded into the docker image.  You will not need to issue apt-get or pip commands from the docker image.

## From the tutorial run the following to save your model graph to Google Cloud Platform:

    gsutil cp out/frozen_inference_graph.pb ${YOUR_GCS_BUCKET}/data/frozen_inference_graph.pb

#### clone this repo and cd to object_predict
    
   git clone https://github.com/marilynwaldman/object_detect.git
   cd object_detect_


#### install Docker if necessary


#### Pull and Run the docker file.

    docker build -t "tf" .
    docker run -it -p 6006:6006 -p 8888:8888 -p 5000:5000 tf
 

###obtain your Google Cloud Platfrom project_id from the GCP console - example :  object-detect-179719

    

#### From the docker terminal run

    gcloud auth login
    export PROJECT=<YourProjectID>
    export YOUR_GCS_BUCKET="gs://${PROJECT}-ml"

#### if you are running the tutorial example:
   
    gsutil cp object_detection/samples/configs/faster_rcnn_resnet101_pets.config \
         ${YOUR_GCS_BUCKET}/data/faster_rcnn_resnet101_pets.config

#### Otherwise get your saved model graph (example:  model.ckpt-14214.index)
    
    export CHECKPOINT_NUMBER="14214"

#### From tensorflow/models

    gsutil cp ${YOUR_GCS_BUCKET}/train/model.ckpt-${CHECKPOINT_NUMBER}.* .
    gsutil cp ${YOUR_GCS_BUCKET}/data/frozen_inference_graph.pb .
    gsutil cp ${YOUR_GCS_BUCKET}/data/faster_rcnn_resnet101_pets.config .

#### Compile the ...

     cd models
     protoc object_detection/protos/*.proto --python_out=.
     export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim

####You can test your installation by running the following command:

     python object_detection/builders/model_builder_test.py

#### Start the notebook.  From docker shell:

     /run_jupyter.sh --allow-root  

#### Change directories to object-detection.  Upload the notebook object_detect.ipynb and run._

##### Run the graph
###### NOTE CHANGE OF PARAMETERS from the tutorial 
 
     python object_detection/export_inference_graph.py \
         --input_type image_tensor \
         --pipeline_config_path object_detection/samples/configs/faster_rcnn_resnet101_pets.config \
         --checkpoint_path model.ckpt-${CHECKPOINT_NUMBER} \
         --inference_graph_path output_inference_graph.pb \
         --trained_checkpoint_prefix model.ckpt-${CHECKPOINT_NUMBER}  \
         --output_directory out


         cd out



#### Once the workshop container is running again, you can exec back into it like this:

	docker exec -it <container_id> bash

Note that you may need to define environment variables from step 9 when you reconnect.
Note also that if you later start a separate new container 'from scratch', you will need to repeat the auth setup.

## If you need to restart the container later

If you later exit your container and then want to restart it again, you can find the container ID by running the following in your VM:

	docker ps -a
	docker start <container_id>

Once the workshop container is running again, you can exec back into it like this:

	docker exec -it <container_id> bash


gsutil cp out/frozen_inference_graph.pb ${YOUR_GCS_BUCKET}/data/frozen_inference_graph.pb
gsutil cp ${YOUR_GCS_BUCKET}/data/frozen_inference_graph.pb .
   
faster_rcnn_resnet101_pets.config

#   gsutil cp object_detection/samples/configs/faster_rcnn_resnet101_pets.config \
gsutil cp ${YOUR_GCS_BUCKET}/data/faster_rcnn_resnet101_pets.config .
gsutil cp ${YOUR_GCS_BUCKET}/data/pet_label_map.pbtxt .

python setup.py sdist
(cd slim && python setup.py sdist)


python object_detection/export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path faster_rcnn_resnet101_pets.config \
    --checkpoint_path model.ckpt-${CHECKPOINT_NUMBER} \
    --inference_graph_path output_inference_graph.pb

