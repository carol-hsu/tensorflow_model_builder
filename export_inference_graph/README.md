# Export a inference graph from slim

The methods are base on [Tensorflow slim library](https://github.com/tensorflow/models/tree/master/research/slim).

## Build model from checkpoint file
TF-Slim is a lightweight library for defining, training and evaluating complex models in TensorFlow.

0. build the docker image and run it up by mounting a local space (ex: `/tmp`

1. export inference graph
```
$ ./bazel-bin/tensorflow_models/research/slim/export_inference_graph --alsologtostderr —model_name=resnet_v2_50 --output_file=/tmp/resnet_v2_50_inf_graph.pb
```
(--labels_offset=1 if the index is 1001, comparing to imagenet’s 1000)  

2. find output layer
$ ./bazel build tensorflow/tools/graph_transforms:summarize_graph
$ ./bazel-bin/tensorflow/tools/graph_transforms/summarize_graph   --in_graph=/tmp/resnet_v2_50_inf_graph.pb.pb

3. freeze exported graph
$ ./bazel-bin/tensorflow/python/tools/freeze_graph --input_graph=/tmp/resnet_v2_50_inf_graph.pb --input_checkpoint=/tmp/resnet_v2_50.ckpt --input_binary=true --output_graph=/tmp/resnet_v2_50.pb --output_node_names=resnet_v2_50/predictions/Reshape_1

