<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "482bccabe1df958496ea71a3667995cd",
  "translation_date": "2025-10-11T12:02:08+00:00",
  "source_file": "7-TimeSeries/3-SVR/README.md",
  "language_code": "ta"
}
-->
# ஆதரவாளர் வெக்டர் ரெக்ரெசர் மூலம் நேரம் வரிசை முன்னறிவிப்பு

முந்தைய பாடத்தில், ARIMA மாதிரியைப் பயன்படுத்தி நேரம் வரிசை கணிப்புகளை எப்படி செய்ய வேண்டும் என்பதை நீங்கள் கற்றுக்கொண்டீர்கள். இப்போது, தொடர்ச்சியான தரவுகளை முன்னறிவிக்க பயன்படுத்தப்படும் ஆதரவாளர் வெக்டர் ரெக்ரெசர் மாதிரியைப் பற்றி பார்க்கப் போகிறீர்கள்.

## [பாடத்திற்கு முன் வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/) 

## அறிமுகம்

இந்த பாடத்தில், [**SVM**: **S**upport **V**ector **M**achine](https://en.wikipedia.org/wiki/Support-vector_machine) மூலம் ரெக்ரெஷனுக்கான மாதிரிகளை உருவாக்க ஒரு குறிப்பிட்ட முறையை நீங்கள் கண்டறிவீர்கள், அல்லது **SVR: Support Vector Regressor**.

### நேரம் வரிசை முன்னறிவிப்பில் SVR [^1]

நேரம் வரிசை முன்னறிவிப்பில் SVR முக்கியத்துவத்தைப் புரிந்துகொள்வதற்கு முன், நீங்கள் அறிந்திருக்க வேண்டிய சில முக்கிய கருத்துக்கள் இங்கே:

- **ரெக்ரெஷன்:** கொடுக்கப்பட்ட உள்ளீடுகளின் தொகுப்பிலிருந்து தொடர்ச்சியான மதிப்புகளை முன்னறிவிக்க ஒரு மேற்பார்வை கற்றல் தொழில்நுட்பம். அதிகபட்ச எண்ணிக்கையிலான தரவுப் புள்ளிகள் உள்ள இடத்தில் ஒரு வளைவு (அல்லது கோடு) பொருத்துவது இதன் நோக்கம். [மேலும் தகவலுக்கு இங்கே கிளிக் செய்யவும்](https://en.wikipedia.org/wiki/Regression_analysis).
- **ஆதரவாளர் வெக்டர் மெஷின் (SVM):** வகைப்படுத்தல், ரெக்ரெஷன் மற்றும் வெளிப்புற புள்ளிகளை கண்டறிய பயன்படுத்தப்படும் ஒரு வகை மேற்பார்வை இயந்திரக் கற்றல் மாதிரி. இந்த மாதிரி அம்ச இடத்தில் ஒரு ஹைப்பர்பிளேன் ஆகும், இது வகைப்படுத்தலில் எல்லையாகவும், ரெக்ரெஷனில் சிறந்த பொருத்த கோடாகவும் செயல்படுகிறது. SVM-இல், ஒரு கர்னல் செயல்பாடு பொதுவாக தரவுத்தொகுப்பை அதிக அளவிலான பரிமாணங்களின் இடத்திற்கு மாற்ற பயன்படுத்தப்படுகிறது, இதனால் அவை எளிதாக பிரிக்கப்படலாம். [SVM-க்கள் பற்றிய மேலும் தகவலுக்கு இங்கே கிளிக் செய்யவும்](https://en.wikipedia.org/wiki/Support-vector_machine).
- **ஆதரவாளர் வெக்டர் ரெக்ரெசர் (SVR):** SVM-இன் ஒரு வகை, அதிகபட்ச எண்ணிக்கையிலான தரவுப் புள்ளிகள் உள்ள சிறந்த பொருத்த கோட்டை (SVM-இன் வழக்கில் இது ஒரு ஹைப்பர்பிளேன்) கண்டறிய.

### ஏன் SVR? [^1]

முந்தைய பாடத்தில் நீங்கள் ARIMA பற்றி கற்றுக்கொண்டீர்கள், இது நேரம் வரிசை தரவுகளை முன்னறிவிக்க மிகவும் வெற்றிகரமான புள்ளிவிவர ரேகியல் முறை. ஆனால், பல சந்தர்ப்பங்களில், நேரம் வரிசை தரவுகளில் *ரேகையற்ற தன்மை* காணப்படுகிறது, இது ரேகியல் மாதிரிகளால் வரைபட முடியாது. இவ்வாறான சந்தர்ப்பங்களில், ரெக்ரெஷன் பணிகளுக்கான தரவின் ரேகையற்ற தன்மையைப் பரிசீலிக்க SVM-இன் திறன் SVR-ஐ நேரம் வரிசை முன்னறிவிப்பில் வெற்றிகரமாக மாற்றுகிறது.

## பயிற்சி - SVR மாதிரியை உருவாக்கவும்

தரவு தயாரிப்புக்கான முதல் சில படிகள் [ARIMA](https://github.com/microsoft/ML-For-Beginners/tree/main/7-TimeSeries/2-ARIMA) பற்றிய முந்தைய பாடத்தின் படிகளுடன் ஒரே மாதிரியானவை.

இந்த பாடத்தில் [_/working_](https://github.com/microsoft/ML-For-Beginners/tree/main/7-TimeSeries/3-SVR/working) கோப்புறையைத் திறந்து [_notebook.ipynb_](https://github.com/microsoft/ML-For-Beginners/blob/main/7-TimeSeries/3-SVR/working/notebook.ipynb) கோப்பை கண்டறியவும்.[^2]

1. நோட்புக் இயக்கி தேவையான நூலகங்களை இறக்குமதி செய்யவும்: [^2]

   ```python
   import sys
   sys.path.append('../../')
   ```

   ```python
   import os
   import warnings
   import matplotlib.pyplot as plt
   import numpy as np
   import pandas as pd
   import datetime as dt
   import math
   
   from sklearn.svm import SVR
   from sklearn.preprocessing import MinMaxScaler
   from common.utils import load_data, mape
   ```

2. `/data/energy.csv` கோப்பிலிருந்து தரவுகளை Pandas டேட்டாபிரேமில் ஏற்றவும் மற்றும் பார்வையிடவும்: [^2]

   ```python
   energy = load_data('../../data')[['load']]
   ```

3. ஜனவரி 2012 முதல் டிசம்பர் 2014 வரை கிடைக்கும் அனைத்து ஆற்றல் தரவுகளையும் வரைபடமாக்கவும்: [^2]

   ```python
   energy.plot(y='load', subplots=True, figsize=(15, 8), fontsize=12)
   plt.xlabel('timestamp', fontsize=12)
   plt.ylabel('load', fontsize=12)
   plt.show()
   ```

   ![முழு தரவு](../../../../translated_images/full-data.a82ec9957e580e976f651a4fc38f280b9229c6efdbe3cfe7c60abaa9486d2cbe.ta.png)

   இப்போது, உங்கள் SVR மாதிரியை உருவாக்குவோம்.

### பயிற்சி மற்றும் சோதனை தரவுத்தொகுப்புகளை உருவாக்கவும்

தரவுகள் ஏற்றப்பட்டுள்ளதால், அதை பயிற்சி மற்றும் சோதனை தொகுப்புகளாகப் பிரிக்கலாம். பின்னர், SVR-க்கு தேவையான நேரம்-படிகள் அடிப்படையிலான தரவுத்தொகுப்பை உருவாக்க தரவுகளை மறுவடிவமைக்க வேண்டும். உங்கள் மாதிரியை பயிற்சி தொகுப்பில் பயிற்சி செய்ய வேண்டும். மாதிரி பயிற்சியை முடித்த பிறகு, அதன் துல்லியத்தை பயிற்சி தொகுப்பு, சோதனை தொகுப்பு மற்றும் முழு தரவுத்தொகுப்பில் மதிப்பீடு செய்ய வேண்டும், இதனால் மொத்த செயல்திறனைப் பார்க்க முடியும். மாதிரி எதிர்கால காலகட்டங்களில் தகவலைப் பெறாமல் இருக்க (அதாவது *Overfitting* எனப்படும் நிலை) சோதனை தொகுப்பு பயிற்சி தொகுப்பிலிருந்து பின்னர் காலகட்டத்தை உள்ளடக்க வேண்டும்.[^2]

1. செப்டம்பர் 1 முதல் அக்டோபர் 31, 2014 வரை இரண்டு மாத காலத்தை பயிற்சி தொகுப்புக்கு ஒதுக்கவும். சோதனை தொகுப்பு நவம்பர் 1 முதல் டிசம்பர் 31, 2014 வரை இரண்டு மாத காலத்தை உள்ளடக்கும்: [^2]

   ```python
   train_start_dt = '2014-11-01 00:00:00'
   test_start_dt = '2014-12-30 00:00:00'
   ```

2. வேறுபாடுகளை காட்சிப்படுத்தவும்: [^2]

   ```python
   energy[(energy.index < test_start_dt) & (energy.index >= train_start_dt)][['load']].rename(columns={'load':'train'}) \
       .join(energy[test_start_dt:][['load']].rename(columns={'load':'test'}), how='outer') \
       .plot(y=['train', 'test'], figsize=(15, 8), fontsize=12)
   plt.xlabel('timestamp', fontsize=12)
   plt.ylabel('load', fontsize=12)
   plt.show()
   ```

   ![பயிற்சி மற்றும் சோதனை தரவுகள்](../../../../translated_images/train-test.ead0cecbfc341921d4875eccf25fed5eefbb860cdbb69cabcc2276c49e4b33e5.ta.png)

### பயிற்சிக்கான தரவுகளை தயாரிக்கவும்

இப்போது, உங்கள் தரவுகளை வடிகட்டல் மற்றும் அளவீடு செய்வதன் மூலம் பயிற்சிக்க தயாரிக்க வேண்டும். உங்கள் தரவுத்தொகுப்பை தேவையான காலகட்டங்கள் மற்றும் நெடுவரிசைகளை மட்டும் உள்ளடக்க வடிகட்டவும், மற்றும் தரவுகளை 0,1 இடைவெளியில் திட்டமிட அளவீடு செய்யவும்.

1. முதன்மை தரவுத்தொகுப்பை மேலே குறிப்பிடப்பட்ட காலகட்டங்கள் மற்றும் 'load' நெடுவரிசை மற்றும் தேதியை மட்டும் உள்ளடக்க வடிகட்டவும்: [^2]

   ```python
   train = energy.copy()[(energy.index >= train_start_dt) & (energy.index < test_start_dt)][['load']]
   test = energy.copy()[energy.index >= test_start_dt][['load']]
   
   print('Training data shape: ', train.shape)
   print('Test data shape: ', test.shape)
   ```

   ```output
   Training data shape:  (1416, 1)
   Test data shape:  (48, 1)
   ```
   
2. பயிற்சி தரவுகளை (0, 1) வரம்பில் அளவீடு செய்யவும்: [^2]

   ```python
   scaler = MinMaxScaler()
   train['load'] = scaler.fit_transform(train)
   ```
   
4. இப்போது, சோதனை தரவுகளை அளவீடு செய்யவும்: [^2]

   ```python
   test['load'] = scaler.transform(test)
   ```

### நேரம்-படிகள் கொண்ட தரவுகளை உருவாக்கவும் [^1]

SVR-க்கு, உள்ளீட்டு தரவுகளை `[batch, timesteps]` வடிவத்தில் மாற்ற வேண்டும். எனவே, உள்ள `train_data` மற்றும் `test_data` ஆகியவற்றை மறுவடிவமைக்க வேண்டும், இதனால் நேரம்-படிகளை குறிக்கும் புதிய பரிமாணம் இருக்கும்.

```python
# Converting to numpy arrays
train_data = train.values
test_data = test.values
```

இந்த எடுத்துக்காட்டில், `timesteps = 5` என எடுத்துக்கொள்கிறோம். எனவே, மாதிரிக்கு உள்ளீடுகள் முதல் 4 நேரம்-படிகளுக்கான தரவுகள், மற்றும் வெளியீடு 5வது நேரம்-படிக்கான தரவாக இருக்கும்.

```python
timesteps=5
```

பயிற்சி தரவுகளை 2D டென்சராக மாற்றுதல்:

```python
train_data_timesteps=np.array([[j for j in train_data[i:i+timesteps]] for i in range(0,len(train_data)-timesteps+1)])[:,:,0]
train_data_timesteps.shape
```

```output
(1412, 5)
```

சோதனை தரவுகளை 2D டென்சராக மாற்றுதல்:

```python
test_data_timesteps=np.array([[j for j in test_data[i:i+timesteps]] for i in range(0,len(test_data)-timesteps+1)])[:,:,0]
test_data_timesteps.shape
```

```output
(44, 5)
```

பயிற்சி மற்றும் சோதனை தரவுகளிலிருந்து உள்ளீடுகள் மற்றும் வெளியீடுகளைத் தேர்ந்தெடுப்பது:

```python
x_train, y_train = train_data_timesteps[:,:timesteps-1],train_data_timesteps[:,[timesteps-1]]
x_test, y_test = test_data_timesteps[:,:timesteps-1],test_data_timesteps[:,[timesteps-1]]

print(x_train.shape, y_train.shape)
print(x_test.shape, y_test.shape)
```

```output
(1412, 4) (1412, 1)
(44, 4) (44, 1)
```

### SVR-ஐ செயல்படுத்தவும் [^1]

இப்போது, SVR-ஐ செயல்படுத்த நேரம். இந்த செயல்பாட்டைப் பற்றி மேலும் படிக்க, [இந்த ஆவணத்தை](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVR.html) பார்க்கலாம். எங்கள் செயல்பாட்டிற்கான படிகள்:

  1. `SVR()`-ஐ அழைத்து மாதிரி ஹைப்பர்பாராமீட்டர்களை: kernel, gamma, c மற்றும் epsilon ஆகியவற்றை வழங்கி மாதிரியை வரையறுக்கவும்
  2. `fit()` செயல்பாட்டை அழைத்து பயிற்சி தரவுக்கான மாதிரியைத் தயாரிக்கவும்
  3. `predict()` செயல்பாட்டை அழைத்து முன்னறிவிப்புகளை செய்யவும்

இப்போது SVR மாதிரியை உருவாக்குவோம். இங்கே, [RBF kernel](https://scikit-learn.org/stable/modules/svm.html#parameters-of-the-rbf-kernel)-ஐ பயன்படுத்துகிறோம், மற்றும் ஹைப்பர்பாராமீட்டர்கள் gamma, C மற்றும் epsilon ஆகியவற்றை முறையே 0.5, 10 மற்றும் 0.05 என அமைக்கிறோம்.

```python
model = SVR(kernel='rbf',gamma=0.5, C=10, epsilon = 0.05)
```

#### பயிற்சி தரவுகளில் மாதிரியை பொருத்தவும் [^1]

```python
model.fit(x_train, y_train[:,0])
```

```output
SVR(C=10, cache_size=200, coef0=0.0, degree=3, epsilon=0.05, gamma=0.5,
    kernel='rbf', max_iter=-1, shrinking=True, tol=0.001, verbose=False)
```

#### மாதிரி முன்னறிவிப்புகளை செய்யவும் [^1]

```python
y_train_pred = model.predict(x_train).reshape(-1,1)
y_test_pred = model.predict(x_test).reshape(-1,1)

print(y_train_pred.shape, y_test_pred.shape)
```

```output
(1412, 1) (44, 1)
```

நீங்கள் உங்கள் SVR-ஐ உருவாக்கி முடித்துவிட்டீர்கள்! இப்போது அதை மதிப்பீடு செய்ய வேண்டும்.

### உங்கள் மாதிரியை மதிப்பீடு செய்யவும் [^1]

மதிப்பீட்டிற்காக, முதலில் தரவுகளை மீண்டும் அதன் முதன்மை அளவுக்கு அளவீடு செய்ய வேண்டும். பின்னர், செயல்திறனைச் சரிபார்க்க, முதன்மை மற்றும் முன்னறிவிக்கப்பட்ட நேரம் வரிசை வரைபடத்தை வரைபடமாக்கவும், மற்றும் MAPE முடிவையும் அச்சிடவும்.

முன்னறிவிக்கப்பட்ட மற்றும் முதன்மை வெளியீட்டை அளவீடு செய்யவும்:

```python
# Scaling the predictions
y_train_pred = scaler.inverse_transform(y_train_pred)
y_test_pred = scaler.inverse_transform(y_test_pred)

print(len(y_train_pred), len(y_test_pred))
```

```python
# Scaling the original values
y_train = scaler.inverse_transform(y_train)
y_test = scaler.inverse_transform(y_test)

print(len(y_train), len(y_test))
```

#### பயிற்சி மற்றும் சோதனை தரவுகளில் மாதிரி செயல்திறனைச் சரிபார்க்கவும் [^1]

எங்கள் வரைபடத்தின் x-அச்சில் காட்ட தரவுத்தொகுப்பிலிருந்து நேரத்தை எடுக்கிறோம். கவனிக்க, முதல் ```timesteps-1``` மதிப்புகளை முதல் வெளியீட்டிற்கான உள்ளீடாக பயன்படுத்துகிறோம், எனவே வெளியீட்டிற்கான நேரம் அதற்குப் பிறகு தொடங்கும்.

```python
train_timestamps = energy[(energy.index < test_start_dt) & (energy.index >= train_start_dt)].index[timesteps-1:]
test_timestamps = energy[test_start_dt:].index[timesteps-1:]

print(len(train_timestamps), len(test_timestamps))
```

```output
1412 44
```

பயிற்சி தரவுகளுக்கான முன்னறிவிப்புகளை வரைபடமாக்கவும்:

```python
plt.figure(figsize=(25,6))
plt.plot(train_timestamps, y_train, color = 'red', linewidth=2.0, alpha = 0.6)
plt.plot(train_timestamps, y_train_pred, color = 'blue', linewidth=0.8)
plt.legend(['Actual','Predicted'])
plt.xlabel('Timestamp')
plt.title("Training data prediction")
plt.show()
```

![பயிற்சி தரவுகளின் முன்னறிவிப்பு](../../../../translated_images/train-data-predict.3c4ef4e78553104ffdd53d47a4c06414007947ea328e9261ddf48d3eafdefbbf.ta.png)

பயிற்சி தரவுகளுக்கான MAPE அச்சிடவும்

```python
print('MAPE for training data: ', mape(y_train_pred, y_train)*100, '%')
```

```output
MAPE for training data: 1.7195710200875551 %
```

சோதனை தரவுகளுக்கான முன்னறிவிப்புகளை வரைபடமாக்கவும்

```python
plt.figure(figsize=(10,3))
plt.plot(test_timestamps, y_test, color = 'red', linewidth=2.0, alpha = 0.6)
plt.plot(test_timestamps, y_test_pred, color = 'blue', linewidth=0.8)
plt.legend(['Actual','Predicted'])
plt.xlabel('Timestamp')
plt.show()
```

![சோதனை தரவுகளின் முன்னறிவிப்பு](../../../../translated_images/test-data-predict.8afc47ee7e52874f514ebdda4a798647e9ecf44a97cc927c535246fcf7a28aa9.ta.png)

சோதனை தரவுகளுக்கான MAPE அச்சிடவும்

```python
print('MAPE for testing data: ', mape(y_test_pred, y_test)*100, '%')
```

```output
MAPE for testing data:  1.2623790187854018 %
```

🏆 சோதனை தரவுத்தொகுப்பில் நீங்கள் மிகவும் நல்ல முடிவுகளை பெற்றுள்ளீர்கள்!

### முழு தரவுத்தொகுப்பில் மாதிரி செயல்திறனைச் சரிபார்க்கவும் [^1]

```python
# Extracting load values as numpy array
data = energy.copy().values

# Scaling
data = scaler.transform(data)

# Transforming to 2D tensor as per model input requirement
data_timesteps=np.array([[j for j in data[i:i+timesteps]] for i in range(0,len(data)-timesteps+1)])[:,:,0]
print("Tensor shape: ", data_timesteps.shape)

# Selecting inputs and outputs from data
X, Y = data_timesteps[:,:timesteps-1],data_timesteps[:,[timesteps-1]]
print("X shape: ", X.shape,"\nY shape: ", Y.shape)
```

```output
Tensor shape:  (26300, 5)
X shape:  (26300, 4) 
Y shape:  (26300, 1)
```

```python
# Make model predictions
Y_pred = model.predict(X).reshape(-1,1)

# Inverse scale and reshape
Y_pred = scaler.inverse_transform(Y_pred)
Y = scaler.inverse_transform(Y)
```

```python
plt.figure(figsize=(30,8))
plt.plot(Y, color = 'red', linewidth=2.0, alpha = 0.6)
plt.plot(Y_pred, color = 'blue', linewidth=0.8)
plt.legend(['Actual','Predicted'])
plt.xlabel('Timestamp')
plt.show()
```

![முழு தரவுகளின் முன்னறிவிப்பு](../../../../translated_images/full-data-predict.4f0fed16a131c8f3bcc57a3060039dc7f2f714a05b07b68c513e0fe7fb3d8964.ta.png)

```python
print('MAPE: ', mape(Y_pred, Y)*100, '%')
```

```output
MAPE:  2.0572089029888656 %
```


🏆 மிகவும் அழகான வரைபடங்கள், நல்ல துல்லியத்துடன் ஒரு மாதிரியை காட்டுகிறது. நல்ல வேலை!

---

## 🚀சவால்

- மாதிரியை உருவாக்கும்போது ஹைப்பர்பாராமீட்டர்களை (gamma, C, epsilon) மாற்றி சோதனை தரவுகளில் மதிப்பீடு செய்து, எந்த ஹைப்பர்பாராமீட்டர் தொகுப்புகள் சோதனை தரவுகளில் சிறந்த முடிவுகளை வழங்குகின்றன என்பதைப் பாருங்கள். இந்த ஹைப்பர்பாராமீட்டர்கள் பற்றிய மேலும் தகவலுக்கு, [இந்த ஆவணத்தை](https://scikit-learn.org/stable/modules/svm.html#parameters-of-the-rbf-kernel) பார்க்கலாம்.
- மாதிரிக்கான வெவ்வேறு கர்னல் செயல்பாடுகளை பயன்படுத்தி, அவற்றின் செயல்திறனை தரவுத்தொகுப்பில் பகுப்பாய்வு செய்ய முயற்சிக்கவும். உதவியாக இருக்கும் ஆவணம் [இங்கே](https://scikit-learn.org/stable/modules/svm.html#kernel-functions) கிடைக்கிறது.
- மாதிரிக்கு முன்னறிவிப்பு செய்ய பின்போக்கு பார்க்க `timesteps`-க்கு வெவ்வேறு மதிப்புகளை பயன்படுத்த முயற்சிக்கவும்.

## [பாடத்திற்கு பின் வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## மதிப்பீடு மற்றும் சுயபடிப்பு

இந்த பாடம் நேரம் வரிசை முன்னறிவிப்புக்கான SVR பயன்பாட்டை அறிமுகப்படுத்தியது. SVR பற்றி மேலும் படிக்க, [இந்த வலைப்பதிவை](https://www.analyticsvidhya.com/blog/2020/03/support-vector-regression-tutorial-for-machine-learning/) பார்க்கலாம். இந்த [scikit-learn ஆவணம்](https://scikit-learn.org/stable/modules/svm.html) பொதுவாக SVM-க்கள், [SVRs](https://scikit-learn.org/stable/modules/svm.html#regression) மற்றும் [கர்னல் செயல்பாடுகள்](https://scikit-learn.org/stable/modules/svm.html#kernel-functions) போன்ற பிற செயல்பாடுகள் மற்றும் அவற்றின் அளவுருக்கள் பற்றிய விரிவான விளக்கத்தை வழங்குகிறது.

## பணிக்கட்டளை

[ஒரு புதிய SVR மாதிரி](assignment.md)

## நன்றி

[^1]: இந்த பிரிவில் உள்ள உரை, குறியீடு மற்றும் வெளியீடு [@AnirbanMukherjeeXD](https://github.com/AnirbanMukherjeeXD) மூலம் வழங்கப்பட்டது
[^2]: இந்த பிரிவில் உள்ள உரை, குறியீடு மற்றும் வெளியீடு [ARIMA](https://github.com/microsoft/ML-For-Beginners/tree/main/7-TimeSeries/2-ARIMA) இலிருந்து எடுக்கப்பட்டது

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. எங்கள் தரச்செயல்முறைகளுக்கு முழு முயற்சி எடுத்தாலும், தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறுகள் இருக்கக்கூடும் என்பதை தயவுசெய்து கவனத்தில் கொள்ளவும். அதன் இயல்பான மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.