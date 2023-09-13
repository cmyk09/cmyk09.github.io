---
title: Java
keywords: java
sidebar: hobby_sidebar
toc: false
permalink: java.html
folder: hobby
---
```java
private List<GoodImage> addGoodsImgFile(MultipartFile mfile1, MultipartFile mfile2, MultipartFile mfile3,
			MultipartFile mfile4, MultipartFile mfile5, MultipartFile mfile6, HttpServletRequest request) 
	{
		ServletContext context = request.getServletContext();
	    String savePath = context.getRealPath("/images/goods");
	    
	    List<MultipartFile> mfiles = new ArrayList<>();
	    mfiles.add(mfile1) ;
	    mfiles.add(mfile2) ;
	    mfiles.add(mfile3) ;
	    mfiles.add(mfile4) ;
	    mfiles.add(mfile5) ;	    
	    mfiles.add(mfile6) ;
	    List<GoodImage> list = null;
	    try
	    {	 
	    	
	    	for (int i=0; i<mfiles.size(); i++)
	    	{
	    		if(mfiles.get(i).getSize()==0) continue;

	            if (list == null) list = new ArrayList<>();
	            
	            String OriginalFilename = mfiles.get(i).getOriginalFilename();
	            
	            System.out.println("순번 "+i+ " 파일이름 : "+OriginalFilename);
	            
	            UUID  uuid = UUID.randomUUID();
		        String uuidStr = uuid.toString();
		         
	            String[] token = OriginalFilename.split("\\.");
		        String f1 = token[0];
		        String f2 = token[1];
		         
		         String FakeFileName = f1 + "_" + uuidStr + "." + f2;
		         		         
	            mfiles.get(i).transferTo(new File(savePath+"/"+FakeFileName));

	            //MultipartFile 주요 메소드
	            String cType = mfiles.get(i).getContentType();         
	            Resource res = mfiles.get(i).getResource();
	            long fSize = mfiles.get(i).getSize();
	            boolean empty = mfiles.get(i).isEmpty();
	            GoodImage img = new GoodImage();
	            img.setImgFileName(OriginalFilename);
	            img.setImgFakeFileName(FakeFileName); 
	            img.setImgSize(fSize);
	            img.setImgType(cType);
	            
	            System.out.println("files : " + img.toString());
	            list.add(img);
	    	}

	    }catch(Exception e)
	    {
	         e.printStackTrace();	    	
	    }
		return list;
	}
```