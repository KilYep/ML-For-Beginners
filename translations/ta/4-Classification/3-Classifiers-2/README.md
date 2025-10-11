<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "49047911108adc49d605cddfb455749c",
  "translation_date": "2025-10-11T11:56:25+00:00",
  "source_file": "4-Classification/3-Classifiers-2/README.md",
  "language_code": "ta"
}
-->
# உணவுப் வகை வகைப்படுத்திகள் 2

இரண்டாவது வகைப்படுத்தல் பாடத்தில், எண் தரவுகளை வகைப்படுத்துவதற்கான மேலும் பல வழிகளை நீங்கள் ஆராய்வீர்கள். மேலும், ஒரு வகைப்படுத்தியை மற்றொன்றுக்கு மேல் தேர்ந்தெடுப்பதற்கான விளைவுகளைப் பற்றியும் நீங்கள் அறிந்துகொள்வீர்கள்.

## [முன்-பாடம் வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

### முன் அறிவு

நீங்கள் முந்தைய பாடங்களை முடித்துவிட்டு, `data` கோப்பகத்தில் _cleaned_cuisines.csv_ என்ற பெயரில் சுத்தமான தரவுத்தொகுப்பை வைத்திருக்கிறீர்கள் என்று நாங்கள் கருதுகிறோம். இது இந்த 4-பாடம் கோப்பகத்தின் அடிப்பகுதியில் உள்ளது.

### தயாரிப்பு

உங்கள் _notebook.ipynb_ கோப்பில் சுத்தமான தரவுத்தொகுப்பை ஏற்றியுள்ளோம், மேலும் அதை X மற்றும் y தரவுத்தொகுப்புகளாகப் பிரித்துள்ளோம், மாடல் உருவாக்க செயல்முறைக்கு தயாராக.

## ஒரு வகைப்படுத்தல் வரைபடம்

முந்தைய பாடத்தில், Microsoft-இன் சாட் ஷீட்டை பயன்படுத்தி தரவுகளை வகைப்படுத்துவதற்கான பல விருப்பங்களைப் பற்றி நீங்கள் கற்றுக்கொண்டீர்கள். Scikit-learn இதே போன்ற, ஆனால் மேலும் விரிவான சாட் ஷீட்டை வழங்குகிறது, இது உங்கள் வகைப்படுத்திகளை (வகைப்படுத்திகள் என்ற மற்றொரு சொல்) குறைக்க உதவுகிறது:

![Scikit-learn-இன் ML வரைபடம்](../../../../translated_images/map.e963a6a51349425ab107b38f6c7307eb4c0d0c7ccdd2e81a5e1919292bab9ac7.ta.png)
> குறிப்புகள்: [இந்த வரைபடத்தை ஆன்லைனில் பாருங்கள்](https://scikit-learn.org/stable/tutorial/machine_learning_map/) மற்றும் பாதையில் கிளிக் செய்து ஆவணங்களைப் படிக்கவும்.

### திட்டம்

இந்த வரைபடம் உங்கள் தரவுகளை தெளிவாகப் புரிந்துகொண்ட பிறகு மிகவும் உதவியாக இருக்கும், ஏனெனில் நீங்கள் அதன் பாதைகளில் 'நடந்து' முடிவுக்கு வரலாம்:

- நமக்கு >50 மாதிரிகள் உள்ளன
- நாங்கள் ஒரு வகையை முன்னறிவிக்க விரும்புகிறோம்
- நமக்கு லேபிள் செய்யப்பட்ட தரவுகள் உள்ளன
- நமக்கு 100K மாதிரிகளுக்கு குறைவாக உள்ளது
- ✨ நாங்கள் Linear SVC-ஐ தேர்ந்தெடுக்கலாம்
- அது வேலை செய்யவில்லை என்றால், எண் தரவுகள் இருப்பதால்
    - ✨ KNeighbors Classifier-ஐ முயற்சிக்கலாம்
      - அது வேலை செய்யவில்லை என்றால், ✨ SVC மற்றும் ✨ Ensemble Classifiers-ஐ முயற்சிக்கவும்

இது பின்பற்ற மிகவும் உதவியாக இருக்கும் பாதை.

## பயிற்சி - தரவுகளைப் பிரிக்கவும்

இந்த பாதையைப் பின்பற்ற, பயன்படுத்த சில நூலகங்களை இறக்குமதி செய்ய வேண்டும்.

1. தேவையான நூலகங்களை இறக்குமதி செய்யவும்:

    ```python
    from sklearn.neighbors import KNeighborsClassifier
    from sklearn.linear_model import LogisticRegression
    from sklearn.svm import SVC
    from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    import numpy as np
    ```

1. உங்கள் பயிற்சி மற்றும் சோதனை தரவுகளைப் பிரிக்கவும்:

    ```python
    X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
    ```

## Linear SVC வகைப்படுத்தி

Support-Vector clustering (SVC) என்பது Support-Vector machines குடும்பத்தின் ML தொழில்நுட்பங்களின் ஒரு கிளையாகும் (கீழே இவற்றைப் பற்றி மேலும் அறிக). இந்த முறையில், லேபிள்களை குழுவாக்க 'kernel' ஒன்றைத் தேர்ந்தெடுக்கலாம். 'C' அளவுரு 'regularization'-ஐ குறிக்கிறது, இது அளவுருக்களின் தாக்கத்தை ஒழுங்குபடுத்துகிறது. kernel [பலவற்றில்](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC) ஒன்று இருக்கலாம்; இங்கு 'linear'-ஐ அமைத்துள்ளோம், இது Linear SVC-ஐ பயன்படுத்துவதை உறுதிப்படுத்துகிறது. Probability இயல்பாக 'false' ஆக உள்ளது; இங்கு probability estimates-ஐ பெற 'true' ஆக அமைத்துள்ளோம். தரவுகளை probability பெற சீரமைக்க 'random state'-ஐ '0' ஆக அமைத்துள்ளோம்.

### பயிற்சி - Linear SVC-ஐ பயன்படுத்தவும்

வகைப்படுத்திகளை ஒரு வரிசையாக உருவாக்கத் தொடங்குங்கள். நாம் சோதிக்கும்போது இந்த வரிசையில் தொடர்ந்து சேர்க்கலாம்.

1. Linear SVC-இன் மூலம் தொடங்குங்கள்:

    ```python
    C = 10
    # Create different classifiers.
    classifiers = {
        'Linear SVC': SVC(kernel='linear', C=C, probability=True,random_state=0)
    }
    ```

2. Linear SVC-ஐ பயன்படுத்தி உங்கள் மாடலை பயிற்றுவித்து, ஒரு அறிக்கையை அச்சிடவும்:

    ```python
    n_classifiers = len(classifiers)
    
    for index, (name, classifier) in enumerate(classifiers.items()):
        classifier.fit(X_train, np.ravel(y_train))
    
        y_pred = classifier.predict(X_test)
        accuracy = accuracy_score(y_test, y_pred)
        print("Accuracy (train) for %s: %0.1f%% " % (name, accuracy * 100))
        print(classification_report(y_test,y_pred))
    ```

    முடிவு மிகவும் நல்லது:

    ```output
    Accuracy (train) for Linear SVC: 78.6% 
                  precision    recall  f1-score   support
    
         chinese       0.71      0.67      0.69       242
          indian       0.88      0.86      0.87       234
        japanese       0.79      0.74      0.76       254
          korean       0.85      0.81      0.83       242
            thai       0.71      0.86      0.78       227
    
        accuracy                           0.79      1199
       macro avg       0.79      0.79      0.79      1199
    weighted avg       0.79      0.79      0.79      1199
    ```

## K-Neighbors வகைப்படுத்தி

K-Neighbors என்பது ML முறைகளின் "neighbors" குடும்பத்தின் ஒரு பகுதியாகும், இது மேற்பார்வை மற்றும் கீழ்ப்பார்வை கற்றலுக்கு பயன்படுத்தப்படலாம். இந்த முறையில், முன்கூட்டியே வரையறுக்கப்பட்ட புள்ளிகள் உருவாக்கப்பட்டு, இந்த புள்ளிகளின் சுற்றளவில் தரவுகள் சேகரிக்கப்படுகின்றன, இதனால் தரவுகளுக்கு பொதுவான லேபிள்கள் முன்னறிவிக்கப்படலாம்.

### பயிற்சி - K-Neighbors வகைப்படுத்தியை பயன்படுத்தவும்

முந்தைய வகைப்படுத்தி நல்லது, மேலும் தரவுகளுடன் நன்றாக வேலை செய்தது, ஆனால் நாங்கள் சிறந்த துல்லியத்தைப் பெறலாம். K-Neighbors வகைப்படுத்தியை முயற்சிக்கவும்.

1. உங்கள் வகைப்படுத்தி வரிசையில் ஒரு வரியைச் சேர்க்கவும் (Linear SVC உருப்படியின் பின்னர் ஒரு கமாவைச் சேர்க்கவும்):

    ```python
    'KNN classifier': KNeighborsClassifier(C),
    ```

    முடிவு கொஞ்சம் மோசமாக உள்ளது:

    ```output
    Accuracy (train) for KNN classifier: 73.8% 
                  precision    recall  f1-score   support
    
         chinese       0.64      0.67      0.66       242
          indian       0.86      0.78      0.82       234
        japanese       0.66      0.83      0.74       254
          korean       0.94      0.58      0.72       242
            thai       0.71      0.82      0.76       227
    
        accuracy                           0.74      1199
       macro avg       0.76      0.74      0.74      1199
    weighted avg       0.76      0.74      0.74      1199
    ```

    ✅ [K-Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#neighbors) பற்றி அறிக

## Support Vector Classifier

Support-Vector வகைப்படுத்திகள் [Support-Vector Machine](https://wikipedia.org/wiki/Support-vector_machine) குடும்பத்தின் ஒரு பகுதியாகும், இது வகைப்படுத்தல் மற்றும் பிழைதிருத்த பணிகளுக்கு பயன்படுத்தப்படுகிறது. SVMs "பயிற்சி எடுத்துக்காட்டுகளை இடங்களில் புள்ளிகளாக வரைபடம்" செய்து இரண்டு வகைகளுக்கு இடையிலான தூரத்தை அதிகபட்சமாக்குகிறது. பின்னர் தரவுகள் இந்த இடத்தில் வரைபடம் செய்யப்படுகிறது, இதனால் அவற்றின் வகையை முன்னறிவிக்க முடியும்.

### பயிற்சி - Support Vector Classifier-ஐ பயன்படுத்தவும்

Support Vector Classifier-ஐ பயன்படுத்தி சிறந்த துல்லியத்தை முயற்சிக்கலாம்.

1. K-Neighbors உருப்படியின் பின்னர் ஒரு கமாவைச் சேர்த்து, இந்த வரியைச் சேர்க்கவும்:

    ```python
    'SVC': SVC(),
    ```

    முடிவு மிகவும் நல்லது!

    ```output
    Accuracy (train) for SVC: 83.2% 
                  precision    recall  f1-score   support
    
         chinese       0.79      0.74      0.76       242
          indian       0.88      0.90      0.89       234
        japanese       0.87      0.81      0.84       254
          korean       0.91      0.82      0.86       242
            thai       0.74      0.90      0.81       227
    
        accuracy                           0.83      1199
       macro avg       0.84      0.83      0.83      1199
    weighted avg       0.84      0.83      0.83      1199
    ```

    ✅ [Support-Vectors](https://scikit-learn.org/stable/modules/svm.html#svm) பற்றி அறிக

## Ensemble Classifiers

முந்தைய சோதனை மிகவும் நல்லது என்றாலும், பாதையின் இறுதிவரை செல்வோம். 'Ensemble Classifiers' ஐ, குறிப்பாக Random Forest மற்றும் AdaBoost-ஐ முயற்சிக்கலாம்:

```python
  'RFST': RandomForestClassifier(n_estimators=100),
  'ADA': AdaBoostClassifier(n_estimators=100)
```

முடிவு மிகவும் நல்லது, குறிப்பாக Random Forest:

```output
Accuracy (train) for RFST: 84.5% 
              precision    recall  f1-score   support

     chinese       0.80      0.77      0.78       242
      indian       0.89      0.92      0.90       234
    japanese       0.86      0.84      0.85       254
      korean       0.88      0.83      0.85       242
        thai       0.80      0.87      0.83       227

    accuracy                           0.84      1199
   macro avg       0.85      0.85      0.84      1199
weighted avg       0.85      0.84      0.84      1199

Accuracy (train) for ADA: 72.4% 
              precision    recall  f1-score   support

     chinese       0.64      0.49      0.56       242
      indian       0.91      0.83      0.87       234
    japanese       0.68      0.69      0.69       254
      korean       0.73      0.79      0.76       242
        thai       0.67      0.83      0.74       227

    accuracy                           0.72      1199
   macro avg       0.73      0.73      0.72      1199
weighted avg       0.73      0.72      0.72      1199
```

✅ [Ensemble Classifiers](https://scikit-learn.org/stable/modules/ensemble.html) பற்றி அறிக

இந்த Machine Learning முறை "பல அடிப்படை வகைப்படுத்திகளின் முன்னறிவிப்புகளை இணைக்கிறது" மாடலின் தரத்தை மேம்படுத்த. எங்கள் எடுத்துக்காட்டில், Random Trees மற்றும் AdaBoost-ஐ பயன்படுத்தினோம்.

- [Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#forest), ஒரு சராசரி முறை, 'decision trees' களின் 'காடு' ஒன்றை உருவாக்குகிறது, இது overfitting-ஐ தவிர்க்க சீரற்ற தன்மையுடன் சேர்க்கப்படுகிறது. n_estimators அளவுரு மரங்களின் எண்ணிக்கையை அமைக்கிறது.

- [AdaBoost](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html) ஒரு வகைப்படுத்தியை தரவுத்தொகுப்பில் பொருத்தி, பின்னர் அதே தரவுத்தொகுப்பில் அந்த வகைப்படுத்தியின் பிரதிகளை பொருத்துகிறது. தவறாக வகைப்படுத்தப்பட்ட உருப்படிகளின் எடைகளில் கவனம் செலுத்தி, அடுத்த வகைப்படுத்தியை சரிசெய்ய பொருத்தத்தை சரிசெய்கிறது.

---

## 🚀சவால்

இந்த முறைகளில் ஒவ்வொன்றுக்கும் நீங்கள் சரிசெய்யக்கூடிய அளவுருக்கள் நிறைய உள்ளன. ஒவ்வொன்றின் இயல்புநிலை அளவுருக்களை ஆராய்ந்து, இந்த அளவுருக்களை சரிசெய்வது மாடலின் தரத்துக்கு என்ன பொருள் என்று யோசிக்கவும்.

## [பாடத்திற்குப் பிந்தைய வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## மதிப்பீடு & சுய கற்றல்

இந்த பாடங்களில் பல தொழில்நுட்ப சொற்கள் உள்ளன, எனவே [இந்த பட்டியலை](https://docs.microsoft.com/dotnet/machine-learning/resources/glossary?WT.mc_id=academic-77952-leestott) ஒரு நிமிடம் பாருங்கள், பயனுள்ள சொற்களை மதிப்பீடு செய்ய!

## பணிக்கட்டளை 

[அளவுரு விளையாட்டு](assignment.md)

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையை பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. எங்கள் தரச்சிறப்பிற்காக முயற்சிக்கின்றோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறுகள் இருக்கக்கூடும் என்பதை கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.