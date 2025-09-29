# Q1_ViT
Code Modifications

#### Modification 1: Dataset Loading Adaptation (`utils/data_utils.py`)

**Original Code Analysis**

```python
if args.dataset == "cifar10":
    trainset = datasets.CIFAR10(...)
else:
    trainset = datasets.CIFAR100(...)
```

- **Problem**: The dataset loading is hardcoded to support only CIFAR datasets with a simple if-else structure, making it inflexible to extend.
    

**Modified Version**

```python
if args.dataset == "cifar10":
    trainset = datasets.CIFAR10(...)
elif args.dataset == "cifar100":
    trainset = datasets.CIFAR100(...)
else:  # Hymenoptera dataset
    trainset = datasets.ImageFolder(root=args.data_root + "/train",
                                    transform=transform_train)
    testset = datasets.ImageFolder(root=args.data_root + "/val",
                                   transform=transform_test)
```

---

#### Modification 2: Command-line Argument Extension (`train.py`)

**Original Code**

```python
parser.add_argument("--dataset", choices=["cifar10", "cifar100"])
```

- **Problem**: The `choices` list restricts dataset options, preventing the addition of new datasets.
    

**Modified Version**

```python
parser.add_argument("--dataset", choices=["cifar10", "cifar100", "hymenoptera"])
parser.add_argument("--data_root", type=str, default="./data")
```

---

#### Modification 3: Classification Head Adaptation (`train.py`)

**Original Code Analysis**

```python
num_classes = 10 if args.dataset == "cifar10" else 100
```

- **Problem**: The ternary operator only handles two datasets and cannot adapt to multiple cases.
    

**Modified Version**

```python
if args.dataset == "cifar10":
    num_classes = 10
elif args.dataset == "cifar100":
    num_classes = 100
else:  # Hymenoptera dataset (ants vs bees)
    num_classes = 2
```
