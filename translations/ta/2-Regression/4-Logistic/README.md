<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "abf86d845c84330bce205a46b382ec88",
  "translation_date": "2025-10-11T11:41:17+00:00",
  "source_file": "2-Regression/4-Logistic/README.md",
  "language_code": "ta"
}
-->
# வகைகளை கணிக்க Logistic Regression

![Logistic vs. linear regression infographic](../../../../translated_images/linear-vs-logistic.ba180bf95e7ee66721ba10ebf2dac2666acbd64a88b003c83928712433a13c7d.ta.png)

## [முன்-வகுப்பு வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

> ### [இந்த பாடம் R-இல் கிடைக்கிறது!](../../../../2-Regression/4-Logistic/solution/R/lesson_4.html)

## அறிமுகம்

Regression பற்றிய இந்த இறுதி பாடத்தில், இது ஒரு அடிப்படை _classic_ ML தொழில்நுட்பமாகும், Logistic Regression பற்றி நாம் பார்ப்போம். நீங்கள் இந்த தொழில்நுட்பத்தை binary வகைகளை கணிக்க patterns கண்டறிய பயன்படுத்துவீர்கள். இந்த கந்தம் சாக்லேட் உள்ளதா இல்லையா? இந்த நோய் தொற்றுநோயா இல்லையா? இந்த வாடிக்கையாளர் இந்த பொருளை தேர்ந்தெடுக்கிறாரா இல்லையா?

இந்த பாடத்தில், நீங்கள் கற்றுக்கொள்வீர்கள்:

- தரவுகளை காட்சிப்படுத்த புதிய நூலகம்
- Logistic regression தொழில்நுட்பங்கள்

✅ இந்த வகை regression-ஐ வேலை செய்யும் உங்கள் புரிதலை ஆழமாக்க இந்த [Learn module](https://docs.microsoft.com/learn/modules/train-evaluate-classification-models?WT.mc_id=academic-77952-leestott) பாருங்கள்

## முன்னோட்டம்

Pumpkin தரவுடன் வேலை செய்த பிறகு, `Color` என்ற ஒரு binary வகையை நாம் வேலை செய்ய முடியும் என்பதை நன்கு அறிந்துள்ளோம்.

சில மாறிகள் கொடுக்கப்பட்டால், _ஒரு கொடுக்கப்பட்ட pumpkin எந்த நிறத்தில் இருக்க வாய்ப்பு உள்ளது_ என்பதை கணிக்க Logistic regression மாதிரியை உருவாக்குவோம் (orange 🎃 அல்லது white 👻).

> Regression பற்றிய பாடம் குழுவில் binary classification பற்றி ஏன் பேசுகிறோம்? மொழி வசதிக்காக மட்டுமே, ஏனெனில் Logistic regression [உண்மையில் ஒரு classification முறை](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression), ஆனால் இது ஒரு linear அடிப்படையிலானது. தரவுகளை வகைப்படுத்துவதற்கான பிற வழிகளை அடுத்த பாடம் குழுவில் கற்றுக்கொள்ளுங்கள்.

## கேள்வியை வரையறுக்கவும்

நமது நோக்கத்திற்காக, இதை binary ஆக வெளிப்படுத்துவோம்: 'White' அல்லது 'Not White'. நமது dataset-இல் 'striped' என்ற ஒரு வகையும் உள்ளது, ஆனால் அதில் சில மாதிரிகள் மட்டுமே உள்ளன, எனவே அதை நாம் பயன்படுத்தமாட்டோம். dataset-இல் null மதிப்புகளை நீக்கிய பிறகு அது எப்படியும் மறைந்து விடுகிறது.

> 🎃 சுவாரஸ்யமான தகவல், சில நேரங்களில் வெள்ளை pumpkins-ஐ 'ghost' pumpkins என்று அழைக்கிறோம். அவற்றை செதுக்குவது மிகவும் எளிதல்ல, எனவே அவை orange pumpkins போல பிரபலமில்லை, ஆனால் அவை குளிர்ச்சியான தோற்றம் கொண்டவை! எனவே நமது கேள்வியை 'Ghost' அல்லது 'Not Ghost' என்று மறுபரிந்துரை செய்யலாம். 👻

## Logistic regression பற்றி

Linear regression-ஐ நீங்கள் முன்பு கற்றுக்கொண்டீர்கள், Logistic regression சில முக்கியமான வழிகளில் வேறுபடுகிறது.

[![ML for beginners - Understanding Logistic Regression for Machine Learning Classification](https://img.youtube.com/vi/KpeCT6nEpBY/0.jpg)](https://youtu.be/KpeCT6nEpBY "ML for beginners - Understanding Logistic Regression for Machine Learning Classification")

> 🎥 Logistic regression பற்றிய சுருக்கமான வீடியோ பார்வைக்காக மேலே உள்ள படத்தை கிளிக் செய்யவும்.

### Binary classification

Logistic regression linear regression போன்ற அம்சங்களை வழங்காது. முன்னதாக "white or not white" போன்ற binary வகையைப் பற்றிய கணிப்பை வழங்குகிறது, ஆனால் பின்னதாக தொடர்ச்சியான மதிப்புகளை கணிக்க முடியும், உதாரணமாக pumpkin-இன் தோற்றம் மற்றும் அறுவடை நேரத்தைப் பொருத்து, _அதன் விலை எவ்வளவு உயரும்_.

![Pumpkin classification Model](../../../../translated_images/pumpkin-classifier.562771f104ad5436b87d1c67bca02a42a17841133556559325c0a0e348e5b774.ta.png)
> Infographic by [Dasani Madipalli](https://twitter.com/dasani_decoded)

### பிற வகைப்படுத்தல்கள்

Logistic regression-இன் பிற வகைகள் உள்ளன, அவை multinomial மற்றும் ordinal:

- **Multinomial**, இது ஒரு வகையை விட அதிகமானவற்றைக் கொண்டுள்ளது - "Orange, White, மற்றும் Striped".
- **Ordinal**, இது வரிசைப்படுத்தப்பட்ட வகைகளை உள்ளடக்கியது, நமது முடிவுகளை தரவுகளின் வரிசைப்படுத்தப்பட்ட அளவுகளால் (mini, sm, med, lg, xl, xxl) வரிசைப்படுத்த விரும்பினால் பயனுள்ளதாக இருக்கும்.

![Multinomial vs ordinal regression](../../../../translated_images/multinomial-vs-ordinal.36701b4850e37d86c9dd49f7bef93a2f94dbdb8fe03443eb68f0542f97f28f29.ta.png)

### மாறிகள் தொடர்புடையதாக இருக்க வேண்டிய அவசியமில்லை

Linear regression தொடர்புடைய மாறிகளுடன் சிறப்பாக வேலை செய்கிறது என்பதை நினைவில் கொள்ளுங்கள். Logistic regression எதிர்மாறானது - மாறிகள் ஒத்திசைவாக இருக்க வேண்டிய அவசியமில்லை. இது தொடர்புகள் பலவீனமாக உள்ள இந்த தரவுக்கு பொருந்துகிறது.

### உங்களுக்கு அதிகமான சுத்தமான தரவுகள் தேவை

Logistic regression அதிகமான தரவுகளைப் பயன்படுத்தினால் மேலும் துல்லியமான முடிவுகளை வழங்கும்; நமது சிறிய dataset இந்த பணிக்குப் பொருத்தமானது அல்ல, எனவே அதை மனதில் வைத்திருங்கள்.

[![ML for beginners - Data Analysis and Preparation for Logistic Regression](https://img.youtube.com/vi/B2X4H9vcXTs/0.jpg)](https://youtu.be/B2X4H9vcXTs "ML for beginners - Data Analysis and Preparation for Logistic Regression")

> 🎥 Linear regression-க்கு தரவுகளைத் தயாரிக்க சுருக்கமான வீடியோ பார்வைக்காக மேலே உள்ள படத்தை கிளிக் செய்யவும்.

✅ Logistic regression-க்கு ஏற்ற தரவுகளின் வகைகளைப் பற்றி சிந்திக்கவும்

## பயிற்சி - தரவுகளை சுத்தமாக்கவும்

முதலில், null மதிப்புகளை நீக்கி, சில நெடுவரிசைகளை மட்டும் தேர்ந்தெடுத்து, தரவுகளை சுத்தமாக்கவும்:

1. பின்வரும் குறியீட்டைச் சேர்க்கவும்:

    ```python
  
    columns_to_select = ['City Name','Package','Variety', 'Origin','Item Size', 'Color']
    pumpkins = full_pumpkins.loc[:, columns_to_select]

    pumpkins.dropna(inplace=True)
    ```

    உங்கள் புதிய dataframe-ஐ எப்போதும் பார்வையிடலாம்:

    ```python
    pumpkins.info
    ```

### Visualization - வகைபடுத்தல் வரைபடம்

இப்போது நீங்கள் [starter notebook](./notebook.ipynb) ஐ மீண்டும் pumpkin தரவுடன் ஏற்றியுள்ளீர்கள் மற்றும் `Color` உட்பட சில மாறிகளை கொண்ட dataset-ஐ பாதுகாக்க சுத்தமாக்கியுள்ளீர்கள். ஒரு வேறுபட்ட நூலகத்தைப் பயன்படுத்தி notebook-இல் dataframe-ஐ காட்சிப்படுத்துவோம்: [Seaborn](https://seaborn.pydata.org/index.html), இது நாம் முன்பு பயன்படுத்திய Matplotlib-ஐ அடிப்படையாகக் கொண்டது.

Seaborn உங்கள் தரவுகளை காட்சிப்படுத்த சில சுவாரஸ்யமான வழிகளை வழங்குகிறது. உதாரணமாக, நீங்கள் `Variety` மற்றும் `Color` தரவுகளின் விநியோகங்களை ஒரு வகைபடுத்தல் வரைபடத்தில் ஒப்பிடலாம்.

1. `catplot` செயல்பாட்டைப் பயன்படுத்தி, நமது pumpkin தரவுகளை `pumpkins` மற்றும் ஒவ்வொரு pumpkin வகைக்கான நிற வரைபடத்தை குறிப்பிடுவதன் மூலம் ஒரு வரைபடத்தை உருவாக்கவும்:

    ```python
    import seaborn as sns
    
    palette = {
    'ORANGE': 'orange',
    'WHITE': 'wheat',
    }

    sns.catplot(
    data=pumpkins, y="Variety", hue="Color", kind="count",
    palette=palette, 
    )
    ```

    ![A grid of visualized data](../../../../translated_images/pumpkins_catplot_1.c55c409b71fea2ecc01921e64b91970542101f90bcccfa4aa3a205db8936f48b.ta.png)

    தரவுகளைப் பார்வையிடுவதன் மூலம், `Color` தரவு `Variety` உடன் எப்படி தொடர்புடையது என்பதை நீங்கள் காணலாம்.

    ✅ இந்த வகைபடுத்தல் வரைபடத்தைப் பார்த்து, நீங்கள் கற்பனை செய்யக்கூடிய சில சுவாரஸ்யமான ஆராய்ச்சிகள் என்ன?

### Data pre-processing: feature and label encoding
நமது pumpkins dataset-இல் அதன் அனைத்து நெடுவரிசைகளுக்கும் string மதிப்புகள் உள்ளன. மனிதர்களுக்கு வகைபடுத்தல் தரவுடன் வேலை செய்வது எளிது, ஆனால் இயந்திரங்களுக்கு இல்லை. Machine learning algorithm-கள் எண்களுடன் நன்றாக வேலை செய்கின்றன. அதனால் encoding என்பது data pre-processing கட்டத்தில் மிகவும் முக்கியமான படியாகும், ஏனெனில் இது வகைபடுத்தல் தரவுகளை எண் தரவுகளாக மாற்ற உதவுகிறது, எந்த தகவலையும் இழக்காமல். நல்ல encoding ஒரு நல்ல மாதிரியை உருவாக்க உதவுகிறது.

Feature encoding-க்கு இரண்டு முக்கியமான வகை encoders உள்ளன:

1. Ordinal encoder: இது ordinal மாறிகளுக்கு பொருந்தும், அவை தரவுகள் ஒரு தரவின் வரிசையைப் பின்பற்றும் வகைபடுத்தல் மாறிகள், நமது dataset-இல் `Item Size` நெடுவரிசை போன்றவை. இது ஒரு mapping-ஐ உருவாக்குகிறது, அதனால் ஒவ்வொரு வகையும் ஒரு எண்ணால் பிரதிநிதித்துவம் செய்யப்படுகிறது, இது நெடுவரிசையில் வகையின் வரிசையாகும்.

    ```python
    from sklearn.preprocessing import OrdinalEncoder

    item_size_categories = [['sml', 'med', 'med-lge', 'lge', 'xlge', 'jbo', 'exjbo']]
    ordinal_features = ['Item Size']
    ordinal_encoder = OrdinalEncoder(categories=item_size_categories)
    ```

2. Categorical encoder: இது nominal மாறிகளுக்கு பொருந்தும், அவை தரவுகள் ஒரு தரவின் வரிசையைப் பின்பற்றாத வகைபடுத்தல் மாறிகள், நமது dataset-இல் `Item Size` தவிர்ந்த அனைத்து அம்சங்களுக்கும் பொருந்தும். இது ஒரு one-hot encoding ஆகும், அதாவது ஒவ்வொரு வகையும் ஒரு binary நெடுவரிசையாக பிரதிநிதித்துவம் செய்யப்படுகிறது: encoded மாறி 1-க்கு சமமாக இருக்கும், pumpkin அந்த Variety-க்கு சொந்தமானது என்றால், இல்லையெனில் 0.

    ```python
    from sklearn.preprocessing import OneHotEncoder

    categorical_features = ['City Name', 'Package', 'Variety', 'Origin']
    categorical_encoder = OneHotEncoder(sparse_output=False)
    ```
பின்னர், `ColumnTransformer` பல encoders-ஐ ஒரு படியாக இணைத்து, அவற்றை பொருத்தமான நெடுவரிசைகளுக்கு பயன்படுத்த உதவுகிறது.

```python
    from sklearn.compose import ColumnTransformer
    
    ct = ColumnTransformer(transformers=[
        ('ord', ordinal_encoder, ordinal_features),
        ('cat', categorical_encoder, categorical_features)
        ])
    
    ct.set_output(transform='pandas')
    encoded_features = ct.fit_transform(pumpkins)
```
மற்றபுறம், label-ஐ encode செய்ய, scikit-learn `LabelEncoder` வகுப்பைப் பயன்படுத்துகிறோம், இது label-களை normalize செய்ய உதவும் utility வகுப்பாகும், அவை 0 மற்றும் n_classes-1 (இங்கு, 0 மற்றும் 1) இடையே மதிப்புகளை மட்டுமே கொண்டிருக்கும்.

```python
    from sklearn.preprocessing import LabelEncoder

    label_encoder = LabelEncoder()
    encoded_label = label_encoder.fit_transform(pumpkins['Color'])
```
Features மற்றும் label-ஐ encode செய்த பிறகு, அவற்றை புதிய dataframe `encoded_pumpkins`-இல் இணைக்கலாம்.

```python
    encoded_pumpkins = encoded_features.assign(Color=encoded_label)
```
✅ `Item Size` நெடுவரிசைக்கு ordinal encoder-ஐ பயன்படுத்துவதன் நன்மைகள் என்ன?

### மாறிகளுக்கிடையேயான உறவுகளை பகுப்பாய்வு செய்யவும்

இப்போது நாங்கள் நமது தரவுகளை pre-process செய்துள்ளோம், மாறிகள் மற்றும் label-இன் உறவுகளை பகுப்பாய்வு செய்து, model-ஐ features கொடுக்கப்பட்ட label-ஐ கணிக்க எவ்வளவு நன்றாக முடியும் என்பதைப் புரிந்து கொள்ளலாம்.
இந்த வகையான பகுப்பாய்வைச் செய்ய சிறந்த வழி தரவுகளை plotting செய்வது. மீண்டும் Seaborn `catplot` செயல்பாட்டைப் பயன்படுத்தி, `Item Size`, `Variety` மற்றும் `Color` ஆகியவற்றின் உறவுகளை ஒரு வகைபடுத்தல் வரைபடத்தில் காட்சிப்படுத்துவோம். தரவுகளை சிறப்பாக plotting செய்ய, encoded `Item Size` நெடுவரிசை மற்றும் unencoded `Variety` நெடுவரிசையைப் பயன்படுத்துவோம்.

```python
    palette = {
    'ORANGE': 'orange',
    'WHITE': 'wheat',
    }
    pumpkins['Item Size'] = encoded_pumpkins['ord__Item Size']

    g = sns.catplot(
        data=pumpkins,
        x="Item Size", y="Color", row='Variety',
        kind="box", orient="h",
        sharex=False, margin_titles=True,
        height=1.8, aspect=4, palette=palette,
    )
    g.set(xlabel="Item Size", ylabel="").set(xlim=(0,6))
    g.set_titles(row_template="{row_name}")
```
![A catplot of visualized data](../../../../translated_images/pumpkins_catplot_2.87a354447880b3889278155957f8f60dd63db4598de5a6d0fda91c334d31f9f1.ta.png)

### Swarm plot பயன்படுத்தவும்

`Color` என்பது ஒரு binary வகை (White அல்லது Not), இது 'ஒரு [சிறப்பு அணுகுமுறை](https://seaborn.pydata.org/tutorial/categorical.html?highlight=bar) visualization' தேவை. இந்த வகையின் மற்ற மாறிகளுடன் உள்ள உறவுகளை காட்சிப்படுத்த பிற வழிகள் உள்ளன.

Seaborn plots-ஐப் பயன்படுத்தி மாறிகளை பக்கம்தோறும் காட்சிப்படுத்தலாம்.

1. மதிப்புகளின் விநியோகத்தை காட்ட 'swarm' plot ஐ முயற்சிக்கவும்:

    ```python
    palette = {
    0: 'orange',
    1: 'wheat'
    }
    sns.swarmplot(x="Color", y="ord__Item Size", data=encoded_pumpkins, palette=palette)
    ```

    ![A swarm of visualized data](../../../../translated_images/swarm_2.efeacfca536c2b577dc7b5f8891f28926663fbf62d893ab5e1278ae734ca104e.ta.png)

**கவனமாக இருங்கள்**: மேலே உள்ள குறியீடு ஒரு எச்சரிக்கையை உருவாக்கக்கூடும், ஏனெனில் seaborn இவ்வளவு அளவிலான datapoints-ஐ ஒரு swarm plot-இல் பிரதிநிதித்துவம் செய்ய முடியாது. ஒரு சாத்தியமான தீர்வு marker-இன் அளவைக் குறைப்பது, 'size' அளவுருவைப் பயன்படுத்துவதன் மூலம். இருப்பினும், இது plot-இன் வாசிப்புத்திறனை பாதிக்கிறது என்பதை கவனத்தில் கொள்ளுங்கள்.

> **🧮 கணிதத்தை காட்டுங்கள்**
>
> Logistic regression 'maximum likelihood' என்ற கருத்தை [sigmoid functions](https://wikipedia.org/wiki/Sigmoid_function) பயன்படுத்தி நம்புகிறது. ஒரு 'Sigmoid Function' plot-இல் 'S' வடிவமாக தெரிகிறது. இது ஒரு மதிப்பை எடுத்து 0 மற்றும் 1 இடையே எங்காவது map செய்கிறது. அதன் curve-ஐ 'logistic curve' என்றும் அழைக்கப்படுகிறது. அதன் formula இவ்வாறு தெரிகிறது:
>
> ![logistic function](../../../../translated_images/sigmoid.8b7ba9d095c789cf72780675d0d1d44980c3736617329abfc392dfc859799704.ta.png)
>
> sigmoid-இன் midpoint x-இன் 0 புள்ளியில் காணப்படுகிறது, L curve-இன் அதிகபட்ச மதிப்பு, மற்றும் k curve-இன் steepness ஆகும். function-இன் முடிவு 0.5-ஐ விட அதிகமாக இருந்தால், label-இன் binary தேர்வின் '1' வகை வழங்கப்படும். இல்லையெனில், அது '0' ஆக வகைப்படுத்தப்படும்.

## உங்கள் மாதிரியை உருவாக்கவும்

இந்த binary classification-ஐ கண்டறிய ஒரு மாதிரியை உருவாக்குவது Scikit-learn-இல் ஆச்சரியமாக எளிதானது.

[![ML for beginners - Logistic Regression for classification of data](https://img.youtube.com/vi/MmZS2otPrQ8/0.jpg)](https://youtu.be/MmZS2otPrQ8 "ML for beginners - Logistic Regression for classification of data")

> 🎥 Linear regression மாதிரியை உருவாக்க சுருக்கமான வீடியோ பார்வைக்காக மேலே உள்ள படத்தை கிளிக் செய்யவும்.

1. உங்கள் classification மாதிரியில் பயன்படுத்த விரும்பும் மாறிகளைத் தேர்ந்தெடுத்து, `train_test_split()` ஐ அழைத்து training மற்றும் test sets-ஐ பிரிக்கவும்:

    ```python
    from sklearn.model_selection import train_test_split
    
    X = encoded_pumpkins[encoded_pumpkins.columns.difference(['Color'])]
    y = encoded_pumpkins['Color']

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
    
    ```

2. இப்போது உங்கள் மாதிரியை train செய்யலாம், உங்கள் training தரவுடன் `fit()` ஐ அழைத்து, அதன் முடிவை print செய்யலாம்:

    ```python
    from sklearn.metrics import f1_score, classification_report 
    from sklearn.linear_model import LogisticRegression

    model = LogisticRegression()
    model.fit(X_train, y_train)
    predictions = model.predict(X_test)

    print(classification_report(y_test, predictions))
    print('Predicted labels: ', predictions)
    print('F1-score: ', f1_score(y_test, predictions))
    ```

    உங்கள் மாதிரியின் scoreboard-ஐ பாருங்கள். உங்கள் dataset-இல் சுமார் 1000 rows மட்டுமே உள்ளதைப் பொருத்து, இது மோசமாக இல்லை:

    ```output
                       precision    recall  f1-score   support
    
                    0       0.94      0.98      0.96       166
                    1       0.85      0.67      0.75        33
    
        accuracy                                0.92       199
        macro avg           0.89      0.82      0.85       199
        weighted avg        0.92      0.92      0.92       199
    
        Predicted labels:  [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 1 0 0 0 0 0 0 0 0 1 0 0 0 0
        0 0 0 0 0 1 0 1 0 0 1 0 0 0 0 0 1 0 1 0 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0
        1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 0 0 0 1 0
        0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 1 0 0 0 1 1 0
        0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1
        0 0 0 1 0 0 0 0 0 0 0 0 1 1]
        F1-score:  0.7457627118644068
    ```

## குழப்பம் matrix மூலம் சிறந்த புரிதல்

மேலே உள்ளவற்றை print செய்வதன் மூலம் நீங்கள் ஒரு scoreboard report [terms](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html?highlight=classification_report#sklearn.metrics.classification_report) பெறலாம், ஆனால் [confusion matrix](https://scikit-learn.org/stable/modules/model_evaluation.html#confusion-matrix) ஐப் பயன்படுத்தி உங்கள் மாதிரியை நீங்கள் எளிதாகப் புரிந்துகொள்ளலாம், இது மாதிரி எப்படி செயல்படுகிறது என்பதைப் புரிந்துகொள்ள உதவுகிறது.

> 🎓 ஒரு '[confusion matrix](https://wikipedia.org/wiki/Confusion_matrix)' (அல்லது 'error matrix') என்பது உங்கள் மாதிரியின் true vs. false positives மற்றும் negatives-ஐ வெளிப்படுத்தும் ஒரு அட்டவணையாகும், இதன் மூலம் predictions-ஐ gauge செய்யும்.

1. Confusion metrics-ஐ பயன்படுத்த `confusion_matrix()` ஐ அழைக்கவும்:

    ```python
    from sklearn.metrics import confusion_matrix
    confusion_matrix(y_test, predictions)
    ```

    உங்கள் மாதிரியின் confusion matrix-ஐ பாருங்கள்:

    ```output
    array([[162,   4],
           [ 11,  22]])
    ```

Scikit-learn-இல் confusion matrices Rows (axis 0) உண்மையான labels மற்றும் columns (axis 1) கணிக்கப்பட்ட labels ஆகும்.

|       |   0   |   1   |
| :---: | :---: | :---: |
|   0   |  TN   |  FP   |
|   1   |  FN   |  TP   |

இங்கே என்ன நடக்கிறது? உங்கள் மாதிரி pumpkins-ஐ இரண்டு binary வகைகளுக்கு வகைப்படுத்த கேட்கப்பட்டதாகக் கூறலாம், 'white' வகை மற்றும் 'not-white' வகை.

- உங்கள் மாதிரி ஒரு pumpkin-ஐ not white என்று கணிக்கிறது, அது உண்மையில் 'not-white' வகையைச் சேர்ந்தது என்றால், அதை true negative என்று அழைக்கிறோம், இது மேல் இடது எண் மூலம் காட்டப்படுகிறது.
- உங்கள் மாதிரி ஒரு pumpkin-ஐ white என்று கணிக்கிறது, அது உண்மையில் 'not-white' வகையைச் சேர்ந்தது என்றால், அதை false negative என்று அழைக்கிறோம், இது கீழ் இடது எண் மூலம் காட்டப்படுகிறது.
- உங்கள் மாதிரி ஒரு pumpkin-ஐ not white என்று கணிக்கிறது, அது உண்மையில் 'white' வகையைச் சேர்ந்தது என்றால், அதை false positive என்று அழைக்கிறோம், இது மேல் வலது எண் மூலம் காட்டப்படுகிறது.
- உங்கள் மாதிரி ஒரு pumpkin-ஐ white என்று கணிக்கிறது, அது உண்மையில் 'white' வகையைச் சேர்ந்தது என்றால், அதை true positive என்று அழைக்கிறோம், இது கீழ் வலது எண் மூலம் காட்டப்படுகிறது.
நீங்கள் நினைத்திருப்பது போலவே, உண்மையான நேர்மையான மற்றும் உண்மையான எதிர்மறை எண்ணிக்கைகள் அதிகமாகவும், தவறான நேர்மையான மற்றும் தவறான எதிர்மறை எண்ணிக்கைகள் குறைவாகவும் இருப்பது சிறந்தது, இது மாடல் சிறப்பாக செயல்படுகிறது என்பதை குறிக்கிறது.

குழப்ப அட்டவணை துல்லியத்துடன் மற்றும் மீளப்பெறுதலுடன் எப்படி தொடர்புடையது? மேலே அச்சிடப்பட்ட வகைப்பாட்டு அறிக்கை துல்லியத்தை (0.85) மற்றும் மீளப்பெறுதலை (0.67) காட்டியது என்பதை நினைவில் கொள்ளுங்கள்.

துல்லியம் = tp / (tp + fp) = 22 / (22 + 4) = 0.8461538461538461

மீளப்பெறுதல் = tp / (tp + fn) = 22 / (22 + 11) = 0.6666666666666666

✅ கேள்வி: குழப்ப அட்டவணையின் அடிப்படையில், மாடல் எப்படி செயல்பட்டது? பதில்: மோசமாக இல்லை; உண்மையான எதிர்மறைகள் நல்ல அளவில் உள்ளன, ஆனால் சில தவறான எதிர்மறைகளும் உள்ளன.

முன்பு பார்த்த சொற்களை TP/TN மற்றும் FP/FN ஆகியவற்றின் குழப்ப அட்டவணை வரைபடத்தின் உதவியுடன் மீண்டும் பார்ப்போம்:

🎓 துல்லியம்: TP/(TP + FP) மீட்கப்பட்ட நிகழ்வுகளில் பொருத்தமான நிகழ்வுகளின் பகுதி (எ.கா. எந்த லேபிள்கள் சரியாக லேபிள் செய்யப்பட்டன)

🎓 மீளப்பெறுதல்: TP/(TP + FN) சரியாக லேபிள் செய்யப்பட்டதா இல்லையா என்பதை பொருட்படுத்தாமல் மீட்கப்பட்ட பொருத்தமான நிகழ்வுகளின் பகுதி

🎓 f1-மதிப்பெண்: (2 * துல்லியம் * மீளப்பெறுதல்)/(துல்லியம் + மீளப்பெறுதல்) துல்லியத்திற்கும் மீளப்பெறுதலுக்கும் இடையிலான எடைசுமை சராசரி, சிறந்தது 1 மற்றும் மோசமானது 0

🎓 ஆதரவு: ஒவ்வொரு லேபிளின் நிகழ்வுகளின் எண்ணிக்கை

🎓 துல்லியமானது: (TP + TN)/(TP + TN + FP + FN) ஒரு மாதிரிக்கான லேபிள்கள் சரியாக கணிக்கப்பட்ட சதவீதம்.

🎓 மாக்ரோ சராசரி: ஒவ்வொரு லேபிளுக்கும் எடைசுமை இல்லாத சராசரி அளவீடுகளை கணக்கிடுதல், லேபிள் சமநிலையின்மையை கணக்கில் எடுத்துக்கொள்ளாமல்.

🎓 எடைசுமை சராசரி: ஒவ்வொரு லேபிளுக்கும் சராசரி அளவீடுகளை கணக்கிடுதல், அவற்றின் ஆதரவால் (ஒவ்வொரு லேபிளுக்கும் உண்மையான நிகழ்வுகளின் எண்ணிக்கை) எடையிடுவதன் மூலம் லேபிள் சமநிலையின்மையை கணக்கில் எடுத்துக்கொள்வது.

✅ உங்கள் மாடல் தவறான எதிர்மறை எண்ணிக்கையை குறைக்க வேண்டும் என்றால் எந்த அளவீட்டை கவனிக்க வேண்டும் என்று நீங்கள் யோசிக்கிறீர்களா?

## இந்த மாடலின் ROC வளைவை காட்சிப்படுத்துங்கள்

[![ML for beginners - Analyzing Logistic Regression Performance with ROC Curves](https://img.youtube.com/vi/GApO575jTA0/0.jpg)](https://youtu.be/GApO575jTA0 "ML for beginners - Analyzing Logistic Regression Performance with ROC Curves")

> 🎥 மேலே உள்ள படத்தை கிளிக் செய்து ROC வளைவுகளின் குறுகிய வீடியோ ஒலிபரப்பைப் பாருங்கள்

'ROC' வளைவைப் பார்க்க இன்னொரு காட்சிப்படுத்தலை செய்யலாம்:

```python
from sklearn.metrics import roc_curve, roc_auc_score
import matplotlib
import matplotlib.pyplot as plt
%matplotlib inline

y_scores = model.predict_proba(X_test)
fpr, tpr, thresholds = roc_curve(y_test, y_scores[:,1])

fig = plt.figure(figsize=(6, 6))
plt.plot([0, 1], [0, 1], 'k--')
plt.plot(fpr, tpr)
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
plt.show()
```

Matplotlib பயன்படுத்தி, மாடலின் [Receiving Operating Characteristic](https://scikit-learn.org/stable/auto_examples/model_selection/plot_roc.html?highlight=roc) அல்லது ROC ஐ வரைபடமாக்குங்கள். ROC வளைவுகள் பொதுவாக ஒரு வகைப்பாட்டாளரின் வெளியீட்டை அதன் உண்மையான மற்றும் தவறான நேர்மையான அடிப்படையில் பார்ப்பதற்காக பயன்படுத்தப்படுகின்றன. "ROC வளைவுகள் பொதுவாக Y அச்சில் உண்மையான நேர்மையான விகிதத்தையும், X அச்சில் தவறான நேர்மையான விகிதத்தையும் கொண்டிருக்கும்." எனவே, வளைவின் சரிவும் நடுப்புள்ளி கோடு மற்றும் வளைவின் இடையே உள்ள இடமும் முக்கியம்: வளைவு விரைவாக மேலே மற்றும் கோட்டிற்கு மேல் செல்ல வேண்டும். எங்கள் நிலைமையில், ஆரம்பத்தில் சில தவறான நேர்மைகள் உள்ளன, பின்னர் கோடு மேலே மற்றும் சரியாக செல்கிறது:

![ROC](../../../../translated_images/ROC_2.777f20cdfc4988ca683ade6850ac832cb70c96c12f1b910d294f270ef36e1a1c.ta.png)

இறுதியாக, Scikit-learn இன் [`roc_auc_score` API](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html?highlight=roc_auc#sklearn.metrics.roc_auc_score) ஐப் பயன்படுத்தி உண்மையான 'வளைவின் கீழ் பகுதி' (AUC) ஐ கணக்கிடுங்கள்:

```python
auc = roc_auc_score(y_test,y_scores[:,1])
print(auc)
```
 முடிவாக `0.9749908725812341` கிடைத்தது. AUC 0 முதல் 1 வரை மாறுபடும் என்பதால், பெரிய மதிப்பெண் வேண்டும், ஏனெனில் 100% சரியான கணிப்புகளை வழங்கும் மாடல் 1 AUC ஐக் கொண்டிருக்கும்; இந்த நிலைமையில், மாடல் _மிகவும் நல்லது_.

வகைப்பாட்டு பாடங்களில் எதிர்கால பாடங்களில், உங்கள் மாடலின் மதிப்பெண்களை மேம்படுத்த எப்படி மீண்டும் முயற்சிக்கலாம் என்பதை நீங்கள் கற்றுக்கொள்வீர்கள். ஆனால் இப்போது, வாழ்த்துக்கள்! இந்த மறுமொழி பாடங்களை நீங்கள் முடித்துவிட்டீர்கள்!

---
## 🚀சவால்

லாஜிஸ்டிக் மறுமொழி குறித்து மேலும் பல விஷயங்களை ஆராய வேண்டும்! ஆனால் கற்றுக்கொள்ள சிறந்த வழி முயற்சி செய்வதே. இந்த வகை பகுப்பாய்வுக்கு ஏற்ற ஒரு தரவுத்தொகுப்பை கண்டுபிடித்து அதில் ஒரு மாடலை உருவாக்குங்கள். நீங்கள் என்ன கற்றுக்கொள்கிறீர்கள்? குறிப்புகள்: [Kaggle](https://www.kaggle.com/search?q=logistic+regression+datasets) இல் சுவாரஸ்யமான தரவுத்தொகுப்புகளை முயற்சிக்கவும்.

## [பாடத்திற்குப் பிந்தைய வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## மதிப்பீடு & சுயபயிற்சி

லாஜிஸ்டிக் மறுமொழிக்கான சில நடைமுறை பயன்பாடுகள் குறித்த [ஸ்டான்ஃபோர்டிலிருந்து இந்த ஆவணத்தின்](https://web.stanford.edu/~jurafsky/slp3/5.pdf) முதல் சில பக்கங்களைப் படியுங்கள். இதுவரை நாம் படித்த மறுமொழி பணிகளின் வகைகளில் எது சிறந்தது என்பதைப் பற்றி யோசிக்கவும். எது சிறந்ததாக இருக்கும்?

## பணிக்குறிப்பு

[இந்த மறுமொழியை மீண்டும் முயற்சிக்கவும்](assignment.md)

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கின்றோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.