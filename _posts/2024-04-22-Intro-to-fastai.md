# Introduction to Fastai

As part of *ELEC4630: Computer Vision & Deep Learning* I am currently completing [Jeremy Howard's](https://en.wikipedia.org/wiki/Jeremy_Howard_(entrepreneur)) Practical Deep Learning For Coder's Fastai Course.

Personally, I have had some exposure to Pytorch in past projects so I had a relatively smooth time wrapping my head around the `fastai` framework.

When it comes to Deep Learning, the generalised steps to approach a problem are as follows:

1. Develop a Dataloader
2. Implement (or develop!) a model architecture
3. Develop a training loop and train the model
4. Run inference on the model and present your evaluation

*Fastai* aims to lower the barrier of entry to Deep Learning by implementing some really handy tools for each of these phases.

## 1 - Fastai Dataloaders

In *pytorch* to implement a Dataloader object from a folder of images, your code would look something along the lines of -

```python
# Define transformations
transform = transforms.Compose([
    transforms.Resize((224, 224)),  # Resize the image
    transforms.RandomRotation(degrees=(-30, 30)),  # Random rotation between -30 to 30 degrees
    transforms.ToTensor(),  # Convert the image to a PyTorch tensor
])

# Define dataset
dataset = ImageFolder(root='your_dataset_path', transform=transform)

# Define dataloader
dataloader = DataLoader(dataset, batch_size=32, shuffle=True)
```

To simplify this process, *fastai* have a `Datablock` class which will return a `DataLoaders` object. The `DataLoaders` objet contains both the train and validation dataloaders based on the dataset. To get obtain the `DataLoaders` object with *fastai*, it is as simple as executing the following -

```python
# Configure the dataloader
dls = DataBlock(
    blocks=(ImageBlock, CategoryBlock),  # model inputs are Images and outputs are categories (labels)
    get_items=get_image_files, # Images available in the listed directory
    splitter=RandomSplitter(valid_pct=0.2, seed=42), # 80/20 split will be used for the train/validation sets 
    get_y=parent_label, # The category truth labels for each of the images
    item_tfms=[Resize(192, method='squish')] # Transforms applied to the images on import into the dataloader -> images are resized to 192px*192px
).dataloaders(path)
```

This approach is much clearer and more straightforward when compared to using the pytorch framework directly.

## 2 - Fastai Training a Model

When training models using *fastai* predefined architectures and models can be used to improve the training time. The use of a pre-trained model is known as the *transfer learning* technique. In essence, the framework provides you with a model which has already been trained on a base dataset to develop an initial set of base filters. To complete the training process of the neural network you then train it on your dataset. Since the neural network has already been trained with base filters it should learn the features within the provided dataset much faster.

To perform training in *fastai* you can use the following code -

```python
# Train the model
learn = vision_learner(dls, resnet18, metrics=error_rate, loss_func=CrossEntropyLossFlat())
learn.fine_tune(10)
```

The first line instantiates the `Learner` object. Think of this as a bundle of your model architecture, training loop, optimiser and loss function. The second line of code then executes the training loop for 10 epochs.

## 3 - Running Inference

After model training is complete the inbuilt functions within the *fastai* frame work can be utilised to run inference on the model. For example, if you model is a multi-class classifier, it can be useful to us the `ClassificationInterpretation` methods availble within *fastai* to run inference and evaluate the model performance.

```python
# Get predictions and targets
interpretation = ClassificationInterpretation.from_learner(learn)

# Print the classification report
interpretation.print_classification_report()

# Plot the confusion/error matrix
interpretation.plot_confusion_matrix(title="Confusion Matrixt", cmap='PuRd')
```

In this code segment, the classification report is first printed followed by the Confusion matrix

These basic functions form the basics required to develop a dataloader, train a model and evaluate it using *fastai*.