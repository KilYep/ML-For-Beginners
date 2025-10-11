<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "134d8759f0e2ab886e9aa4f62362c201",
  "translation_date": "2025-10-11T11:13:08+00:00",
  "source_file": "TROUBLESHOOTING.md",
  "language_code": "ta"
}
-->
# பிழைத்திருத்த வழிகாட்டி

இந்த வழிகாட்டி, Machine Learning for Beginners பாடத்திட்டத்துடன் வேலை செய்யும்போது ஏற்படும் பொதுவான பிரச்சினைகளை தீர்க்க உங்களுக்கு உதவுகிறது. இங்கே தீர்வு கிடைக்காவிட்டால், எங்கள் [Discord Discussions](https://aka.ms/foundry/discord) அல்லது [ஒரு பிரச்சினையைத் திறக்கவும்](https://github.com/microsoft/ML-For-Beginners/issues) சென்று பார்க்கவும்.

## உள்ளடக்க அட்டவணை

- [நிறுவல் பிரச்சினைகள்](../..)
- [Jupyter Notebook பிரச்சினைகள்](../..)
- [Python Package பிரச்சினைகள்](../..)
- [R சூழல் பிரச்சினைகள்](../..)
- [வினாடி வினா பயன்பாட்டு பிரச்சினைகள்](../..)
- [தரவு மற்றும் கோப்பு பாதை பிரச்சினைகள்](../..)
- [பொதுவான பிழை செய்திகள்](../..)
- [செயல்திறன் பிரச்சினைகள்](../..)
- [சூழல் மற்றும் கட்டமைப்பு](../..)

---

## நிறுவல் பிரச்சினைகள்

### Python நிறுவல்

**பிரச்சினை**: `python: command not found`

**தீர்வு**:
1. [python.org](https://www.python.org/downloads/) இல் இருந்து Python 3.8 அல்லது அதற்கு மேல் நிறுவவும்
2. நிறுவலை சரிபார்க்கவும்: `python --version` அல்லது `python3 --version`
3. macOS/Linux இல், `python` பதிலாக `python3` பயன்படுத்த வேண்டியிருக்கும்

**பிரச்சினை**: பல Python பதிப்புகள் மோதல்களை ஏற்படுத்துகிறது

**தீர்வு**:
```bash
# Use virtual environments to isolate projects
python -m venv ml-env

# Activate virtual environment
# On Windows:
ml-env\Scripts\activate
# On macOS/Linux:
source ml-env/bin/activate
```

### Jupyter நிறுவல்

**பிரச்சினை**: `jupyter: command not found`

**தீர்வு**:
```bash
# Install Jupyter
pip install jupyter

# Or with pip3
pip3 install jupyter

# Verify installation
jupyter --version
```

**பிரச்சினை**: Jupyter உலாவியில் துவங்கவில்லை

**தீர்வு**:
```bash
# Try specifying the browser
jupyter notebook --browser=chrome

# Or copy the URL with token from terminal and paste in browser manually
# Look for: http://localhost:8888/?token=...
```

### R நிறுவல்

**பிரச்சினை**: R தொகுப்புகள் நிறுவப்படவில்லை

**தீர்வு**:
```r
# Ensure you have the latest R version
# Install packages with dependencies
install.packages(c("tidyverse", "tidymodels", "caret"), dependencies = TRUE)

# If compilation fails, try installing binary versions
install.packages("package-name", type = "binary")
```

**பிரச்சினை**: IRkernel Jupyter இல் கிடைக்கவில்லை

**தீர்வு**:
```r
# In R console
install.packages('IRkernel')
IRkernel::installspec(user = TRUE)
```

---

## Jupyter Notebook பிரச்சினைகள்

### Kernel பிரச்சினைகள்

**பிரச்சினை**: Kernel தொடர்ந்து முடிவடைகிறது அல்லது மீண்டும் துவங்குகிறது

**தீர்வு**:
1. Kernel ஐ மீண்டும் துவங்கவும்: `Kernel → Restart`
2. வெளியீட்டை அழித்து மீண்டும் துவங்கவும்: `Kernel → Restart & Clear Output`
3. நினைவக பிரச்சினைகளை சரிபார்க்கவும் ([செயல்திறன் பிரச்சினைகள்](../..) பார்க்கவும்)
4. பிரச்சனையுள்ள கோடுகளை கண்டறிய ஒவ்வொரு செல்களையும் தனித்தனியாக இயக்கவும்

**பிரச்சினை**: தவறான Python kernel தேர்ந்தெடுக்கப்பட்டுள்ளது

**தீர்வு**:
1. தற்போதைய kernel ஐ சரிபார்க்கவும்: `Kernel → Change Kernel`
2. சரியான Python பதிப்பைத் தேர்ந்தெடுக்கவும்
3. Kernel காணப்படவில்லை என்றால், அதை உருவாக்கவும்:
```bash
python -m ipykernel install --user --name=ml-env
```

**பிரச்சினை**: Kernel துவங்கவில்லை

**தீர்வு**:
```bash
# Reinstall ipykernel
pip uninstall ipykernel
pip install ipykernel

# Register the kernel again
python -m ipykernel install --user
```

### Notebook செல்கள் பிரச்சினைகள்

**பிரச்சினை**: செல்கள் இயக்கப்படுகின்றன ஆனால் வெளியீடு காட்டவில்லை

**தீர்வு**:
1. செல்கள் இன்னும் இயங்குகிறதா என்று சரிபார்க்கவும் (`[*]` குறியீட்டை பாருங்கள்)
2. Kernel ஐ மீண்டும் துவங்கி அனைத்து செல்களையும் இயக்கவும்: `Kernel → Restart & Run All`
3. உலாவி கன்சோலில் JavaScript பிழைகளை சரிபார்க்கவும் (F12)

**பிரச்சினை**: செல்களை இயக்க முடியவில்லை - "Run" கிளிக் செய்தால் பதில் இல்லை

**தீர்வு**:
1. Jupyter சர்வர் இன்னும் டெர்மினலில் இயங்குகிறதா என்று சரிபார்க்கவும்
2. உலாவி பக்கத்தை புதுப்பிக்கவும்
3. Notebook ஐ மூடி மீண்டும் திறக்கவும்
4. Jupyter சர்வரை மீண்டும் துவங்கவும்

---

## Python Package பிரச்சினைகள்

### இறக்குமதி பிழைகள்

**பிரச்சினை**: `ModuleNotFoundError: No module named 'sklearn'`

**தீர்வு**:
```bash
pip install scikit-learn

# Common ML packages for this course
pip install scikit-learn pandas numpy matplotlib seaborn
```

**பிரச்சினை**: `ImportError: cannot import name 'X' from 'sklearn'`

**தீர்வு**:
```bash
# Update scikit-learn to latest version
pip install --upgrade scikit-learn

# Check version
python -c "import sklearn; print(sklearn.__version__)"
```

### பதிப்பு மோதல்கள்

**பிரச்சினை**: தொகுப்பு பதிப்பு பொருந்தாத பிழைகள்

**தீர்வு**:
```bash
# Create a new virtual environment
python -m venv fresh-env
source fresh-env/bin/activate  # or fresh-env\Scripts\activate on Windows

# Install packages fresh
pip install jupyter scikit-learn pandas numpy matplotlib seaborn

# If specific version needed
pip install scikit-learn==1.3.0
```

**பிரச்சினை**: `pip install` அனுமதி பிழைகளுடன் தோல்வியடைந்தது

**தீர்வு**:
```bash
# Install for current user only
pip install --user package-name

# Or use virtual environment (recommended)
python -m venv venv
source venv/bin/activate
pip install package-name
```

### தரவுகளை ஏற்றும் பிரச்சினைகள்

**பிரச்சினை**: CSV கோப்புகளை ஏற்றும்போது `FileNotFoundError`

**தீர்வு**:
```python
import os
# Check current working directory
print(os.getcwd())

# Use relative paths from notebook location
df = pd.read_csv('../../data/filename.csv')

# Or use absolute paths
df = pd.read_csv('/full/path/to/data/filename.csv')
```

---

## R சூழல் பிரச்சினைகள்

### தொகுப்பு நிறுவல்

**பிரச்சினை**: தொகுப்பு நிறுவல் தொகுப்பு பிழைகளுடன் தோல்வியடைந்தது

**தீர்வு**:
```r
# Install binary version (Windows/macOS)
install.packages("package-name", type = "binary")

# Update R to latest version if packages require it
# Check R version
R.version.string

# Install system dependencies (Linux)
# For Ubuntu/Debian, in terminal:
# sudo apt-get install r-base-dev
```

**பிரச்சினை**: `tidyverse` நிறுவப்படவில்லை

**தீர்வு**:
```r
# Install dependencies first
install.packages(c("rlang", "vctrs", "pillar"))

# Then install tidyverse
install.packages("tidyverse")

# Or install components individually
install.packages(c("dplyr", "ggplot2", "tidyr", "readr"))
```

### RMarkdown பிரச்சினைகள்

**பிரச்சினை**: RMarkdown உருவாக்கப்படவில்லை

**தீர்வு**:
```r
# Install/update rmarkdown
install.packages("rmarkdown")

# Install pandoc if needed
install.packages("pandoc")

# For PDF output, install tinytex
install.packages("tinytex")
tinytex::install_tinytex()
```

---

## வினாடி வினா பயன்பாட்டு பிரச்சினைகள்

### கட்டமைப்பு மற்றும் நிறுவல்

**பிரச்சினை**: `npm install` தோல்வியடைந்தது

**தீர்வு**:
```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install

# If still fails, try with legacy peer deps
npm install --legacy-peer-deps
```

**பிரச்சினை**: Port 8080 ஏற்கனவே பயன்படுத்தப்படுகிறது

**தீர்வு**:
```bash
# Use different port
npm run serve -- --port 8081

# Or find and kill process using port 8080
# On Linux/macOS:
lsof -ti:8080 | xargs kill -9

# On Windows:
netstat -ano | findstr :8080
taskkill /PID <PID> /F
```

### கட்டமைப்பு பிழைகள்

**பிரச்சினை**: `npm run build` தோல்வியடைந்தது

**தீர்வு**:
```bash
# Check Node.js version (should be 14+)
node --version

# Update Node.js if needed
# Then clean install
rm -rf node_modules package-lock.json
npm install
npm run build
```

**பிரச்சினை**: கட்டமைப்பைத் தடுக்கும் Linting பிழைகள்

**தீர்வு**:
```bash
# Fix auto-fixable issues
npm run lint -- --fix

# Or temporarily disable linting in build
# (not recommended for production)
```

---

## தரவு மற்றும் கோப்பு பாதை பிரச்சினைகள்

### பாதை பிரச்சினைகள்

**பிரச்சினை**: Notebook களை இயக்கும்போது தரவுக் கோப்புகள் கிடைக்கவில்லை

**தீர்வு**:
1. **Notebook களை அவற்றின் உள்ளடக்க கோப்பகத்திலிருந்து எப்போதும் இயக்கவும்**
   ```bash
   cd /path/to/lesson/folder
   jupyter notebook
   ```

2. **கோடில் உறவுப்பாதைகளை சரிபார்க்கவும்**
   ```python
   # Correct path from notebook location
   df = pd.read_csv('../data/filename.csv')
   
   # Not from your terminal location
   ```

3. **தேவைப்பட்டால் முழு பாதைகளைப் பயன்படுத்தவும்**
   ```python
   import os
   base_path = os.path.dirname(os.path.abspath(__file__))
   data_path = os.path.join(base_path, 'data', 'filename.csv')
   ```

### தரவுக் கோப்புகள் காணவில்லை

**பிரச்சினை**: தரவுத்தொகுப்பு கோப்புகள் காணவில்லை

**தீர்வு**:
1. தரவு களஞ்சியத்தில் இருக்க வேண்டுமா என்று சரிபார்க்கவும் - பெரும்பாலான தரவுத்தொகுப்புகள் சேர்க்கப்பட்டுள்ளன
2. சில பாடங்கள் தரவுகளை பதிவிறக்கம் செய்ய வேண்டும் - பாடம் README ஐ சரிபார்க்கவும்
3. சமீபத்திய மாற்றங்களை நீங்கள் இழுத்துள்ளீர்களா என்பதை உறுதிப்படுத்தவும்:
   ```bash
   git pull origin main
   ```

---

## பொதுவான பிழை செய்திகள்

### நினைவக பிழைகள்

**பிழை**: `MemoryError` அல்லது kernel தரவுகளை செயலாக்கும்போது முடிவடைகிறது

**தீர்வு**:
```python
# Load data in chunks
for chunk in pd.read_csv('large_file.csv', chunksize=10000):
    process(chunk)

# Or read only needed columns
df = pd.read_csv('file.csv', usecols=['col1', 'col2'])

# Free memory when done
del large_dataframe
import gc
gc.collect()
```

### ஒருங்கிணைப்பு எச்சரிக்கைகள்

**எச்சரிக்கை**: `ConvergenceWarning: Maximum number of iterations reached`

**தீர்வு**:
```python
from sklearn.linear_model import LogisticRegression

# Increase max iterations
model = LogisticRegression(max_iter=1000)

# Or scale your features first
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

### வரைதல் பிரச்சினைகள்

**பிரச்சினை**: Jupyter இல் வரைபடங்கள் காட்டப்படவில்லை

**தீர்வு**:
```python
# Enable inline plotting
%matplotlib inline

# Import pyplot
import matplotlib.pyplot as plt

# Show plot explicitly
plt.plot(data)
plt.show()
```

**பிரச்சினை**: Seaborn வரைபடங்கள் வேறுபடுகின்றன அல்லது பிழைகளை ஏற்படுத்துகின்றன

**தீர்வு**:
```python
import warnings
warnings.filterwarnings('ignore', category=UserWarning)

# Update to compatible version
# pip install --upgrade seaborn matplotlib
```

### Unicode/Encoding பிழைகள்

**பிரச்சினை**: `UnicodeDecodeError` கோப்புகளைப் படிக்கும்போது

**தீர்வு**:
```python
# Specify encoding explicitly
df = pd.read_csv('file.csv', encoding='utf-8')

# Or try different encoding
df = pd.read_csv('file.csv', encoding='latin-1')

# For errors='ignore' to skip problematic characters
df = pd.read_csv('file.csv', encoding='utf-8', errors='ignore')
```

---

## செயல்திறன் பிரச்சினைகள்

### மெதுவாக Notebook செயல்படுதல்

**பிரச்சினை**: Notebook கள் மிகவும் மெதுவாக இயங்குகின்றன

**தீர்வு**:
1. **நினைவகத்தை விடுவிக்க Kernel ஐ மீண்டும் துவங்கவும்**: `Kernel → Restart`
2. **பயன்படுத்தப்படாத Notebook களை மூடவும்** நினைவகத்தை விடுவிக்க
3. **சோதனைக்காக சிறிய தரவுத் மாதிரிகளைப் பயன்படுத்தவும்**:
   ```python
   # Work with subset during development
   df_sample = df.sample(n=1000)
   ```
4. **உங்கள் கோடுகளை சோதிக்கவும்**: 
   ```python
   %time operation()  # Time single operation
   %timeit operation()  # Time with multiple runs
   ```

### அதிக நினைவக பயன்பாடு

**பிரச்சினை**: கணினி நினைவகம் குறைவாக உள்ளது

**தீர்வு**:
```python
# Check memory usage
df.info(memory_usage='deep')

# Optimize data types
df['column'] = df['column'].astype('int32')  # Instead of int64

# Drop unnecessary columns
df = df[['col1', 'col2']]  # Keep only needed columns

# Process in batches
for batch in np.array_split(df, 10):
    process(batch)
```

---

## சூழல் மற்றும் கட்டமைப்பு

### மெய்நிகர் சூழல் பிரச்சினைகள்

**பிரச்சினை**: மெய்நிகர் சூழல் செயல்படவில்லை

**தீர்வு**:
```bash
# Windows
python -m venv venv
venv\Scripts\activate.bat

# macOS/Linux
python3 -m venv venv
source venv/bin/activate

# Check if activated (should show venv name in prompt)
which python  # Should point to venv python
```

**பிரச்சினை**: தொகுப்புகள் நிறுவப்பட்டுள்ளன ஆனால் Notebook இல் காணப்படவில்லை

**தீர்வு**:
```bash
# Ensure notebook uses the correct kernel
# Install ipykernel in your venv
pip install ipykernel
python -m ipykernel install --user --name=ml-env --display-name="Python (ml-env)"

# In Jupyter: Kernel → Change Kernel → Python (ml-env)
```

### Git பிரச்சினைகள்

**பிரச்சினை**: சமீபத்திய மாற்றங்களை இழுக்க முடியவில்லை - இணைப்பு மோதல்கள்

**தீர்வு**:
```bash
# Stash your changes
git stash

# Pull latest
git pull origin main

# Reapply your changes
git stash pop

# If conflicts, resolve manually or:
git checkout --theirs path/to/file  # Take remote version
git checkout --ours path/to/file    # Keep your version
```

### VS Code ஒருங்கிணைப்பு

**பிரச்சினை**: Jupyter notebook களை VS Code இல் திறக்க முடியவில்லை

**தீர்வு**:
1. VS Code இல் Python நீட்டிப்பை நிறுவவும்
2. VS Code இல் Jupyter நீட்டிப்பை நிறுவவும்
3. சரியான Python மொழிபெயர்ப்பாளரைத் தேர்ந்தெடுக்கவும்: `Ctrl+Shift+P` → "Python: Select Interpreter"
4. VS Code ஐ மீண்டும் துவங்கவும்

---

## கூடுதல் வளங்கள்

- **Discord Discussions**: [#ml-for-beginners சேனலில் கேள்விகளை கேட்டு தீர்வுகளைப் பகிரவும்](https://aka.ms/foundry/discord)
- **Microsoft Learn**: [ML for Beginners தொகுப்புகள்](https://learn.microsoft.com/en-us/collections/qrqzamz1nn2wx3?WT.mc_id=academic-77952-bethanycheum)
- **வீடியோ பயிற்சிகள்**: [YouTube Playlist](https://aka.ms/ml-beginners-videos)
- **பிழை கண்காணிப்பு**: [பிழைகளை அறிக்கையிடவும்](https://github.com/microsoft/ML-For-Beginners/issues)

---

## இன்னும் பிரச்சினைகள் உள்ளதா?

மேலே உள்ள தீர்வுகளை முயற்சித்த பிறகும் இன்னும் பிரச்சினைகளை சந்தித்தால்:

1. **இருக்கும் பிரச்சினைகளை தேடவும்**: [GitHub Issues](https://github.com/microsoft/ML-For-Beginners/issues)
2. **Discord விவாதங்களில் சரிபார்க்கவும்**: [Discord Discussions](https://aka.ms/foundry/discord)
3. **புதிய பிரச்சினையைத் திறக்கவும்**: இதில் சேர்க்கவும்:
   - உங்கள் இயக்க முறைமுறை மற்றும் பதிப்பு
   - Python/R பதிப்பு
   - பிழை செய்தி (முழு traceback உடன்)
   - பிரச்சினையை உருவாக்கிய படி
   - நீங்கள் ஏற்கனவே முயற்சித்தது

நாங்கள் உதவ தயாராக இருக்கிறோம்! 🚀

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையை பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. எங்கள் தரத்தை உறுதிப்படுத்த முயற்சிக்கிறோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறுகள் இருக்கக்கூடும் என்பதை கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.