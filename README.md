# spacesuit-detector
Detects Spacesuits in images

# Introduction
The idea behind this project is to create software that could be used to detect an astronaut from a camera. The goal for the project was to incorporate it on a manned rover that could identify where the astronaut is.

# Dataset Collection
I collected a set of over 150 images and put bounding boxes around the astronaut in every image. The images were collected from the University of Maryland Space Systems Lab website and other online images of similar spacesuits. This is an example of few images that I collected:

![090422_MoonYard_101_jpg rf 3a227dddc5d2cb4c0dda0d468e5990cf](https://github.com/user-attachments/assets/a521d8a3-9e28-41cf-9c9b-3a44bedfacd3)

![090519_MoonYard_11_jpg rf 9e77dc4135daf9f69a9ebdfc52ae6956](https://github.com/user-attachments/assets/69661c35-3338-4d76-b2bf-541b27fdd7d0)

![090519_MoonYard_02_jpg rf 9b5d564eceae96d7aa0d6ad2150231d8](https://github.com/user-attachments/assets/c0a4c003-cdce-4451-b911-7f02ae997e7a)

I found a bounding box for every image in this dataset and put them in textfiles. The textfile looked something like this:

class x1 x2 y1 y2

Since we are only classifying spacesuits, we only have one class (0). x1 and x2 refer to the x coordinates for the top left corner and bottom right corner of the bounding box respectively and y1 and y2 refer to the y coordinates for the top left corner and bottom right corner of the bounding box respectively. These label.txt files are used for being compared to the predictions of the YOLO model on training and testing.

# Training
I used the yolov8m.pt model from the Ultralytics YOLOv8 library to train a custom object detector. This model is pretrained on the COCO dataset, which contains 80 common object classes, allowing it to start with a strong baseline understanding of object features like shapes and edges.

I chose the YOLOv8 Medium (yolov8m) model because it offers a balanced tradeoff between speed and accuracy — making it well-suited for most real-world applications without requiring extreme computing power.

Using my own labeled dataset (specified in ssdata/data.yaml), I fine-tuned the model for 5 epochs to help it adapt to the specific objects in my dataset. Fine-tuning a pretrained model like this speeds up training and improves performance, especially when working with a relatively small or specialized dataset.

# Results
My model was successfully able to predict bounding boxes around the astronaut with high confidence. This is the result from the validations:

![val_batch0_pred](https://github.com/user-attachments/assets/e6402c3a-09ab-4294-b29a-369ccd49efa7)

![val_batch1_pred](https://github.com/user-attachments/assets/2fc49094-2080-4b21-a13c-092b7e179c77)


As can be seen, the model was able to predict these images with high confidence, between 0.85 to 1. The following graphic includes the confusion matrix of the predicted images and other graphs:

Confusion Matrix:
![confusion_matrix](https://github.com/user-attachments/assets/ce0a6f15-8bb4-4701-9208-fac7acb29305)

Normalized Confusion Matrix:
![confusion_matrix_normalized](https://github.com/user-attachments/assets/ec28cdfd-b718-40ee-93fd-c3a30849d3cc)

Precision-Confidence Curve:
![P_curve](https://github.com/user-attachments/assets/ee159d2f-33d1-499a-a63b-77209cf5092c)

R-Curve:
![R_curve](https://github.com/user-attachments/assets/4e1b7c70-cdef-4eb4-b8d7-a7848251719a)

PR-Curve:
![PR_curve](https://github.com/user-attachments/assets/6fda9160-25d3-4a7d-9822-a40cf0c3b9f8)

F1-Curve:
![F1_curve](https://github.com/user-attachments/assets/655e11e5-7eb6-47bd-ba0e-38b9dc735e3b)

These are the final predictions:

![final_prediction_4](https://github.com/user-attachments/assets/5a6f802c-3f30-4891-9adf-61b04d59f06c)

![final_prediction_6](https://github.com/user-attachments/assets/8f2c229a-2385-4ddd-9e6f-94231e46adf8)

![final_prediction_10](https://github.com/user-attachments/assets/ae577837-f1be-40bf-9a3b-d700387f4f5a)
 
As you can see, the confidence is pretty high for most of the test predictions. 14/16 test cases were predicted properly with a bounding box confidence of over 0.85. here are some of the cases which are not correctly 

![final_prediction_15](https://github.com/user-attachments/assets/92749b3d-6dba-4962-8a2d-cc6f48440ccc)

![final_prediction_12](https://github.com/user-attachments/assets/9b7a7039-7ead-472d-bbe4-93f01b1e21d6)

Some improvements I would make include standardizing the way I drew bounding boxes. In my current dataset, some boxes included the astronaut's face while others did not, leading to inconsistency. To fix this, I would go back and relabel the dataset to ensure all annotations follow the same guidelines.

I would also like to fine-tune the model further by increasing the number of training epochs. Since I trained the model using a CPU, I was limited in training time. In the future, I’d like to run the training on a GPU, which would allow for more extensive training and potentially better performance.
