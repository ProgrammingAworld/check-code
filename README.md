# check-code
canvas实现验证码，不区分大小写

#HTML 部分
	
	<input type="text" id="myCheckCode" placeholder="请输入验证码">
	<canvas id="canvas" width="120" height="40"></canvas>
	<a href="#" id="changeImg">看不清，换一张</a>
	
# JavaScript 部分
	
	/**用于保存图片显示的验证码的字符**/
	var checkCode = '';
	
	/**生成一个随机数**/
	function randomNum(min,max){
	  return Math.floor( Math.random()*(max-min)+min);
	}
	/**生成一个随机色**/
	function randomColor(min,max){
	  var r = randomNum(min,max);
	  var g = randomNum(min,max);
	  var b = randomNum(min,max);
	  return "rgb("+r+","+g+","+b+")";
	}
	drawPic();
	document.getElementById("changeImg").onclick = function(e){
	  e.preventDefault();
	  drawPic();
	}
	
	/**绘制验证码图片**/
	function drawPic(){
	  var canvas=document.getElementById("canvas");
	  var width=canvas.width;
	  var height=canvas.height;
	  var ctx = canvas.getContext('2d');
	  ctx.textBaseline = 'bottom';
	
	  /**绘制背景色**/
	  ctx.fillStyle = randomColor(240,256); //颜色若太深可能导致看不清
	  ctx.fillRect(0,0,width,height);
	  /**绘制文字**/
	  var str = 'ABCEFGHJKLMNPQRSTWXYabcdefghigklmnopqrstuvwxyz123456789';
	
	  for(var i=0; i<4; i++){
	    var txt = str[randomNum(0,str.length)];
	    ctx.fillStyle = randomColor(50,160);  //随机生成字体颜色
	    ctx.font = randomNum(25,40)+'px SimHei'; //随机生成字体大小
	    var x = 10+i*25;
	    var y = randomNum(37,37);
	    var deg = randomNum(-30, 30);
	    //修改坐标原点和旋转角度
	    ctx.translate(x,y);
	    ctx.rotate(deg*Math.PI/180);
	    ctx.fillText(txt, 0,0);
	    //恢复坐标原点和旋转角度
	    ctx.rotate(-deg*Math.PI/180);
	    ctx.translate(-x,-y);
	    checkCode += txt;
	    // console.log(checkCode);
	  }
	  /**绘制干扰线**/
	  for(var i=0; i<8; i++){
	    // ctx.strokeStyle = randomColor(40,180);
	    // ctx.beginPath();
	    // ctx.moveTo( randomNum(0,width), randomNum(0,height) );
	    // ctx.lineTo( randomNum(0,width), randomNum(0,height) );
	    // ctx.stroke();
	  }
	  /**绘制干扰点**/
	  for(var i=0; i<100; i++){
	    ctx.fillStyle = randomColor(0,255);
	    ctx.beginPath();
	    ctx.arc(randomNum(0,width),randomNum(0,height), 1, 0, 2*Math.PI);
	    ctx.fill();
	  }
	}

	  /**用于验证输入的字符和验证码是否匹配**/
	  var myCheckCode = document.getElementById('myCheckCode');
	    myCheckCode.onchange = function (e){
	      console.log(e.target.value+" "+checkCode);
	      if(checkCode.toLowerCase() == e.target.value.toString().toLowerCase()){
	        console.log("成功");
	        checkCode= "";
	        drawPic();
	      }else{
	        console.log("失败");
	        checkCode= "";
	        drawPic();
	      }
	    }
