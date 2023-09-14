---
title: Java
keywords: java
sidebar: hobby_sidebar
toc: false
permalink: java.html
folder: hobby
---
```java
// MultipartFile 을 이용한 파일 업로드/다운로드
=========================
@Controller
@RequestMapping("/files")
public class FileController
{
   @Autowired
   ResourceLoader resourceLoader;

   @GetMapping("/upload")
   public String getForm()
   {
      return "upload/upload_form";
   }

   @PostMapping("/upload")
   @ResponseBody
   public String upload(@RequestParam("files")MultipartFile[] mfiles,
                     HttpServletRequest request,
                     @RequestParam("author") String author)
   {
      ServletContext context = request.getServletContext();
      String savePath = context.getRealPath("/WEB-INF/files");

      /* static/upload 디렉토리에 업로드하려면, 아래처럼 절대경로를 구하여 사용하면 된다
      * Resource resource = resourceLoader.getResource("classpath:/static");
      * String absolutePath = resource.getFile().getAbsolutePath();
      */

      try {
         for(int i=0;i<mfiles.length;i++) {
            if(mfiles[0].getSize()==0) continue;
            mfiles[i].transferTo(
              new File(savePath+"/"+mfiles[i].getOriginalFilename()));
            /* MultipartFile 주요 메소드
            String cType = mfiles[i].getContentType();
            String pName = mfiles[i].getName();
            Resource res = mfiles[i].getResource();
            long fSize = mfiles[i].getSize();
            boolean empty = mfiles[i].isEmpty();
            */
         }

         String msg = String.format("파일(%d)개 저장성공 (작성자:%s)", mfiles.length,author);
         return msg;
      } catch (Exception e) {
         e.printStackTrace();
         return "파일 저장 실패:";
      }
   }

   @GetMapping("/download/{filename}")
   public ResponseEntity<Resource> download(
                              HttpServletRequest request,
                              @PathVariable String filename)
   {
      Resource resource = resourceLoader.getResource("WEB-INF/files/"+filename);
      System.out.println("파일명:"+resource.getFilename());
        String contentType = null;
        try {
            contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath());
        } catch (IOException e) {
            e.printStackTrace();
        }
        if(contentType == null) {
            contentType = "application/octet-stream";
        }

        /* 파일명이 한글로 되어 있을 때 다운로드가 안되는 경우... */
        try {
         filename = new String(filename.getBytes("UTF-8"), "ISO-8859-1");
      } catch (UnsupportedEncodingException e) {
         e.printStackTrace();
      }

        return ResponseEntity.ok()
                .contentType(MediaType.parseMediaType(contentType))
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + filename + "\"")
                .body(resource);
   }
}

==========================================
controller

@RequestMapping("/ezen/goods")
@Controller
@SessionAttributes("uid")
public class GoodsController 
{
	@Autowired
	private GoodsMgtSVC SVC;
	
	@Autowired
	private GoodsVO goods;

	// 초기 goodsIndex Form 호출
	@GetMapping("/")
	public String index(Model model)
	{
		model.addAttribute("userid","smith");
		return "ezen/goods/goodsIndex";
	}
	
	// 상품추가 폼 호출
	@GetMapping("/addGoodsForm/{sellernum}")
	public String addGoodsForm(Model model, @PathVariable int sellernum)
	{
		List<Map> list1 = SVC.getGoodsCategoryList1();
		
		System.out.println("userid" + model.getAttribute("userid"));
		model.addAttribute("list1", list1);
		model.addAttribute("sellernum", sellernum);
		System.out.print("리스트: "+ list1);
		return "ezen/goods/addGoodsForm";
	}
	
	// 상품추가 품의 카테고리 선택시(2차 카테고리)
	@PostMapping("/scd")
	@ResponseBody
	public Map selectScd(@RequestParam("key") String code)
	{		
		System.out.println("1차 code : "+code);
		List<Map> list2 = SVC.getGoodsCategoryList2(code); 	
		
		System.out.println("2차 리스트 : "+list2);
		
		Map map = new HashMap<>();
		map.put("selected", true);
		map.put("list2", list2);
		return map;
	}
	
	// 상품추가 품의 카테고리 선택시(3차 카테고리)
	@PostMapping("/thd")
	@ResponseBody
	public Map selectThd(@RequestParam("key") String code)
	{		
		System.out.println("3차 code : "+code);
		List<Map> list3 = SVC.getGoodsCategoryList3(code); 	
		Map map = new HashMap<>();
		map.put("selected", true);
		map.put("list3", list3);
		return map;
	}
	
	// 상품추가 (상품추가 폼의 상품추가 버튼 클릭 시)
	@PostMapping("/addGoods")
	@ResponseBody
	public Map<String, Boolean> addBoard(Model model,
			GoodsVO goods,
			@RequestParam( value ="files1", required=false) MultipartFile mfile1,
			@RequestParam( value ="files2", required=false) MultipartFile mfile2,
			@RequestParam( value ="files3", required=false) MultipartFile mfile3,
			@RequestParam( value ="files4", required=false) MultipartFile mfile4,
			@RequestParam( value ="files5", required=false) MultipartFile mfile5,
			@RequestParam( value ="files6", required=false) MultipartFile mfile6,
			HttpServletRequest request)
	{
		System.out.println("상품추가 시작 name : " + goods.toString());
		System.out.println("상품추가 시작 fname 1: " + mfile1.getOriginalFilename());
		System.out.println("상품추가 시작 fname 2: " + mfile2.getOriginalFilename());
		System.out.println("상품추가 시작 fname 3: " + mfile3.getOriginalFilename());
		System.out.println("상품추가 시작 fname 4: " + mfile4.getOriginalFilename());
		System.out.println("상품추가 시작 fname 5: " + mfile5.getOriginalFilename());
		System.out.println("상품추가 시작 fname 6: " + mfile6.getOriginalFilename());
		
		boolean added = SVC.addGoods(goods, mfile1, mfile2, mfile3, mfile4, mfile5, mfile6, request);
		
		Map<String, Boolean> map = new HashMap<>();
		map.put("added", added);
		return map;
	}
	
	// 상품상세 폼 호출
	@GetMapping("/detailGoodsForm/{goodsno}")
	public String detailGoodsForm(Model model, @PathVariable int goodsno)
	{
		//System.out.println("userid" + model.getAttribute("userid"));
		List<Map> list = SVC.getGoodsCategoryListAll();		
		model.addAttribute("list", list);
		System.out.println("Ctl_detailGoodsForm" +list);
		
		Map map = SVC.detailGoodsForm(goodsno);
		
		System.out.println(map);
		model.addAttribute("map", map);
		return "ezen/goods/detailGoodsForm";
	}
	

	// 상품상세 품의 카트에 담기 버튼 클릭시
	@PostMapping("/putInCart")
	@ResponseBody
	public Map putInCart(CartVO cart)
	{	
		boolean putInCart = false;
		System.out.println("cart : "+cart);
		
		putInCart = SVC.putInCart(cart);
		
		Map map = new HashMap<>();
		map.put("putInCart", putInCart);
		return map;
	}
	
	// 상품 업데이트 폼 호출
	@GetMapping("/updateGoodsForm/{goodsno}/{sellerno}")
	public String updateGoodsForm(Model model, @PathVariable("goodsno") int goodsno , @PathVariable("sellerno") int sellernum)
	{
		Map map = SVC.updateGoodsForm(goodsno);
		List<Map> list1 = SVC.getGoodsCategoryList1();	
			
		System.out.println("업데이트 폼 로드 : "+ map);
		model.addAttribute("map", map);
		model.addAttribute("list1", list1);
		model.addAttribute("sellernum", sellernum);
		return "ezen/goods/updateGoodsForm";
	}
	
	// 상품 업데이트 폼의 업데이트 버튼 클릭시
	@PostMapping("/updateGoods")
	@ResponseBody
	public Map<String, Boolean> updateGoods(Model model,
			GoodsVO goods, GoodsImgFileName imgFileName,
			@RequestParam( value ="files1", required=false) MultipartFile mfile1,
			@RequestParam( value ="files2", required=false) MultipartFile mfile2,
			@RequestParam( value ="files3", required=false) MultipartFile mfile3,
			@RequestParam( value ="files4", required=false) MultipartFile mfile4,
			@RequestParam( value ="files5", required=false) MultipartFile mfile5,
			@RequestParam( value ="files6", required=false) MultipartFile mfile6,
			HttpServletRequest request)
	{		
		boolean updateed = SVC.updateGoods(goods, mfile1, mfile2, mfile3, mfile4, mfile5, mfile6, 
				imgFileName, request);
		
		Map<String, Boolean> map = new HashMap<>();
		map.put("updateed", updateed);
		return map;
	}
	 
	// 상품 업데이트 폼의 파일삭제 버튼 클릭시
	@PostMapping("/deleteGoodsImg")
	@ResponseBody
	public Map deleteGoodsImg(Model model,
			@RequestParam("goodsno") int goodsno,
			@RequestParam("fileno") int fileno,
			@RequestParam("name") String name,
			@RequestParam("fakeName") String fakeName,
			HttpServletRequest request)
	{		
		System.out.println("Ctl_deleteGoodsImg_변수 : " + goodsno +" / "+ fileno +" / "+ name +" / "+ fakeName );
		boolean deleteFile = SVC.deleteFile(goodsno, fileno, name, fakeName,  request);
		
		Map map = new HashMap<>();
		map.put("deleteFile", deleteFile);
		map.put("goodsno", goodsno);
		return map;
	}
	
	// 카트 폼 호출
	@GetMapping("/cartList/{mnum}")
	public String cartList(Model model, @PathVariable int mnum)
	{
		int userno = mnum;
		List<Map> cart = SVC.getCartList(userno);
		
		System.out.println("Ctl_cartList_cart" + cart);
		model.addAttribute("cart", cart);
		return "ezen/goods/cartList";
	}
	
	// 카트에서 구매하기 클릭
	@PostMapping("/purchase")
	@ResponseBody
	public Map purchase(Model model,@RequestParam("checked") int[] value, @SessionAttribute(value = "uid", required = false) Integer uid)
	{	
		
		int userno = uid;
		boolean purchase = SVC.purchase(userno, value);
		
		Map map = new HashMap<>();
		map.put("purchase", purchase);
		return map;
	}
	
	// 오더 폼 호출
	@GetMapping("/orderList/{mnum}")
	public String orderListMember(Model model, @PathVariable int mnum)
	{
		int userno = mnum;
		List<Map> orderList = SVC.getOrderListMember(userno);
		
		System.out.println("Ctl_orderListMember_order" + orderList);
		model.addAttribute("orderList", orderList);
		return "ezen/goods/orderList";
	}
	
	
	// 메인 폼 호출(jsp)
	@GetMapping("/main")
	public String viewMain2(Model model)
	{
		List<Map> list = SVC.getGoodsCategoryListAll();
		List<Map> goods = SVC.getGoodsMain();
		
		System.out.println("Ctl_viewMain_list" +list);
		model.addAttribute("list", list);
		model.addAttribute("goods", goods);
		return "main";
	}
	
	// 서치 폼 호출
	@GetMapping("/search/{keyword}/{categoryLv}/{categoryCode}/{page}")
	public String searchList(Model model, @PathVariable("keyword") String keyword, @PathVariable("categoryLv") int lvl,
											@PathVariable("categoryCode") String code, @PathVariable("page") int page)
	{
		
		List<Map> list = SVC.getGoodsCategoryListAll();
		
		int pageNum = page;
		int pageSize = 8;
		
		PageInfo<Map> pageInfo = SVC.getGoodsSearch(pageNum, pageSize, keyword, lvl, code);
		
		model.addAttribute("list", list);
		model.addAttribute("pageInfo", pageInfo);
		model.addAttribute("keyword", keyword);
		model.addAttribute("lvl", lvl);
		model.addAttribute("code", code);
		return "search";
	}
	
	// 상품 리스트 폼 호출
	@GetMapping("/goodsList/{sellernum}")
	public String goodsList(Model model, @PathVariable int sellernum)
	{
		List<Map> goodsList = SVC.goodsList(sellernum);
		
		System.out.println("Ctl_goodsList_goods" +goodsList);
		model.addAttribute("goodsList", goodsList);
		return "ezen/goods/goodsList";
	}
	
	// 상품 리스트에서 판매중지 클릭
	@PostMapping("/endSale")
	@ResponseBody
	public Map endSale(Model model,@RequestParam("goodsno") int goodsno, 
			@SessionAttribute(value = "sellerno", required = false) Integer sellerno,
			@SessionAttribute(value = "uid", required = false) Integer uid)
	{	
		boolean endSale = SVC.endSale(goodsno);
		
		Map map = new HashMap<>();
		map.put("endSale", endSale);
		return map;
	}
	
	// 상품 리스트에서 판매 재개 클릭
	@PostMapping("/startSale")
	@ResponseBody
	public Map startSale(Model model,@RequestParam("goodsno") int goodsno, 
			@SessionAttribute(value = "sellerno", required = false) Integer sellerno,
			@SessionAttribute(value = "uid", required = false) Integer uid)
	{	
		
		boolean startSale = SVC.startSale(goodsno);
		
		Map map = new HashMap<>();
		map.put("startSale", startSale);
		return map;
	}
	
	// 상품 리스트에서 판매 재개 클릭
	@PostMapping("/changeSaleQty")
	@ResponseBody
	public Map changeSaleQty(Model model, @RequestParam("cartno") int cartno, @RequestParam("qty") int qty,
			@SessionAttribute(value = "sellerno", required = false) Integer sellerno,
			@SessionAttribute(value = "uid", required = false) Integer uid)
	{	
		System.out.println("Ctrl_changeSaleQty_변수 : " + cartno + " / "+qty);
		boolean changeSaleQty = SVC.changeSaleQty(cartno, qty);
		
		Map map = new HashMap<>();
		map.put("changeSaleQty", changeSaleQty);
		return map;
	}
	
	// 등록 상품 삭제 클릭
	@PostMapping("/deleteGoods")
	@ResponseBody
	public Map deleteGoods(Model model, @RequestParam("goodsno") int goodsno, @RequestParam("sellerno") int sno,
			@SessionAttribute(value = "sellerno", required = false) Integer sellerno,
			@SessionAttribute(value = "uid", required = false) Integer uid)
	{	
		System.out.println("Ctrl_changeSaleQty_변수 : " + goodsno + " / "+sno);
		boolean deleteGoods = SVC.deleteGoods(goodsno, sno);
		
		Map map = new HashMap<>();
		map.put("deleteGoods", deleteGoods);
		return map;
	}
	
	// 구매 확정 버튼
	@PostMapping("/purConfirm")
	@ResponseBody
	public Map<String, Object> purConfirm(@ModelAttribute OrderVO ovo)
	{
		boolean purConfirm = SVC.purConfirm(ovo);
        Map<String, Object> map = new HashMap<>();
        map.put("purConfirm", purConfirm);
        return map;
	}
}
------------------------------------------------
service

@Service
public class GoodsMgtSVC 
{
	
	@Autowired
	private GoodsMgtDAO DAO;
	
	// 1차 카테고리 리스트 호출용 쿼리 호출
	public List<Map> getGoodsCategoryList1() 
	{
		List<Map> map =DAO.getGoodsCategoryList1();
		return map;
	}

	// 2차 카테고리 리스트 호출용 쿼리 호출
	public List<Map> getGoodsCategoryList2(String code) 
	{
		List<Map> map =DAO.getGoodsCategoryList2(code);
		return map;
	}

	// 3차 카테고리 리스트 호출용 쿼리 호출
	public List<Map> getGoodsCategoryList3(String code) 
	{
		List<Map> map =DAO.getGoodsCategoryList3(code);
		return map;
	}

	// 상품 추가 루틴
	@Transactional
	public boolean addGoods(GoodsVO goods, MultipartFile mfile1, MultipartFile mfile2, MultipartFile mfile3,
			MultipartFile mfile4, MultipartFile mfile5, MultipartFile mfile6, HttpServletRequest request) 
	{
		boolean added = false;
		// 1.Img 저장
		List<GoodImage> imgList = addGoodsImgFile(mfile1, mfile2, mfile3, mfile4, mfile5, mfile6, request);
		
		// 2.DB 저장
		// 2-0. goodsVO 빈값 채우기
		goods.setGoodsStatus("I");   // 상품이 등록만 된 상태임을 나타냄
				
	 	Date d = new Date();
	 	long dt = d.getTime();
		java.sql.Date inDate = new java.sql.Date(dt);
		
		goods.setGoodsRegistryDay(inDate);
		
		System.out.println("SVC_addGoods_ellerCode : " + goods.getSellerCode());
		
		// 2-1. goods 테이블에 저장
		int r1 = DAO.addGoods(goods);
		
		// 2-2. goods_img_list 테이블에 저장
		Map imgMap = new HashMap<>();
		
		for (int i=0 ; i<imgList.size(); i++)
		{
			String imgName = imgList.get(i).getImgFileName();
			String imgFakeName = imgList.get(i).getImgFakeFileName();
			
			if (i == 0)
			{
				imgMap.put("Goodsno", goods.getGoodsno());
				imgMap.put("imgName1", imgName);
				imgMap.put("imgFakeName1", imgFakeName);
			}else if (i == imgList.size()-1)
			{
				imgMap.put("imgName6", imgName);
				imgMap.put("imgFakeName6", imgFakeName);
			}else
			{
				String name = "imgName" +(i+1);
				String fakeName = "imgFakeName" +(i+1);
				
				System.out.println("SVC_addGoods_name : " + name);
				System.out.println("SVC_addGoods_fakeName : " + fakeName);
				
				imgMap.put(name, imgName);
				imgMap.put(fakeName, imgFakeName);
			}
			
		}
		int r2 = DAO.saveGoodsImgFile(imgMap);
		
		System.out.println("SVC_addGoods_r 값 - r1 : "+r1 + " / r2 : "+r2);
		
		if ((r1 + r2) > 1) added = true;
		return added;
	}

	// 이미지 파일 저장용 루틴
	private List<GoodImage> addGoodsImgFile(MultipartFile mfile1, MultipartFile mfile2, MultipartFile mfile3,
			MultipartFile mfile4, MultipartFile mfile5, MultipartFile mfile6, HttpServletRequest request) 
	{   
	    List<MultipartFile> mfiles = new ArrayList<>();
	    mfiles.add(mfile1) ;
	    mfiles.add(mfile2) ;
	    mfiles.add(mfile3) ;
	    mfiles.add(mfile4) ;
	    mfiles.add(mfile5) ;	    
	    mfiles.add(mfile6) ;
	    
	    List<GoodImage> list = null;
	    	
	    	for (int i=0; i<mfiles.size(); i++)
	    	{
	    		if(mfiles.get(i).getSize()==0) continue;

	            if (list == null) list = new ArrayList<>();
	            
	            GoodImage img = saveImgFile(mfiles.get(i), request);
  
	            System.out.println("files : " + img.toString());
	            list.add(img);
	    	}
		return list;
	}

	// 상품 상세 호출용 쿼리 호출
	public Map detailGoodsForm(int goodsno) 
	{
		Map map = DAO.detailGoodsForm(goodsno);
		return map;
	}

	// 상품 업데이트 호출용 쿼리 호출
	public Map updateGoodsForm(int goodsno) 
	{
		Map map = DAO.updateGoodsForm(goodsno);
		return map;
	}

	// 상품 업데이트 루틴
	public boolean updateGoods(GoodsVO goods, MultipartFile mfile1, MultipartFile mfile2, MultipartFile mfile3,
			MultipartFile mfile4, MultipartFile mfile5, MultipartFile mfile6, GoodsImgFileName imgFileName, HttpServletRequest request) 
	{
		System.out.println("SVC_updateGoods_goodVO : "+goods);
		System.out.println("SVC_updateGoods_GoodsImgFileName : "+imgFileName);
		boolean updated = false;
		// 1. Img 파일 저장
		//    : 이 경우 4가지로 분류 되는데
		//      1) mfile 객체가 null 인 경우와 아닌 경우 만약 null 이 아니면 해당 mfile를 파일로 저장
		//      2) mfile 객체가 null 이면서 nameOfImg 이 'X' 가 아닌 경우 이경우는 기존 파일이 있으므로 새로 파일을 저장하지 않음.
	    //      3) 여기서 Goods_img_list 테이블에 넣을 값도 만든다.(SQL 구문)
		
		String strSet = "";
		List<GoodImage> imgList = new ArrayList<>();
	    for (int i=0 ; i <= 5; i++)
	    {
	    	MultipartFile mfile = null;
	    	String nameOfImg = "";
	    	String fakeNameOfImg = "";
	    	switch (i)
	    	{
	    		case 0 : mfile = mfile1; nameOfImg=imgFileName.getNameOfImg1(); fakeNameOfImg=imgFileName.getFakeNameOfImg1();  break;
	    		case 1 : mfile = mfile2; nameOfImg=imgFileName.getNameOfImg2(); fakeNameOfImg=imgFileName.getFakeNameOfImg2();  break;
	    		case 2 : mfile = mfile3; nameOfImg=imgFileName.getNameOfImg3(); fakeNameOfImg=imgFileName.getFakeNameOfImg3();  break;
	    		case 3 : mfile = mfile4; nameOfImg=imgFileName.getNameOfImg4(); fakeNameOfImg=imgFileName.getFakeNameOfImg4();  break;
	    		case 4 : mfile = mfile5; nameOfImg=imgFileName.getNameOfImg5(); fakeNameOfImg=imgFileName.getFakeNameOfImg5();  break;
	    		case 5 : mfile = mfile6; nameOfImg=imgFileName.getNameOfImg6(); fakeNameOfImg=imgFileName.getFakeNameOfImg6();  break;
	    		default : 
	    	}
	    	
	    	System.out.println("SVC_updateGoods_순번"+i + " : "+mfile);
	    	System.out.println("SVC_updateGoods_순번"+i + " : "+nameOfImg);
	    	System.out.println("SVC_updateGoods_순번"+i + " : "+fakeNameOfImg);
	    	
	    	GoodImage img =repeatImgFileChk(mfile, nameOfImg, fakeNameOfImg, request);
	    	
	    	System.out.println("SVC_updateGoods_img"+i + " : "+img);
	    	imgList.add(img);
	    }
	    
	    System.out.println("SVC_updateGoods_imgList  : "+imgList);
	    System.out.println("SVC_updateGoods_imgList cnt : "+imgList.size());
	    
	    // 2. Goods DB 업데이트
	    int r1 = DAO.updateGoods(goods);
	    System.out.println("SVC_updateGoods_goods  : "+goods);
	    System.out.println("SVC_updateGoods_updateGoods  : "+r1);
	    
	    // 3. Goods_Img_list DB 업데이트
	    Map imgMap = new HashMap<>();
		
		for (int i=0 ; i<imgList.size(); i++)
		{			
			if (i == 0)
			{
				imgMap.put("Goodsno", goods.getGoodsno());
				imgMap.put("ImgFileName1", imgList.get(i).getImgFileName());
				imgMap.put("ImgFileFakeName1", imgList.get(i).getImgFakeFileName());
			}else if (i == imgList.size()-1)
			{
				imgMap.put("ImgFileName6", imgList.get(i).getImgFileName());
				imgMap.put("ImgFileFakeName6", imgList.get(i).getImgFakeFileName());
			}else
			{
				String name = "ImgFileName" +(i+1);
				String fakeName = "ImgFileFakeName" +(i+1);
				
				if (imgList.get(i) == null) 
				{
					System.out.println(i);
					System.out.println(name);
					System.out.println(fakeName);
					imgMap.put(name, "");
					imgMap.put(fakeName, "");
				}else
				{
					imgMap.put(name, imgList.get(i).getImgFileName());
					imgMap.put(fakeName, imgList.get(i).getImgFakeFileName());
				}
				
			}
		}
		
		System.out.println(imgMap);
		int r2 = DAO.updateGoodsImgFile(imgMap);
			
		System.out.println("SVC_updateGoods_r 값 - r1 : "+r1 + " / r2 : "+r2);
		
		if ((r1 + r2) > 1) updated = true;
		
		
		return updated;
	}
	
	// 상품 업데이트시 파일들 정보 가공을 위한 루틴
	public GoodImage repeatImgFileChk(MultipartFile mfile, String nameOfImg, String fakeNameOfImg, HttpServletRequest request)
	{
		if (mfile == null)
	    {
	    	if (nameOfImg == "X")
	    	{
	    		return null;
	    	}else
	    	{
	    		GoodImage img = new GoodImage();
	    		img.setImgFileName(nameOfImg);
	    		img.setImgFakeFileName(fakeNameOfImg);
	    		return img;
	    	}
	    }else
	    {
	    	String OriginalFilename = mfile.getOriginalFilename();
	    	if (OriginalFilename != "")
	    	{
	    		return saveImgFile(mfile, request);
	    	}else
	    	{
	    		return null;
	    	}
	    }	
		
	
		
	}
	
	// 이미지 파일 폴더에 생성용 루틴
	public GoodImage saveImgFile(MultipartFile mfile, HttpServletRequest request )
	{
		ServletContext context = request.getServletContext();
	    String savePath = context.getRealPath("/images/goods");
	    
		 UUID  uuid = UUID.randomUUID();
	     String uuidStr = uuid.toString();
	     
	     String OriginalFilename = mfile.getOriginalFilename();
	     
         String[] token = OriginalFilename.split("\\.");
	     String f1 = token[0];
	     String f2 = token[1];
	         
	     String FakeFileName = f1 + "_" + uuidStr + "." + f2;
	         		         
	     try {
			mfile.transferTo(new File(savePath+"/"+FakeFileName));
		} catch (IllegalStateException | IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

         //MultipartFile 주요 메소드
         String cType = mfile.getContentType();         
         Resource res = mfile.getResource();
         long fSize = mfile.getSize();
         boolean empty = mfile.isEmpty();
         GoodImage img = new GoodImage();
         img.setImgFileName(OriginalFilename);
         img.setImgFakeFileName(FakeFileName); 
         img.setImgSize(fSize);
         img.setImgType(cType);
         
		return img;
	}

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

	public boolean putInCart(CartVO cart) 
	{			
		Date d = new Date();
	 	long dt = d.getTime();
		java.sql.Date inDate = new java.sql.Date(dt);
		
		cart.setCartRegistryDay(inDate);
		
		int r = DAO.putInCart(cart); 
		if (r > 0) return true;
		return false;
	}

	public List<Map> getCartList(int userno) 
	{
		List<Map> cart = DAO.getCartList(userno);
		return cart;
	}

	@Transactional
	public boolean purchase(int userno, int[] value) 
	{
		System.out.println("SVC_purchase_userno : "+userno);
		// 1. orderNo 만들기
		int orderNo = DAO.getOrderNo();
		if (orderNo == 0) orderNo = 1;
		else orderNo += 1;
		
		System.out.println("SVC_purchase_orderNo : "+orderNo);
		
		// 2. 입력일 만들기
		Date d = new Date();
	 	long dt = d.getTime();
		java.sql.Date inDate = new java.sql.Date(dt);
		
		// 3. 상태 값 만들기
		String status = "payCompl";
		
		// 4. Cart 값 orderVO에 가져오기
		List<OrderVO> orderList = new ArrayList<>();
		List<CartVO> cartList = new ArrayList<>();
		for(int i = 0 ; i < value.length ; i++)
		{
			Map map = new HashMap<>();
			map.put("userno", userno);
			map.put("cartno", value[i]);
			OrderVO orderVO = DAO.getCartOrderList(map);
			orderVO.setOrderNo(orderNo);
			orderVO.setOrderRegistryDay(inDate);
			orderVO.setOrderStatus(status);
			
			CartVO cartVO = new CartVO();
			cartVO.setCartno(value[i]);
			
			System.out.println("SVC_purchase_orderVO : "+orderVO);
			
			orderList.add(orderVO);
			cartList.add(cartVO);
		}
				
		// 5. order Table 에 저장
		boolean saveChk = false;
		int r1 = DAO.addOrder(orderList);
		
		System.out.println("SVC_purchase_r1 : "+r1);
		
		if (value.length == r1) saveChk = true;
		
		
		// 6. cart Table 삭제
		boolean deleteChk = false;
		int r2 = 0;
		for(int i = 0 ; i < cartList.size(); i++)
		{
			int r = DAO.deletedCart(cartList.get(i));
			r2 += r;
		}
		
		// 7. goods 테이블 재고수량 업데이트
		for (int i = 0 ; i < orderList.size(); i++)
		{
			System.out.println("SVC_purchase_orderList : "+orderList.get(i));
			
			Map map = new HashMap<>();
			map.put("goodsNo", orderList.get(i).getGoodsno());
			map.put("goodsSaleQty", orderList.get(i).getGoodsSaleQty());
			int r = DAO.updateGoodsQty(map);
		}
		
			
		System.out.println("SVC_purchase_r2 : "+r2);
		
		if (cartList.size() == r2) deleteChk = true;

		if (saveChk == true && deleteChk == true) return true;
		
		return false;
	}

	public List<Map> getOrderListMember(int userno) 
	{
		List<Map> orderList = DAO.getOrderListMember(userno);
		return orderList;
	}

	public List<Map> getGoodsCategoryListAll() 
	{
		List<Map> list = DAO.getGoodsCategoryListAll();
		return list;
	}

	public List<Map> getGoodsMain() 
	{
		List<Map> list = DAO.getGoodsMain();
		return list;
	}

	public PageInfo<Map> getGoodsSearch(int pageNum, int pageSize, String keyword, int lvl, String code) 
	{
		PageHelper.startPage(pageNum, pageSize);
		PageInfo<Map> pageInfo = null;
		
		if (keyword.equals("N")) 
		{
			String srch = "";
			if (lvl == 1) srch = code.substring(0, 2);
			else if (lvl == 2) srch = code.substring(0, 4);
			else srch = code;
			
			System.out.println("SVC_getGoodsSearch_srch : "+srch);
			
			Map map = new HashMap<>();
			map.put("lvl", lvl);
			map.put("code", srch);
			pageInfo = new PageInfo<>(DAO.getGoodsSearchCategory(map));			
		}
		else 
		{
			String key = "%"+keyword+"%";
			pageInfo = new PageInfo<>(DAO.getGoodsSearchKeyword(key));
		}
		
		return pageInfo;
	}

	public List<Map> goodsList(int sellernum) 
	{
		return DAO.goodsList(sellernum);
	}

	public boolean endSale(int goodsno) 
	{
		return DAO.endSale(goodsno);
	}

	public boolean startSale(int goodsno) 
	{
		return DAO.startSale( goodsno) ;
	}

	public boolean changeSaleQty(int cartno, int qty) 
	{
		boolean changeSaleQty = false;
		Map map = new HashMap<>();
		map.put("cartno", cartno);
		map.put("goodsSaleQty", qty);
		int r = DAO.changeSaleQty(map);
		if ( r > 0 ) changeSaleQty = true;
		return  changeSaleQty ;
	}

	public boolean deleteGoods(int goodsno, int sno) 
	{
		boolean deleteGoods = false;
		
		Date d = new Date();
		long dt = d.getTime();
		java.sql.Date delDate = new java.sql.Date(dt);
		
		Map map = new HashMap<>();
		map.put("goodsno", goodsno);
		map.put("goodsno", goodsno);
		map.put("deleteDay", delDate);
		
		int r = DAO.deleteGoods(map);
		if ( r > 0 ) deleteGoods = true;
		return  deleteGoods ;
	}

	public boolean purConfirm(OrderVO ovo) {
		return DAO.purConfirm(ovo);
	}

}
```