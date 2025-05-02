# 9th-sol

5-2 의 코드는 css01.html 파일을 BeautifulSoup로 파싱하고, 다양한 CSS 선택자와 정규 표현식을 활용해 데이터를 추출하는 것이다. 

<문제점>
해당 코드 그대로는 DataFrame을 생성하거나 엑셀 파일을 저장하지 않으므로, 다음과 같은 추가 작업이 필요하다.


1. Colab에 HTML 파일 만들어 저장

html_content = """
<html>
  <body>
    <div id="cartoon">
      <h1>만화 리스트</h1>
      <ul class="elements">
        <li id="item1">원피스</li>
        <li id="item2">드래곤볼</li>
        <li id="item3">도라에몽</li>
        <li id="item4">짱구</li>
        <li id="item5">포켓몬</li>
      </ul>
    </div>

    <ul id="itemlist">
      <li id="item1">마블</li>
      <li id="item2">DC</li>
      <li id="item3">지브리</li>
      <li id="item4">픽사</li>
    </ul>

    <ul id="vegatables">
      <li class="us">상추</li>
      <li class="us">치커리</li>
      <li id="ko" class="cn">배추</li>
    </ul>

    <a href="http://google.com">구글</a>
    <a href="http://daum.net">다음</a>
    <a href="https://naver.com">네이버</a>
  </body>
</html>
"""

with open("css01.html", "w", encoding="utf-8") as f:
    f.write(html_content)


2. 원래 코드 실행 + 추출 데이터를 리스트에 저장

from bs4 import BeautifulSoup
import pandas as pd
import re

# 파일 불러오기
with open("css01.html", encoding='utf-8') as f:
    soup = BeautifulSoup(f, 'html.parser')

# 모든 <li> 태그 추출
li_tags = soup.find_all('li')

# 리스트로 저장
li_data = [{'text': li.string} for li in li_tags]

# DataFrame 생성
df = pd.DataFrame(li_data)
print(df)



3. 구글 드라이브에 엑셀 파일로 저장

from google.colab import drive
drive.mount('/content/drive')

# 경로 지정 후 저장
excel_path = "/content/drive/MyDrive/li_output.xlsx"
df.to_excel(excel_path, index=False)
print(f"엑셀 저장 완료: {excel_path}")


