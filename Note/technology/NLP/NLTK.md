# NLTK

### 去除停用词

~~~python
import nltk
from nltk.corpus import stopwords
nltk.download('stopword')
stopwords = set(stopwords.words('english'))
~~~

