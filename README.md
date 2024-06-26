# subject-12-21-023-webapp-misc

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

  - ### JavaScript の基本テクニック
    - ローカルのテキストファイルを読み込む
      - var reader = new FileReader();
        ```javascript
        // ファイルオブジェクト
        var file = $("#file").get(0).files[0];

        // ファイルを読み込む為のオブジェクト
        var reader = new FileReader();

        // ファイルが読込み完了時に行う処理をイベントとして登録
        reader.onload = function(reader_event) {
            $("#text").val(reader_event.target.result);
        };

        // キャラクタセットに合わせてテキストとして読み込み
        if ( encoding == "EUCJP" ) {
            reader.readAsText(file, "euc-jp");
        }
        if ( encoding == "SJIS" ) {
            reader.readAsText(file, "shift_jis");
        }
        if ( encoding == "UTF8" ) {
            reader.readAsText(file);
        }        
        ```
    - ローカルにファイルを保存する
      - FileSaver ライブラリを使用すると簡単に実装可能 ( 以下は UTF-8 の場合 )
        ```javascript
        saveAs(
            new Blob(
                [str]
                , {type: "text/plain;charset=utf-8"}
            )
            , "保存ファイル.txt"
        );

        ```


    - localStorage にテキストデータを保存( & 読み込み )
      ```javascript
      // *************************************
      // jQuery UI の DatePicker 用
      // *************************************
      var datepicker_option = {
          dateFormat: 'yy/mm/dd',
          dayNamesMin: ['日', '月', '火', '水', '木', '金', '土'],
          monthNames: ["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"],
          changeMonth: true,
          showMonthAfterYear: true,
          yearSuffix: '年',
          changeYear: true,
          showAnim: 'fadeIn',
          yearRange: "c-70:c"
      }
      
      $(function(){
      
          // ***************************
          // DatePicker
          // ***************************
          $("#key").datepicker( datepicker_option );
      
          // ***************************
          // 日付変更でデータがあれば表示
          // ***************************
          $("#key").on("change", function(){
      
              if (  ($("#key").val()).trim() != "" ) {
                  if ( typeof(localStorage[$("#key").val()]) != 'undefined' ) {
                      $("#text").val(localStorage[$("#key").val()]);
                  }
                  // データが無い場合は本文クリア
                  else {
                      $("#text").val("");
                  }
              }
      
          });
      
          // ***************************
          // 保存ボタンで更新
          // ***************************
          $("#save").on("click", function(){
      
              if ( $("#text").val() == "" ) {
                  return;
              }
      
              localStorage[$("#key").val()] = $("#text").val();
      
          });
      
      
      });
      ```
        
    - クリップボードにテキストをコピー
      - clipboard.js ( 外部ライブラリ )
        ```javascript
        var clipbpardText = "";
        var clipboard;
        $(function(){
        
            // **********************************************
            // テーブルデータ用クリップボード処理オブジェクト
            // **********************************************
            clipboard = new ClipboardJS('.clip-btn' , {
                text: function(trigger) {
                    // clipboard.js に渡す( このデータがクリップポードに転送される )
                    return clipbpardText;
                }
            });
        
            // **********************************************
            // クリップボードへの転送に成功した時に
            // 実行されるイベント( 無くても良い )
            // **********************************************
            clipboard.on('success', function(e) {
                alert("テーブルデータをクリップボードにコピーしました");
            });
        
            $("#save").on("click", function(){
                // **********************************************
                // clipbpardText にクリップボードに転送したい文字列をセットする
                // **********************************************
                var work = "";
        
                $("table tr").each( function( row_cnt ){
        
                    $(this).find("td,th").each(function( col_cnt ){
                        if ( col_cnt != 0 ) {
                            work += "\t";
                        }
                        work += $(this).text();
        
                    });
                    work += "\r\n";
        
                });
        
                clipbpardText = work;
            })
        
        });
        ```

    - CSV をテーブルにして表示
      ```javascript
      $("#action_load").on( "click", function(){
      
          // 既存の表示データを完全クリア
          $("#tbl").html("");
      
          // FileReader は毎回作成(同時に複数のファイルを扱えない)
          var reader = new FileReader();
      
          // FileReader にデータが読み込まれた時のイベント
          var rows = "";
          var cols = "";
          var tr = null;
          $(reader).on("load", function () {
      
              // \r を全て削除
              var data = this.result.replace(/\r/g,"");
      
              // \n で行を分ける
              rows = this.result.split("\n");
              $.each( rows, function( idx, value ){
                  // 空行を無視
                  if ( value == "" ) {
                      return;
                  }
                  cols = value.split(",");
                  // 行を作成
                  tr = $("<tr></tr>").appendTo("#tbl");
                  $.each( cols, function( idx, value ){
                      // TD を追加して、テキストをセット
      
                      switch( idx ) {
                          case 7:
                          case 8:
                              // 数値項目はカンマ編集で右寄せ
                              $("<td></td>").appendTo(tr)
                                  .text(value)
                                  .css({"text-align": "right" });
                              break;
      
                          default:
                              $("<td></td>").appendTo(tr)
                                  .text(value);
                      }
      
                  } )
              } )
          });
      
          reader.readAsText($("#target").get(0).files[0],"shift_jis");
          // reader.readAsText($("#target").get(0).files[0],"utf-8");
      
      });
      ```
