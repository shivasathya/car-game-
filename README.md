

<head>
    <title>JavaScript</title>
    <style>
        .hide {
            display: none;
        }

        .car, .enemy {
            position: absolute;
            bottom: 100px;
            margin: auto;
            width: 50px;
            height: 100px;
            background-color: white;
            background-image:url(hello.jpg) ;
            
        }

        .line {
            position: absolute;
            height: 100px;
            width: 10px;
            margin-left: 45%;
            background-color: white;
        }

        .gameArea {
            background-color: black;
            width: 400px;
            height: 100%;
            overflow: hidden;
            position: absolute;
            margin: auto;
            left: 37%;;
                /* background-color: black; */
                /* top: 0;
                left: 0; */
                /* width: 50vh;
                height: 100vh;  */
                /* overflow: hidden; */
                /* position: relative;
                margin: auto; */
        }
        .startScreen{
            text-align: center;
            margin-top: 300px;
            margin:auto;
            border: 1px solid red;
            color: white;
            background-color: blue;
            /* padding-left: 30px 30px 30px 30px; */
            cursor: pointer;
            width: 300px;
            /* height: 300px; */
            
            
        }
        .score{
            background-color: black;
            height: 70px;
            text-align: center;
            color: white;
            font-size: 1.5em;
            font-family: fantasy;
            
        }
        
    </style>
</head>

<body>
    <div class="score"></div>
    <div class="game" >
        <div class="startscreen">Press here to start<br>Arrow To move<br>If you hit a another car you will lose</div>
        <div class="gameArea "></div>
    </div>
    
    <script>
        const score = document.querySelector(".score");
        const startScreen = document.querySelector(".startScreen");
        const gameArea = document.querySelector(".gameArea");
        let player = {
            speed: 5,
            score:0
        };
        let keys = {
            ArrowUp: false
            , ArrowDown: false
            , ArrowRight: false
            , ArrowLeft: false,
            
        };
        startScreen.addEventListener("dblclick", start);
        window.addEventListener("keydown", pressOn);
        window.addEventListener("keyup", pressOff);

         function movelines()
         {
             let lines = document.querySelectorAll(".line");
             lines.forEach(function(item){

            //  console.log(item.y);
             if(item.y>1500)
             {
                 item.y-=1500;
             }
             item.y+=player.speed;
             item.style.top=item.y+"px";
                 
             });
            }
            function movenemy(car)
         {
             let ele = document.querySelectorAll(".enemy");
             ele.forEach(function(item){

             if(collide(car,item)){
                endgame();
             }
             if(item.y>1500)
             {
                 item.y=-600;
                 item.style.left=Math.floor(Math.random()*350)+"px";
             }
             item.y+=player.speed;
             item.style.top=item.y+"px";
                 
             });

             function collide(a,b){
                 arect=a.getBoundingClientRect();
                 brect=b.getBoundingClientRect();
                 return!(
                     (arect.bottom<brect.top)||
                     (arect.top>brect.bottom)||
                     (arect.right<brect.left)||
                     (arect.left>brect.right)
                 )
             }
            }


        function playGame() {
            let car = document.querySelector(".car");
            let road = gameArea.getBoundingClientRect();
            // console.log(road);
            movelines();
            movenemy(car);
            
            if (player.start) {
                if (keys.ArrowUp  &&  player.y >road.top - 537) {
                    player.y -= player.speed;
                }
               
                
                if (keys.ArrowDown && player.y < road.bottom-237) {
                    player.y += player.speed;
                }
                if (keys.ArrowLeft && player.x > 0) {
                    player.x -= player.speed;
                }
                if (keys.ArrowRight && player.x < (road.width - 54)) {
                    player.x += player.speed;
                }
                car.style.left = player.x + 'px';
                car.style.top = player.y + 'px ';
                window.requestAnimationFrame(playGame);
                 player.score++;
                 score.innerText="Score:" +player.score;
            }
        }

        function pressOn(e) {
            e.preventDefault();
            keys[e.key] = true;
            // console.log(keys);
        }

        function pressOff(e) {
            e.preventDefault();
            keys[e.key] = false;
            // console.log(keys);
        }

         function endgame(){
             player.start=false;
             score.innerHTML="Game Over<br> Score was "+player.score;
             startScreen.classList.remove("hide");
         }
        function start() {
            startScreen.classList.add("hide");
            // gameArea.classList.remove("hide");
            gameArea.innerHTML="";
            player.start = true;
            player.score=0
        
            for (let x = 0; x < 10; x++) {
                let div = document.createElement("div");
                div.classList.add("line");
                div.y=x*150;
                div.style.top = (x * 150) + "px";
                gameArea.appendChild(div);
            }
            window.requestAnimationFrame(playGame);
    
            
            
            let car = document.createElement("div");
            
            car.innerText = "BENZ";
            car.setAttribute("class", "car");
            gameArea.appendChild(car);
            player.x = car.offsetLeft;
            player.y = car.offsetTop;
            console.log(player);
        
            for (let x = 0; x < 3; x++) {
                let enemy = document.createElement("div");
                enemy.classList.add("enemy");
                enemy.y=((x+1)*600)* -1;
                enemy.style.left=Math.floor(Math.random()*350)+"px";
                enemy.style.top=enemy.y+"px"
                enemy.style.backgroundColor=colorcar();
                gameArea.appendChild(enemy);
            }}
           function colorcar(){
               function c(){
                     let hex = Math.floor(Math.random()*256).toString(16);
                     return("a"+String(hex)).substr(-2)
               }
               return "#"+c()+c()+c()
           }
        
    </script>
</body>

</html> 
