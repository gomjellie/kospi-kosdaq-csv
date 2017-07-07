# kospi-kosdaq-csv
manage kospi, kosdaq historical Data
kosdaq 1232개 종목, kospi 769개 종목

# requirements
pandas_datareader

pandas

<a href='https://github.com/ranaroussi/fix-yahoo-finance'>fix_yahoo_finance</a>
```
pandas_datareader.data.get_data_yahoo 함수를 사용할수 있게 해준다.
```
맨위에 이 코드를 작성한다.
```js
import fix_yahoo_finance as yf
yf.pdr_override()
```

# what is CSV?
comma seperated value

# CSV를 읽어오는 법
```js
import pandas as pd

ticker = '067160' # 아프리카 TV 종목코드.
path = './kosdaq/{}.csv'.format(ticker)
pd.read_csv(path)
```

# 최신 데이터로 업데이트 하기
```
현재 상태 : 2017년 07월 07일
```
```js
empty

```

# 빈 CSV파일들을 업데이트 하기
```
야후에 쿼리를 많이보내면 빈파일이 저장되는 경우가 있는데,
그때 사용한다.
```
```js
import re
import glob
from pandas_datareader import data


def reload_empty(market):
    file_list = glob.glob('./{}/*.csv'.format(market))
    six_digit = re.compile('\d{6}')

    for file_name in file_list:
        file = pd.read_csv(file_name)
        if file.empty:
            print("empty file {} is updated".format(file_name))
            ticker = six_digit.findall(file_name)[0]
            tmp_df = data.get_data_yahoo(ticker+'.KQ', start='1996-05-06', thread=20)
            tmp_df.to_csv(file_name)

reload_empty('kosdaq')
```

