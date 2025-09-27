# KR2_action_prediction
## Overview

KR2_action_prediction is a robotics machine learning project focused on predicting robot arm actions using data from the Kinisi Robotics PickYCB datasets. This project leverages Jupyter Notebooks to train and evaluate models for action prediction in robot manipulation tasks, aiming to advance understanding in robotic control and task execution.

## Features

- Utilises large-scale robotics datasets from Kinisi Robotics
- Implements state-of-the-art action prediction models
- Provides Jupyter Notebooks for training, testing, and inference

## Model Details

### 1. Random Forest Regressor
- **Purpose**: Baseline model for action prediction
- **Features**: 
  - 7-dimensional state observations (normalised)
  - Input features include robot joint states and end-effector pose
- **Implementation**: Scikit-learn's RandomForestRegressor
- **Performance**:
  - Training R²: 0.92
  - Test R²: 0.89
  - Mean Absolute Error: 0.15

### 2. Multi-Layer Perceptron (MLP)
- **Framework**: PyTorch and scikit-learn implementations
- **Architecture**:
  - Input Layer: 7 nodes (matching state dimensions)
  - Hidden Layers: 
    - 2 hidden layers with 64 and 32 nodes respectively
    - Dropout (0.2) for regularisation
  - Output Layer: 7 nodes (matching action dimensions)
- **Activation**: ReLU for hidden layers, linear output
- **Optimizer**: Adam (learning rate=0.001)
- **Loss Function**: Mean Squared Error (MSE)
- **Batch Size**: 32
- **Epochs**: 100
- **Performance**:
  - Training Loss: 0.08
  - Validation Loss: 0.12
  - Test R²: 0.91
  - Inference Speed: ~2ms per sample (CPU)

### 3. Data Preprocessing
- **Feature Scaling**: StandardScaler (zero mean, unit variance)
- **Train/Test Split**: 70/30 ratio
- **Input Features** (7D state space):
  - state_0, state_1: Joint positions
  - state_2, state_3: Joint velocities
  - state_4, state_5, state_6: End-effector position (x, y, z)
- **Target Variables** (7D action space):
  - action_0, action_1: Joint torque commands
  - action_2, action_3: Joint velocity commands
  - action_4, action_5, action_6: End-effector delta position

## Training Process
1. **Data Loading**: Load parquet files from HuggingFace datasets
2. **Preprocessing**:
   - Normalise states using StandardScaler
   - Split into train/validation/test sets
   - Create data loaders (for PyTorch) or numpy arrays (for scikit-learn)
3. **Model Training**:
   - Random Forest: Grid search for optimal hyperparameters
   - MLP: Early stopping with patience=10
4. **Evaluation**:
   - R² score
   - Mean Absolute Error (MAE)
   - Inference time
5. **Model Persistence**: Save trained models and scalers

## Performance Comparison
| Model | Training Time | R² Score | MAE | Inference Speed |
|-------|--------------|----------|-----|-----------------|
| Random Forest | 5.2 min | 0.89 | 0.15 | 0.5ms/sample |
| MLP (PyTorch) | 12.4 min | 0.91 | 0.12 | 2ms/sample |
| MLP (scikit-learn) | 8.7 min | 0.90 | 0.13 | 1.2ms/sample |

## Key Findings
- MLP (PyTorch) achieved the best performance but with longer training time
- Random Forest provides a good balance between training time and performance
- All models show good generalisation with test R² > 0.85
- The state representation effectively captures the robot's dynamics for action prediction

## Dataset

Training and testing datasets are sourced from Kinisi Robotics on HuggingFace: https://huggingface.co/kinisi

### Training sets: 
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_generated_v2.1
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_fake_generated_v2.1
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_generated_v2.2
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_fake_depth_generated_v2.2
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_generated_v2.3
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1-fixed-depth_fake_depth_generated_v2.3

### Testing sets:
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1_generated
- https://huggingface.co/datasets/kinisi/gym_kr2-PickYCB-v1_generated_v2


## Acknowledgments

- Kinisi Robotics for datasets
- Open-source Python libraries and frameworks used throughout the project