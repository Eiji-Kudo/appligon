```erDiagram
    CUSTOMER_MST {
        string 顧客ID PK
        string ユーザ名
        string 氏名
        string email
        string 年代
        string 性別
        string 職業
        datetime 登録日
        string 利用店舗ID FK
        boolean 退会済みフラグ
    }
    CUSTOMER_ACTIVITY_SUMMERY { #クーポン履歴やスタンプ履歴などの複数の他のHISTORYテーブルで取得できる情報をサマリーとして表示するテーブル
        string 顧客ID PK
        datetime 最終ログイン日
        int 月間来店数
        int 合計来店数
        int スタンプ数_現在
        int スタンプ数_過去合計
        datetime スタンプ最終利用日
        int おみくじ利用数
        datetime おみくじ最終利用日
        int クーポン利用数
        datetime クーポン最終利用日
        boolean Google口コミ完了フラグ
        datetime Google口コミ完了日時
        boolean SNS口コミ完了フラグ
        datetime SNS口コミ完了日時
    }
    STORE_MST {
        string 店舗ID PK
        string 店舗名
        string 住所
        string 電話番号
        string 営業時間
        string 定休日
        int 座席数
        int 駐車場台数
        datetime 開店日
        datetime 閉店日
    }
    STAMP_GET_HISTORY {
        string 獲得履歴ID PK
        string 顧客ID FK
        string 店舗ID FK
        int 獲得スタンプ数
        datetime スタンプ獲得日時
    }
    STAMP_USE_HISTORY {
        string 使用履歴ID PK
        string 顧客ID FK
        int 使用スタンプ数
        string 交換クーポンID FK
        datetime スタンプ使用日時
    }
    COUPON_MST {
        string クーポンID PK
        string クーポン名
        string 説明
        int 必要スタンプ数
        string 利用可能店舗ID FK #複数の店舗IDが入る想定
        datetime 新規導入日
        int 有効期限日数
    }
    
    COUPON_GET_HISTORY {
        string 獲得履歴ID PK
        string 顧客ID FK
        string クーポンID FK
        datetime クーポン獲得日時
    }
    
    COUPON_USE_HISTORY {
        string 使用履歴ID PK
        string 顧客ID FK
        string クーポンID FK
        string 店舗ID FK
        datetime クーポン使用日時
    }
    LOTTERY_MST {
        string おみくじID PK
        string おみくじ名
        string 説明
        datetime 新規導入日
    }
    
    LOTTERY_USE_HISTORY {
        string 利用履歴ID PK
        string 顧客ID FK
        string 獲得おみくじID FK
        datetime おみくじ利用日時
    }
    
    PUSH_NOTIFICATION_MST {
        string プッシュ通知ID PK
        string 内容
        string 種類
    }
    
    PUSH_NOTIFICATION_CLICK_HISTORY {
        string クリック履歴ID PK
        string 顧客ID FK
        string プッシュ通知ID FK
        datetime クリック日時
    }
    INFORMATION_MST {
        string お知らせID PK
        string タイトル
        string 詳細
        string リンク
        string 対象店舗ID FK #複数の店舗IDが入る想定
        datetime 配信日時
    }
    
    INFORMATION_CHECK_HISTORY {
        string お知らせ確認履歴ID PK
        string 顧客ID FK
        string お知らせID FK
        datetime お知らせ確認日時
    }
    EXTERNAL_SITE_BANNER_MST { #ホーム画面に表示のSNSページや店舗公式サイト、店舗採用ページなどの外部サイトへの遷移バナーのマスタ
        string 遷移バナーID PK
        string タイトル
        string リンク
    }
    
    EXTERNAL_SITE_BANNER_CLICK_HISTORY { #外部サイトへの遷移バナーのクリック日時
        string クリック履歴ID PK店舗採用ページなどの
        string 顧客ID FK
        string 遷移バナーID FK
        datetime クリック日時
    }
    MENU_MST {
        string メニューID PK
        string メニュー名
        int 価格
        string カテゴリ
        string 説明
    }
    
    # 顧客マスタ関連
    CUSTOMER_MST ||--||{ CUSTOMER_ACTIVITY_SUMMERY : "顧客ID"
    STORE_MST ||--o{ CUSTOMER_MST : "利用店舗ID"
    # スタンプ関連
    CUSTOMER_MST ||--o{ STAMP_GET_HISTORY : "顧客ID"
    STORE_MST ||--o{ STAMP_GET_HISTORY : "店舗ID"
    CUSTOMER_MST ||--o{ STAMP_USE_HISTORY : "顧客ID"
    COUPON_MST ||--o{ STAMP_USE_HISTORY : "交換クーポンID"
    # クーポン関連
    STORE_MST ||--o{ COUPON_MST : "利用可能店舗ID"
    CUSTOMER_MST ||--o{ COUPON_GET_HISTORY : "顧客ID"
    STORE_MST ||--o{ COUPON_GET_HISTORY : "店舗ID"
    COUPON_MST ||--o{ COUPON_GET_HISTORY : "クーポンID"
    CUSTOMER_MST ||--o{ STAMP_USE_HISTORY : "クーポン利用"
    COUPON_MST ||--o{ COUPON_USE_HISTORY : "クーポンID"
    # おみくじ関連
    CUSTOMER_MST ||--o{ LOTTERY_USE_HISTORY : "顧客ID"
    LOTTERY_MST ||--o{ LOTTERY_USE_HISTORY : "獲得おみくじID"
    # プッシュ通知関連
    CUSTOMER_MST ||--o{ PUSH_NOTIFICATION_CLICK_HISTORY : "顧客ID"
    PUSH_NOTIFICATION_MST ||--o{ PUSH_NOTIFICATION_CLICK_HISTORY : "プッシュ通知ID"
    # お知らせ関連
    STORE_MST ||--o{ INFORMATION_MST : "対象店舗ID"
    CUSTOMER_MST ||--o{ INFORMATION_CHECK_HISTORY : "顧客ID"
    INFORMATION_MST ||--o{ INFORMATION_CHECK_HISTORY : "お知らせID"
    # 外部サイト遷移バナー関連
    CUSTOMER_MST ||--o{ EXTERNAL_SITE_BANNER_CLICK_HISTORY : "顧客ID"
    EXTERNAL_SITE_BANNER_MST ||--o{ EXTERNAL_SITE_BANNER_CLICK_HISTORY : "遷移バナーID"
```