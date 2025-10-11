<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "1a6e9e46b34a2e559fbbfc1f95397c7b",
  "translation_date": "2025-10-11T11:53:24+00:00",
  "source_file": "4-Classification/2-Classifiers-1/README.md",
  "language_code": "ta"
}
-->
# உணவுப் வகை வகைப்படுத்திகள் 1

இந்த பாடத்தில், நீங்கள் சமநிலையான மற்றும் சுத்தமான தரவுகளால் நிரம்பிய, உணவுப் வகைகள் பற்றிய தரவுத்தொகுப்பைப் பயன்படுத்துவீர்கள்.

இந்த தரவுத்தொகுப்பைப் பயன்படுத்தி, _ஒரு குறிப்பிட்ட நாட்டின் உணவுப் வகையை பொருட்களின் குழுவின் அடிப்படையில் கணிக்க_ பல வகைப்படுத்திகளைப் பயன்படுத்துவீர்கள். இதைச் செய்யும் போது, வகைப்படுத்தல் பணிகளுக்கான ஆல்கொரிதங்களை எவ்வாறு பயன்படுத்தலாம் என்பதைப் பற்றி மேலும் அறிந்து கொள்வீர்கள்.

## [பாடத்திற்கு முன் வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)
# தயாரிப்பு

நீங்கள் [பாடம் 1](../1-Introduction/README.md) முடித்திருக்கிறீர்கள் என்று கருதினால், இந்த நான்கு பாடங்களுக்கான அடிப்படை `/data` கோப்பகத்தில் _cleaned_cuisines.csv_ கோப்பு உள்ளதா என்பதை உறுதிப்படுத்துங்கள்.

## பயிற்சி - ஒரு நாட்டின் உணவுப் வகையை கணிக்க

1. இந்த பாடத்தின் _notebook.ipynb_ கோப்பகத்தில் Pandas நூலகத்துடன் அந்த கோப்பை இறக்குமதி செய்யுங்கள்:

    ```python
    import pandas as pd
    cuisines_df = pd.read_csv("../data/cleaned_cuisines.csv")
    cuisines_df.head()
    ```

    தரவுகள் இவ்வாறு இருக்கும்:

|     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
| --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
| 0   | 0          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 1   | 1          | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 2   | 2          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 3   | 3          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 4   | 4          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |

1. இப்போது மேலும் சில நூலகங்களை இறக்குமதி செய்யுங்கள்:

    ```python
    from sklearn.linear_model import LogisticRegression
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    from sklearn.svm import SVC
    import numpy as np
    ```

1. X மற்றும் y கோர்டினேட்டுகளை பயிற்சிக்கான இரண்டு தரவத்தொகுப்புகளாகப் பிரிக்கவும். `cuisine` லேபிள் தரவத்தொகுப்பாக இருக்கலாம்:

    ```python
    cuisines_label_df = cuisines_df['cuisine']
    cuisines_label_df.head()
    ```

    இது இவ்வாறு இருக்கும்:

    ```output
    0    indian
    1    indian
    2    indian
    3    indian
    4    indian
    Name: cuisine, dtype: object
    ```

1. `Unnamed: 0` மற்றும் `cuisine` பத்திகளை `drop()` மூலம் நீக்கவும். மீதமுள்ள தரவங்களை பயிற்சிக்க கூடிய அம்சங்களாக சேமிக்கவும்:

    ```python
    cuisines_feature_df = cuisines_df.drop(['Unnamed: 0', 'cuisine'], axis=1)
    cuisines_feature_df.head()
    ```

    உங்கள் அம்சங்கள் இவ்வாறு இருக்கும்:

|      | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | artemisia | artichoke |  ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood |  yam | yeast | yogurt | zucchini |
| ---: | -----: | -------: | ----: | ---------: | ----: | -----------: | ------: | -------: | --------: | --------: | ---: | ------: | ----------: | ---------: | ----------------------: | ---: | ---: | ---: | ----: | -----: | -------: |
|    0 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    1 |      1 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    2 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    3 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    4 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      1 |        0 | 0 |

இப்போது உங்கள் மாதிரியைப் பயிற்சிக்க தயாராக இருக்கிறீர்கள்!

## உங்கள் வகைப்படுத்தியைத் தேர்ந்தெடுக்கவும்

தரவுகள் சுத்தமாகவும் பயிற்சிக்க தயாராகவும் உள்ளதால், இந்த பணிக்கான ஆல்கொரிதத்தை தேர்ந்தெடுக்க வேண்டும்.

Scikit-learn வகைப்படுத்தலை Supervised Learning கீழ் குழுவாக்குகிறது, மற்றும் அந்த வகையில் பல வகைப்படுத்தல் முறைகளை காணலாம். [வகைகளின் பல்வேறு](https://scikit-learn.org/stable/supervised_learning.html) முறைகள் முதலில் குழப்பமாக தோன்றலாம். பின்வரும் முறைகள் அனைத்தும் வகைப்படுத்தல் தொழில்நுட்பங்களை உள்ளடக்கியவை:

- நேரியல் மாதிரிகள்
- Support Vector Machines
- Stochastic Gradient Descent
- Nearest Neighbors
- Gaussian Processes
- Decision Trees
- Ensemble methods (voting Classifier)
- Multiclass மற்றும் multioutput ஆல்கொரிதங்கள் (multiclass மற்றும் multilabel வகைப்படுத்தல், multiclass-multioutput வகைப்படுத்தல்)

> [நரம்பியல் வலைகளைப்](https://scikit-learn.org/stable/modules/neural_networks_supervised.html#classification) பயன்படுத்தி தரவுகளை வகைப்படுத்தலாம், ஆனால் அது இந்த பாடத்தின் வரம்புக்கு வெளியே உள்ளது.

### எந்த வகைப்படுத்தியைத் தேர்ந்தெடுப்பது?

அப்படியென்றால், எந்த வகைப்படுத்தியை நீங்கள் தேர்ந்தெடுக்க வேண்டும்? பலவற்றைச் சோதித்து நல்ல முடிவைத் தேடுவது ஒரு வழியாக இருக்கலாம். Scikit-learn ஒரு [பக்கமொத்த ஒப்பீட்டை](https://scikit-learn.org/stable/auto_examples/classification/plot_classifier_comparison.html) வழங்குகிறது, KNeighbors, SVC இரண்டு வழிகள், GaussianProcessClassifier, DecisionTreeClassifier, RandomForestClassifier, MLPClassifier, AdaBoostClassifier, GaussianNB மற்றும் QuadraticDiscrinationAnalysis ஆகியவற்றை ஒப்பிட்டு, முடிவுகளை காட்சிப்படுத்துகிறது:

![வகைப்படுத்திகளின் ஒப்பீடு](../../../../translated_images/comparison.edfab56193a85e7fdecbeaa1b1f8c99e94adbf7178bed0de902090cf93d6734f.ta.png)
> Scikit-learn ஆவணங்களில் உருவாக்கப்பட்ட வரைபடங்கள்

> AutoML இந்தப் பிரச்சினையை எளிதாகத் தீர்க்கிறது, இந்த ஒப்பீடுகளை மேகத்தில் இயக்கி, உங்கள் தரவுக்கான சிறந்த ஆல்கொரிதத்தைத் தேர்ந்தெடுக்க அனுமதிக்கிறது. [இங்கே முயற்சிக்கவும்](https://docs.microsoft.com/learn/modules/automate-model-selection-with-azure-automl/?WT.mc_id=academic-77952-leestott)

### ஒரு சிறந்த அணுகுமுறை

குறிப்பிட்ட ஆல்கொரிதத்தைத் தேர்ந்தெடுப்பதற்கான ஒரு சிறந்த வழி, இந்த பதிவிறக்கக்கூடிய [ML Cheat sheet](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=academic-77952-leestott) ஐப் பின்பற்றுவது. இங்கே, நமது multiclass பிரச்சினைக்கான சில தேர்வுகளை காணலாம்:

![multiclass பிரச்சினைகளுக்கான cheat sheet](../../../../translated_images/cheatsheet.07a475ea444d22234cb8907a3826df5bdd1953efec94bd18e4496f36ff60624a.ta.png)
> Microsoft's Algorithm Cheat Sheet இன் ஒரு பகுதி, multiclass வகைப்படுத்தல் விருப்பங்களை விவரிக்கிறது

✅ இந்த cheat sheet ஐ பதிவிறக்கி, அச்சிட்டு, உங்கள் சுவரில் தொங்கவிடுங்கள்!

### காரணம்

நாம் கொண்டுள்ள கட்டுப்பாடுகளைப் பொருத்து, பல்வேறு அணுகுமுறைகளைப் பற்றி சிந்திக்கலாம்:

- **நரம்பியல் வலைகள் மிகப்பெரியது**. நமது சுத்தமான ஆனால் குறைந்த தரவுத்தொகுப்பை கருத்தில் கொண்டு, மற்றும் பயிற்சியை உள்ளூர் notebook களில் இயக்குவதால், நரம்பியல் வலைகள் இந்த பணிக்காக மிகப்பெரியது.
- **இரு வகை வகைப்படுத்தி இல்லை**. இரண்டு வகை வகைப்படுத்தியைப் பயன்படுத்தமாட்டோம், எனவே one-vs-all ஐ தவிர்க்க வேண்டும்.
- **Decision tree அல்லது logistic regression வேலை செய்யலாம்**. Decision tree வேலை செய்யலாம், அல்லது multiclass தரவுக்கான logistic regression.
- **Multiclass Boosted Decision Trees வேறு பிரச்சினையைத் தீர்க்கிறது**. Multiclass Boosted Decision Tree nonparametric பணிகளுக்கு மிகவும் பொருத்தமானது, உதாரணமாக தரவரிசைகளை உருவாக்கும் பணிகள், எனவே இது நமக்கு பயனுள்ளதாக இல்லை.

### Scikit-learn ஐப் பயன்படுத்துதல்

நாம் Scikit-learn ஐப் பயன்படுத்தி நமது தரவுகளைப் பகுப்பாய்வு செய்ய உள்ளோம். ஆனால், Scikit-learn இல் logistic regression ஐப் பயன்படுத்த பல வழிகள் உள்ளன. [கொடுக்க வேண்டிய அளவுருக்களைப்](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression) பாருங்கள்.

முக்கியமாக இரண்டு முக்கியமான அளவுருக்கள் - `multi_class` மற்றும் `solver` - உள்ளன, அவற்றை நாம் Scikit-learn ஐ logistic regression செய்ய கேட்கும்போது குறிப்பிட வேண்டும். `multi_class` மதிப்பு ஒரு குறிப்பிட்ட நடத்தைப் பயன்படுத்துகிறது. solver இன் மதிப்பு எந்த ஆல்கொரிதத்தைப் பயன்படுத்துவது என்பதை குறிப்பிடுகிறது. அனைத்து solvers ஐயும் அனைத்து `multi_class` மதிப்புகளுடன் இணைக்க முடியாது.

ஆவணங்களின் படி, multiclass வழக்கில், பயிற்சி ஆல்கொரிதம்:

- **one-vs-rest (OvR) திட்டத்தைப் பயன்படுத்துகிறது**, `multi_class` விருப்பம் `ovr` ஆக அமைக்கப்பட்டால்
- **cross-entropy loss ஐப் பயன்படுத்துகிறது**, `multi_class` விருப்பம் `multinomial` ஆக அமைக்கப்பட்டால். (தற்போது `multinomial` விருப்பம் ‘lbfgs’, ‘sag’, ‘saga’ மற்றும் ‘newton-cg’ solvers மூலம் மட்டுமே ஆதரிக்கப்படுகிறது.)

> 🎓 இங்கே 'scheme' என்பது 'ovr' (one-vs-rest) அல்லது 'multinomial' ஆக இருக்கலாம். Logistic regression உண்மையில் binary வகைப்படுத்தலுக்கு ஆதரவு அளிக்க வடிவமைக்கப்பட்டதால், இந்த திட்டங்கள் multiclass வகைப்படுத்தல் பணிகளைச் சிறப்பாக கையாள அனுமதிக்கின்றன. [source](https://machinelearningmastery.com/one-vs-rest-and-one-vs-one-for-multi-class-classification/)

> 🎓 'solver' என்பது "optimization பிரச்சினையில் பயன்படுத்த வேண்டிய ஆல்கொரிதம்" என வரையறுக்கப்பட்டுள்ளது. [source](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression).

Scikit-learn இந்த அட்டவணையை வழங்குகிறது, solvers வெவ்வேறு தரவுத்தொகுப்புகளால் ஏற்படும் சவால்களை எவ்வாறு கையாளுகின்றன என்பதை விளக்க:

![solvers](../../../../translated_images/solvers.5fc648618529e627dfac29b917b3ccabda4b45ee8ed41b0acb1ce1441e8d1ef1.ta.png)

## பயிற்சி - தரவுகளைப் பிரிக்க

நீங்கள் சமீபத்தில் ஒரு முந்தைய பாடத்தில் logistic regression பற்றி கற்றுக்கொண்டதால், நமது முதல் பயிற்சி முயற்சிக்காக அதைப் பயன்படுத்தலாம்.
`train_test_split()` ஐ அழைப்பதன் மூலம் உங்கள் தரவுகளை பயிற்சி மற்றும் சோதனை குழுக்களாகப் பிரிக்கவும்:

```python
X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
```

## பயிற்சி - logistic regression ஐப் பயன்படுத்தவும்

நீங்கள் multiclass வழக்கைப் பயன்படுத்துகிறீர்கள், எனவே எந்த _scheme_ ஐப் பயன்படுத்த வேண்டும் மற்றும் எந்த _solver_ ஐ அமைக்க வேண்டும் என்பதைத் தேர்ந்தெடுக்க வேண்டும். LogisticRegression ஐ multiclass அமைப்புடன் மற்றும் **liblinear** solver ஐப் பயன்படுத்தி பயிற்சி செய்யுங்கள்.

1. multi_class ஐ `ovr` ஆக அமைத்து மற்றும் solver ஐ `liblinear` ஆக அமைத்து ஒரு logistic regression ஐ உருவாக்கவும்:

    ```python
    lr = LogisticRegression(multi_class='ovr',solver='liblinear')
    model = lr.fit(X_train, np.ravel(y_train))
    
    accuracy = model.score(X_test, y_test)
    print ("Accuracy is {}".format(accuracy))
    ```

    ✅ `lbfgs` போன்ற வேறு solver ஐ முயற்சிக்கவும், இது அடிக்கடி இயல்பாக அமைக்கப்படுகிறது

    > கவனிக்கவும், Pandas [`ravel`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.ravel.html) செயல்பாட்டை உங்கள் தரவுகளை flatten செய்ய பயன்படுத்தவும்.

    துல்லியம் **80%** க்கும் மேல் உள்ளது!

1. இந்த மாதிரியை ஒரு தரவுத்தொகுப்பு (#50) ஐ சோதனை செய்வதன் மூலம் செயல்படுவது பார்க்கலாம்:

    ```python
    print(f'ingredients: {X_test.iloc[50][X_test.iloc[50]!=0].keys()}')
    print(f'cuisine: {y_test.iloc[50]}')
    ```

    முடிவு அச்சிடப்படுகிறது:

   ```output
   ingredients: Index(['cilantro', 'onion', 'pea', 'potato', 'tomato', 'vegetable_oil'], dtype='object')
   cuisine: indian
   ```

✅ வேறு வரிசை எண்ணை முயற்சித்து முடிவுகளை சரிபார்க்கவும்

1. மேலும் ஆழமாக, இந்த கணிப்பின் துல்லியத்தை சரிபார்க்கலாம்:

    ```python
    test= X_test.iloc[50].values.reshape(-1, 1).T
    proba = model.predict_proba(test)
    classes = model.classes_
    resultdf = pd.DataFrame(data=proba, columns=classes)
    
    topPrediction = resultdf.T.sort_values(by=[0], ascending = [False])
    topPrediction.head()
    ```

    முடிவு அச்சிடப்பட்டுள்ளது - இந்திய உணவு அதன் சிறந்த கணிப்பு, நல்ல சாத்தியத்துடன்:

    |          |        0 |
    | -------: | -------: |
    |   indian | 0.715851 |
    |  chinese | 0.229475 |
    | japanese | 0.029763 |
    |   korean | 0.017277 |
    |     thai | 0.007634 |

    ✅ மாடல் இது இந்திய உணவாக இருக்கிறது என்று நம்புவதற்கான காரணத்தை விளக்க முடியுமா?

1. ரிக்ரஷன் பாடங்களில் செய்தது போல, ஒரு வகைப்பாட்டு அறிக்கையை அச்சிட்டு மேலும் விவரங்களைப் பெறுங்கள்:

    ```python
    y_pred = model.predict(X_test)
    print(classification_report(y_test,y_pred))
    ```

    |              | precision | recall | f1-score | support |
    | ------------ | --------- | ------ | -------- | ------- |
    | chinese      | 0.73      | 0.71   | 0.72     | 229     |
    | indian       | 0.91      | 0.93   | 0.92     | 254     |
    | japanese     | 0.70      | 0.75   | 0.72     | 220     |
    | korean       | 0.86      | 0.76   | 0.81     | 242     |
    | thai         | 0.79      | 0.85   | 0.82     | 254     |
    | accuracy     | 0.80      | 1199   |          |         |
    | macro avg    | 0.80      | 0.80   | 0.80     | 1199    |
    | weighted avg | 0.80      | 0.80   | 0.80     | 1199    |

## 🚀சவால்

இந்த பாடத்தில், நீங்கள் உங்கள் சுத்தமான தரவுகளைப் பயன்படுத்தி ஒரு இயந்திரக் கற்றல் மாடலை உருவாக்கினீர்கள், இது பொருட்களின் தொடரின் அடிப்படையில் ஒரு தேசிய உணவை கணிக்க முடியும். Scikit-learn பல்வேறு தரவுகளை வகைப்படுத்துவதற்கான விருப்பங்களைப் படிக்க சில நேரம் ஒதுக்கவும். 'solver' என்ற கருத்தை மேலும் ஆழமாகப் புரிந்து கொள்ளுங்கள், பின்னணியில் என்ன நடக்கிறது என்பதைப் புரிந்து கொள்ள.

## [பாடத்திற்குப் பிந்தைய வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## மதிப்பீடு & சுயபடிப்பு

லாஜிஸ்டிக் ரிக்ரஷனின் கணிதத்தை [இந்த பாடத்தில்](https://people.eecs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf) மேலும் ஆழமாகப் படிக்கவும்.
## பணிக்கூற்று 

[சால்வர்களை படிக்கவும்](assignment.md)

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கின்றோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.