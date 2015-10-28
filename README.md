multiNetX is a python package for the manipulation and study of multilayer networks. It is based on NetworkX (http://networkx.github.io/)

              
#### Import standard libraries for numerics and plots


    import numpy as np
    import matplotlib.pyplot as plt

##### Import lil_matrix to use sparse matrices


    from scipy.sparse import lil_matrix

#### Import the package NetworkX for single layer networks


    import networkx as nx

#### Import the package MultiNetX


    import multinetx as mx

#### Create three Erd"os- R'enyi networks with N nodes for each layer


    N = 8
    g1 = nx.erdos_renyi_graph(N,0.5,seed=218)
    g2 = nx.erdos_renyi_graph(N,0.6,seed=211)
    g3 = nx.erdos_renyi_graph(N,0.7,seed=208)

#### Create an 3Nx3N lil sparse matrix. It will be used to describe the layers interconnection


    adj_block = lil_matrix(np.zeros((N*3,N*3)))

#### Define the type of interconnection among the layers (here we use identity matrices thus connecting one-to-one the nodes among layers)


    adj_block[0:  N,  N:2*N] = np.identity(N)    # L_12
    adj_block[0:  N,2*N:3*N] = np.identity(N)    # L_13
    adj_block[N:2*N,2*N:3*N] = np.identity(N)    # L_23
    
    # use symmetric inter-adjacency matrix
    adj_block += adj_block.T

#### Create an instance of the MultilayerGraph class


    mg = mx.MultilayerGraph(list_of_layers=[g1,g2,g3],
                            inter_adjacency_matrix=adj_block)
