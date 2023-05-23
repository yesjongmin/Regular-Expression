# 정규 표현식(Regular-Expression)
### 정규 표현식이란 ?
* 정규 표현식(Regular Expression)은 텍스트 내에서 특정 패턴을 찾거나 매칭시키기 위해 사용되는 문자열의 표현방식입니다. 
* 정규 표현식은 강력하면서도 유연한 패턴 매칭 도구로, 주로 프로그래밍 언어와 텍스트 편집기 등에서 문자열의 검색과 치환을 위한 용도로 쓰이고 있습니다.

### 정규 표현식의 필요성
* 정규 표현식을 사용하면 복잡하거나 반복적인 문자열 처리 작업을 매우 간단하고 효율적으로 수행할 수 있고, 다양한 상황에서 유용하게 사용할 수 있습니다.
1. 문자열 검색 및 매칭 : 특정한 패턴을 가진 문자열을 검색하거나 매칭시킬 수 있습니다.
2. 문자열 대체 : 특정한 패턴을 가진 문자열을 다른 문자열로 대체할 수 있습니다.
3. 데이터 추출 : 텍스트에서 원하는 정보를 추출할 수 있습니다.
4. 유효성 검사 : 특정한 형식이나 규칙에 부합하는지 검사할 수 있습니다.

# 메타문자
* 메타문자는 문자를 설명하기 위한 문자로, 문자의 구성을 설명하기 위해 원래의 의미가 아닌 다른 의미로 쓰이는 문자를 말합니다.

###### 정규표현식에는 아래와 같은 메타문자를 사용합니다.

        . - ^ $ * + ? {} [] \ | ()

<img width="80%" src="https://user-images.githubusercontent.com/134346376/240307099-cd171ac6-3879-4546-8b3b-983be5a66054.png"/>
<img width="80%" src="https://user-images.githubusercontent.com/134346376/240308447-e0c65980-6df9-4aad-92a1-9e223e595d60.png"/>

#### ex) 주민등록번호 정규 표현식
 
        ^\d{2}[0-1]\d{1}[0-3]\d{1}\-[1-4]\d{6}$
        
* ^ : Start of String
* \d{3}: 1-2번째(년도) 숫자 0-9
* [0-1]: 3번째(월도 앞자리) 0,1
* \d{1}: 4번째(월도 뒷자리) 숫자 0-9
* [0-3]: 5번째(일자 앞자리) 0,1,2,3
* \d{1}: 6번째(일자 뒷자리) 숫자 0-9
* \- : 7번째(구분자) -
* [1-4]: 8번째(성별) 90년대생 1,2 2000년대생 3,4
* \d{6}: 9-14번째(뒷자리 6자리) 숫자 0-9
* $: End of String

# 파이썬과 정규 표현식
* 파이썬은 정규 표현식을 지원하기 위해 re(regular expression)모듈을 제공합니다.

        import re
        pattern = re.compile("정규식")
    
re.compile을 사용하여 정규 표현식을 컴파일합니다. re.compile의 결과인 객체 pattern에 입력한 정규식의 대한 정보가 담겨있습니다.

컴파일된 패턴 객체는 다음과 같은 4가지 메서드를 제공합니다.

<img width="80%" src="https://user-images.githubusercontent.com/134346376/240319819-d0fda706-f613-4e14-a110-b3e1e06ff4fa.png"/>

        import re
        p = re.compile('[a-z]+')

위 코드를 실행해 생성된 객체 p로 메서드를 살펴보겠습니다.

### match()
* match 메서드는 문자열의 처음부터 정규식과 매치되는지 조사하며, 반환값으로 match 객체를 돌려줍니다.

        >>> m = p.match("python")
        >>> print(m)
        <_sre.SRE_Match object at 0x01F3F9F8>

python문자열은 [a-z]+정규식에 부합되므로 match 객체를 돌려줍니다.

따라서 파이썬 정규식 프로그램은 다음과 같이 작성해 매치 여부를 확인합니다.

        p = re.compile(정규표현식)
        m = p.match(문자열)
        
        if m:
            print("정규식 일치")
        else:
            print("정규식 불일치")

### search()
* 동일하게 컴파일된 객체 p를 갖고 search 메서드를 수행해보겠습니다. 

        >>> m = p.search("3 python")
        >>> print(m)
        <_sre.SRE_Match object at 0x01F3FA30>

3 python문자열은 첫번째 문자가 숫자이므로 match메서드에서는 None을 반환합니다. 하지만 search메서드는 문자열의 처음부터 검색하는 것이 아니라 문자열 전체를 검색하기 때문에 “python”문자열과 매치돼서 match객체를 반환하게 됩니다.

### findall()
* findall 메서드는 이름 그대로 문자열에서 정규식과 일치하는 부분을 찾아 리스트로 반환시켜줍니다.

        >>> result = p.findall("life is too short")
        >>> print(result)
        ['life', 'is', 'too', 'short']

정규식과 일치하는 부분인 각 단어들이 반환되는 것을 확인할 수 있습니다.

### finditer()
* finditer는 findall과 동일하지만 그 결과로 반복 가능한 객체를 돌려줍니다.

        >>> result = p.finditer("life is too short")
        >>> print(result)
        <callable_iterator object at 0x01F5E390>
        >>> for r in result: print(r)
        ...
        <_sre.SRE_Match object at 0x01F3F9F8>
        <_sre.SRE_Match object at 0x01F3FAD8>
        <_sre.SRE_Match object at 0x01F3FAA0>
        <_sre.SRE_Match object at 0x01F3F9F8>

반복 가능한 객체가 포함하는 각각의 요소는 match 객체입니다.

# match 객체의 메서드
* 반환된 match 객체는 4개의 메서드를 제공합니다.

<img width="80%" src="https://user-images.githubusercontent.com/134346376/240320107-d00a067b-64c5-445c-b207-7afbba93d206.png"/>

        >>> m = p.match("python")
        >>> m.group()
        'python'
        >>> m.start()
        0
        >>> m.end()
        6
        >>> m.span()
        (0, 6)    

만약 match메서드를 사용해 반환된 match메서드라면 start()의 결과값은 항상 0일 것입니다. match메서드는 항상 문자열의 시작부터 조사하기 때문이죠.

search 메서드를 사용한다면 start()값은 다르게 나올 것입니다.

        >>> m = p.search("3 python")
        >>> m.group()
        'python'
        >>> m.start()
        2
        >>> m.end()
        8
        >>> m.span()
        (2, 8)


###### 참고사이트 : https://sooftware.io/regex/, https://fhaktj8-18.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D-%EA%B8%B0%EC%B4%88%EC%99%80-%EC%98%88%EC%A0%9C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0, https://chat.openai.com/
