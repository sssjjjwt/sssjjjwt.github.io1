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
    <h1>sum 구하기</h1>
    <script>
        sum = 0
        //data = [1,2,3,4,5]
        data = prompt('input Numbers')
        data = data.split(',')
        data = data.map(Number)

        for ( i = 0 ; i < data.length ; i++) {
          sum = sum + data[i]
          document.write("i is " + i + " data is " + data[i] + "<br>")
        }
        

        document.write(" sum is " + sum + "<br>")
    </script>
</body>
</html>
