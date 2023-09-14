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
```