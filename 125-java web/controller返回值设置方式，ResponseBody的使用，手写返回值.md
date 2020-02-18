# controller返回值设置方式，ResponseBody的使用，手写返回值

```java
	//方法返回值设置为对象Object
	//responsebody将对象使用jackson转换为json
	//同时设置contentType为application/json;charset=UTF-8,固定格式
	@RequestMapping("addProject")
	@ResponseBody
	public Object addProject(DrawProject object,HttpSession hs) {
		AjaxResult msg = new AjaxResult();
		try {
		    //其他代码
			msg.setSuccess(true);
			msg.setData(ResultEnum.ADD_DRAW_PROJECT_SUCCESS.getMessage());
		} catch (Exception e) {
			msg.setSuccess(false);
			msg.setData(e.toString());
		}
		return msg;
	}




//方法返回值设置为对象String
	//手工将返回对象转换为json字符串
	//同时手工设置contentType为text/html; charset=utf-8,防止中文出现乱码
	//适用extjs 3.2的文件上传，因为
	//If the server is using JSON to send the return object, 
	//then the Content-Type header must be set to "text/html" 
	//in order to tell the browser to insert the text unchanged into the document body.
	@RequestMapping(value="addDraw",produces="text/html; charset=utf-8")
	@ResponseBody
	public String addDraw(DrawDrawing object,@RequestParam("uploadFile") MultipartFile uploadFile,HttpServletRequest req) throws JsonProcessingException {
		AjaxResult msg = new AjaxResult();
		ObjectMapper mapper = new ObjectMapper();  
		try {
		    //其他的代码
			msg.setSuccess(true);
			msg.setData(ResultEnum.ADD_DRAW_DRAWING_SUCCESS.getMessage());
		} catch (Exception e) {
			msg.setSuccess(false);
			msg.setData(e.toString());
		}
		return mapper.writeValueAsString(msg);
	}


//纯手工设置，返回值为void不跳转
    //使用HttpServletResponse 来输出内容并设置编码
    //手工转换对象为json
    //手工输出数据
    @RequestMapping("addDraw1")
	public void addDraw1(DrawDrawing object,@RequestParam("uploadFile") MultipartFile uploadFile,HttpServletResponse res,HttpServletRequest req) {
		AjaxResult msg = new AjaxResult();
		PrintWriter out = null;
		res.setCharacterEncoding("UTF-8");
		res.setContentType("text/html; charset=utf-8");
		
		ObjectMapper mapper = new ObjectMapper();  
		try {
			out = res.getWriter();
		} catch (IOException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
		try {
		    //其他的一些代码
			msg.setSuccess(true);
			msg.setData(ResultEnum.ADD_DRAW_DRAWING_SUCCESS.getMessage());
		} catch (Exception e) {
			msg.setSuccess(false);
			msg.setData(e.toString());
		} finally {
			
			try {
				out.print(mapper.writeValueAsString(msg));
			} catch (JsonProcessingException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			out.flush();
			out.close();
		}
	}
```

