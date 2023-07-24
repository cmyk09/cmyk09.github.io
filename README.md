# 마크다운 간단 문법

# cmyk09.github.io
# 제목표시하기
# h1
## h2
### h3
#### h4

mark
====
down
----

# 수평선
---
***
___

# 목록(순서, 순서 없는 경우)
## 순번이 있는 목록(ol)
1. python
2. Javascript
3. Java

## 순번이 없는 목록(ul)
- 국어
- 영어
- 수학
* 음악
+ 체육

# 들여쓰기 표현
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa    

    bbbbbbbbbbbbbbbbbbbbbbbbbbbbb        
    ccccccccccccccccccccccccccccc    

# 코드 블럭
```java
public boolean deleteFile(int goodsno, int fileno, String name, String fakeName, HttpServletRequest request) 
	{
		boolean deleted = false;
		// 1. DB 업데이트 (goods_img_list 테이블의 해당 컬럼의 파일 명을 삭제-업데이트 한다.)
		Map imgMap = new HashMap<>();
		imgMap.put("goodsno", goodsno);
		imgMap.put("fileno", fileno);
		
		System.out.println("SVC_deleteFile_변수 : goodsno - "+imgMap.get("goodsno") + " / fileno - "+imgMap.get("fileno"));
		
		int r1 = DAO.deleteGoodsImgFile(imgMap);
		
		// 2. 파일 삭제
		ServletContext context = request.getServletContext();
	    String savePath = context.getRealPath("/images/goods");
		String fname = fakeName;
		File delFile = new File(savePath, fname);
		boolean delF = delFile.delete();	
		
		if (r1 > 0 && delF == true) deleted = true;
		return deleted;
	}
```

# 링크
[구글바로가기](http://google.com)

## 아이디를 사용하여 보여지는 문자열 표시하기
[구글 바로 가기][google]
[google]: http://google.com "Google 사이트로 이동합니다."

# 문서 내부 참조
[문서의 처음으로 이동](#마크다운-간단-문법)

# 강조
Hello *World*    
_Hello_ World    
Hello **World**    
__Hello__ World    
~~Hello~~ World    

# 이미지(img 태그를 사용시 파일을 리파지토리에 올리고 적용한다.)
<img src="xxx.jpg" alt="배경사진" title="바다풍경">

# 인용문(BlockQuote)
아래는 인용문 입니다.
>첫번째 인용문
>>인용문 안의 인용문
