# tensorflow1_model

# tensorflow_notebooks

# Tensorflow version1 (Copy_of_fm_object_detection)
# steps for Tensorflow 1

Step1:
=======
==> You have to mount the google drive
==> Cloning model using, git clone https://github.com/Manjunatha1997/tensorflow1_model/


Step2:
======
==> You have to select the version as 
%tensorflow_version 2.X

Step3:
======
==> You have to change the directory to research folder for this follow below command.
%cd /content/drive/MyDrive/tensorflow1_model/models/research

==> Then You have to run the following commands like below
!python setup.py build
!python setup.py install

Step4:
======
==> You have to change the directory to slim which is insode research folder for this use below command
%cd /content/drive/MyDrive/tensorflow1_model/models/research/slim

==> Then You have to run the following commands like below
!python setup.py build
!python setup.py install

Step5:
======
==> You have to change the directory to research folder , for this use below command
%cd /content/drive/MyDrive/tensorflow1_model/models/research

==> Then You have to run the following command
!protoc --python_out=. ./object_detection/protos/anchor_generator.proto ./object_detection/protos/argmax_matcher.proto ./object_detection/protos/bipartite_matcher.proto ./object_detection/protos/box_coder.proto ./object_detection/protos/box_predictor.proto ./object_detection/protos/eval.proto ./object_detection/protos/faster_rcnn.proto ./object_detection/protos/faster_rcnn_box_coder.proto ./object_detection/protos/grid_anchor_generator.proto ./object_detection/protos/hyperparams.proto ./object_detection/protos/image_resizer.proto ./object_detection/protos/input_reader.proto ./object_detection/protos/losses.proto ./object_detection/protos/matcher.proto ./object_detection/protos/mean_stddev_box_coder.proto ./object_detection/protos/model.proto ./object_detection/protos/optimizer.proto ./object_detection/protos/pipeline.proto ./object_detection/protos/post_processing.proto ./object_detection/protos/preprocessor.proto ./object_detection/protos/region_similarity_calculator.proto ./object_detection/protos/square_box_coder.proto ./object_detection/protos/ssd.proto ./object_detection/protos/ssd_anchor_generator.proto ./object_detection/protos/string_int_label_map.proto ./object_detection/protos/train.proto ./object_detection/protos/keypoint_box_coder.proto ./object_detection/protos/multiscale_anchor_generator.proto ./object_detection/protos/graph_rewriter.proto ./object_detection/protos/calibration.proto ./object_detection/protos/flexible_grid_anchor_generator.proto


Step6:
======

==> You have to split the dataset into test and train folders and you have to push the folders into images folder

==> You have to change the directory to object_detection folder, for this use below command
%cd /content/drive/MyDrive/tensorflow1_model/models/research/object_detection

==> Then You have to run the followig command 
!python xml_to_csv.py

Step7:
======

==> You have to change the directory to object_detection folder
==> Open generate_tfrecord.py file and there you need to edit the classnames according to your classnames which is you can find in class_text_to_int function in that file.
==> Then run the following commands,which will create a train.record and test.record files

!python generate_tfrecord.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=train.record
!python generate_tfrecord.py --csv_input=images/test_labels.csv  --image_dir=images/test --output_path=test.record


Step8:
======
==> Open faster_rcnn_resnet152_v1_800x1333_coco17_gpu-8.config file which is you can find in training folder
==> Edit the file according to your dataset like
1. num_classes 
2. num_steps 
3. fine_tune_checkpoint
4. label_map_path
5. input_path
6. label_map_path
7. input_path

==> Open the labelmap.pbtxt and edit the file according to your dataset
==> Then run the following command for training the data.
!python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_inception_v2_pets.config

Step9:
=====
==> Change the directory to object_detection folder
==> You need to export the inference_graph for this, use below command
==> edit the check point number instead of xxxx in the given below command
!python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/faster_rcnn_inception_v2_pets.config --trained_checkpoint_prefix training/model.ckpt-xxxx --output_directory inference_graph

==> It will created inference_graph folder

Step10:
======
==> Now it's time to start testing on a image. For that use the given command
!python Object_detection_image.py

