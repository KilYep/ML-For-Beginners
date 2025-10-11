<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "917dbf890db71a322f306050cb284749",
  "translation_date": "2025-10-11T11:58:44+00:00",
  "source_file": "7-TimeSeries/2-ARIMA/README.md",
  "language_code": "ta"
}
-->
# ARIMA மூலம் நேர்முகம் கணிப்பு

முந்தைய பாடத்தில், நேர்முகம் கணிப்பு பற்றிய சில அடிப்படைகளை நீங்கள் கற்றுக்கொண்டீர்கள் மற்றும் ஒரு குறிப்பிட்ட காலத்தில் மின்சார சுமை மாறுபாடுகளை காட்டும் தரவுத்தொகுப்பை ஏற்றீர்கள்.

[![ARIMA அறிமுகம்](https://img.youtube.com/vi/IUSk-YDau10/0.jpg)](https://youtu.be/IUSk-YDau10 "ARIMA அறிமுகம்")

> 🎥 மேலே உள்ள படத்தை கிளிக் செய்யவும்: ARIMA மாதிரிகள் பற்றிய சுருக்கமான அறிமுகம். உதாரணம் R-ல் செய்யப்பட்டுள்ளது, ஆனால் கருத்துக்கள் பொதுவானவை.

## [பாடத்திற்கு முன் வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## அறிமுகம்

இந்த பாடத்தில், [ARIMA: *A*uto*R*egressive *I*ntegrated *M*oving *A*verage](https://wikipedia.org/wiki/Autoregressive_integrated_moving_average) மூலம் மாதிரிகளை உருவாக்க ஒரு குறிப்பிட்ட முறையை நீங்கள் கற்றுக்கொள்வீர்கள். ARIMA மாதிரிகள் குறிப்பாக [non-stationarity](https://wikipedia.org/wiki/Stationary_process) காட்டும் தரவுகளுக்கு பொருத்தமாக இருக்கும்.

## பொதுவான கருத்துக்கள்

ARIMA-யுடன் வேலை செய்ய, சில முக்கிய கருத்துகளை நீங்கள் அறிந்திருக்க வேண்டும்:

- 🎓 **Stationarity**. புள்ளியியல் பார்வையில், stationarity என்பது நேரத்தில் மாற்றியமைக்கும்போது அதன் விநியோகம் மாறாத தரவுகளை குறிக்கிறது. Non-stationary தரவுகள், பின்னர், பருவத்தால் ஏற்படும் மாறுபாடுகளை காட்டுகிறது, அவற்றை பகுப்பாய்வு செய்ய மாற்றம் செய்ய வேண்டும். உதாரணமாக, பருவத்தால் ஏற்படும் மாறுபாடுகளை 'seasonal-differencing' மூலம் நீக்கலாம்.

- 🎓 **[Differencing](https://wikipedia.org/wiki/Autoregressive_integrated_moving_average#Differencing)**. Differencing என்பது non-stationary தரவுகளை stationarity ஆக மாற்றுவதற்கான செயல்முறையாகும், அதாவது அதன் non-constant trend-ஐ நீக்குவது. "Differencing ஒரு நேர்முகத் தொடரின் நிலையை மாற்றுவதைக் குறைக்கிறது, trend மற்றும் seasonality-ஐ நீக்கி, அதன் சராசரியை நிலைப்படுத்துகிறது." [Shixiong et al-இன் ஆய்வு](https://arxiv.org/abs/1904.07632)

## நேர்முகத்தின் சூழலில் ARIMA

நேர்முகத் தரவுகளை மாதிரியாக்க ARIMA எவ்வாறு உதவுகிறது என்பதை நன்கு புரிந்துகொள்ள அதன் பகுதிகளைப் பார்ப்போம்.

- **AR - AutoRegressive**. Autoregressive மாதிரிகள், பெயரிலேயே உள்ளது போல, உங்கள் தரவின் முந்தைய மதிப்புகளை 'பின்' நோக்கி பார்த்து அவற்றைப் பகுப்பாய்வு செய்கின்றன. இந்த முந்தைய மதிப்புகள் 'lags' என்று அழைக்கப்படுகின்றன. உதாரணமாக, மாதாந்திர பென்சில் விற்பனை தரவுகள். ஒவ்வொரு மாதத்தின் விற்பனை மொத்தம் 'evolving variable' ஆகக் கருதப்படும். இந்த மாதிரி "evolving variable of interest is regressed on its own lagged (i.e., prior) values." [wikipedia](https://wikipedia.org/wiki/Autoregressive_integrated_moving_average)

- **I - Integrated**. 'ARMA' மாதிரிகளுடன் ஒப்பிடும்போது, ARIMA-யில் உள்ள 'I' அதன் *[integrated](https://wikipedia.org/wiki/Order_of_integration)* அம்சத்தை குறிக்கிறது. Non-stationarity-ஐ நீக்க differencing படிகள் பயன்படுத்தப்படும் போது தரவுகள் 'integrated' ஆகின்றன.

- **MA - Moving Average**. [Moving-average](https://wikipedia.org/wiki/Moving-average_model) அம்சம் தற்போதைய மற்றும் முந்தைய lags மதிப்புகளைப் பார்த்து output variable-ஐ தீர்மானிக்கிறது.

முக்கியமாக: ARIMA நேர்முகத் தரவின் சிறப்பு வடிவத்துக்கு மிக அருகில் பொருந்தும் மாதிரியை உருவாக்க பயன்படுத்தப்படுகிறது.

## பயிற்சி - ARIMA மாதிரியை உருவாக்கவும்

இந்த பாடத்தில் [_/working_](https://github.com/microsoft/ML-For-Beginners/tree/main/7-TimeSeries/2-ARIMA/working) கோப்புறையை திறந்து [_notebook.ipynb_](https://github.com/microsoft/ML-For-Beginners/blob/main/7-TimeSeries/2-ARIMA/working/notebook.ipynb) கோப்பை கண்டறியவும்.

1. `statsmodels` Python நூலகத்தை ஏற்றவும்; இது ARIMA மாதிரிகளுக்கு தேவைப்படும்.

1. தேவையான நூலகங்களை ஏற்றவும்

1. தரவுகளை `/data/energy.csv` கோப்பிலிருந்து Pandas dataframe-இல் ஏற்றவும் மற்றும் பார்வையிடவும்:

    ```python
    energy = load_data('./data')[['load']]
    energy.head(10)
    ```

1. 2012 ஜனவரி முதல் 2014 டிசம்பர் வரை கிடைக்கும் அனைத்து மின்சார தரவுகளையும் வரைபடமாக்கவும். இந்த தரவுகளை முந்தைய பாடத்தில் பார்த்ததால் எந்த ஆச்சரியமும் இருக்காது:

    ```python
    energy.plot(y='load', subplots=True, figsize=(15, 8), fontsize=12)
    plt.xlabel('timestamp', fontsize=12)
    plt.ylabel('load', fontsize=12)
    plt.show()
    ```

    இப்போது, ஒரு மாதிரியை உருவாக்குவோம்!

### பயிற்சி மற்றும் சோதனை தரவுத்தொகுப்புகளை உருவாக்கவும்

தரவுகளை ஏற்றிய பிறகு, அதை பயிற்சி மற்றும் சோதனை தொகுப்புகளாக பிரிக்கவும். பயிற்சி தொகுப்பில் உங்கள் மாதிரியை பயிற்சி செய்யவும். வழக்கமாக, மாதிரி பயிற்சி முடிந்த பிறகு, அதன் துல்லியத்தை சோதனை தொகுப்பைப் பயன்படுத்தி மதிப்பீடு செய்யவும். மாதிரி எதிர்கால காலகட்டங்களில் இருந்து தகவலைப் பெறாமல் இருக்க, சோதனை தொகுப்பு பயிற்சி தொகுப்பின் பின்னர் காலகட்டத்தை உள்ளடக்க வேண்டும்.

1. 2014 செப்டம்பர் 1 முதல் அக்டோபர் 31 வரை இரண்டு மாத காலத்தை பயிற்சி தொகுப்புக்கு ஒதுக்கவும். சோதனை தொகுப்பு 2014 நவம்பர் 1 முதல் டிசம்பர் 31 வரை இரண்டு மாத காலத்தை உள்ளடக்கும்:

    ```python
    train_start_dt = '2014-11-01 00:00:00'
    test_start_dt = '2014-12-30 00:00:00'
    ```

    இந்த தரவுகள் தினசரி மின்சார பயன்பாட்டை பிரதிபலிப்பதால், ஒரு வலுவான பருவ முறை உள்ளது, ஆனால் சமீபத்திய நாட்களில் பயன்பாட்டுடன் மிகவும் ஒத்ததாக உள்ளது.

1. வேறுபாடுகளை காட்சிப்படுத்தவும்:

    ```python
    energy[(energy.index < test_start_dt) & (energy.index >= train_start_dt)][['load']].rename(columns={'load':'train'}) \
        .join(energy[test_start_dt:][['load']].rename(columns={'load':'test'}), how='outer') \
        .plot(y=['train', 'test'], figsize=(15, 8), fontsize=12)
    plt.xlabel('timestamp', fontsize=12)
    plt.ylabel('load', fontsize=12)
    plt.show()
    ```

    ![பயிற்சி மற்றும் சோதனை தரவுகள்](../../../../translated_images/train-test.8928d14e5b91fc942f0ca9201b2d36c890ea7e98f7619fd94f75de3a4c2bacb9.ta.png)

    எனவே, தரவுகளை பயிற்சி செய்ய ஒரு சிறிய கால சாளரத்தைப் பயன்படுத்துவது போதுமானதாக இருக்கும்.

    > குறிப்பு: ARIMA மாதிரியை பொருத்த பயன்படுத்தும் செயல்பாடு fitting போது in-sample validation பயன்படுத்துவதால், validation தரவுகளை தவிர்க்கலாம்.

### பயிற்சிக்கான தரவுகளை தயாரிக்கவும்

தரவுகளை வடிகட்டி மற்றும் அளவீடு செய்வதன் மூலம் பயிற்சிக்கான தரவுகளை தயாரிக்க வேண்டும். தரவுத்தொகுப்பை தேவையான காலகட்டங்கள் மற்றும் நெடுவரிசைகளை மட்டும் உள்ளடக்க வடிகட்டவும், தரவுகளை 0,1 இடைவெளியில் project செய்ய அளவீடு செய்யவும்.

1. முதலில் குறிப்பிடப்பட்ட காலகட்டங்கள் மற்றும் 'load' நெடுவரிசை மட்டும் உள்ளடக்க தரவுத்தொகுப்பை வடிகட்டவும்:

    ```python
    train = energy.copy()[(energy.index >= train_start_dt) & (energy.index < test_start_dt)][['load']]
    test = energy.copy()[energy.index >= test_start_dt][['load']]

    print('Training data shape: ', train.shape)
    print('Test data shape: ', test.shape)
    ```

    தரவின் வடிவத்தை நீங்கள் காணலாம்:

    ```output
    Training data shape:  (1416, 1)
    Test data shape:  (48, 1)
    ```

1. தரவுகளை (0, 1) வரம்பில் அளவீடு செய்யவும்.

    ```python
    scaler = MinMaxScaler()
    train['load'] = scaler.fit_transform(train)
    train.head(10)
    ```

1. அசல் தரவுடன் ஒப்பிடும் அளவீடு செய்யப்பட்ட தரவுகளை காட்சிப்படுத்தவும்:

    ```python
    energy[(energy.index >= train_start_dt) & (energy.index < test_start_dt)][['load']].rename(columns={'load':'original load'}).plot.hist(bins=100, fontsize=12)
    train.rename(columns={'load':'scaled load'}).plot.hist(bins=100, fontsize=12)
    plt.show()
    ```

    ![அசல்](../../../../translated_images/original.b2b15efe0ce92b8745918f071dceec2231661bf49c8db6918e3ff4b3b0b183c2.ta.png)

    > அசல் தரவுகள்

    ![அளவீடு செய்யப்பட்ட](../../../../translated_images/scaled.e35258ca5cd3d43f86d5175e584ba96b38d51501f234abf52e11f4fe2631e45f.ta.png)

    > அளவீடு செய்யப்பட்ட தரவுகள்

1. இப்போது அளவீடு செய்யப்பட்ட தரவுகளை சரிசெய்த பிறகு, சோதனை தரவுகளை அளவீடு செய்யவும்:

    ```python
    test['load'] = scaler.transform(test)
    test.head()
    ```

### ARIMA-ஐ செயல்படுத்தவும்

இப்போது ARIMA-ஐ செயல்படுத்த நேரம்! நீங்கள் முன்பு நிறுவிய `statsmodels` நூலகத்தைப் பயன்படுத்துவீர்கள்.

இப்போது சில படிகளை பின்பற்ற வேண்டும்:

   1. `SARIMAX()`-ஐ அழைத்து மாதிரியின் அளவுருக்களை: p, d, மற்றும் q அளவுருக்கள், மற்றும் P, D, மற்றும் Q அளவுருக்கள் வழங்கி மாதிரியை வரையறுக்கவும்.
   2. `fit()` செயல்பாட்டை அழைத்து பயிற்சி தரவுக்கான மாதிரியை தயாரிக்கவும்.
   3. `forecast()` செயல்பாட்டை அழைத்து முன்னறிவிப்பு செய்யவும் மற்றும் முன்னறிவிப்பு செய்ய வேண்டிய படிகள் (the `horizon`) குறிப்பிடவும்.

> 🎓 இந்த அளவுருக்கள் எதற்காக? ARIMA மாதிரியில், நேர்முகத்தின் முக்கிய அம்சங்களை: பருவம், போக்கு, மற்றும் சத்தம் மாதிரியாக்க உதவும் 3 அளவுருக்கள் உள்ளன:

`p`: மாதிரியின் auto-regressive அம்சத்துடன் தொடர்புடைய அளவுரு, இது *முந்தைய* மதிப்புகளை உள்ளடக்குகிறது.
`d`: மாதிரியின் integrated பகுதிக்கு தொடர்புடைய அளவுரு, இது நேர்முகத் தொடரில் *differencing* (🎓 differencing-ஐ நினைவில் கொள்ளுங்கள் 👆?) அளவை பாதிக்கிறது.
`q`: மாதிரியின் moving-average பகுதிக்கு தொடர்புடைய அளவுரு.

> குறிப்பு: உங்கள் தரவுகளில் பருவ அம்சம் இருந்தால் - இந்த தரவுகளில் உள்ளது - , seasonal ARIMA மாதிரி (SARIMA) பயன்படுத்த வேண்டும். அந்த நேரத்தில், `p`, `d`, மற்றும் `q`-க்கு ஒத்த seasonal கூறுகளை விவரிக்க `P`, `D`, மற்றும் `Q` அளவுருக்கள் தேவைப்படும்.

1. உங்கள் விருப்பமான horizon மதிப்பை அமைக்க தொடங்குங்கள். 3 மணி நேரம் முயற்சிக்கலாம்:

    ```python
    # Specify the number of steps to forecast ahead
    HORIZON = 3
    print('Forecasting horizon:', HORIZON, 'hours')
    ```

    ARIMA மாதிரியின் அளவுருக்களுக்கு சிறந்த மதிப்புகளை தேர்ந்தெடுப்பது சற்று சிக்கலானது மற்றும் நேரம் பிடிக்கும். [`pyramid` நூலகத்தின்](https://alkaline-ml.com/pmdarima/0.9.0/modules/generated/pyramid.arima.auto_arima.html) `auto_arima()` செயல்பாட்டைப் பயன்படுத்த பரிந்துரைக்கலாம்.

1. தற்போது சில கையேடு தேர்வுகளை முயற்சித்து ஒரு நல்ல மாதிரியை கண்டறியவும்.

    ```python
    order = (4, 1, 0)
    seasonal_order = (1, 1, 0, 24)

    model = SARIMAX(endog=train, order=order, seasonal_order=seasonal_order)
    results = model.fit()

    print(results.summary())
    ```

    முடிவுகளின் ஒரு அட்டவணை அச்சிடப்படுகிறது.

நீங்கள் உங்கள் முதல் மாதிரியை உருவாக்கியுள்ளீர்கள்! இப்போது அதை மதிப்பீடு செய்ய ஒரு வழியை கண்டறிய வேண்டும்.

### உங்கள் மாதிரியை மதிப்பீடு செய்யவும்

உங்கள் மாதிரியை மதிப்பீடு செய்ய, `walk forward` validation எனப்படும் முறையை செயல்படுத்தலாம். நடைமுறையில், நேர்முக மாதிரிகள் புதிய தரவுகள் கிடைக்கும் ஒவ்வொரு முறையும் மீண்டும் பயிற்சி செய்யப்படுகின்றன. இது ஒவ்வொரு நேரக்கட்டத்திலும் சிறந்த முன்னறிவிப்பை செய்ய மாதிரியை அனுமதிக்கிறது.

இந்த தொழில்நுட்பத்தைப் பயன்படுத்தி நேர்முகத் தொடரின் தொடக்கத்தில் இருந்து, பயிற்சி தரவுத்தொகுப்பில் மாதிரியை பயிற்சி செய்யவும். பின்னர், அடுத்த நேரக்கட்டத்தில் ஒரு முன்னறிவிப்பு செய்யவும். முன்னறிவிப்பு அறியப்பட்ட மதிப்புடன் மதிப்பீடு செய்யப்படுகிறது. பின்னர் பயிற்சி தொகுப்பு அறியப்பட்ட மதிப்பை உள்ளடக்க விரிவாக்கப்படுகிறது, மற்றும் செயல்முறை மீண்டும் செய்யப்படுகிறது.

> குறிப்பு: பயிற்சி தொகுப்பு சாளரத்தை நிலையாக வைத்திருப்பது பயிற்சியை மேலும் திறமையாக செய்ய உதவும், ஒவ்வொரு முறையும் நீங்கள் பயிற்சி தொகுப்பில் புதிய கண்காணிப்பைச் சேர்க்கும் போது, தொகுப்பின் தொடக்கத்தில் இருந்து கண்காணிப்பை நீக்க வேண்டும்.

இந்த செயல்முறை மாதிரி நடைமுறையில் எப்படி செயல்படும் என்பதை மேலும் வலுவான மதிப்பீட்டை வழங்குகிறது. இருப்பினும், இது பல மாதிரிகளை உருவாக்குவதற்கான கணினி செலவுடன் வருகிறது. தரவுகள் சிறியதாக இருந்தால் அல்லது மாதிரி எளிமையானதாக இருந்தால் இது ஏற்றுக்கொள்ளக்கூடியது, ஆனால் அளவிலான தரவுகளுக்கு இது சிக்கலாக இருக்கலாம்.

Walk-forward validation நேர்முக மாதிரி மதிப்பீட்டின் தங்க நிலைமையாகும் மற்றும் உங்கள் சொந்த திட்டங்களுக்கு பரிந்துரைக்கப்படுகிறது.

1. ஒவ்வொரு HORIZON படிக்கட்டிற்கான சோதனை தரவுப் புள்ளியை உருவாக்கவும்.

    ```python
    test_shifted = test.copy()

    for t in range(1, HORIZON+1):
        test_shifted['load+'+str(t)] = test_shifted['load'].shift(-t, freq='H')

    test_shifted = test_shifted.dropna(how='any')
    test_shifted.head(5)
    ```

    |            |          | load | load+1 | load+2 |
    | ---------- | -------- | ---- | ------ | ------ |
    | 2014-12-30 | 00:00:00 | 0.33 | 0.29   | 0.27   |
    | 2014-12-30 | 01:00:00 | 0.29 | 0.27   | 0.27   |
    | 2014-12-30 | 02:00:00 | 0.27 | 0.27   | 0.30   |
    | 2014-12-30 | 03:00:00 | 0.27 | 0.30   | 0.41   |
    | 2014-12-30 | 04:00:00 | 0.30 | 0.41   | 0.57   |

    தரவுகள் அதன் horizon புள்ளிக்கு ஏற்ப கிடைமட்டமாக மாற்றப்படுகின்றன.

1. சோதனை தரவுகளில் முன்னறிவிப்புகளை இந்த sliding window அணுகுமுறையைப் பயன்படுத்தி சோதனை தரவின் நீளத்திற்கு சமமான ஒரு மடக்கத்தில் செய்யவும்:

    ```python
    %%time
    training_window = 720 # dedicate 30 days (720 hours) for training

    train_ts = train['load']
    test_ts = test_shifted

    history = [x for x in train_ts]
    history = history[(-training_window):]

    predictions = list()

    order = (2, 1, 0)
    seasonal_order = (1, 1, 0, 24)

    for t in range(test_ts.shape[0]):
        model = SARIMAX(endog=history, order=order, seasonal_order=seasonal_order)
        model_fit = model.fit()
        yhat = model_fit.forecast(steps = HORIZON)
        predictions.append(yhat)
        obs = list(test_ts.iloc[t])
        # move the training window
        history.append(obs[0])
        history.pop(0)
        print(test_ts.index[t])
        print(t+1, ': predicted =', yhat, 'expected =', obs)
    ```

    பயிற்சி நடைபெறுவதை நீங்கள் காணலாம்:

    ```output
    2014-12-30 00:00:00
    1 : predicted = [0.32 0.29 0.28] expected = [0.32945389435989236, 0.2900626678603402, 0.2739480752014323]

    2014-12-30 01:00:00
    2 : predicted = [0.3  0.29 0.3 ] expected = [0.2900626678603402, 0.2739480752014323, 0.26812891674127126]

    2014-12-30 02:00:00
    3 : predicted = [0.27 0.28 0.32] expected = [0.2739480752014323, 0.26812891674127126, 0.3025962399283795]
    ```

1. முன்னறிவிப்புகளை உண்மையான load-ஐ ஒப்பிடவும்:

    ```python
    eval_df = pd.DataFrame(predictions, columns=['t+'+str(t) for t in range(1, HORIZON+1)])
    eval_df['timestamp'] = test.index[0:len(test.index)-HORIZON+1]
    eval_df = pd.melt(eval_df, id_vars='timestamp', value_name='prediction', var_name='h')
    eval_df['actual'] = np.array(np.transpose(test_ts)).ravel()
    eval_df[['prediction', 'actual']] = scaler.inverse_transform(eval_df[['prediction', 'actual']])
    eval_df.head()
    ```

    வெளியீடு
    |     |            | timestamp | h   | prediction | actual   |
    | --- | ---------- | --------- | --- | ---------- | -------- |
    | 0   | 2014-12-30 | 00:00:00  | t+1 | 3,008.74   | 3,023.00 |
    | 1   | 2014-12-30 | 01:00:00  | t+1 | 2,955.53   | 2,935.00 |
    | 2   | 2014-12-30 | 02:00:00  | t+1 | 2,900.17   | 2,899.00 |
    | 3   | 2014-12-30 | 03:00:00  | t+1 | 2,917.69   | 2,886.00 |
    | 4   | 2014-12-30 | 04:00:00  | t+1 | 2,946.99   | 2,963.00 |

    மணிநேர தரவின் முன்னறிவிப்பை, உண்மையான load-ஐ ஒப்பிடவும். இது எவ்வளவு துல்லியமாக உள்ளது?

### மாதிரி துல்லியத்தைச் சரிபார்க்கவும்

முன்னறிவிப்புகளின் mean absolute percentage error (MAPE)-ஐ சோதனை செய்து உங்கள் மாதிரியின் துல்லியத்தைச் சரிபார்க்கவும்.

> **🧮 கணிதத்தை காட்டவும்**
>
> ![MAPE](../../../../translated_images/mape.fd87bbaf4d346846df6af88b26bf6f0926bf9a5027816d5e23e1200866e3e8a4.ta.png)
>
> [MAPE](https://www.linkedin.com/pulse/what-mape-mad-msd-time-series-allameh-statistics/) என்பது மேலே கொடுக்கப்பட்ட சமன்பாட்டின் மூலம் வரையறுக்கப்பட்ட விகிதமாக கணிப்பு துல்லியத்தை காட்ட பயன்படுத்தப்படுகிறது. உண்மையான<sub>t</sub> மற்றும் கணிக்கப்பட்ட<sub>t</sub> இடையேயான வேறுபாடு உண்மையான<sub>t</sub> மூலம் வகுக்கப்படுகிறது. "இந்த கணக்கீட்டில் உள்ள முழு மதிப்பு ஒவ்வொரு கணிக்கப்பட்ட நேர புள்ளிக்கும் சேர்க்கப்பட்டு, பொருத்தப்பட்ட புள்ளிகளின் எண்ணிக்கையால் n வகுக்கப்படுகிறது." [wikipedia](https://wikipedia.org/wiki/Mean_absolute_percentage_error)

1. சமன்பாட்டை குறியீட்டில் வெளிப்படுத்தவும்:

    ```python
    if(HORIZON > 1):
        eval_df['APE'] = (eval_df['prediction'] - eval_df['actual']).abs() / eval_df['actual']
        print(eval_df.groupby('h')['APE'].mean())
    ```

1. ஒரு படியின் MAPE ஐ கணக்கிடவும்:

    ```python
    print('One step forecast MAPE: ', (mape(eval_df[eval_df['h'] == 't+1']['prediction'], eval_df[eval_df['h'] == 't+1']['actual']))*100, '%')
    ```

    ஒரு படி கணிப்பு MAPE:  0.5570581332313952 %

1. பல படி கணிப்பு MAPE ஐ அச்சிடவும்:

    ```python
    print('Multi-step forecast MAPE: ', mape(eval_df['prediction'], eval_df['actual'])*100, '%')
    ```

    ```output
    Multi-step forecast MAPE:  1.1460048657704118 %
    ```

    ஒரு சிறந்த குறைந்த எண்ணிக்கை சிறந்தது: MAPE 10 என்றால், அது 10% தவறாக உள்ளது என்று பொருள்.

1. ஆனால் எப்போதும் போல, இந்த மாதிரியான துல்லிய அளவீட்டை கண்ணுக்கு தெளிவாக பார்க்க எளிதாக இருக்கும், எனவே இதை வரைபடமாக வரைவோம்:

    ```python
     if(HORIZON == 1):
        ## Plotting single step forecast
        eval_df.plot(x='timestamp', y=['actual', 'prediction'], style=['r', 'b'], figsize=(15, 8))

    else:
        ## Plotting multi step forecast
        plot_df = eval_df[(eval_df.h=='t+1')][['timestamp', 'actual']]
        for t in range(1, HORIZON+1):
            plot_df['t+'+str(t)] = eval_df[(eval_df.h=='t+'+str(t))]['prediction'].values

        fig = plt.figure(figsize=(15, 8))
        ax = plt.plot(plot_df['timestamp'], plot_df['actual'], color='red', linewidth=4.0)
        ax = fig.add_subplot(111)
        for t in range(1, HORIZON+1):
            x = plot_df['timestamp'][(t-1):]
            y = plot_df['t+'+str(t)][0:len(x)]
            ax.plot(x, y, color='blue', linewidth=4*math.pow(.9,t), alpha=math.pow(0.8,t))

        ax.legend(loc='best')

    plt.xlabel('timestamp', fontsize=12)
    plt.ylabel('load', fontsize=12)
    plt.show()
    ```

    ![ஒரு நேர வரிசை மாதிரி](../../../../translated_images/accuracy.2c47fe1bf15f44b3656651c84d5e2ba9b37cd929cd2aa8ab6cc3073f50570f4e.ta.png)

🏆 ஒரு மிக அழகான வரைபடம், நல்ல துல்லியத்துடன் ஒரு மாதிரியை காட்டுகிறது. நல்ல வேலை!

---

## 🚀சவால்

ஒரு நேர வரிசை மாதிரியின் துல்லியத்தை சோதிக்க பல வழிகளை ஆராயுங்கள். இந்த பாடத்தில் நாங்கள் MAPE பற்றி பேசுகிறோம், ஆனால் நீங்கள் பயன்படுத்தக்கூடிய பிற முறைகள் உள்ளதா? அவற்றை ஆராய்ந்து குறிப்பிட்டு எழுதுங்கள். [இங்கே](https://otexts.com/fpp2/accuracy.html) ஒரு பயனுள்ள ஆவணம் கிடைக்கிறது.

## [பாடத்திற்குப் பிந்தைய வினாடி வினா](https://ff-quizzes.netlify.app/en/ml/)

## மதிப்பீடு & சுயபடிப்பு

இந்த பாடம் ARIMA உடன் நேர வரிசை கணிப்பின் அடிப்படைகளை மட்டுமே தொடுகிறது. [இந்த களஞ்சியத்தை](https://microsoft.github.io/forecasting/) மற்றும் அதன் பல்வேறு மாதிரி வகைகளை ஆராய்ந்து நேர வரிசை மாதிரிகளை உருவாக்கும் பிற வழிகளை அறிய உங்கள் அறிவை ஆழமாக்க நேரம் ஒதுக்குங்கள்.

## பணிக்கட்டளை

[ஒரு புதிய ARIMA மாதிரி](assignment.md)

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கிறோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை தயவுசெய்து கவனத்தில் கொள்ளுங்கள். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.