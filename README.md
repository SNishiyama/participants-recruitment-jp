# participants-recruitment-jp
An easy way of recruiting participants.

本ハックは、中規模までの心理学実験を行う際に、参加者の募集・連絡を円滑に行うものです。
google sites、google form、google calendar及びgmailを利用することで、参加者の募集を行うことができます。
心理学実験以外にも参加者の募集・連絡に用いることもできます。

なお、her0m31さんの「Google Form -> SpreadSheet -> Calendar && Mailで簡単な予約フォームを作る。」
http://qiita.com/her0m31/items/0a67d52179341380dd31
のご説明・コードを大変参考にさせていただきました。

### 何ができるのか？
1. 参加者は、空き時間を確認し、空いている時間帯に心理学実験の仮予約をすることができます。
1. 実験者は、仮予約された時間帯をスプレッドシート上で確認し、予約完了を1と入力することで行うことができます。
1. これに連動して、参加者には実験日時など必要な情報が記載されたメールが送信されます。

### 何ができないのか？
1. 参加者が既に埋まっている時間帯に仮予約をすることを防ぐことはできません。恐らく、動的にWebページを変化させる必要があり、本ハックの範疇を超えています。

## 導入
### googleアカウントを予め取っておいて下さい
本コードで使用するメールアドレスはGmailのものとします。

### participants-recruitment-jpのコードをダウンロードして下さい
本ページの右上にある緑の"Clone or download"より"Download ZIP"を選択してダウンロードすることができます。

### ダウンロードしたmain.gsファイルをメモ帳などのテキストエディタで開き、以下の変数の値を変更し保存しておいて下さい

sendToCalendar関数内の変数（2行目以降）

| 変数名 | 説明 |
|:---|:---|
| experimenterMailAddress | 実験者のGmailアドレスを入れてください |

updateCalendar関数内の変数（36行目以降）

| 変数名 | 説明 |
|:---|:---|
| experimenterName | 実験者の名前を入れてください |
| experimenterMailAddress | 実験者のGmailアドレスを入れてください |
| experimenterPhone | 実験者の電話番号を入れてください |
| experimentRoom | 実験室の名前を入れてください |
  
### google sitesで参加者を募集するためのページを作成して下さい
完成例として、https://sites.google.com/site/ishiguroshinri/ をご覧ください。
  
### ページに、参加者の情報及び希望日時を入力してもらう質問フォームを配置します
- googleフォームを利用して質問フォームを作成します。
- まず、googleフォームと検索して出てくるページに飛んで、googleフォームを使うをクリックし、その次のページの右下の赤い「＋」ボタンをクリックしてフォームを作成して下さい。
- googleフォームでは名前、メールアドレスを記述式の形式で尋ね、希望日時については日付を問う形にして下さい。項目をクリックすると質問の形式が選択できます。
- 日付に関しては右下の設定ボタンをクリックして時刻を含めるにもチェックを入れて下さい。また、設定ボタンより説明をクリックすると説明文を加えることができます。実験期間について説明文を加えておくと良いと思います。
- また、それぞれの回答を必須の回答にしておくことも忘れないで下さい。
- 次に、フォームで得られたデータをスプレッドシートに送るように設定します。回答タブから右上に表示されるスプレッドシートの作成をクリックし、スプレッドシートを作成します。以降、フォームに入力されたデータはスプレッドシートに反映されるようになります。
- フォームが作成できましたらページに配置して下さい。

### 同様に実験で使用するgoogleカレンダーもこのページに配置します
- googleカレンダーのページに飛んでください。
- ページ左側にカレンダー一覧がありますが、今回はマイカレンダーの一番上にあるカレンダーを使用します。
- このカレンダーの右に現れる矢印をクリックし、「このカレンダーを共有」を選択して下さい。
- ページが変わりますので、このカレンダーを一般公開する、予定の時間枠のみを一般に公開（詳細は非表示、検索の対象にもならない）のどちらにもチェックを入れて下さい。
#### 注意
**イベント名に参加者の名前が入るので個人情報を表示しないために、必ず予定の時間枠のみを一般に公開にはチェックを入れて下さい。**

- カレンダーが作成できましたらページに配置して下さい。

### スプレッドシートでmain.gsを走らせて下さい
- 質問フォームの回答が記録されるスプレッドシートを開き、ツール->スクリプトエディタをクリックして下さい。
- スクリプトエディタが開かれますので、ここにmain.gsの内容を貼り付けます。

### main.gsが実行されるように設定して下さい
- スクリプトエディタの編集->現在のプロジェクトのトリガーを選択し、新しいトリガーをクリックし、以下の項目を設定して下さい。

| 実行 | イベント |
|:---|:---|
| sendToCalendar |スプレッドシートから　フォーム送信時 |
| updateCalendar | スプレッドシートから　値の変更 |

### 以上で導入は終了です

## 運用
### 予約を完了させます
一度、参加者役になって仮予約をして下さい。仮予約ができましたら、スプレッドシート上にデータとして回答が記録されます。データは、

| タイムスタンプ | お名前（ふりがな） | メールアドレス | 希望日時 | 予約ステータス | 連絡したか |
|---:|:---|:---|---:|---:|---:|
| 2017/04/05 12:30:35 | 参加者太郎（さんかしゃたろう） | pro.sankasha@sample.com | 2017/04/09 16:00:00 |  |  |

という形にします。まず、スプレッドシートに5列と6列を追加してください。その後、5列目と6列目の列名を「予約ステータス」や「連絡したか」という列名に変更しておいて下さい。

#### 注意
現段階では、
**仮予約の時間帯が重複していても仮予約はできてしまう状況になっています。**
実験者が実験可能で他の参加者との重複もないかを確認して、大丈夫そうでしたら半角数字1を予約ステータスに入れてください（重複の確認はスプレッドシートを並び替えたり、関数を使用すればチェックできると思います）。

半角数字1が入力されると、参加者名や希望日時の情報を取得され、参加者に実験予約完了のメールが送信されます。なお、送信される内容は、
```js
var text = ParticipantName + "様\n\nこの度は心理学実験への応募ありがとうございました。\n" +
          hizuke + "からの心理学実験の予約が完了しましたのでメールいたします。\n" +
          "場所は" + grocio.experimentRoom + "です。当日は直接お越しください。\n" +
          "ご不明な点などありましたら、" + grocio.experimenterMailAddress +"までご連絡ください。\n" +
          "当日もよろしくお願いいたします。\n\n実験責任者 " + grocio.experimenterName +
          "（当日は他の者が実験担当する可能性があります）\n" +
          "当日の連絡は" + grocio.experimenterPhone + "までお願いいたします。";
```
としています。メールの本文はこのような感じになっていますが、変えたい方は上記の内容を変更して下さい。なお、\nは改行を示す特殊な記号です。

メールが送信されたら、参加者に送った内容と同じ内容のメールが実験者にも届くようになっています。メールが送信できた場合には、連絡したかという列に1という数字が表示されます。また、カレンダー上で「予約完了：参加者名」というイベントが作成されます。

## 免責事項
作成者は、本ハックを利用することにより生じる一切の損害、損失の責任を負いません。

## ライセンス
2条項BSDライセンスとします。
