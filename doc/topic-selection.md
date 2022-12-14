연구의 중요성(연구의 동기)
--------------------
* 대용량 데이터를 대상으로 한 서비스를 다루는 것이 본 연구의 동기이다. 구체적으로는, 검색엔진 알고리즘, 인덱스를 개선한 서비스를 만들 생각이다.
* 굳이 왜?
  * 대외적인 이유
    * 단적으로, 먹고살기 위해서다.
    * 기업에서는 대용량 데이터를 다루는 대규모 서비스를 운용중이라, 이러한 서비스에 대한 경험이 필요하다. 간단하게 토이 프로젝트 만들어서는 취직될 리가 없다..
  * 대내적인 이유
    * 예전의 추억 때문이다. 해커되고 싶어서 이것저것 했던 기억이 있다. 커맨드라인이 문자열로 뒤덮이는 걸 보는 해커의 모습이 그렇게 멋있을 수가 없더라. 대용량 데이터를 대상으로 한 서비스를 다루면, 대규모 로깅 메세지가 출력되는 등 저럴 일을 접할 일도 늘어날 것이라는 (?) 개인적이고도 희한한 계기로 이런 연구를 수행하게 되었다.


연구 목표
-------
* 지금까지 검색엔진 알고리즘, 인덱스에는 상당한 발전이 있었다. 
  * 특히, 구글의 scoring 알고리즘인 pageRank, indexing 알고리즘인 inverted indexing 등은 최고의 퍼포먼스를 내는 것으로 평가받고 있다. 
* 그러나, 구글의 scoring, indexing 에도 경험적으로 보아 개선의 여지가 있다.
  * pageRank 라는 scoring 방식이 global optimizing 한지에 대한 의문을 해소해야 한다. (구체적으로는 어떠한 의문인지는 더 문헌 조사를 한 후에 기술하도록 한다.)
  * inverted indexing 시 morphological analyzer 가 일반적이지 않은 search query (예를 들자면, 고유명사 등) 에 대응하지 못하면, 의도하지 않은 key 가 선택되어 엉뚱한 value 의 문서를 검색할 수 있다.
    * inverted indexing 시 morphological analyzer 가 '한국외국어대학교' 이라는 search query 를 하나의 고유명사로 인식하지 못하고, '한국' + '외국어' + '대학교' 라고 인식하여, key '한국' 에 대응하는 value 문서, key '외국어' 에 대응하는 value 문서, key '대학교' 에 대응하는 value 문서를 검색결과로 제공할 수 있다.
  * 마찬가지로, inverted indexing 시 morphological analyzer 가 일반적이지 않은 document 내용 (예를 들자면, 고유명사 등을 포함한 document 의 내용) 에 대응하지 못하면, 의도하지 않은 key 로 index 가 작성될 수 있다.
* 위를 개선하기 위해서 다음의 방식을 제안한다. 
  * 단순무식한 방법이지만, random sampling 방법으로 문서를 추출해서, 구글이 놓치는 문서도 검색되도록 한다.
  * 고유명사, 유행어 등을 수록한 사전을 도입하여 morphological analyzer 에 추가한다.
  * bfs 를 이용한 방식은 왜 안하냐면,
    * 구글의 난점을 극복하려는 목적은 bfs, random sampling 둘 다 같은데,
    * random sampling 방식이 더 elegance 한 것 같아서, 이 방식으로 순회하였기 때문이다.


연구 수행단계
----------
* 단계별
  * 문헌조사를 수행한다.
    * pageRank, inverted indexing 등 구글의 검색엔진 알고리즘, 인덱싱에 대해 조사한다.
    * morphological analyzer 에 고유명사 등 사전을 추가하는 기법에 대해 상세히 조사한다.
    * random sampling 등 통계학 지식에 대해서 상세히 조사한다.
    * 그 외 필요한 것이 있으면 조사한다.
  * 연구 목표애 제시한 방법대로 구현한다. 
    * 구현 중 난점이 생길 수도 있다. 이는 추후에 기술한다.


연구에 필요한 이론/기술
---------------
* 이론
  * 구글 검색 알고리즘, 인덱싱 등
    * pageRank 
    * inverted indexing (구체적으로는 hash indexing) 
      * morphological analyzer 
        * 일본어의 경우 kuromoji-ipadic-neolodgd 의 기법
    * 그 외 관련 이론
  * 통계 관련 지식
    * random sampling
  * 이들에 대해 개략적인 지식을 갖추고 있다.
* 기술
  * 대용량 데이터를 open api 를 거쳐 수집한다.
    * google 은 search query api quota, index api quota 에도 제한을 두고 있다.
      * 따라서, google 이 개선대상임에도 불구하고, google 을 활용해서 개선할 수는 없다.
    * twitter 는 google 보다 널널한 quota 를 제공하는 듯 하니 이를 사용할 예정이다.
      * 이걸 대신 활용해 개선된 모습을 보여주어야 한다.
      * 구체적인 방법은 나중에...
  * 수집한 데이터를 dbms 에 저장한다.
    * flask/django 의 데이터베이스 ORM 활용한다.
    * apache lucene 에 저장한다.
  * 수집한 데이터는 정해진 시각에 inverted indexing 수행하고, 이를 통해 search 처리 담당한다.
    * apache lucene 활용한다.
  * 웹뷰에서 검색어를 입력하면 결과를 웹뷰에 표시한다.
    * 웹뷰는 bootstrap + javascript 를 사용한다. 
  * 이들에 대해 개략적인 지식을 갖추고 있다.


연구 활용계획
----------
* 구글 검색 알고리즘과 인덱싱을 개선한 서비스를 만들었지만, 이론으로서 깊이가 없을 경우
  * 졸업논문으로 이를 활용한다.
  * 대용량 데이터를 다루는 포트폴리오로써 (실무에 가까운 경험을 한 포트폴리오로써) 이를 활용한다.
  * 아키텍처 설계, UML 등을 이용한 상위 설계, 함수 입출력 등을 지정한 하위 설계의 문서를, github에 코드와 함께 배포할 생각이다. (여유가 있으면)
* 구글 검색 알고리즘과 인덱싱을 개선한 서비스를 만들었는데, 이론으로서도 깊이가 있을 경우
  * 학술지 출판을 노려본다.
  * 출판 가능성은 미지이므로, 지금 단계에서 구체적인 계획을 세우는 것은 시기상조라 생각한다.


연구 참고문헌 등
------------
* morphological analyzer 참고문헌: 日本語形態素解析の裏側を覗く！MeCab はどのように形態素解析しているか (https://techlife.cookpad.com/entry/2016/05/11/170000) 
* inverted indexing 참고문헌: 대규모 서비스를 지탱하는 기술(이토 나오야 등 저), apache lucene documentation
* 연구에 필요한 기술 참고문헌: 여러 프레임워크와 api documentation
* 연구 진행하면서 추가할 계획
