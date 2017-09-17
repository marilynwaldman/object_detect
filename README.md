model.ckpt-14214.index
object-detect-179719

cd models

gcloud auth login
export PROJECT=object-detect-179719
export YOUR_GCS_BUCKET="gs://${PROJECT}-ml"

gsutil cp object_detection/samples/configs/faster_rcnn_resnet101_pets.config \
       ${YOUR_GCS_BUCKET}/data/faster_rcnn_resnet101_pets.config


export CHECKPOINT_NUMBER="14214"
# From tensorflow/models
gsutil cp ${YOUR_GCS_BUCKET}/train/model.ckpt-${CHECKPOINT_NUMBER}.* .

*** NOTE CHANGE OF PARAMETERS
python object_detection/export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path object_detection/samples/configs/faster_rcnn_resnet101_pets.config \
    --checkpoint_path model.ckpt-${CHECKPOINT_NUMBER} \
    --inference_graph_path output_inference_graph.pb \
    --trained_checkpoint_prefix model.ckpt-${CHECKPOINT_NUMBER}  \
    --output_directory out


cd out

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

