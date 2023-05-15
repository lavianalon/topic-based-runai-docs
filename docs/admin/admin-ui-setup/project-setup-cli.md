# Configure projects CLI






## Grouping Nodes using Node Pools  
To create a node pool you must first annotate nodes with a label or use an existing node label, as the key for grouping nodes into pools. You can use any unique label (in the format `key:value`) to form a node pool. a node pool is characterized by a label but also has its own unique node pool name.

To get the list of nodes and their current labels, run:

```
kubectl get nodes --show-labels
```

To annotate a specific node with the label `dgx-2`, run:

```
kubectl label node <node-name> node-model=dgx-2
```
You can annotate multiple nodes with the same label.

To create a node pool with the chosen common label use the [create node pool](https://app.run.ai/api/docs/#/NodePools/createNodePool){target=_blank} Run:ai API.

## Grouping Nodes using Node Affinities  

To set node affinities, you must first annotate nodes with labels. These labels will later be associated with Projects. 

To get the list of nodes, run:

```
kubectl get nodes
```

To annotate a specific node with the label "dgx-2", run:

```
kubectl label node <node-name> run.ai/type=dgx-2
```

* Each node can only be annotated with a **single** label.
* You can annotate multiple nodes with the same label.
