# wonae
<html>
    <head>
    <title>이원태의 홈페이지</title>
    <meta name="viewport" content="width=device-width, initial-scale=3.0">
    </head>
<body>
    <h1>이원태의 홈페이지</h1>
    <a href="https://segyo.hs.kr/">새교고</a><br>
     <a href="chat0.html">이원태 식당</a><br>
    <iframe width="560" height="315" src="https://www.youtube.com/embed/eojiSgzA3hU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
   
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
*{ margin: 0; padding: 0; }
.chat_wrap .header { font-size: 14px; padding: 15px 0; background: #ff8282; color: white; text-align: center;  }
.chat_wrap .chat { padding-bottom: 80px; background-color: #a7bdfc; }
.chat_wrap .chat ul { width: 100%; list-style: none; }
.chat_wrap .chat ul li { width: 100%; }
.chat_wrap .chat ul li.left { text-align: left; }
.chat_wrap .chat ul li.right { text-align: right; }
 
.chat_wrap .chat ul li > div { font-size: 14px;  }
.chat_wrap .chat ul li > div.dsender { margin: 5px 5px 5px 5px; font-weight: bold; }
.chat_wrap .chat ul li > span.sender {  margin: 10px 10px 10px 10px; font-weight: bold; }
.chat_wrap .chat ul li > div.dmessage { display: inline-block; word-break:break-all; margin: 5px 5px; max-width: 75%; border: 1px solid #888; padding: 5px; border-radius: 5px; background-color: #FCFCFC; color: #555; text-align: left; }
.chat_wrap .chat ul li > div.message {  margin: 10px 10px; max-width: 75%; border: 1px solid #888; padding: 10px; border-radius: 5px; background-color: #FCFCFC; color: #555; text-align: left; }
 
.chat_wrap .input-div { position: fixed; bottom: 0; width: 100%; text-align: center; border-top: 1px solid #F18C7E; }
input[type=text] { width: 70%; height: 28px; border: 1px solid #ff0000; padding: 10px 0; }
input[type=button] { height: 2em; }

.format { display: none; }
</style>
<script>
    const qanda = [
          ["안녕", "안녕하세요. 무었을 도와 드릴까요?"],
          ["메뉴", "우리 식당의 메뉴는 아이스아메리카노,카라멜마끼아또,스무디,아메리카노 등이 있습니다."],
          ["감사", "네, 감사합니다. 즐거운 시간 보내시기 바랍니다."],
    ];
    const menu = ["아이스아메리카노","카라멜마끼아또","스무디","아메리카노" ];
    var user_menu = [];
    var myName = "me";
    const hostName = "Host"

    // init 함수
    function init() {
        console.log("init");
        user_menu = [];

        // enter 키 이벤트
        document.querySelector(".userin").onkeydown = function(e){
            if(e.keyCode == 13 && !e.shiftKey) {
                e.preventDefault();
                const message = e.target.value;
                console.log(message.length);
                if ( message.length > 0 ) {
 
                  // 메시지 전송
                  checkMessage(message);
                  // 입력창 clear
                  clearTextarea();
                }
            }
        };
        initMessage();
    }
 
    // 메세지 태그 생성
    function createMessageTag(LR_className, hostName, message) {
        // 형식 가져오기
        let chatLi = document.querySelector('div.chat.format ul li').cloneNode(true);
 
        // 값 채우기
        chatLi.classList.add(LR_className);
        let now = new Date()
        var hours = ('0' + now.getHours()).slice(-2); 
        var minutes = ('0' + now.getMinutes()).slice(-2);
        var timeString = hours + ':' + minutes;
        if ( LR_className == "left" )
            chatLi.querySelector('.tsender').innerHTML = timeString;
        else
            chatLi.querySelector('.tmessage').innerHTML = timeString;
        chatLi.querySelector('.sender').innerHTML = hostName;
        chatLi.querySelector('.message').innerHTML = message;
 
        return chatLi;
    }
 
    // 메세지 태그 append
    function appendMessageTag(LR_className, hostName, message) {
        const chatLi = createMessageTag(LR_className, hostName, message);
        document.querySelector('div.chat:not(.format) ul').append(chatLi);
        // 스크롤
        chatLi.scrollIntoView({behavior: "smooth"})
    }
 
    // 메세지 전송
    function checkMessage(message) {
        if ( message.startsWith("주문") ) {
            order()
            return;
        }
        else if ( message.startsWith("다시") ) {
            init()
            return;
        }
        else if ( message.startsWith("?") ) {
            initMessage()
            return;
        }
        // 서버에 전송하는 코드로 후에 대체
        let data = {
            "hostName"    : myName,
            "message"     : message
        };
        showQueryMessage(data);
 
        // 통신하는 기능이 없으므로 여기서 receive한것처럼 처리
        data.hostName = "Host"
        showReplyMessage(data);
    }

    // 초기 메세지 표시
    function initMessage() {
        // 서버에 전송하는 코드로 후에 대체
        let message = [ "안녕하십니까? " + myName + "님",
                        "커피점에 오신것을 환영합니다.",
                        "우리 식당의 메뉴는 아이스아메리카노,카라멜마끼아또,스무디,아메리카노 등입니다.",
                        "원하시는 메뉴의 이름과 수량을 띄어서 입력하여 주세요. ",
                        "주문을 완료하시려면 주문 글자입력 주문하기 버튼을 눌러주세요",
                        "처음부터 다시 시작하려면 다시 글자입력 또는 다시하기 버튼을 눌러주세요",
                        "즐거운 시간 되시기 바랍니다."
        ];

        const LR = "left";
        for ( let i=0 ; i<message.length ; i++ ) 
            appendMessageTag(LR, hostName, message[i]);

        // set focus to input box
        document.querySelector('div.input-div .userin').focus()
    }
 
    // 메세지 입력박스 내용 지우기
    function clearTextarea() {
        //document.querySelector('div.input-div .userin').value = '';
        document.querySelector('.userin').value = '';
    }
 
    // 메세지 표시
    function showQueryMessage(data) {
        const LR = "right";
        appendMessageTag(LR, data.hostName, data.message);
        
    }

    // 입력 메세지 처리후 답변하기
    function showReplyMessage(data) {
        let found = -1;
        for ( let i=0 ; i<qanda.length ; i++ ) {
            if ( qanda[i][0].includes(data.message) ) {
                found = i;
                break
            }
        }
        if ( found != -1 ) {
            data.message = qanda[found][1];
        }
        else  {
            let fname = data.message.split(" ")
            for ( let i=0 ; i<menu.length ; i++ ) {
                if ( menu[i].includes(fname[0]) ) {
                    let fnum = Number(fname[1])
                    if ( isNaN(fnum) ) {
                        data.message = "메뉴의 갯수가 없습니다."
                    }
                    else  {
                        found = i;
                        user_menu.push([menu[i], fnum])
                        data.message = "지금까지의 주문내역은 " + menuToString(user_menu)
                    }
                    break
                }
            }
        }
        if ( found == -1 ) {
            data.message = "죄송합니다. 다시 말씀해주세요";
        }

        // 응답 메시지 표시
        const LR = "left";
        appendMessageTag(LR, data.hostName, data.message);
        
    }

    // 주문후 답변하기
    function order() {
        const LR = "left";
        if ( user_menu.length == 0 )  {
            appendMessageTag(LR, myName, "주문할 내역이 없습니다");
            return;
        }

        let data = {
            "hostName"    : myName,
            "message"        : myName + "님께서 " + menuToString(user_menu) + "을 주문하셨습니다."
        };
        appendMessageTag(LR, data.hostName, data.message);
        data.sender = hostName
        data.message = "감사합니다. 또 오세요~"
        appendMessageTag(LR, data.hostName, data.message);
    }

    // menu to string
    function menuToString(arr) {
        let narr = []
        for ( let i=0 ; i<arr.length ; i++ ) {
            narr.push(arr[i].join(":"));
        }
        return ("[" + narr.toString() + "]");
    }

    // 처음 시작하는 코드
    window.onload = function() {
        myName = prompt("반갑습니다. 성함이 어떻게 되시나요?", "me");
        init();
    }

</script>
</head>
<body>
<div class="chat_wrap">
    <div class="header">
        CHAT BOT
    </div>
    <div class="chat">
        <ul id="content">
            <!-- 동적 추가 생성 -->
        </ul>
    </div>
    <div class="input-div">
        <input type="button" value="다시하기" onclick="init()"><input type="text" class="userin"><input type="button" value="주문하기" onclick="order()">
    </div>
 
    <!-- format -->
    <div class="chat format">
        <ul>
            <li>
                <div class="dsender">
                    <label class="tmessage"></label>
                    <span class="sender"><span></span></span>
                    <label class="tsender"></label>
                </div>
                <div class="dmessage">
                    <div class="message"><span></span></div>
                </div>
            </li>
        </ul>
    </div>
</div>
</body>
</html>
