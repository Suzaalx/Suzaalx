
![animesher com_manga-black-and-white-gif-813453](https://user-images.githubusercontent.com/122628569/232551881-68585433-6383-43d5-a439-8e0bb19b159c.gif)



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Creative art</title>
  <style>
    *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
canvas {
    border: 10px solid black;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
#patternImage{
    display: none;
}
  </style>
</head>
<body>
    <canvas id="canvas1"></canvas>
    <img id="patternImage" src="patternImage.png">
    <script>
    const canvas = document.getElementById('canvas1');
const ctx= canvas.getContext('2d');
canvas.height=700;
canvas.width=600;
canvas.style.width = '600px';
canvas.style.height= '700px';
//global settings
ctx.lineWidth =10;
//ctx.strokeStyle ="magneta";

//canvas shadow
ctx.ShadowOffsetX =2;
ctx.ShadowOffsetY=2;
ctx,shadowColor='white';

//gradinets
const gradient1= ctx.createLinearGradient(0,0,canvas.width,canvas.height);
gradient1.addColorStop('0.2','black')
gradient1.addColorStop('0.3','red')
gradient1.addColorStop('0.4','black')
gradient1.addColorStop('0.5','red')
gradient1.addColorStop('0.6','black')
gradient1.addColorStop('0.7','red')
gradient1.addColorStop('0.8','black')

const gradient2 = ctx.createRadialGradient(canvas.width * 0.5,canvas.height*0.5,30,canvas.width*0.5,canvas.height*0.5,300);
gradient2.addColorStop('0.1','black');
gradient2.addColorStop('0.3','red');
gradient2.addColorStop('0.9','black');

//canvas pattern
const patternImage= document.getElementById('patternImage');
const pattern1= ctx.createPattern(patternImage,'no-repeat');


ctx.strokeStyle =gradient2;


class line{
    constructor(){
        this.x=Math.random() * canvas.width;
        this.y=Math.random() * canvas.height;
        this.history= [{x: this.x, y: this.y }];
        this.lineWidth=Math.floor(Math.random() * 15 +1);
        this.hue= Math.floor(Math.random()* 365)
        this.maxLength=Math.floor(Math.random()* 150 +10);
        this.speedX=Math.random()*1-0.5;
        this.speedY=3;
        this.lifeSpan=this.maxLength *2;
        this.timer=0;
        
    }
    draw(){
        //ctx.strokeStyle = `hsl(${this.hue},100%,50%)`;

        ctx.lineWidth=this.lineWidth;
        ctx.beginPath();
        ctx.moveTo(this.history[0],this.history[1])

        for (let i=0;i<this.history.length;i++){
            ctx.lineTo(this.history[i].x, this.history[i].y)
        }
        
        ctx.stroke();
    }

update(){
    this.timer++;
    if (this.timer < this.lifeSpan){
// change the below numbers to generate diff types of lines
        this.x+=this.speedX + Math.random()*20 -10;
        this.y+=this.speedY + Math.random()*20 -10;
        // this.x=Math.random() * canvas.width;
        // this.y=Math.random() * canvas.height;
        this.history.push({x: this.x, y: this.y });
        if (this.history.length>this.maxLength){
            this.history.shift();
        }
    }
    else if (this.history.length<=1){
            this.reset()
    }else{
        this.history.shift()
    }
    }
    reset(){
        this.x=Math.random() * canvas.width;
        this.y=Math.random() * canvas.height;
        this.history= [{x: this.x, y: this.y }];
        this.timer=0;
    }
}



const linesArray=[];
// change this too
numberOfLines=150
for (let i=0;i<numberOfLines;i++){
    linesArray.push(new line());
}


function animate(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    linesArray.forEach(line=>{
        line.draw();
        line.update();
    });
    requestAnimationFrame(animate);
}
animate();
  </script>
</body>
</html>
