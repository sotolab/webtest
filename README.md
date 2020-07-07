# webtest
webtest


index.html

    <!Doctype html>
    <html>
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script type="text/javascript" src="./js/coinstack-1.1.19.min.js"></script>
    <script src="http://code.jquery.com/jquery-latest.js "></script>

    <!-- bootsrtap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">

    <title>Blocko Coinstack</title>

    <script>
    $(document).ready(function(){
      let DEBUG = 1;
      let client = new CoinStack('c7dbfacbdf1510889b38c01b8440b1', '10e88e9904f29c98356fd2d12b26de', 'c3sp2.blocko.io', 'https');

      if (DEBUG) {
      console.log("client: ", client);
        console.log('starting...');
      }

      $('#sendCoin').on('click', function(){

      let fromaddress = $("#fromaddress").val();
      let toaddress = $("#toaddress").val();
      let coin = $("#coin").val();
      let privateKey = $("#mypassword").val();

         let txBuilder = client.createTransactionBuilder();
         txBuilder.addOutput(toaddress, CoinStack.Math.toSatoshi(coin));
         txBuilder.setInput(fromaddress);
         txBuilder.buildTransaction(function (err, tx) {
         try {

          tx.sign(privateKey);

           let rawTx = tx.serialize();
           client.sendTransaction(rawTx, function (err) {
       if (!err) {
           console.log("definition: ", tx.getHash());
           alert("거래가 완료 되었습니다..!!!");
       }
           });
        } catch (e) {
           console.log(e)

          } //end of try

        }) // end of txbuilder

      })

      $('#getBalance').on('click', function(){

        let fromaddress = $("#fromaddress").val();

      client.getBalance(fromaddress, function (err, balance) {
          if (!err) {
          var total = CoinStack.Math.toBitcoin(balance);
          $('#message').text(" total: " + total);
          console.log("address: ", fromaddress);
          console.log('total: ',total);
        }
         });

      })

      $('#newAccount').on('click', function(){
        let privateKey = CoinStack.ECKey.createKey();
        if (DEBUG) console.log("privateKey : ", privateKey);

        let account = CoinStack.ECKey.deriveAddress(privateKey);
        if (DEBUG) console.log("account: ", account);

      $('#fromaddress').val(account);

      $('#message').text(" account: " + account);
      alert("Account 생성이 완료 되었습니다..!!!" + account );
      // $('#fromaddress').val(privateKey);
      // <input id="myField" type="text" name="email"/>
            // getting the value
            // let fromaddress = $("#fromaddress").val();
            // setting the value
            // $("#fromaddress").val( "new value here" );
      // <h5>송신처  <input id="fromaddress" size="45" placeholder=""></input> </h5>
      })
    })
    </script>
    </head>

    <body>
      <br>
      <br>
      <div class="container " role="main">
        <h2><strong> Welcome to 나의 전자 지갑 </strong></h2>
        <div id="tablePlace"></div>
      <button id="newAccount">New Account</button>
      <button id="getBalance">Get Balance</button>
      <button id="sendCoin">Send Coin</button>
      <h5>송신처  <input id="fromaddress" size="45" value="1DLoBkiHmnuMuQ7zdZ9bQC9hdE43wG8FqC" placeholder=""></input> </h5>
      <h5>수신처  <input id="toaddress" size="45" placeholder=""></input> </h5>
      <h5>코인  <input id="coin" size="45" placeholder=""></input> </h5>
      <h5>비밀번호  <input id="mypassword" type="password" size="45" placeholder=""></input> </h5>
      </div>
      <br>
      <br>
      <div class="container " role="main">
      <h4><strong> Message </strong></h4>
        <div id="message"></div>
      </div>
    <hr>
    <footer class="py-5 bg-dark">
      <div class="container">
        <p class="m-0 text-center text-white"><b>Copyright &copy; Blocko 2020</b></p>
      </div>
    </footer>
    </body>
    </html>
