<html>
<head>
<title>貪食蛇</title>
<script type="text/javascript">

window.onload=onPageLoaded();

var Block_size=20
var Block_count=20

var gameInterval
var snack
var apple
var score

function gameStart(){
   
   //蛇一開始的位置
   snack ={
     body:[
	   {x:Block_size/2,y:Block_count/2}
	 ],
	 size:3,
	 direction:{x:0,y:-1} //蛇頭向上
   }
   
   putApple()
   updateScore(0)
   gameInterval=setInterval(gameRoutine,100);
}

function gameRoutine(){
   moveSnack();
   
   if(snackIsDead()){
     ggler();
	 return
   }
   
   //吃掉蘋果
   if(snack.body[0].x ==apple.x &&
      snack.body[0].y ==apple.y){
	     eatApple()  
 	  }
   
   updateCanvas();
}
function moveSnack(){
   var newBlock={
      x:snack.body[0].x+ snack.direction.x,
	  y:snack.body[0].y+ snack.direction.y
    }
	snack.body.unshift(newBlock)
	
	while(snack.body.length>snack.size){
	  snack.body.pop();
	}
}


function updateCanvas(){
   var canvas=document.getElementById("canvas_id");
   var context = canvas.getContext("2d");  //Canvas Context Object
   
   context.fillStyle="black";
   context.fillRect(0,0,canvas.width,canvas.height);
   
   context.fillStyle="lime";
   for(var i=0;i<snack.body.length;i++){
     context.fillRect(
	    snack.body[i].x*Block_size+1,
		snack.body[i].y*Block_size+1,
		Block_size-1,
		Block_size-1
	 )
   }
   
   context.fillStyle="red";
   context.fillRect(
     apple.x*Block_size+1,
	 apple.y*Block_size+1,
	 Block_size-1,
	 Block_size-1
   );
}


function onPageLoaded(){
  document.addEventListener('keydown',handleKeyDown)
}

function handleKeyDown(event){
  var originX = snack.direction.x;
  var originY = snack.direction.y;

  if(event.keyCode==37){ // turn left
    snack.direction.x=originY;
	snack.direction.y=-originX;
  }else if(event.keyCode==39){ // turn right
        snack.direction.x=-originY;
	    snack.direction.y=originX;
   } 
}

function snackIsDead(){

   // hit walls
   if (snack.body[0].x <0 ){
     return true
   }else if (snack.body[0].x >=Block_count){
     return true
   }else if (snack.body[0].y <0 ){
     return true
   }else if (snack.body[0].y >=Block_count){
     return true
   }
   
   //hit body
   for(var i=1;i<snack.body.length;i++){
      if(snack.body[0].x == snack.body[i].x &&
	    snack.body[0].y == snack.body[i].y ){
		  return true
		}
   }
   return false
}

function ggler(){
  clearInterval(gameInterval)
  alert("遊戲結束，您的分數為："+ score);
}

function putApple(){
   apple = {
     x:Math.floor(Math.random()*Block_count),
	 y:Math.floor(Math.random()*Block_count)
   }
   
   for(var i =0;i<snack.body.length;i++){
     if(snack.body[i].x == apple.x &&
	    snack.body[i].y == apple.y){
		    putApple()
			break
	  }
   }
}

function eatApple(){
   snack.size +=1
   putApple()
   updateScore(score +1)
}

function updateScore(newScore){
   score = newScore;
   document.getElementById("score_id").innerHTML = score;
}
</script>
<style type="text/css">
</style>
</head>
<body>
<button onclick="gameStart();">開始遊戲</button>
<h1 id="score_id">0</h1>
<canvas id="canvas_id" width="400" height="400"></canvas>
</body>
</html>
