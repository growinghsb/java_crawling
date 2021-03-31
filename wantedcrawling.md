# 원티드 크롤링을 위한 사전     
* 일단 url은 고정, 하지만 스크롤을 내리면     
  데이터가 계속 나오는 형식으로 페이징 구현    
  
* https://www.wanted.co.kr/wdlist/518/872?country=kr&job_sort=job.popularity_order&years=0&locations=seoul.all      
  위 url이 인기순 정렬, 신입, 서울 전체로 필터를 주고 검색한 내용     
  또한 wdlist/518 뒤에 숫자가 서버개발자라는 의미(872), 자바 개발자는(660)       
  그 앞에 숫자 518은 개발이라는 좀 더 큰 분류를 의미한다.
  
* 위 url에 값을 주고, 서버개발자, 자바개발자 두 분류만 2년차까지 서칭하는거 가능     
  각 링크는 전체 링크가 아닌 상대링크로 되어 있음         
  /wd/52044 이런 형태          
 
* 기본적으로 상세한 구직 내용은 www.wanted.co.kr/wd/52044 이렇게 되어 있음     
  즉, 하나의 공고는 각 wd/뒤에 번호를 가짐, 이는 공고만 따로 모아 index를   
  준 거 같음. 회사마다 고유의 번호가 있거나 하진 않고, 무작위로 보여주는 듯.       
  
* url도 다 서칭 되면 검색 조건 다르게 주고 긁어 오면 될 거 같은데    
  관건은 어떻게 스크롤을 구현 할 지 고민해야 할 듯.        
  
* 동적 웹 페이지를 크롤링 해야 한다.   
  url이 변하지 않고, 전형적인 페이징이 아닌     
  스크롤 형태의 모바일과 동일하게 페이징이 된다.     
  
* 이것만 되면 url 조건 각각 주면서 크롤링 해오는거 어렵지 않을 듯 하다.    
  공고 url도 li 태그에 다 모여 있고, 링크만 가져와서 중복되지 않도록    
  DB에 저장해주면 된다.        
***
* 크롤링이란 무엇인가?    
  * 크롤링이란 온라인 웹 상의 데이터를   
    긁어와 가공하고, 내가 원하는 데이터를     
    추출하는 작업까지 폭넓게 쓰이는 단어이다.     
    
  * 핵심은 `웹상의 데이터` 인 듯 하다.         
    이런 방대한 데이터를 가져와     
    내가 원하는대로 가공해, 최종적으로     
    원하는 결과물을 얻는것이 목적이다.      
    
* 웹상의 데이터는 뭐가 있을까?      
  * 웹은 html 문서로 이루어져 있다.      
    즉 문서들을 링크로 연결해 놓은 것을     
    웹이라 한다.     
    
  * 결국 웹 상의 데이터는 html 문서와      
    xml, json 정도가 있는 듯 하다.      
    
  * 하지만 지배적으로 많은 것이     
    html 문서이고, 이 문서를 읽어와     
    이미지면 이미지, 글이면 글,     
    영상이면 영상 등을 가져오는 것이    
    목적이다.          
    
* html 문서를 읽어와 이를 분석해       
  내가 원하는 정보만 추출하는 것이     
  크롤링이다.     
  
* 웹상의 데이터가 html 밖에 없을까?       
  * 그건 아니다. api를 통한 json, xml 형식도     
    있지만 내가 가지고 오려는 데이터의 대부분이    
    웹상의 html을 기반으로 하고 있기 때문에    
    html 파일을 읽어 오는 것을 포커싱 해도    
    좋을 거 같다.       
    
* 어떻게 웹상의 데이터를 읽어올 것인가?     
  * 자바의 Jsoup이라는 라이브러리를    
    이용할 생각이다.    
    
  * 절차는 대강 이렇다.   
    1. 크롤링할 url(페이지)를 선정한다.     
        * 요청도 가능하다. 즉, GET, POST 요청을   
          통해 html 문서를 그대로 서버로부터   
          받아올 수도 있다.    
  
    2. 선정한 페이지의 HTML 정보를 읽어온다.   
       이는 url을 매개변수로 넘겨주고,    
       라이브러리를 통해 받을 수 있다.     
    
    3. 가져온 HTML 파일을 DOM 형식으로    
       만들어주는 parsar를 사용한다.     
    
    4. parsar를 통해 분석한 html 파일에서    
       원하는 데이터만 추출한다.     
       일단 DB에 저장하는 것을 목표로 한다.    
       추후 이를 분류할 것이다.     
       일단 필요한 정보만 추려서 DB에    
       저장하도록 한다.   
    
* 자바로 크롤링을 하려면 어떻게 해야할까?     
  * 현재는 Jsoup이라는 라이브러리를 이용하는 것이    
    거의 지배적이다.     
    
  * 다른 방법은 없는가?      
    * Jsoup 외에도 여러 크롤러가 있지만    
      Jsoup이 오픈소스로써 마지막 commit이     
      두 달 전이고, 많은 최신 예제가 Jsoup으로    
      작성되어져 있는 만큼 처음 접근하기    
      쉬울 거 같다.       

* 잘 정리 된 글이 있어 한 번 더 절차를 정리하자면     
  1. 크롤링 대상 선정     
      * 웹상의 데이터는 고유한 ID를 가진다(URL)    
        즉 어떤 URL을 크롤링 할 것인지 정한다.    
  
  2. 데이터 로드      
      * 데이터 로드는 웹사이트를 켜는 것과 같다.     
        만약 API라면 xml, json일 것이고,     
        웹 페이지라면 HTML문서를 다운받는 것이다.       
       
  3. 데이터 분석     
      * 로드 된 데이터에서 어떤 부분을 수집할 지    
        수집하지 않을지를 선정하는 과정이다.    
        불필요한 데이터가 많기 때문에     
        이 과정에서 데이터를 선별한다.    
        
  4. 데이터 수집    
      * 분석을 통해 수집할 데이터를 선별 했다면     
        이를 추출하여 파일, DB에 저장한다.     
***
정리     

* 크롤링이란 웹상의 데이터를 수집, 추출, 가공하는   
  행위를 말한다. 물론 세부적으로 나눌 수 있지만    
  크롤링 한다라는 말로 어느정도 통용되는 듯 하여   
  궂이 의미는 없을 거 같다.     
  
* 웹상의 데이터는 HTML, Json, Xml등이 있고,     
  서버에 요청을 해 데이터를 받아 온다면   
  Json, Xml 형태가 될 것이고,     
  웹페이지에서 데이터를 가져온다면    
  HTML문서 형태로 가져오게 될 것이다.      
  
* 내 목적은 웹 상에 있는 url과 데이터를      
  가져 오는 것이기 때문에 일단 API를 이용한    
  서버 요청은 논외로 진행하겠다.       
  
* 그럼 웹 상의 데이터인 HTML을 가져와        
  수집, 추출, 가공을 한다는 것인데 어떻게??      
  
* 일단 Jsoup이라는 자바 라이브러리를 사용해    
  웹 상에서 데이터를 가져와, 가공까지 진행할   
  것이고, 이후에는 JPA와 MariaDB를 이용해    
  DB에 저장할 것이다.      
  
* 필수로 알아야 할 것은 Jsoup 라이브러리와     
  웹 DOM의 구조, HTML을 알고 이를 분석해서    
  내가 원하는 정보만 가져올 수 있어야 한다.     
***
* 학습해야할 것     
  * Jsoup 라이브러리에 대한 기본적인 이해        
    * [jsoup 공부](https://github.com/growinghsb/til_blog/blob/main/March/30-2.md)        
  
  * Jsoup 라이브러리를 사용한 예제 코드 분석     
  
  * HTML 돔 구조와 Jsoup 라이브러리를 이용해   
    원하는 데이터를 가져오는 방법