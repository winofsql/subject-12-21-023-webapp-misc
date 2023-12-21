# subject-231221

- ### [標準化の概要 - 経済産業省](https://www.meti.go.jp/policy/economy/hyojun-kijun/katsuyo/business-senryaku/pdf/001.pdf)
  ```
  標準化とは、⼀定のメンバーの合意を得て規格(仕様書)を制定し、当該規 格を普及する⾏為
  ・標準化によって、⼀定の⽔準の製品・サービスを提供する事業者が増え、当該市場が拡⼤する可能性がある。
  ・標準化によって、粗悪品や類似商品の排除、製品・サービスの質の保証が実現される可能性がある。
  ```
  - ### 雛型を作成する

- ### [subject-231220](https://github.com/winofsql/subject-231220)

- ## WEBアプリに必要で重要なテクニカル
  - ### スマホ対応とUI の標準化
    - [Bootstrap](https://getbootstrap.jp/docs/5.3/getting-started/introduction/)
      - UI に使用するクラス
        - [ボタン](https://getbootstrap.jp/docs/5.3/components/buttons/)
          ![image](https://github.com/winofsql/subject-231221/assets/1501327/64bf7cfc-c688-40d3-a179-3d9ee78e37c7)
        - [タイトル](https://getbootstrap.jp/docs/5.3/components/alerts/)\
          ![image](https://github.com/winofsql/subject-231221/assets/1501327/29f14559-eecc-47a9-a97b-1aa5ede9cd88)
    - 🔴 スマホ対応は CSS で行う
      ```css
      /* PC 用 */
      @media screen and ( min-width:480px ) {
      
          input {
              width: 400px;
          }
          textarea {
              width: 500px;
              height: 100px;
          }
      }
      
      /* スマホ 用 */
      @media screen and ( max-width:479px ) {
      
          input,textarea {
              width:100%;
      
          }
      
          textarea {
              height: 70px;
          }
      }       
      ```

 - ### IFRAME を画面下部にフィットさせる CSS と PHP の埋め込み
    ```css
    /* IFRAME で表示する */
    html,body {
        height: 100%;
    }
    
    body {
        margin: 0;
    }
    /* PC 用 */
    @media screen and ( min-width:480px ) {
    
      #bbs {
          height: <?= $view_head_height ?>px;
      }
      
      #extend {
          height: calc( 100% - <?= $view_head_height ?>px - 2px );
      }
    
    }
    /* スマホ 用 */
    @media screen and ( max-width:479px ) {
    
      #bbs {
          height: <?= $view_head_height + 40 ?>px;
      }
      
      #extend {
          height: calc( 100% - <?= $view_head_height - 40 ?>px - 2px );
      }
    }   
    ```  

  - ### Ajax( jQuery )
    - GET
      ```javascript
      $.ajax({
        url: "syain.php",
        cache: false,
        data: { "type": "get" }
      })
      .done(function( data, textStatus ){
        console.log( "status:" + textStatus );
        console.log( "data:" + JSON.stringify(data, null, "    ") );
      
        $("#message").text( data.length  + "件のデータがあります" );
        loadTable( data );
      })
      // 失敗
      .fail(function(jqXHR, textStatus, errorThrown ){
        console.log( "status:" + textStatus );
        console.log( "errorThrown:" + errorThrown );
      
        // エラーメッセージを表示
        alert("システムエラーです");
      
      })
      // 常に実行
      .always(function() {
      })
      ;
      ```

    - POST
      ```javascript
      $.ajax({
          url: "./upload.php",
          type: "POST",
          data: formData,
          processData: false,  // jQuery がデータを処理しないよう指定
          contentType: false   // jQuery が contentType を設定しないよう指定
      })
      .done(function( data, textStatus ){
          console.log( "status:" + textStatus );
          console.log( "data:" + JSON.stringify(data, null, "    ") );
    
      })
      .fail(function(jqXHR, textStatus, errorThrown ){
          console.log( "status:" + textStatus );
          console.log( "errorThrown:" + errorThrown );
      })
      .always(function() {
    
          // 操作不可を解除
          $("#content input").prop("disabled", false);
      })
      ;      
      ```
      - FormData
        ```javascript
        var formData = new FormData();

        // 画像データサイズの制限
        formData.append("MAX_FILE_SIZE", 10000000);

        var file_cnt = 0;
        var target = $("#target").get(0);

        // formData に全てのファイルを追加
        for( i = 0; i < target.files.length; i++ ) {

            formData.append("image"+(i+1), target.files[i]);

            file_cnt++;

        }
        
        formData.append("FILE_COUNT", file_cnt );
        ``` 
