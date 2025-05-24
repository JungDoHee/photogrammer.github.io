---
layout: post
title:  "[개발기] Python 엑셀 가계부 만들기3"
date:   2025-05-23 00:54:00 +0900
categories: python 파이썬 자동화 pandas
---

## 목차

- [Todo List](#Todo-List)
- [헤더별 색상 변경](#헤더별-색상-변경)
- [내용이 있는 셀에 테두리 추가](#내용이-있는-셀에-테두리-추가)
- [행 너비 조절](#행-너비-조절)
- [회고](#회고)

&nbsp;

지난번에 마무리하지 못한 마지막 요구사항은 아래와 같다.

&nbsp;

# Todo List

- [x]  UI 추가
    - 헤더별 색상 변경
    - 금액 오른쪽 정렬
    - 행 너비 조절
    - 내용이 있는 셀에 테두리 추가

&nbsp;

---

&nbsp;

### 헤더별 색상 변경

파이썬에서 엑셀 디자인을 다룰 때 사용할 두 가지 방법을 찾았다. pandas의 **`Styler 객체`**와 openpyxl의 **`PatternFill 라이브러리`**이다.

[Styler 링크](https://pandas.pydata.org/docs/reference/api/pandas.io.formats.style.Styler.html)


[PatternFill 링크](https://openpyxl.readthedocs.io/en/3.1/api/openpyxl.styles.fills.html#openpyxl.styles.fills.PatternFill)

- Styler
    - 공식 문서에 의하면 HTML과 CSS로 DataFrame과 Series의 스타일을 지정할 수 있다고 한다
    - 데이터에 스타일을 적용한 후 파일에 적용 가능하다
- PatternFill
    - 파일에 저장된 하나씩 내용을 읽어서 스타일을 지정한다 (해당 클래스가 **serialisable** 이라 그런 것 같다)
    - 엑셀에 최적화 된 클래스

이번에는 PatternFill을 이용해서 디자인을 수정하려고 한다. 선정 이유는 앞으로 정렬, 행 너비 조절도 수정해야 하는데 openpyxl을 사용하면 알아보기 쉬울 것 같고, 엑셀에 최적화되었다고 했기 때문이다.

```python
# PatternFill 사용법
# (FFF2CC)연한 노랑색으로 해당 셀을 (solid)채움
cell.fill = PatternFill(start_color='FFF2CC', fill_type='solid')

# 사용할 함수
def cell_color(sheet, column_len, column_no, color, type='solid') :
    for col_num in range(column_len) :
            cell = sheet.cell(row=1, column=column_no+col_num)
            cell.fill = PatternFill(start_color=color, fill_type=type)
```

- start_color
    - color 변수에는 원하는 색상의 **`HEX 코드`**를 입력하면 된다
- fill_type
    - 공식 문서를 확인해보면 PatternFill의 fill_type을 지정하지 않으면 아무런 효과가 없다고 한다.
    - fill_type에 다양한 값을 넣어서 확인해봤지만 **`solid`** 이외의 값은 명확히 확인하지 못했다.
- PatternFill의 return
    - return parent
    - PatternFill은 Fill 클래스를 상속 받고 return 형식은 xml이다
        - xml 내용 : `<class ‘openpyxl.styles.colors.Color’>` 와 같은 스타일 정보
    - (인터넷에서 찾은 내용)
        - PattenrFill은 **각 셀**에 대한 스타일 클래스
        - 각 셀에 PatternFill을 적용하려면 셀의 스타일을 지정하는 {셀}.fill 을 사용해야 한다


&nbsp;

---

&nbsp;

### 숫자 오른쪽 정렬

정렬도 openpyxl을 사용해보려고 한다. **`Alignment`** 라이브러리를 활용했다.

[Alignment 링크](https://openpyxl.readthedocs.io/en/3.1/api/openpyxl.styles.alignment.html#module-openpyxl.styles.alignment)


```python
# Alignment 사용법
sheet.cell(row={적용할 행 숫자}, column={적용할 열 숫자}).alignment = Alignment(horizontal='right', vertical='center')

# 사용할 함수
def cell_align(sheet, column_list, align_type) :
    for col_num in column_list : 
        for row in range(2, sheet.max_row+1) : 
            sheet.cell(row=row, column=col_num).alignment = Alignment(horizontal=align_type, vertical='center')
```

- horizontal
    - 가로 정렬을 의미한다 (엑셀 > 셀 서식 > 맞춤 > 텍스트 맞춤 > 가로 항목)
    - ‘fill’, ‘right’, ‘general’, ‘left’, ‘centerContinuous’, ‘distributed’, ‘center’, ‘justify’
- vertical
    - 세로 정렬을 의미한다 (엑셀 > 셀 서식 > 맞춤 > 텍스트 맞춤 > 세로 항목)
    - ‘top’, ‘distributed’, ‘center’, ‘justify’, ‘bottom’

&nbsp;

---

&nbsp;

### 행 너비 조절

제일 피곤한 항목이었다. 한글, 영어, 숫자를 모두 올바르게 표기하고 싶었다. 영어나 숫자는 1byte로 계산하고 한글이나 일본어는 2byte로 계산한다는 것은 알고 있었지만 문자 in (숫자, 영어)로만 계산하기에는 1byte로 계산될 수 있는 예외가 너무 많았다.

[Python의 unicodedata 라이브러리 링크](https://docs.python.org/ko/3.13/library/unicodedata.html)

```python
import unicodedata

# 문자열의 unicode를 확인해 반각, 전각에 대한 너비 자동 계산
def get_display_width(text):
    width = 0
    for char in str(text) : 
        if unicodedata.east_asian_width(char) in ('F', 'W'):
            width += 2
        else : 
            width += 1
    return width
```

- east_asian_width
    - 문자열을 하나씩 돌아가면서 동아시아 언어의 너비를 적용해야 하는지 반환하는 함수이다
    - 반환하는 문자열이 전각인 (한글, 일본어처럼 2byte로 읽어야 하는 문자) 경우 F:Fullwidth, W:wide 값을 반환한다고 한다 ⇒ (’F’, ‘W’)
        
        ![image-center]({{ '/images/20250523_python_household_project3/동아시아_문자_반환.png' | absolute_url }}){: .align-center}

&nbsp;

---

&nbsp;

## 고려해야 할 사항

지금은 테스트 엑셀 파일이라서 데이터 수가 적지만 수시로 입출금하는 통장의 이체내역이 추가된다고 생각해보자. 총 4개의 시트, 각 20행 정도의 데이터를 처리하는데 **약 5초**나 걸린다. (노트북 소음 또한 심해진다..)

아무래도 데이터 전처리에 사용하는 `for`, 엑셀 스타일을 지정할 때 사용하는 `for` 등 여러 개의 `for` 때문에 연산에 시간이 걸릴 수는 있다. 하지만 모수가 적은 상황에서 5초는 비정상적인 일이다.

지금까지 만든 코드는 동작하는 코드지 **효율적인 코드는 아니라고 생각한다**. 효율적인 코드를 만들기 위해서는 아래의 내용을 더 살펴보는 것이 좋다.

> - 데이터 전처리  
> - 파일 최적화  
> - 반복문 최적화, 조건문 정리 (리팩토링)
> 

&nbsp;

---

&nbsp;

# 회고

파이썬을 배우고 (인프런) + pandas 기본을 확인하고 (인프런) + 엑셀 가계부 자동화 코드를 만드는 데 `약 20시간 6분 소요`했다. 기록하느라 자료 조사하는 데에 시간을 가장 많이 썼던 것 같다.

파이썬을 사용하며 인상 깊었던 부분은 문자열 너비 계산이나 엑셀 스타일 지정 같은 작업을 라이브러리로 지원한다는 점이었다. 세미 콜론이 없어도 동작한다는 것은 어색하지만 개행과 들여쓰기를 정확히 해줘야 동작하는 구조라 코드를 구분하기에는 편하게 느껴졌다.

이것으로 나의 파이썬 배움 단계는 마무리 하겠다. 지금까지는 인터넷에 있는 코드를 끼워 맞추고 동작하는지 확인했다면 이 다음 단계에서는 공식 문서와 그 함수를 쓰는 방식을 유추하면서 진행해보려 한다. v2 또한 블로그에 기록할지는 모르겠지만 v1을 사용하면서 불편한 점을 수정하고 고도화, 데이터 전처리 추가(카드, 은행사 추가 예정), 오픈소스 인공지능을 활용한 재정 위기 관리도 추가하는 게 다음 목표다.