<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "b28a3a4911584062772c537b653ebbc7",
  "translation_date": "2025-10-11T12:04:49+00:00",
  "source_file": "5-Clustering/README.md",
  "language_code": "ta"
}
-->
# மெஷின் லெர்னிங் க்கான கிளஸ்டரிங் மாடல்கள்

கிளஸ்டரிங் என்பது மெஷின் லெர்னிங் பணியாகும், இதில் ஒருவருக்கொருவர் ஒத்த பொருட்களை கண்டறிந்து அவற்றை கிளஸ்டர்கள் எனப்படும் குழுக்களாக பிரிக்கிறது. மற்ற மெஷின் லெர்னிங் அணுகுமுறைகளிலிருந்து கிளஸ்டரிங் மாறுபடுவது என்னவென்றால், இது தானாகவே நடக்கிறது; உண்மையில், இது மேற்பார்வை கற்றலின் எதிர்மாறாக இருக்கிறது என்று கூறலாம்.

## பிராந்திய தலைப்பு: நைஜீரிய பார்வையாளர்களின் இசை விருப்பங்களுக்கு கிளஸ்டரிங் மாடல்கள் 🎧

நைஜீரியாவின் பல்வகை பார்வையாளர்களுக்கு பல்வகை இசை விருப்பங்கள் உள்ளன. [இந்த கட்டுரையால்](https://towardsdatascience.com/country-wise-visual-analysis-of-music-taste-using-spotify-api-seaborn-in-python-77f5b749b421) ஊக்கமடைந்து Spotify-இல் இருந்து சேகரிக்கப்பட்ட தரவுகளைப் பயன்படுத்தி, நைஜீரியாவில் பிரபலமான சில இசைகளைப் பார்ப்போம். இந்த தரவுத்தொகுப்பில் பாடல்களின் 'danceability' மதிப்பெண், 'acousticness', ஒலியளவு, 'speechiness', பிரபலத்தன்மை மற்றும் ஆற்றல் பற்றிய தகவல்கள் அடங்கும். இந்த தரவுகளில் முறைமைகள் கண்டறிவது 흥மையாக இருக்கும்!

![ஒரு டர்ன்டேபிள்](../../../translated_images/turntable.f2b86b13c53302dc106aa741de9dc96ac372864cf458dd6f879119857aab01da.ta.jpg)

> புகைப்படம் <a href="https://unsplash.com/@marcelalaskoski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Marcela Laskoski</a> மூலம் <a href="https://unsplash.com/s/photos/nigerian-music?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a> இல்

இந்த பாடங்களின் தொடரில், கிளஸ்டரிங் தொழில்நுட்பங்களைப் பயன்படுத்தி தரவுகளைப் பகுப்பாய்வு செய்ய புதிய வழிகளை நீங்கள் கண்டறிவீர்கள். உங்கள் தரவுத்தொகுப்பில் லேபல்கள் இல்லாதபோது கிளஸ்டரிங் மிகவும் பயனுள்ளதாக இருக்கும். லேபல்கள் இருந்தால், நீங்கள் முந்தைய பாடங்களில் கற்றுக்கொண்ட வகைப்படுத்தல் தொழில்நுட்பங்கள் அதிக பயனுள்ளதாக இருக்கலாம். ஆனால் லேபல்கள் இல்லாத தரவுகளை குழுவாக பிரிக்க முயற்சிக்கும் சந்தர்ப்பங்களில், முறைமைகளை கண்டறிவதற்கான சிறந்த வழியாக கிளஸ்டரிங் இருக்கும்.

> கிளஸ்டரிங் மாடல்களுடன் வேலை செய்வதற்கான பயிற்சியில் உதவக்கூடிய பயனுள்ள குறைந்த குறியீட்டு கருவிகள் உள்ளன. [Azure ML](https://docs.microsoft.com/learn/modules/create-clustering-model-azure-machine-learning-designer/?WT.mc_id=academic-77952-leestott) ஐ இந்த பணிக்காக முயற்சிக்கவும்.

## பாடங்கள்

1. [கிளஸ்டரிங் அறிமுகம்](1-Visualize/README.md)
2. [K-Means கிளஸ்டரிங்](2-K-Means/README.md)

## கிரெடிட்ஸ்

இந்த பாடங்கள் 🎶 உடன் [Jen Looper](https://www.twitter.com/jenlooper) மூலம் எழுதப்பட்டவை, [Rishit Dagli](https://rishit_dagli) மற்றும் [Muhammad Sakib Khan Inan](https://twitter.com/Sakibinan) ஆகியோரின் பயனுள்ள மதிப்பீடுகளுடன்.

[Nigerian Songs](https://www.kaggle.com/sootersaalu/nigerian-songs-spotify) தரவுத்தொகுப்பு Spotify-இல் இருந்து சேகரிக்கப்பட்டது மற்றும் Kaggle-இல் இருந்து பெறப்பட்டது.

இந்த பாடத்தை உருவாக்க உதவிய பயனுள்ள K-Means உதாரணங்களில் [iris exploration](https://www.kaggle.com/bburns/iris-exploration-pca-k-means-and-gmm-clustering), [அறிமுக நோட்புக்](https://www.kaggle.com/prashant111/k-means-clustering-with-python), மற்றும் [hypothetical NGO example](https://www.kaggle.com/ankandash/pca-k-means-clustering-hierarchical-clustering) ஆகியவை அடங்கும்.

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கின்றோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை தயவுசெய்து கவனத்தில் கொள்ளுங்கள். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.