# 스프링 어노테이션 컨트롤러를 만들기 위해      
* 스프링의 뜻은 알고, 어노테이션 컨트롤러란 무엇인가?    
  * 어노테이션을 일단 알아 봐야겠다.    
    * 어노테이션이란?     
      1. 자바 어노테이션은 자바 JDK 1.5 버전 이상에서   
         사용 가능하다.    
         클래스 파일에 임베디드 되어, 컴파일러에 의해     
         생성된 후, JVM에 표함되어 작동한다.      
         
      2. 주석으로 활용되기도 하고, 클래스, 메서드에 붙어    
         특정한 기능을 수행하기도 한다.   
         
      3. 어노테이션의 기능은 메타 어노테이션인      
         @Retention의 속성값으로 알아볼 수    
         있을 거 같다.   
         @Retention 메타 어노테이션은    
         해당 어노테이션이 어떤 범위까지 영향을    
         끼치는지를 표시하는 어노테이션 위에 붙이는   
         메타 어노테이션이다.    
         속성으로는 Source, class, runtime이 있다.    
         
         Source는 가시적인 소스 상에서만 정보를 유지한다.   
         소스코드를 분석할 때만 의미가 있고, 바이트코드 파일에는    
         정보가 남지 않는다. 즉 주석으로써 의미를 가진다.    
         대표적으로 @Override가 있다.   
         
         Class는 바이트코드 파일 까지는 정보를 유지한다.    
         하지만 리플렉션을 이용해서 어노테이션 정보를 얻을 수는 없다.    
         즉 class 객체를 이용해서 어노테이션이 붙어 있는 메서드나   
         어노테이션을 얻어낼 수는 없는 듯하다.    
         
         Runtime은 바이트코드 파일까지 정보를 유지하며,   
         리플랙션을 이용해 어노테이션의 정보를 얻을 수 있다.    
         
      4. @Retention 어노테이션 말고도 어노테이션의 메타 정보를   
         지정할 수 있는 어노테이션이 있다.     
         @Target 어노테이션은 해당 어노테이션이 class인지, method인지     
         field인지 등등 허용 범위를 지정한다.    
         
      5. 어노테이션 내부에는 메서드의 시그니처를 정의 하고,    
         기본값을 지정할 수 있다.   
         이 메서드는 변수처럼 외부에서 값을 지정해줄 수 있고,   
         리플랙션을 이용해 어노테이션의 정보를 얻어와     
         세팅된 값을 얻어와 사용할 수 있다.        
         
* 스프링 어노테이션 컨트롤러란?    
  * 위같은 어노테이션을 이용해 컨트롤러를 정의하고,     
    요청을 처리할 수 있는 컨트롤러를 말한다.   
    
  * 일단 자유도가 높을 거 같다. 기존에 만든 컨트롤러들은    
    각각 인터페이스를 구현해 같은 도메인이라도 요청마다    
    컨트롤러를 만들어야 했다.   
    
  * 하지만 어노테이션이 붙어 있으면    
    그렇게 하지 않아도 될 듯 하다.     
    인터페이스를 이용하는 이유는 같은    
    도메인의 컨트롤러를 추상화를 통해     
    유연성과 확장성을 염두해 뒀지만   
    어노테이션을 이용하면 그냥 하나의   
    클래스에서 모든 도메인에 대한    
    요청을 처리할 수 있기 때문이다.    
    
  * 어댑터에서는 리플랙션을 통해 어노테이션의 정보를 읽어와    
    거기에 맞는 메서드를 실행시켜 값을 가져오면 되는 것이다.    
    
  * 그럼 컨트롤러 인터페이스도 필요없다.    
    그냥 컨트롤러 1개에, 어댑터 1개 해서    
    확장만 하면 된다.       
    
  * 일단 만들어 보자.      