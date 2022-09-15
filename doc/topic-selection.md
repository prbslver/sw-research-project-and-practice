연구목표
------
대용량 데이터를 대상으로 한 서비스를 다루는 것이 일차적 목적이다. 이를 달성하기 위해, (1) 구글 검색엔진을 모방한 서비스를 구현하거나, 혹은, (2) 구글 검색엔진 알고리즘을 응용한 서비스를 구현한다. 목표를 달성하는 데 있어서의 우선순위로는, (2) 를 달성하는 것이 가장 이상적이겠으나, 상황이 여의치 않으면 (1) 을 달성하기로 한다. (2) 에 대해 구체적으로 논하자면, (2) 는 구글 검색엔진을 사용하면서 느낀 불편하였던 점을 개선한 서비스이다. 보다 더 자세한 내용은 아래에 작성하기로 한다.


연구 중요성
---------
* 구글 검색엔진을 사용하면서 느낀 불편한 점은 다음과 같다.
  * 구글의 검색결과는 검색어에 대한 결과를 폭넓게 보여주지만, 자세히는 보여주지 않는다. 예를 들어, 구글에서 '한국외대' 를 검색하면 한국외대에 대한 전반적인 내용을 알 수 있지만(한국외대가 외국학을 가르친다는 사실, 사이버한국외대를 소유한다는 사실 등), 그 세부적인 내용은 알 수 없다(외국학이 무엇인지에 대해 궁금증이 생기면, 외국학에 대해 별개로 검색해야 한다). 바꾸어 말하자면, 검색결과를 보고 키워드를 '일일이' 습득한 후에, 그 키워드에 대해 '개별적으로' 검색을 '일일이' 수행해야 하므로 번거롭다는 것이다. 
* 이에 대한 해결책으로, 다음의 방식을 제안한다. 
  * 최초로 검색 키워드가 입력되면, 구글 검색 알고리즘을 기반으로 최초 검색을 수행한다. 
  * 이때 얻어진 검색결과에 대해 자동 추출된 키워드로, 구글 검색 알고리즘을 재귀적으로 호출하되, bfs 의 방식으로 다시 검색을 수행하고자 한다. 
  * 이를 원하는 깊이까지 반복한다.
* 이때, 주요 난제는 다음과 같다. 
  * 만일 추출한 키워드가 많으면 bfs 의 효율적인 처리에 방해가 될 수 있는데, 이를 어떻게 처리하면 좋을지 고민하여야 한다. 
  * 키워드의 자동추출은 형태소 분석이라는 방식으로 구현할 생각인데, 이 방식의 시간복잡도가 클 경우에는 이에 대한 대책도 필요하다.


연구 수행단계
----------
* 단계별
  * 나름 문헌조사를 한 것이 있으나 아직 완수하지는 못하였으므로, 이를 마저 완수한다.
    * 교수님 등 여러 사람에게서 문헌조사방법론에 대해 조언도 받는다.
  * 위의 주요 난제를 해결한다.


연구 필요 이론/기술
---------------
* 이론
  * 구글 검색 알고리즘인 inverted index (구체적으로는 hash index) 사용한다.
  * 구글 검색 알고리즘을 응용할 때에는, 재귀호출, bfs 사용한다.
  * 키워드 자동추출시, 형태소 분석 기법 (구체적으로는, 한국어의 경우 konlpy 등 오픈소스, 일본어의 경우 mecab 등 오픈소스, 영어의 경우 nltk 등 오픈소스) 사용한다.
  * 형태소 분석 기법을 제외하고, 나머지에 대해 기본 지식을 갖추고 있다.
* 기술
  * 공통
    * 대용량 데이터를 open api 를 거친 REST API 방식으로 수집한다.
  * 최소수준으로서, 커맨드라인 환경에서 서비스를 만든다.
  * 이상적으로는, 웹서비스에서 서비스를 만든다.
    * 직접 제작한 웹서버를 활용해서, 수집한 데이터를 데이터베이스에 저장한다.
      * flask/django (web application framework) 를 사용한다.
      * 데이터베이스는 AWS 에서 제공하는 RDB 를 활용한다.
      * flask/django 가 데이터베이스에 CRUD 시, ORM 이용한다.
    * 저장한 데이터는 직접 제작한 웹뷰에 표시한다.
      * bootstrap 프레임워크(html, css 의 wrapper 와 비슷하다) + javascript 를 사용한다. 
      * 희망 취업 분야는 백엔드이므로, 웹뷰는 간단하게 구성한다.
  * open api, REST API, flask/django, ORM, bootstrap, javascript 에 대해 기본 지식을 갖추고 있다.
   


연구 활용계획
----------
* 구현 연구 결과일 경우/최소수준으로 구글 검색 알고리즘을 구현할 경우/구글 검색 알고리즘을 응용한 서비스를 만들었지만, 이론으로서 깊이가 없을 경우
  * 졸업논문으로 이를 활용한다.
  * 대용량 데이터를 다루는 포트폴리오로써 (실무에 가까운 경험을 한 포트폴리오로써) 이를 활용한다.
  * 아키텍처 설계, UML 등을 이용한 상위 설계, 함수 입출력 등을 지정한 하위 설계의 문서를, github에 코드와 함께 배포할 생각이다.
* 이론 연구 결과일 경우/구글 검색 알고리즘을 응용한 서비스를 만들고도, 이론으로서 깊이가 있을 경우
  * 학술지 출판을 노려본다.
  * 가능성은 미지이므로, 지금 단계에서 구체적인 계획을 세우는 것은 시기상조라 생각한다.


연구 참고문헌 등
------------
* 진행하고 있는 문헌조사가 끝나고, 조사방법론에 대해 조언을 받은 후 작성할 예정이다.