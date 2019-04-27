# ワンナイト人狼GMbot ver1.0

discord.pyと自作モジュールによって動くDiscord Bot用のスクリプトです。不具合だらけですのでよく読んでプレイしてください(可能であれば「占い師の操作」以下をプレイヤーにも読ませておいてください)。
上限人数の増加、役職の追加、カード構成の変更などはmanagement/JobManager.pyを編集してください(行動を持つ役職などについてはGameManagerやPlayerManagerも編集する必要があります)。
メッセージのほとんどはmanagement/GameManager.pyに記載してありますので、内容変更の際はそちらからどうぞ。

現在の設定はプレイヤー人数4〜8、カード構成はJobManagerを参照してください。

## 遊び方

 1. ids.txtの内容をDiscordのbot管理画面からコピーできるトークンとゲームを行うテキストチャンネルのIDに書き換えてください。
 2. `$ python botclient_1_0.py`でbotを起動します。
 3. ゲームは常に待機状態です。`!参加`をそれぞれのプレイヤーが送ることで参加が受理され、プレイヤー人数が更新されていきます。またすでに参加しているプレイヤーがもう一度送ることで参加を取り消すこともできます。参加していないプレイヤーは`!観戦`を送ることでゲーム開始時に配布された役職の内容を知ることができます。
 4. 人数が揃い準備が整ったら、`!開始`でゲームが開始されます。このときからノンストップでゲームが進行するので、全員の準備が完了していることを確認してから送ってください。`!開始 7`のように開始コマンドの後ろに半角スペースを開けて数字を付けることで討論時間(分)を設定することができます。付けない場合はデフォルトの5分でゲームが開始されます。
 5. ゲーム開始の合図がメインチャンネルに送られ、ダイレクトメッセージにて各参加者に役職が通知されます。この時点から夜明けのメッセージが送られるまで各プレイヤーはマイクミュートすることを推奨します。
 7. 役職の通知の10秒後、夜になります。占い師、怪盗、人狼、共有者のプレイヤーにそれぞれメッセージが送られます。占い師と怪盗のプレイヤーは送られたメッセージに従ってコマンドを送ることで行動ができます(行動しないことも可能です)。人狼と共有者のプレイヤーには自分の仲間がいるかどうか、仲間が誰なのかが通知されます。夜は30秒で終了しますので、占い師と怪盗のプレイヤーは考えすぎないようにしてください。
 8. 30秒後、夜明けとなり討論の時間になります。設定された討論時間に応じてゲーム内のタイマーが1分ごとに通知を送ります。このタイマーは目安ですので、タイマーが終了するまでに投票アクションを行うことも可能です。
 9. 討論の時間が終了したら、`!投票 0`のように投票先のIDを半角スペースを開けて指定して投票を行います(また、タイマーが終了したのを確認して各プレイヤーはマイクミュートすることを推奨します)。DMに送ることを推奨しますが、オープンのテキストチャンネルでも可能です。平和村に投票する場合は`!投票 p`と送ってください。
 10. 投票が終了したら、`!結果`で試合結果を開示します。誰が吊られたか(複数同票の場合その中からランダムで吊られます)、そして最終的なプレイヤー全員の役職と勝敗が開示されます。
 11. 試合結果の開示をもってゲームの終了となりますが、この状態では参加プレイヤーの情報は保持しています。同じメンバーで再度試合を始めるにはそのまま`!開始`を送ってください。この状態で他のプレイヤーが追加で`!参加`することもできます。
 14. botの終了には`!じゃあな`をご利用ください。

## 占い師の操作

夜になると、占い師のプレイヤー宛に
>占う対象を選び、以下のコマンドを送ってください：

>「!占い 0」: @PLAYER1

> 「!占い 1」: @PLAYER2

> 「!占い 3」: @PLAYER4

> 「!占い d」: 山札

というようなメッセージがダイレクトメッセージで送られてきます。表示されているコマンドをコピーして(手打ちしても問題ありませんが)返信してください。

>@PLAYER2は怪盗です。

というように占いの結果が返ってきます。

占いは一度しか行えませんので占い先の変更はできません。

エラー処理をしていないので最初にゲームを開始したテキストチャンネルなどにコマンドを送ってもそのメッセージは消えません(占い結果はDMに返ってきます)。デバッグでなければゲーム崩壊ものですのでコマンドは**絶対にダイレクトメッセージで送ってください**。

占いの結果は**個人を占う場合も山札を占う場合も役職が判明します**(人間か人狼かのみ判明ではありません)。

## 怪盗の操作

夜になると、怪盗のプレイヤー宛に
>役職を盗む対象を選び、以下のコマンドを送ってください。

>「!怪盗 0」: @PLAYER1

>(以下略)

というようなメッセージがダイレクトメッセージで送られてきます。表示されているコマンドをコピーして(手打ちしても問題ありませんが)返信してください。

>@PLAYER1と役職を入れ替えました。あなたは現在人狼です。

というようにカード交換の結果が返ってきます。

怪盗は一度しか行なえませんのでカード交換先の変更はできません。

怪盗の結果は**役職が判明します**(人間か人狼かのみ判明ではありません)。

## 人狼の操作

夜になると、人狼のプレイヤー宛に
>人狼はあなた一人です。

または
>人狼はあなたと@PLAYER1の二人です。

というようなメッセージがダイレクトメッセージで送られてきます(メッセージ内容は怪盗の行動前のものです)。

人狼にはコマンド操作はありませんので返信する必要はありません。

## 共有者の操作

夜になると、共有者のプレイヤー宛に
>共有者はあなた一人です。

または
>共有者はあなたと@PLAYER1の二人です。

というようなメッセージがダイレクトメッセージで送られてきます(メッセージ内容は怪盗の行動前のものです)。

共有者にはコマンド操作はありませんので返信する必要はありません。

## 投票の操作

工事中なので投票のコマンドを通知するメッセージを作り忘れましたが、ゲーム開始時のメッセージのメンションの順番に0, 1, 2, ...がプレイヤーのIDです。`!投票 1`というようにIDをつけてコマンドを送れば投票が完了します。

最初にゲームを開始したテキストチャンネルに投票コマンドを送っても問題ありません。ただし投票は開示まで伏せられているもの、というルールを厳密にするのであれば**絶対にダイレクトメッセージで投票してください**。

全員の投票が済んでいない場合、`!結果`してもエラーを吐きます。

処理の関係上、どうしても何人かのプレイヤーについて`!投票`コマンドを受け付けなくなってしまうことがあります(短い時間に多くのプレイヤーがコマンドを送ると陥りがちです)。その際はゲームマスターの方がメインのテキストチャンネルにて`!GM投票 0 1`というようにコマンドを送ることで投票を代わりに行うことが出来ます。内容は`!投票 [投票を行うプレイヤーのID] [投票先のプレイヤーのID]`です。前の例では1番目のプレイヤーが2番目のプレイヤーに投票するアクションを代わりに行っています。

## その他

discord.pyの仕様により、アンドロイド版のDiscordではプレイヤー名が正しく表示されません。アンドロイド版のDiscordから参加しているプレイヤーはPCでの参加に誘導してください、