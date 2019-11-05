# Export a inference graph from slim

The methods are base on [Tensorflow slim library](https://github.com/tensorflow/models/tree/master/research/slim).

## Build model from checkpoint file
TF-Slim is a lightweight library for defining, training and evaluating complex models in TensorFlow.

0. build the docker image, and run it up with mounting a local space (ex: `/tmp`

After jumping in the container, download the checkpoint file you want, put it under `/tmp`.
Or you can mount the space where the file already exist

1. export inference graph

Bringing out the DNN structure in pb file

Take ResNet v2 50 for example:
```
$ python export_inference_graph.py --alsologtostderr —model_name=resnet_v2_50 --output_file=/tmp/resnet_v2_50_inf_graph.pb
```

2. find last layer of DNN, ignore this step if you already know the output layer of the model

Will need the name of last layer for freezing the DNN
```
$ ./bazel build tensorflow/tools/graph_transforms:summarize_graph
$ ./bazel-bin/tensorflow/tools/graph_transforms/summarize_graph --in_graph=/tmp/resnet_v2_50_inf_graph.pb.pb

```

3. freeze exported graph

For example, the name of last node in DNN is `resnet_v2_50/predictions/Reshape_1`:

```
$ freeze_graph --input_graph=/tmp/resnet_v2_50_inf_graph.pb --input_checkpoint=/tmp/resnet_v2_50.ckpt --input_binary=true --output_graph=/tmp/resnet_v2_50.pb --output_node_names=resnet_v2_50/predictions/Reshape_1
```

Then you got the ready-to-serve model named `resnet_v2_50.pb`
